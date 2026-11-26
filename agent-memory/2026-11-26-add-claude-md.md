# Add CLAUDE.md with workflow rules
- Date: 2026-11-26
- Branch / PR: feature/claude-md / (see PR to dev)

## What was done
- Added `CLAUDE.md` with the mandatory workflow rules: branch from `dev` (never develop on `main`), PR before pushing to `dev`/`main`, regression + E2E tests after coding, `/security-review`, and an agent memory log at the end of every task.
- Created this `agent-memory/` folder as the agent's secondary memory.

## Decisions and why
- E2E tool: Playwright is the intended choice (PRD §14 asks for browser tests of the 3D controls and editor sync). No E2E suite exists yet; the first feature task that touches a UI flow should add it.

## Tests run and results
- Documentation-only change, so no code tests were needed. The security review had no code to check.

## Open issues / next steps
- Set up a Playwright E2E harness.
- Consider branch protection on `main`/`dev` in GitHub (PRD §12.2 recommends it).

## Notes for the next agent
- Existing backend: FastAPI skeleton with `/api/health` and `pytest` tests in `backend/tests/`.
- Frontend: static pages in `frontend/`, served on port 5500, which the backend CORS setting allows by default.
