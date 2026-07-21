# Filmpire Microservices — Autonomous Work Contract

This file governs unattended/scheduled Claude Code runs on this repo (see the
`filmpire-autonomous-dev` scheduled task). A human-driven session can ignore
this file, but should still respect the branch/commit conventions below.

**No PRs.** This repo has a single collaborator (the user), who cannot
approve or merge their own PRs — so never run `git push` or `gh pr create`.
Commit locally and stop there; the user reviews, pushes, and merges by hand
whenever they're ready.

## Source of truth for remaining work

- GitHub Issues on `pehlivanu/filmpire-microservices` are the backlog.
  `.github/issues/PROJECT_ROADMAP.md` gives the phase-level narrative; the
  issues themselves (`gh issue list`) are the authoritative status.
- Architecture reference: `docs/architecture/ARCHITECTURE.md`.
- Existing completed services (`movie-service`, `discovery-service`,
  `config-service`, `api-gateway`) are the pattern to follow for structure,
  package layout, test style (JUnit + WireMock for external calls), and
  Spring Boot/Gradle conventions. Match them rather than inventing new
  patterns.

## Picking the next task

1. Run `gh issue list --repo pehlivanu/filmpire-microservices --state open`.
2. Check `git branch -a` for a `issue-<number>-*` branch that already exists —
   if one does, that issue is already claimed (in progress or done locally);
   pick a different one, or continue it if it's marked `WIP` (see below) and
   still needs work.
3. Pick the highest-priority open issue (`P0-critical` > `P1-high` > ...)
   with no existing branch.
4. If an issue is an Epic, work its next unfinished child task issue instead
   of the epic itself.

## Branch / commit conventions

- Branch per issue: `issue-<number>-<short-slug>` (e.g. `issue-17-user-service`).
- Never commit directly to `main`. Never `git push` or `gh pr create` — see
  the no-PR note above.
- Commit messages reference the issue: `feat: Implement User Service (#17)`.
  Do not add a `Co-Authored-By` trailer or any other AI-attribution line —
  the user is the sole author of these commits.
- Before committing: build and run tests for any module you touched
  (`./gradlew :backend:<module>:test` or the relevant module path) and make
  sure it's green. Don't commit code you haven't run tests against.
- If you must stop mid-issue (context or budget runs out before the issue is
  done), still leave the branch in a buildable, committed state — no
  uncommitted work, no broken build. Prefix the commit title with `WIP:` and
  include a short note in the commit body of exactly what's left to do, so
  the next scheduled run (or the user) can pick it up cold.

## Scope discipline

- One issue (or one clearly-bounded chunk of a large issue) per run. Don't
  jump between unrelated issues in the same run.
- Don't touch other branches, other open PRs, or issues you didn't pick.
- Don't edit CI/CD workflows, branch protection, or repo settings.
- Don't add dependencies or upgrade framework versions unless the issue
  specifically calls for it.
