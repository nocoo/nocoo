---
name: zhengli-update-github-readme
description: Maintain nocoo's GitHub profile and repository descriptions together with the hexly.ai project directory, logo backups, palettes, and project profiles.
---

# GitHub profile and hexly.ai catalogue

Read the canonical workflow skill at `../workflow/agents/skills/zhengli-update-github-readme/SKILL.md`, relative to this repository root. It owns the section rules, source-provenance requirements, and delivery procedure.

Every project-list change must also update the matching entry in `../hexly.ai/src/data/projects.json`, its current artwork backup and evidenced palette, and its generated project profile. Preserve existing entries and ordering outside the requested scope. Follow `hexly.ai/docs/02-identity-rules.md`, regenerate previews/profiles, and run the affected quality gates.

Profile and site changes are one maintenance task. Existing user authorization governs publishing; do not request approval again for an already authorized push. If the workflow checkout is unavailable, retrieve its `agents/skills/zhengli-update-github-readme/SKILL.md` before starting a full catalogue refresh.
