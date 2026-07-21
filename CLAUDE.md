# Filmpire Microservices — Autonomous Work Contract

This file governs unattended/scheduled Claude Code runs on this repo (see the
`filmpire-autonomous-dev` scheduled task). A human-driven session can ignore
this file, but should still respect the branch/PR conventions below.

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
2. Pick the highest-priority open issue (`P0-critical` > `P1-high` > ...)
   that has no open PR already targeting it. Check open PRs with
   `gh pr list --repo pehlivanu/filmpire-microservices`.
3. If an issue is an Epic, work its next unfinished child task issue instead
   of the epic itself.

## Branch / commit / PR conventions

- Branch per issue: `issue-<number>-<short-slug>` (e.g. `issue-17-user-service`).
- Never commit or push directly to `main`.
- Commit messages reference the issue: `feat: Implement User Service (#17)`.
- Before committing: build and run tests for any module you touched
  (`./gradlew :backend:<module>:test` or the relevant module path) and make
  sure it's green. Don't commit code you haven't run tests against.
- Push the branch and open a PR against `main` with `gh pr create`
  (`Closes #<issue>` in the body). Never merge the PR yourself — a human
  merges it.
- If you must stop mid-issue (context or budget runs out before the issue is
  done), still leave the branch in a buildable, committed state — no
  uncommitted work, no broken build. Prefix the commit/PR title with `WIP:`
  and write exactly what's left to do in the PR description, so the next
  scheduled run (or a human) can pick it up cold.

## Scope discipline

- One issue (or one clearly-bounded chunk of a large issue) per run. Don't
  jump between unrelated issues in the same run.
- Don't touch other branches, other open PRs, or issues you didn't pick.
- Don't edit CI/CD workflows, branch protection, or repo settings.
- Don't add dependencies or upgrade framework versions unless the issue
  specifically calls for it.
