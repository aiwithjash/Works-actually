<div align="center">

<img src="assets/banner.svg" alt="Works, Actually - Does it actually work?" width="100%" />

# Works, Actually

**Does it actually work?**

A beginner-friendly **Claude Code skill** that checks whether a website or web app feature actually works from a real user's perspective, proves what happened with runtime and backend evidence, explains issues in plain English, and prepares an actionable fix brief for your AI coding agent.

[![License: MIT](https://img.shields.io/badge/License-MIT-10B981.svg)](LICENSE)
[![Format: Agent Skills](https://img.shields.io/badge/Agent_Skills-SKILL.md-6366F1.svg)](https://agentskills.io/specification)
[![Primary Target: Claude Code](https://img.shields.io/badge/Claude_Code-Supported-0F172A.svg)](https://code.claude.com/docs/en/skills)

[Quick Start](#quick-start) • [Installation](#installation) • [How to Invoke](#how-to-invoke-the-skill) • [Sample Report](#sample-report-and-fix-handoff) • [How It Works](#how-the-skill-works) • [Safety Boundaries](#permissions-privacy-and-external-actions)

</div>

## The Problem It Solves

When you build a website or web app with an AI coding assistant (vibe coding), features often **look** finished long before they actually work:

- The **Sign Up** button shows a green `"Welcome aboard!"` toast, even though the API route crashed with a `500` error and created zero database rows.
- The **Add to Cart** button updates a number in the header, but refreshing the page wipes the cart completely.
- The **Contact Form** clears its inputs on submit, yet never sends a network request or displays an error when validation fails.

Most existing QA tools are built for experienced test engineers who want to maintain sprawling end-to-end test suites. **Works, Actually** is built for **vibe coders, indie hackers, and nontechnical founders** who want to ask one simple question:

> *"Can a real person use this feature successfully, and what evidence do we have?"*

## What It Does (and What It Does Not Promise)

| What `Works, Actually` Does | What It Does Not Promise |
| :--- | :--- |
| Checks **one user-specified website or web app feature** at a time | Does not run a massive, slow full-repository audit on every prompt |
| Exercises the real user journey (happy path + key failure states) | Does not silently edit your application code during the check |
| Checks whether backend state (database, session, storage) actually updated | Does not treat a rendered button or green toast as proof of success |
| Labels blocked or untestable steps honestly (`Blocked / not tested`) | Does not charge live cards, send real emails, or touch production data |
| Explains every finding in plain English and writes a coding-agent **Fix Brief** | Does not certify whole-app security, legal compliance, or production readiness |
| Waits for your confirmation (`"Would you like me to fix them?"`) before editing code | Does not invent file paths, fake screenshots, or fabricated test logs |

## Quick Start

1. **Install the skill** into your project's `.claude/skills/works-actually/` or personal `~/.claude/skills/works-actually/` folder (see [Installation](#installation) below).
2. **Open Claude Code** in your web project directory (`claude`).
3. **Ask about a specific feature** or run the slash command:
   ```text
   /works-actually signup flow
   ```
4. **Review the plain-English report** and reply `yes` if you want Claude Code to apply the fix brief and re-test the flow.

## Installation

> **Publication Note**: This repository is prepared for [`https://github.com/aiwithjash/works-actually`](https://github.com/aiwithjash/works-actually). Once pushed to GitHub, the remote `git clone` and `npx skills add aiwithjash/works-actually` commands below work directly. If you already have this repository downloaded locally, use the **From a Local Checkout** commands right away.

### Option 1: Install for All Your Projects (Personal Scope)

Installing to `~/.claude/skills/works-actually` makes `/works-actually` available across every repository on your machine ([Claude Code Skills Docs](https://code.claude.com/docs/en/skills)).

**macOS / Linux (Manual Git Clone - Zero Dependencies)**
```bash
git clone https://github.com/aiwithjash/works-actually.git /tmp/works-actually
mkdir -p ~/.claude/skills
cp -R /tmp/works-actually/skills/works-actually ~/.claude/skills/works-actually
rm -rf /tmp/works-actually
```

**Windows (PowerShell)**
```powershell
git clone https://github.com/aiwithjash/works-actually.git "$env:TEMP\works-actually"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills" | Out-Null
Copy-Item -Recurse -Force "$env:TEMP\works-actually\skills\works-actually" "$env:USERPROFILE\.claude\skills\works-actually"
Remove-Item -Recurse -Force "$env:TEMP\works-actually"
```

### Option 2: Install for a Single Project (Project Scope)

Installing to `.claude/skills/works-actually` inside your repository lets you commit the skill to Git so everyone working on the project gets it automatically.

**macOS / Linux**
```bash
git clone https://github.com/aiwithjash/works-actually.git /tmp/works-actually
mkdir -p .claude/skills
cp -R /tmp/works-actually/skills/works-actually .claude/skills/works-actually
rm -rf /tmp/works-actually
```

**Windows (PowerShell)**
```powershell
git clone https://github.com/aiwithjash/works-actually.git "$env:TEMP\works-actually"
New-Item -ItemType Directory -Force -Path ".\.claude\skills" | Out-Null
Copy-Item -Recurse -Force "$env:TEMP\works-actually\skills\works-actually" ".\.claude\skills\works-actually"
Remove-Item -Recurse -Force "$env:TEMP\works-actually"
```

### Option 3: Install via `npx skills` CLI

If you use the open-source [`skills` CLI (`vercel-labs/skills`)](https://github.com/vercel-labs/skills/blob/main/docs/commands.md):

```bash
# Install into the current project (.claude/skills/works-actually)
npx skills add aiwithjash/works-actually --skill works-actually --agent claude-code

# Or install globally across all projects (~/.claude/skills/works-actually)
npx skills add aiwithjash/works-actually --skill works-actually --agent claude-code --global

# Or install from a local checkout of this repository
npx skills add ./ --skill works-actually --agent claude-code
```

### Verify That the Skill Is Installed and Loaded

1. Confirm `SKILL.md` is in place:
   ```bash
   # Personal install check
   ls -la ~/.claude/skills/works-actually/SKILL.md

   # Project install check
   ls -la .claude/skills/works-actually/SKILL.md
   ```
2. Start `claude` in your terminal and run:
   ```text
   /skills
   ```
   (If you created the top-level `skills` directory while a Claude Code session was already open, run `/reload-skills` first.)
3. Confirm `works-actually` appears in the skill list, or type `/works-actually` to see the autocomplete hint `[feature, user flow, or page to check]`.

## How to Invoke the Skill

**Works, Actually** supports **both** explicit slash-command invocation and automatic natural-language activation:

1. **Direct Slash Command (Explicit)**:
   ```text
   /works-actually signup flow
   /works-actually add to cart and cart persistence
   /works-actually contact form on /contact
   ```
2. **Natural Language Prompts (Automatic)**:
   Because the skill's audit phase is strictly read-only with respect to your code, Claude Code can also load it automatically when you ask questions that match its description:
   - *"Does my signup flow actually work?"*
   - *"Check whether customers can add a product to the cart."*
   - *"Does the contact form really send a message?"*
   - *"Check my image upload feature."*
   - *"Does checkout work up to the point where a payment would be made?"*
   - *"Check whether the booking form saves the appointment."*

## Examples for Real Website and Web App Features

| User Request | What `Works, Actually` Checks | Evidence Gathered |
| :--- | :--- | :--- |
| *"Does my signup flow actually work?"* | Submits valid user details + empty email test + checks `users` table | HTTP status, UI message vs. actual DB row count, password storage check |
| *"Check whether customers can add a product to the cart."* | Clicks Add to Cart, checks cart badge/drawer, reloads page, updates quantity | Cart DOM state, `localStorage`/session/DB persistence across reload, price math |
| *"Does the contact form really send a message?"* | Submits valid message + empty message error test + checks email/DB configuration | Network payload, `400` error UI visibility, DB row vs. `Blocked` SMTP note |
| *"Check my image upload feature."* | Uploads a small test image + rejects invalid file type + reloads page | Upload endpoint status, file presence on disk/storage, image preview URL load |
| *"Does checkout work up to the point where a payment would be made?"* | Validates shipping form, verifies order draft total, **stops before live charge** | Draft order record, shipping validation errors, live payment boundary stop |
| *"Check whether the booking form saves the appointment."* | Selects date/time slot, submits booking, checks duplicate slot handling | Database appointment record, schedule reload persistence, conflict error state |

## Sample Report and Fix Handoff

Here is an example of the report produced when checking a signup form that displays a fake success banner while failing on the backend (see more in [`examples/sample-reports/`](examples/sample-reports/)):

```markdown
## Works, Actually Report

**Feature**: Signup Flow (`/signup`)
**Result**: Not working (Failed)
**Environment**: Local dev server (`http://localhost:8000`), SQLite (`dev.db`), Headless Chromium

### Summary in Plain English
The signup page looks finished and even shows a green "Welcome aboard!" banner when a user clicks Create Account, but no account is actually created. Behind the scenes, the server crashes with an error and saves nothing to the database.

### Check Breakdown
- **[Passed]** Empty email validation: Submitting a blank email blocks submission and shows "Please enter your email address."
- **[Failed]** Valid signup submission: Submitting `wa-test-user@example.com` returns `HTTP 500` and creates `0` database records, while the UI falsely displays a green success banner.
- **[Blocked / not tested]** Welcome email delivery: `SMTP_HOST` is not configured in the local environment.

### What Works
- The signup page renders at `/signup` with name, email, and password inputs.
- Client-side validation rejects an empty email address before sending a network request.

### Problems Found

#### 1. Valid signup returns a 500 error and shows a false success message
- **What happened**: The frontend displays "Welcome aboard! Check your inbox" without checking if the server request succeeded. Meanwhile, `POST /api/signup` fails with status `500` and no user row is written to the `users` table.
- **Evidence**:
  - Network response: `POST /api/signup` returned `500 Internal Server Error` (`{"error": "table users has no column named full_name"}`).
  - Database check: `SELECT count(*) FROM users WHERE email = 'wa-test-user@example.com'` returned `0`.
- **Likely area**:
  - `app.py:88` (confirmed: `INSERT INTO users` references `full_name` instead of `name`).
  - `templates/signup.html:41` (confirmed: `fetch('/api/signup')` shows the success banner before checking `if (!res.ok)`).
- **Suggested fix**:
  1. Update `app.py` to insert into column `name`.
  2. Update `templates/signup.html` to check `if (!response.ok)` and render the error message in `#signup-error`.
- **How to verify**:
  1. Submit `/signup` with `wa-test-user@example.com` -> confirm `201 Created` and `1` row in `users`.
  2. Trigger a duplicate signup -> confirm `#signup-error` displays the error message.

### Could Not Check (Blocked / Needs Human Review)
- **Welcome email delivery**: **Blocked / not tested** - No email service (`SMTP_HOST`) is configured in the test environment.

### Next Step
I found these issues. Would you like me to fix them?
```

## How the Skill Works

<div align="center">
<img src="assets/workflow-diagram.svg" alt="Works, Actually Workflow: Feature to Checks to Findings to Fix Handoff" width="100%" />
</div>

### The 10-Step Workflow

1. **Understand the feature and intended outcome**: Maps what the user clicks/enters, what feedback should appear, and what backend state must change.
2. **Inspect project instructions and code**: Reads `CLAUDE.md`, `README.md`, frontend components, API routes, and database queries.
3. **Identify how the app runs and available tools**: Reuses existing browser tools (Playwright MCP, headless Chromium, Cypress) or falls back gracefully to local HTTP + database inspection without installing unapproved packages.
4. **Select the smallest useful check set**: Picks 2 to 4 high-signal checks (happy path, invalid/error state, persistence, and safety boundaries).
5. **Exercise the feature safely**: Runs the checks against your local or authorized test environment.
6. **Inspect backend and persistence effects**: Verifies database records, session state, or stored files rather than trusting UI messages alone.
7. **Assign honest result labels**: Labels every check as `Passed`, `Failed`, `Blocked / not tested`, or `Needs human review`.
8. **Present a plain-English report**: Explains what works and what is broken in clear, jargon-free language.
9. **Give the coding agent a Fix Brief**: Lists observed evidence, verified file/line locations, concrete fix instructions, and re-verification steps.
10. **Ask before fixing**: Pauses with `"I found these issues. Would you like me to fix them?"` unless you already explicitly requested both a check and fixes.

## Supported Environments and Limitations

| Environment / Agent | Support Status | Notes |
| :--- | :--- | :--- |
| **Claude Code (CLI / Desktop / IDE)** | **Tested Support (Primary)** | Built specifically for [Claude Code's skill system](https://code.claude.com/docs/en/skills) (`.claude/skills/` and `~/.claude/skills/`), including `/works-actually` and `$ARGUMENTS`. |
| **Agent Skills Compatible Agents** (Cursor, Codex, Gemini CLI, OpenCode) | **Expected Compatibility** | Follows the open [Agent Skills (`agentskills.io`) specification](https://agentskills.io/specification). Claude-specific frontmatter fields (`argument-hint`) are safely ignored by other agents. |
| **Native iOS / Android Mobile Apps** | **Not Supported (Out of v1 Scope)** | Version 1 focuses strictly on websites and web applications. |

**Important Limitations**:
- **Missing evidence is never a pass**: If your database, `.env` credentials, or local server cannot run, those checks are marked `Blocked / not tested`.
- **Feature checks are not whole-app certifications**: A `Passed` result on your signup or checkout flow means that specific flow worked under the tested conditions. It does not certify that your entire application is secure, accessible, compliant, or ready for production traffic.

## Permissions, Privacy, and External Actions

- **Read-Only Audit Phase**: The skill never modifies your source code, project configuration, or package dependencies during an audit.
- **No Unapproved Tool Pre-Grants**: `SKILL.md` intentionally omits `allowed-tools` pre-approvals so every shell or browser action continues to respect your normal Claude Code permission settings.
- **Hard Stop on External Side Effects**:
  - Never makes real purchases or charges live payment methods (`Stripe`, `PayPal`, `Razorpay`).
  - Never sends real emails, SMS messages, or webhooks to real people.
  - Never deletes or mutates production data.
  - Never prints, logs, or commits secret keys or `.env` values.
- **Zero Telemetry or Paid Keys**: Requires no API keys, external accounts, or tracking to use.

## Troubleshooting

- **`/works-actually` does not appear in the `/` menu**:
  Confirm the file path is `~/.claude/skills/works-actually/SKILL.md` (personal) or `.claude/skills/works-actually/SKILL.md` (project). If you created the top-level `skills/` directory while Claude Code was already running, execute `/reload-skills` or restart your session.
- **The skill did not trigger automatically on a natural-language question**:
  Phrase your request around verifying a specific feature (for example, *"Does my signup flow actually work?"*) or invoke it explicitly with `/works-actually [feature name]`.
- **Browser checks are reported as `Blocked / not tested`**:
  If no browser tool (such as Playwright MCP or headless Chromium) is installed, the skill automatically falls back to Tier 2 (local HTTP + DOM + database inspection) and notes that interactive browser rendering was not tested. To enable full browser checks, connect the [Playwright MCP server](https://claude.com/marketplace/plugins/playwright) or use an environment with Playwright/Chromium installed.

## Testing & Evaluation Suite

This repository includes an automated validation and 7-scenario evaluation suite in [`tests/`](tests/) and [`examples/fixtures/demo-storefront/`](examples/fixtures/demo-storefront/):

```bash
# 1. Validate SKILL.md frontmatter, structure, links, assets, and secret scan
python3 tests/validate_skill.py

# 2. Test clean installation simulation (project & personal scopes)
python3 tests/test_install_simulation.py

# 3. Run the 7-scenario end-to-end evaluation harness (headless Chromium + SQLite)
python3 tests/run_evaluation.py
```

For testing interactively inside a live Claude Code session, see [`docs/claude-code-test-plan.md`](docs/claude-code-test-plan.md).

## Contributing & License

Contributions, bug reports, and new journey checklists are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) and our [Security Policy](SECURITY.md) before opening a pull request.

Licensed under the [MIT License](LICENSE).
