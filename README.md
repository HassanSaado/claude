# Claude Settings

This repository contains my Claude Code settings, commands, and skills.

## Structure

- `commands/` - Custom slash commands
- `skills/` - Custom skills
- `settings.json` - Plugin and feature settings

## Setup

These settings are symlinked from `~/.claude/` to this repository:
- `~/.claude/commands` → `~/claude/commands`
- `~/.claude/skills` → `~/claude/skills`
- `~/.claude/settings.json` → `~/claude/settings.json`
- `~/.claude/settings.local.json` → `~/claude/settings.local.json`

Runtime data (cache, history, credentials, etc.) remains in `~/.claude/`.

Project-specific `claude.md` files remain in their respective project directories.