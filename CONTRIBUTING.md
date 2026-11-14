# Contributing to Works, Actually

Thank you for helping make **Works, Actually** better for vibe coders, founders, and AI coding agents!

## Core Design Principles

Before opening a pull request, please keep these rules in mind:

1. **Keep `SKILL.md` Concise (< 500 lines)**: Claude Code loads the entire `SKILL.md` into context whenever the skill activates. Put core workflow rules in `SKILL.md` and detailed feature checklists or templates in `references/`.
2. **Read-Only Audit First**: Never add instructions that allow the skill to silently edit application code, install packages, or alter project configurations during the checking phase.
3. **Honest Evidence Labels**: Every check must map to `Passed`, `Failed`, `Blocked / not tested`, or `Needs human review`. Missing evidence is never a pass.
4. **Plain English + Technical Precision**: Keep reports easy for a nontechnical founder to read at a glance while providing exact file paths and reproduction steps in the Fix Brief.
5. **No Em-Dashes or En-Dashes in `SKILL.md`**: To pass strict Agent Skills validation, use standard hyphens (`-`), colons, commas, or periods in `SKILL.md`.

## Development & Validation Workflow

1. Edit the canonical skill files in `skills/works-actually/`.
2. Sync the project-level copy in `.claude/skills/works-actually/`:
   ```bash
   cp -R skills/works-actually/* .claude/skills/works-actually/
   ```
3. Run the validation and evaluation suite before submitting a PR:
   ```bash
   python3 tests/validate_skill.py
   python3 tests/test_install_simulation.py
   python3 tests/run_evaluation.py
   ```

## License

By contributing to this repository, you agree that your contributions will be licensed under the [MIT License](LICENSE).
