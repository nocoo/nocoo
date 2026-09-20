# nocoo

GitHub profile README and a single linked skill for updating that profile.
Profile: docs-config
Direction: [README.md](README.md). Frameworks must not rewrite this file.

## Sources of Truth

This file is the **contract**. Hooks, CI, and config are **enforcement**. If they disagree, that is a failure — raise enforcement to match this file; never lower the contract to a weaker hook.

| Fact | Where |
|---|---|
| Agent handbook | this file |
| Human docs | README.md |
| Version | omit (no package.json) |
| Enforcement | none (no CI, hooks, or test runner) |
| Machine rules | global `AGENTS.md`, `rules/git-commit.md` |
| Accidents | [Retrospective.md](Retrospective.md) |
| Env files | omit |

## Project Invariants

- This repository is the public GitHub profile for `nocoo`, not an application.
- Keep README badges/projects factual; do not invent stats or unpublished repos.
- The only extra tree is `skills/system0-github-profile/` (markdown skill). It is not executable code in this repo.
- Do not add Cloudflare, databases, or test infrastructure here.

## Stack / Layout

| Component | Choice |
|---|---|
| Language | Markdown |
| Package manager | omit |
| Runtime | omit |
| Lint | omit |
| Tests | omit |
| Data | none |

```
README.md
skills/system0-github-profile/SKILL.md
```

## Commands

No install/dev/test/build scripts. Edit Markdown; preview on GitHub.

## Verification

Status: `enforced` | `planned` | `manual` | `N/A`.
6DQ = L1/L2/L3 + G1/G2 + D1. docs-config still records N/A reasons. No executable helper to test.

| Change | Proof | Status | Evidence |
|---|---|---|---|
| Logic | L1 ≥ 95% | N/A | no runtime source |
| API / schema | L2 real HTTP | N/A | no API |
| UI path | L3 | N/A | no UI/CLI process |
| Types / lint | G1 | N/A | no compiler/linter |
| Deps / secrets | G2 osv-scanner + gitleaks | N/A | no lockfile or publishable package; markdown-only helper |
| Test isolation | D1 | N/A | no tests or data stores |
| Bundler output | — | N/A | no bundler |
| Docs | README is the product | manual | human review |
| Release | — | N/A | profile pages are not versioned releases |

No husky. If hooks are added: index-snapshot G1+L1 <30s; stdin-ref G2 <3min (targets). `--no-verify` forbidden.

## Retrospective

| Kind | Where |
|---|---|
| Accident narrative | [Retrospective.md](Retrospective.md) |
| Project-specific rule that will recur | one line here (cap ~10) |
| Cross-project lesson | nmem / global `AGENTS.md` / `rules/` |
| Deterministically checkable rule | hook or test, not prose |

- (none yet)
