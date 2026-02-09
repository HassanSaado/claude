# Claude Settings

This repository contains my Claude Code settings and skills.

## Structure

- `skills/` - Custom skills (invocable via `/skill-name`)
- `settings.json` - Plugin and feature settings

## Setup

These settings are symlinked from `~/.claude/` to this repository:
- `~/.claude/skills` → `~/claude/skills`
- `~/.claude/settings.json` → `~/claude/settings.json`
- `~/.claude/settings.local.json` → `~/claude/settings.local.json`

Runtime data (cache, history, credentials, etc.) remains in `~/.claude/`.

Project-specific `claude.md` files remain in their respective project directories.

---

## Skills

Skills are modular capabilities that extend Claude's functionality. Each skill is a folder containing a `SKILL.md` file with instructions, plus optional supporting files like scripts and templates.

> All slash commands live as skills. Each skill directory contains a `SKILL.md` and becomes invocable via `/skill-name`.

### Skill Structure

```
skills/
└── my-skill-name/
    ├── SKILL.md          # Required - main instructions
    ├── reference.md      # Optional - additional docs
    ├── examples.md       # Optional - usage examples
    ├── scripts/          # Optional - utility scripts
    │   └── helper.py
    └── templates/        # Optional - templates
        └── template.txt
```

### Creating a Skill

Create a `SKILL.md` file with YAML frontmatter and markdown content:

```markdown
---
name: my-skill-name
description: Brief description of what this skill does and when to use it.
---

# My Skill Name

## Instructions
Step-by-step guidance for Claude.

## Examples
Concrete examples of using this skill.
```

### Frontmatter Fields

| Field | Purpose | Default |
|-------|---------|---------|
| `name` | Skill name (becomes `/skill-name`). Lowercase, numbers, hyphens only. Max 64 chars. | **Required** |
| `description` | What the skill does and when to use it. Max 1024 chars. Claude uses this to decide when to auto-invoke. | **Required** |
| `allowed-tools` | Restrict which tools Claude can use (e.g., `Read, Grep, Glob, Bash`) | Inherits from conversation |
| `disable-model-invocation` | `true` = only user can invoke via `/skill-name` | `false` |
| `user-invocable` | `false` = only Claude can invoke (background knowledge) | `true` |
| `model` | Specify model (`haiku`, `sonnet`, `opus`, or full model string) | Inherits from conversation |
| `argument-hint` | Shows expected arguments (e.g., `[filename] [options]`) | None |
| `hooks` | Define hooks that run while skill is active | None |

### Invocation Control

By default, both you and Claude can invoke any skill:
- **You** type `/skill-name` to invoke directly
- **Claude** loads it automatically when relevant based on the description

Two frontmatter fields restrict this:

#### User-only skill (you control timing)
Use for workflows with side effects like deploy, commit, or sending messages:
```yaml
---
name: deploy
description: Deploy the application to production
disable-model-invocation: true
---
```

#### Claude-only skill (background knowledge)
Use for reference content that isn't actionable as a command:
```yaml
---
name: legacy-system-context
description: How our old authentication system works
user-invocable: false
---
```

### Example Skills

#### Simple skill (read-only code reviewer)
```yaml
---
name: code-reviewer
description: Review code for best practices. Use when reviewing PRs or analyzing code quality.
allowed-tools: Read, Grep, Glob
---

# Code Reviewer

## Review Checklist
1. Code organization and structure
2. Error handling
3. Performance considerations
4. Security concerns
5. Test coverage
```

#### Skill with hooks
```yaml
---
name: secure-operations
description: Perform operations with security validation
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/security-check.sh"
---
```

#### Skill with specific model
```yaml
---
name: quick-lint
description: Quick code linting for small files
model: haiku
---
```

### Skill Locations

| Location | Scope | Use Case |
|----------|-------|----------|
| `~/.claude/skills/` | Personal | Available across all projects |
| `.claude/skills/` | Project | Shared with team via git |
| Plugin skills | Plugin | Installed via plugin marketplace |

### Tips

- **Write specific descriptions** - Include both what the skill does AND when to use it with trigger keywords
- **Keep skills focused** - One skill = one capability
- **Use `disable-model-invocation: true`** for user-only skills to save context (descriptions won't load at startup)
- **Reference supporting files** from SKILL.md - Claude loads them only when needed
- **Test skills** by asking questions that match your description

### Debugging

If Claude doesn't use your skill:
1. Check the description is specific enough
2. Verify file path: `~/.claude/skills/skill-name/SKILL.md`
3. Check YAML syntax (no tabs, `---` on line 1)
4. Run `claude --debug` to see loading errors

---

## References

- [Agent Skills Documentation](https://code.claude.com/docs/en/skills)
- [Slash Commands Reference](https://code.claude.com/docs/en/slash-commands)
- [Best Practices](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices)
