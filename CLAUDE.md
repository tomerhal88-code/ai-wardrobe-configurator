# CLAUDE.md

Guidance for Claude Code (and other AI coding agents) working in this repository.

## Project

AI-powered wardrobe configurator for a carpentry business: customers design a wardrobe in interactive 3D with an AI design agent and submit a quotation request; carpentry staff review, revise and quote manually.

- **[PRD.md](PRD.md) is authoritative.** Follow the approved decision register (§1.2), the roadmap order (§13) and the agent instructions (§17). Do not add unapproved features or swap the stack.
- Stack: Python 3.13 + FastAPI (`backend/`), vanilla HTML/CSS/JS + Three.js (`frontend/`), Supabase, Railway.
- `stitch/`: Google Stitch design export and Tailwind design tokens (see `stitch/DESIGN.md`).
- Run/test commands are in [README.md](README.md).

## Mandatory workflow rules

These rules apply to every task. Do not skip them.

### 1. Branching: never develop on `main`

- Every new piece of work starts on a **new branch created from `dev`**:
  ```bash
  git fetch origin
  git checkout -b <type>/<short-name> origin/dev   # e.g. feature/3d-scene, fix/cors-origin
  ```
- Commit only to that branch. Never commit directly to `main` or `dev`.
- `main` = production (Railway Production). `dev` = development (Railway Development).

### 2. Pull request before anything reaches GitHub branches

- Do not push straight to `dev` or `main`. Push the work branch and **open a pull request** (`gh pr create --base dev`).
- Merges into `main` only happen via a PR from `dev`, after testing in Railway Development.
- Never merge a PR yourself unless the user explicitly asks.

### 3. Testing after writing code: regression + E2E

When code changes are finished, before opening the PR:

- **Regression:** run the full existing test suite, not only new tests.
  ```bash
  cd backend && pytest
  ```
- **E2E:** run end-to-end browser tests of the affected user flows (customer flow and/or `staff/` flow) against the locally running backend (port 8000) and frontend (port 5500). Playwright is the intended tool; if the E2E suite for a flow doesn't exist yet, add it as part of the task.
- Add tests for every new behavior (PRD §14 lists the test layers and acceptance criteria AC-01…AC-13).
- Report results honestly in the PR description. If anything fails or was skipped, say so.

### 4. Security review

- Run the `/security-review` skill on the branch's changes before opening the PR.
- Fix real findings, or list in the PR description why a finding is not applicable.
- Always: no secrets in git (`.env` is ignored, only `.env.example` is committed), staff authorization enforced server-side, anonymous users can never read other customers' data (PRD §2, §11).

### 5. Agent memory log, at the end of every task

At the end of every task, write a log entry in **[`agent-memory/`](agent-memory/)**. This folder is the agent's secondary memory between sessions. **Read the recent entries there at the start of a task.**

- One file per task: `agent-memory/YYYY-MM-DD-<short-task-name>.md`
- Template:
  ```markdown
  # <Task title>
  - Date: YYYY-MM-DD
  - Branch / PR: <branch> / <PR link>

  ## What was done
  ## Decisions and why
  ## Tests run and results (regression, E2E, security review)
  ## Open issues / next steps
  ## Notes for the next agent (gotchas, non-obvious context)
  ```
- Commit the log entry on the same branch as the work, so it goes into the same PR.

## Task checklist

1. Read the recent `agent-memory/` entries and the relevant PRD sections.
2. Create a branch from `dev`.
3. Implement the change, with tests.
4. Run regression + E2E tests.
5. Run `/security-review` and address the findings.
6. Write the `agent-memory/` log entry.
7. Push the branch and open a PR to `dev`.
