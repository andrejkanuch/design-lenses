# Changelog

All notable changes to the Compound Design Loop plugin.

## [2.0.0] - 2026-03-17

### Added
- Mandatory context gathering phase (platform, theme, scope, domain)
- 4 reference files for modular knowledge (domain-experts, agent-roles, critique-framework, accessibility-checklist)
- Auto-apply synthesis protocol between iterations
- Input validation with clear error messages
- Agent failure recovery (timeout, empty output, context exhaustion)
- Progress file schema (shared contract between design-loop and design-status)
- NEVER section with anti-patterns and reasons
- design-status skill with real progress parsing
- CLAUDE.md, LICENSE, .gitignore, CHANGELOG.md

### Changed
- Consolidated 18 pseudo-agents into 8 focused agents
- All agent prompts parameterized by DESIGN_CONTEXT (no hardcoded platform/theme)
- design-brainstorm prompts auto-detect interface type
- Architecture: 4 iterations with 2 agents each (DIAGNOSE → FOUNDATIONS → ENHANCE → SHIP)

### Fixed
- Email consistency (kanuchandrej@gmail.com everywhere)
- Progress tracking: design-loop now creates the file design-status reads

## [1.0.0] - 2026-03-17

### Added
- Initial release
- 3 skills: design-loop, design-brainstorm, design-status
- 6 domain presets (motorcycle, fitness, finance, ecommerce, medical, default)
- Ralph Loop integration for persistent iteration
