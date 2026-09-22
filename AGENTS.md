# nocoo

Public GitHub profile for `nocoo`: [README.md](README.md) is the product, with a
Markdown skill under `skills/system0-github-profile/`. There is no application,
package manifest, executable helper, build or versioned release here.

## Scope and sources

This root file is the only maintained project handbook. Preserve it during
scaffolding; do not create CLAUDE.md, a legacy alias or a copied handbook.
Machine rules belong in global AGENTS.md and its Git rules. Profile maintenance
instructions live in [the profile skill](skills/system0-github-profile/SKILL.md).
Tool-specific skill links are consumers, not separate maintained sources.

## Project boundaries

- Keep badges, project descriptions and links factual. Do not invent statistics,
  unpublished repositories or unsupported capabilities.
- Maintain the profile's human-facing overview and established project groups.
  Verify project facts against the relevant source repository before publication.
- Do not add Cloudflare resources, databases, package tooling or test frameworks
  to this documentation-only repository.
- Do not include credentials or private project content in the public profile.
- Preserve the maintained skill source and its links; resolve a linked source
  before editing rather than replacing it with a local copy.

## Commands and review

Run from the repository root. Git is required; there is no dependency install,
development server, test runner or build command.

```sh
git diff --check
git diff -- README.md AGENTS.md Retrospective.md
```

Review changed links, descriptions and Markdown rendering. GitHub preview is a
manual publication check, not an automated test result. Commands supplied by a
linked maintenance skill retain that skill's scope and operation permissions.

## Quality contract and current evidence

The personal framework remains named 6DQ. Since 2026-09-21, former G1 static
checks belong to unified L1; L2/L3, G2 and D1 remain distinct. Statuses are
`enforced`, `planned`, `manual`, or justified `N/A`. No project CI, test runner or
commit/push hooks are configured; do not claim automated enforcement.

| Dimension | Applicability and current evidence |
| --- | --- |
| L1 | Runtime UT, four coverage metrics each >=95%, strict types/lint and their automatic index-snapshot pre-commit/rejection are N/A: no executable source or compiler. Markdown/diff review is manual; no automated documentation gate exists. |
| L2 | N/A: no owned API, process or storage integration. |
| L3 | N/A for application/CLI journeys: this is a profile document. GitHub rendering is reviewed manually. |
| G2 dependencies | N/A: no package manifest, lockfile or executable dependency lane. |
| G2 secrets | Planned: public Markdown can still expose secrets; no configured secret scanner or failure-blocking gate exists. Never treat the absence of dependencies as a secret-scan exemption. |
| D1 | N/A for test fixtures: no automated tests or data stores. Any future checks must isolate their outputs from production and daily development. |
| Documentation | Manual: inspect the full diff, link destinations, factual claims and rendered profile. |

If applicable executable checks are introduced, retain the shared contract:
index-snapshot L1 pre-commit under 30 seconds, with strict check-only diagnostics,
coverage and failure rejection; applicable L2/G2 pre-push against stdin push refs
under three minutes. Required scanners must fail when missing. These are targets,
not installed gates. Never bypass hooks or use autofix as a check.

## Completion and retrospective

Stage explicit task paths and make an atomic commit under the global Git rules.
Publication requires authorization for the operation; verify the intended remote
revision and rendered profile after an authorized push. Report checks actually
performed and unresolved evidence gaps.

Accident narratives belong in [Retrospective.md](Retrospective.md). Keep recurring
project rules brief here, route cross-project lessons to global rules/nmem, and
put deterministic safeguards into checks when applicable. Do not invent incidents.
