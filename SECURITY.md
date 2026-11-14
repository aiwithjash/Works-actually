# Security Policy

## Scope and Safety Design

**Works, Actually** is a prompt-based Agent Skill (`SKILL.md` and markdown references) along with local test fixtures in `examples/` and `tests/`.

To protect users and their applications:
- The skill does **not** use `allowed-tools` pre-approvals in its YAML frontmatter, ensuring all commands run through the user's normal Claude Code permission prompts.
- The skill does **not** use `!` dynamic shell injection in `SKILL.md`.
- The skill instructs the agent to default to local/test environments, never expose `.env` secrets, and never trigger live payments or unsolicited external communications.

## Reporting a Vulnerability

If you discover a prompt-injection risk, an unsafe instruction in `SKILL.md`, or a security issue in the test fixtures:

1. **Do not open a public issue** if the vulnerability could put users' local environments or credentials at risk.
2. Use [GitHub Private Vulnerability Reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability) on `https://github.com/aiwithjash/works-actually/security/advisories/new`.
3. Include the affected file, reproduction steps, and potential impact. We aim to review and respond within 72 hours.
