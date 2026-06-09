# vibekit

A Claude Code plugin marketplace. The `vibekit` family bundles two plugins:

- **vibekit-code** — coding-lifecycle skills (6): `spec-first-build`, `calibration-battery`,
  `debugging-discipline`, `production-readiness-review`, `agent-sop-generator`,
  `claude-md-manager`. Invoked as `vibekit-code:<skill>`.
- **vibekit-ops** — knowledge-work operator skills (1): `chief-of-staff-plays`,
  invoked as `vibekit-ops:chief-of-staff-plays`. Seed for a future operator suite.

---

## For users: install from GitHub

```
/plugin marketplace add androsland/vibekit
/plugin install vibekit-code@vibekit
/plugin install vibekit-ops@vibekit
/reload-plugins
```

Verify with `/plugin` (both should show installed) and `/skills` (skills appear
namespaced, e.g. `vibekit-code:debugging-discipline`). Force one directly with
`/vibekit-code:production-readiness-review`.

> Note: this marketplace uses relative plugin paths (`./plugins/...`), which resolve
> only when users add it via **Git** (GitHub/GitLab/git URL). Adding it by a raw link
> to `marketplace.json` will not work — keep it on a git host.

---

## Maintaining

To ship an update: edit files, bump the `version` in the relevant `plugin.json`
(if the version string doesn't change, existing users won't get the update), commit,
and push. Users refresh with:

```
/plugin marketplace update vibekit
/reload-plugins
```

(Or omit `version` from `plugin.json` entirely; on a git-hosted marketplace each new
commit then counts as a new version automatically.)

---

## Structure

```
vibekit/                              <- repo root (contents of this folder)
├── .claude-plugin/marketplace.json
├── LICENSE
├── README.md
└── plugins/
    ├── vibekit-code/
    │   ├── .claude-plugin/plugin.json
    │   └── skills/<six skill folders>/SKILL.md
    └── vibekit-ops/
        ├── .claude-plugin/plugin.json
        └── skills/chief-of-staff-plays/SKILL.md
```

Only the `*.json` manifests live in `.claude-plugin/`; each plugin's `skills/`
directory sits at the plugin root.
