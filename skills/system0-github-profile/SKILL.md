---
name: system0-github-profile
description: Follow the canonical system0 workflow for nocoo's GitHub profile and the matching hexly.ai catalogue, descriptions, identity provenance and project onboarding.
---

# GitHub profile maintenance

The maintained skill is [system0-github-profile](https://github.com/nocoo/workflow/blob/main/agents/skills/system0-github-profile/SKILL.md)
in `nocoo/workflow`. Resolve its local checkout first; the usual path from this
repository root is `../workflow/agents/skills/system0-github-profile/SKILL.md`.
Read that source and this repository's AGENTS.md before editing. This entry is
only a reference; do not duplicate the standard here or globally install it
alongside the canonical workflow skill.

Profile membership/description changes also update the corresponding
`hexly.ai/src/data/projects/<id>.json` and `index.json`, with the provenance,
identity, generation and validation rules in the canonical skill. Preserve
unrelated profile content. Existing user authorization governs commit/push.
