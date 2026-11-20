# nocoo

Public GitHub profile repository for `nocoo`: [README.md](README.md) is the deliverable, with no application code, package manifest, build, or versioned release.
Docs: [README.md](README.md) · [Retrospective](Retrospective.md) · Profile skill: `system1-github-profile` (in `nocoo/system`, `system1/skills/system1-github-profile/SKILL.md`)

## Commands

Run from the repository root (Git only; no dependency install, dev server, test runner, or build step).

```sh
git diff --check
git diff -- README.md AGENTS.md Retrospective.md
```

- Review changed links, factual claims, and Markdown rendering manually; GitHub preview is a `manual` publication check, not an automated gate.
- Commands supplied by `system1-github-profile` retain that skill's own scope and authorization rules.

## Invariants

- **Factual content only**: keep badges, descriptions, and links accurate; verify facts against each source repository and never invent statistics, unpublished repos, or capabilities.
- **Layout & `hexly.ai` sync**: preserve unrelated entries, established project groups, and `<!-- BLOG-POST-LIST:START/END -->` markers; sync `hexly.ai` per `system1-github-profile`.
- **Public privacy boundary**: never commit credentials, secrets, or private project content; absence of dependencies does not exempt public Markdown from secret review.
- **No automated gates**: no CI, test runner, or Git hooks are installed (`manual` diff/render review; secret scanning is `planned`); never claim automated enforcement.
- **Documentation-only scope**: do not add Cloudflare resources, databases, package tooling, or test frameworks to this repository.
- **Canonical skill vs. local `skills/`**: profile rules live in `nocoo/system` (`system1-github-profile`); local `skills/` still exists only as a legacy pointer stub and is not authoritative.
- **Consumer symlinks**: `.claude/skills` and `.opencode/skills` symlink to `skills/` as tool consumers; resolve linked sources before editing and never duplicate the canonical skill locally.
- **Authorized publication**: pushing to publish requires explicit authorization; afterward, verify the remote revision and rendered GitHub profile, and report checks run plus evidence gaps.

## Retrospective

Record incident narratives in [Retrospective.md](Retrospective.md) (never invent incidents); promote recurring project constraints to `Invariants` above, cross-project lessons to global rules, and enforceable safeguards to checks or hooks.
