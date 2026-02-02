# Todo Parallel Sandbox — Design

## Context

We want a tiny real repo to validate an “agentic” development workflow end‑to‑end:

- Source of truth for work: **Beads issues** (strict scope per task).
- Execution: **parallel Coder Tasks** running **Claude Code**.
- Integration: **GitHub PRs** (small patches), with automated checks where possible.

This repo intentionally stays simple so the workflow is what we’re testing, not architecture.

## Goals

1) Build a minimal **single‑user** todo list app with **no backend** and **no database**.
2) Prove we can run **multiple tasks in parallel** and converge via PRs safely.
3) Add a basic automated gate (prek + GitHub Actions) to support “verify before merge”.

## Non‑Goals (explicit anti‑drift)

- Multi-user, auth, sync, accounts, server/API, DB.
- Frameworks/build tools (React/Vite/etc), TypeScript, complex state management.
- “Nice to have” features unless they are in an approved task.

If a task uncovers a good idea: create a **new Beads issue**; do not expand scope mid-task.

## App Architecture (static)

Files:

- `index.html`: static markup shell.
- `style.css`: minimal styling.
- `app.js`: app logic (render + events + persistence).

Data model:

- `TodoItem`: `{ id: string, text: string, done: boolean, createdAt: number }`
- Storage: `localStorage["todos"]` as JSON array.

Data flow:

1) On load: read + parse storage → default to `[]` on error.
2) Render list from in-memory array.
3) UI actions (add/toggle/delete) mutate array and immediately persist + rerender.

## Workflow Architecture (Beads → Coder → PR)

### Strict scope per task

Each Beads issue must include:

- **Objective** (1 sentence)
- **Must-haves** (≤3 bullets)
- **Non-goals** (explicit)
- **Verification** steps (runnable commands)

Coder task prompt mirrors the issue and repeats: “no extra improvements; open a new issue”.

### Memory hygiene

- Beads issues (`.beads/issues.jsonl`) act as the shared “task ledger”.
- Before starting work: re-read the issue; confirm scope.
- After finishing: update issue state and sync (`bd sync`).

### Layered constraints (anti drift)

- Before coding: scope checklist in the issue.
- During coding: small PRs (one issue per PR).
- After coding: `uvx prek run --all-files` and CI.
- Operational safety: no secret material committed; `.env` ignored.

## Verification

Local:

- `uvx prek run --all-files`
- `python3 -m http.server 8000 --bind 127.0.0.1` then open `http://127.0.0.1:8000/`

CI (optional but desired):

- GitHub Actions workflow runs `uvx prek run --all-files` on `push` and `pull_request`.

