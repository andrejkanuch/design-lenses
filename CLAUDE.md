# Compound Design Loop — Development Guide

## Purpose
Claude Code plugin that orchestrates multi-persona design reviews through parallel agent teams and 19 Impeccable design commands.

## Repository Structure
```
compound-design-loop/
├── .claude-plugin/          # Plugin manifest + marketplace metadata
├── .claude/skills/          # 3 skills (design-loop, design-brainstorm, design-status)
│   └── design-loop/reference/  # Modular knowledge files
├── CLAUDE.md                # This file
├── CHANGELOG.md
├── README.md
└── LICENSE
```

## Version Sync
When bumping version, update ALL of these:
- `.claude-plugin/plugin.json` → `version` field
- `.claude-plugin/marketplace.json` → `version` field in plugins array
- `CHANGELOG.md` → add new version section

## Pre-Commit Checklist
- [ ] Version numbers match across all 3 files
- [ ] All skills have `user-invokable: true` in frontmatter
- [ ] No hardcoded platform/theme in agent prompts (must use placeholders)
- [ ] Reference files are linked from SKILL.md, not duplicated inline
- [ ] CHANGELOG.md updated with changes

## Development & Testing
Test locally: `claude --plugin-dir /path/to/compound-design-loop`
Reload in session: `/reload-plugins`
Verify skills: `/help` should show all 3 skills

## Skill Compliance
- SKILL.md under 500 lines (deep content goes in reference/ files)
- Frontmatter: name, description, user-invokable: true
- Agent prompts use DESIGN_CONTEXT placeholders: [PLATFORM], [THEME], [SCOPE], [DOMAIN]
- Every NEVER item has a "why" clause
- Follow Assess → Execute → Verify structure
