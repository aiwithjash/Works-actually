# Changelog

All notable changes to **Works, Actually** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2026-11-14

### Added
- Initial release of the `works-actually` skill (`skills/works-actually/SKILL.md` and `.claude/skills/works-actually/SKILL.md`) following the official Claude Code and Agent Skills (`agentskills.io`) specifications.
- 10-step read-only user-journey verification workflow with explicit diagnosis-to-fix handoff (`"I found these issues. Would you like me to fix them?"`).
- Four honest result labels: `Passed`, `Failed`, `Blocked / not tested`, and `Needs human review`.
- Supporting reference guides:
  - `references/journey-checklists.md` (signup, login, cart, checkout, contact form, file upload, booking/CRUD).
  - `references/report-templates.md` (plain-English reports, Agent Fix Brief, and Phase 2 re-verification templates).
  - `references/tool-fallbacks.md` (3-tier fallback from real browser automation to local HTTP/DB checks and static trace).
- Original vector and raster visual identity assets (`assets/banner.*`, `assets/social-preview.*`, `assets/workflow-diagram.*`).
- Sample reports (`examples/sample-reports/`) and a realistic 7-scenario test storefront + evaluation suite (`examples/fixtures/demo-storefront/` and `tests/`).
