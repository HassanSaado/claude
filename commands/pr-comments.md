---
description: Address, fix, and resolve PR review comments. Auto-fixes "claude fix" comments, handles actionable feedback, summarizes discussions.
argument-hint: [PR# | owner/repo#PR | URL | empty for current branch]
allowed-tools: [Bash, Read, Edit, Write, Grep, Glob, AskUserQuestion]
---

# PR Comment Reviewer

You are addressing review comments on a pull request. Your job is to:
1. Fetch and categorize all comments
2. Auto-fix comments marked with "claude fix"
3. Present actionable comments for user decision
4. Provide clear progress updates throughout
5. Resolve threads on GitHub after addressing them

## CRITICAL: Progress Updates
After EVERY action, print a clear status update:
- `✅ FIXED & RESOLVED: [file:line] - brief description`
- `⏭️ SKIPPED: [file:line] - reason`
- `💬 REPLIED: [file:line] - summary`
- `🔍 REVIEWING: [file:line] - comment preview`

## Step 1: Identify the PR (MUST DO FIRST)

Parse `$ARGUMENTS` to determine the PR. Supported formats:
- Empty → use current branch's PR
- `123` → PR number in current repo
- `owner/repo#123` → PR in specific repo
- `owner/repo 123` → PR in specific repo
- `https://github.com/owner/repo/pull/123` → full URL

**If arguments provided:**
```bash
# For full URL - extract owner, repo, number from URL
# For owner/repo#123 - parse and use: gh pr view 123 --repo owner/repo
# For just number - use current repo: gh pr view 123
```

**If no arguments:**
```bash
gh pr view --json number,title,url,headRefName,baseRefName 2>/dev/null
```

**If no PR found:** Ask user to provide PR number or URL.

## Step 2: Confirm PR Details

Get full PR info and repository context:
```bash
# Get repo info
gh repo view --json owner,name --jq '"\(.owner.login)/\(.name)"'

# Get PR details
gh pr view {number} --repo {owner/repo} --json number,title,url,headRefName,baseRefName,author,state
```

**IMMEDIATELY show the user which PR you're working on:**

```
╔═══════════════════════════════════════════════════════════╗
║  🔍 PR IDENTIFIED                                         ║
╠═══════════════════════════════════════════════════════════╣
║  Repository:  {owner}/{repo}                              ║
║  PR #:        {number}                                    ║
║  Title:       {title}                                     ║
║  Author:      @{author}                                   ║
║  Branch:      {head} → {base}                             ║
║  Status:      {state}                                     ║
║  URL:         {url}                                       ║
╚═══════════════════════════════════════════════════════════╝
```

**Wait for this to display before proceeding.** This confirms to the user exactly which PR will be modified.

## Step 3: Fetch All Review Comments

Get review comments (comments on specific code lines):
```bash
gh api repos/{owner}/{repo}/pulls/{number}/comments --jq '.[] | {id, path, line: (.line // .original_line), body, user: .user.login, in_reply_to_id, pull_request_review_id, created_at, html_url}'
```

Get review threads to find thread IDs for resolving:
```bash
gh pr view {number} --json reviewDecision,reviews,comments
```

Also get pending review threads with their resolution status:
```bash
gh api graphql -f query='
query($owner: String!, $repo: String!, $pr: Int!) {
  repository(owner: $owner, name: $repo) {
    pullRequest(number: $pr) {
      reviewThreads(first: 100) {
        nodes {
          id
          isResolved
          isOutdated
          path
          line
          comments(first: 10) {
            nodes {
              id
              body
              author { login }
              createdAt
            }
          }
        }
      }
    }
  }
}' -f owner="{owner}" -f repo="{repo}" -F pr={number}
```

## Step 4: Display Comment Summary

After fetching comments, show what you found:

```
───────────────────────────────────────────────────────────
📝 COMMENTS FOUND: {total} unresolved threads
───────────────────────────────────────────────────────────

Found {total} unresolved comment threads:

🔴 AUTO-FIX ({count}) - Will address automatically
   Comments containing "claude fix", "cf:", "@claude fix"

🟡 ACTIONABLE ({count}) - Need your decision
   Direct code feedback and suggestions

🟢 DISCUSSION ({count}) - Informational only
   Questions, praise, general conversation

═══════════════════════════════════════════════════════════
```

## Step 5: Categorize Comments

### 🔴 AUTO-FIX Detection
Match comments containing (case-insensitive):
- `claude fix`
- `claude: fix`
- `@claude fix`
- `cf:` (shorthand at start of comment)
- `claude,? (please)? fix`

### 🟡 ACTIONABLE Detection
Comments that:
- Contain words like: "should", "could", "consider", "change", "update", "fix", "bug", "issue", "wrong", "incorrect", "missing", "add", "remove"
- Are suggestions (GitHub suggestion blocks)
- Request specific changes
- NOT already marked as "claude fix"

### 🟢 DISCUSSION Detection
Comments that:
- Are questions without actionable request
- Contain "LGTM", "looks good", "nice", "great", "thanks"
- Are acknowledgments or explanations
- Are replies in a conversation thread

## Step 6: Process AUTO-FIX Comments

For each "claude fix" comment:

1. Print: `🔍 REVIEWING: [{path}:{line}] - "{first 50 chars of comment}..."`

2. Read the file and understand the context (read 20 lines around the comment)

3. Understand what fix is needed from the comment

4. Make the fix using Edit tool

5. Resolve the thread:
```bash
gh api graphql -f query='
mutation($threadId: ID!) {
  resolveReviewThread(input: {threadId: $threadId}) {
    thread { isResolved }
  }
}' -f threadId="{thread_id}"
```

6. Print: `✅ FIXED & RESOLVED: [{path}:{line}] - {what you fixed}`

## Step 7: Process ACTIONABLE Comments

For each actionable comment, present it clearly:

```
───────────────────────────────────────
🟡 ACTIONABLE [{n}/{total}]
📁 {path}:{line}
👤 @{reviewer}
───────────────────────────────────────
{full comment text}
───────────────────────────────────────
```

Then use AskUserQuestion to ask:
- **Fix & Resolve** - Address this feedback and resolve the thread
- **Skip** - Leave for later / disagree with feedback
- **Reply** - Respond without code changes
- **Show Code** - See context before deciding

Based on choice:
- **Fix & Resolve**: Read file, make fix, resolve thread, print `✅ FIXED & RESOLVED`
- **Skip**: Print `⏭️ SKIPPED: [{path}:{line}] - User chose to skip`
- **Reply**: Ask for reply text, post it:
  ```bash
  gh api repos/{owner}/{repo}/pulls/{number}/comments/{comment_id}/replies -f body="{reply}"
  ```
  Print `💬 REPLIED: [{path}:{line}]`
- **Show Code**: Read file around that line, then ask again

## Step 8: Summarize Discussion Comments

Print a summary of discussion comments (no action needed):
```
───────────────────────────────────────
🟢 DISCUSSION COMMENTS (no action needed)
───────────────────────────────────────
• [{path}] @{user}: "{preview}..."
• [General] @{user}: "LGTM!"
───────────────────────────────────────
```

Ask if user wants to reply to any of these.

## Step 9: Final Report

Print comprehensive summary:

```
═══════════════════════════════════════════════════════════
📊 FINAL SUMMARY - PR #{number}
═══════════════════════════════════════════════════════════

✅ FIXED & RESOLVED ({count})
   • [{path}:{line}] {description}
   • [{path}:{line}] {description}

⏭️ SKIPPED ({count})
   • [{path}:{line}] {reason if given}

💬 REPLIED ({count})
   • [{path}:{line}]

🟢 DISCUSSION ({count}) - No action needed

═══════════════════════════════════════════════════════════
📝 NEXT STEPS:
═══════════════════════════════════════════════════════════
{If changes were made:}
1. Review changes: git diff
2. Commit: git add . && git commit -m "address PR review comments"
3. Push: git push

{If no changes:}
All comments have been addressed or skipped. No code changes made.
═══════════════════════════════════════════════════════════
```

## Important Rules

1. **Always resolve threads after fixing** - Don't just fix code, mark the thread resolved
2. **Read before fixing** - Always read the file context before making changes
3. **Never guess** - If a "claude fix" comment is ambiguous, ask the user
4. **Atomic updates** - Print status immediately after each action, not batched
5. **Preserve user intent** - For skipped items, the user made a conscious choice
6. **Don't over-fix** - Only address what the comment asks for, nothing more
7. **Handle outdated comments** - If a thread is on outdated code, note this to user

## Error Handling

- If `gh` is not authenticated: Tell user to run `gh auth login`
- If PR not found: Ask user for correct PR number
- If thread resolution fails: Note it but continue with other comments
- If file doesn't exist (deleted): Skip and note to user

## User Arguments
$ARGUMENTS
