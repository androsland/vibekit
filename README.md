# vibekit

A Claude Code plugin marketplace. The `vibekit` family bundles two plugins:

- **vibekit-code** — coding-lifecycle skills (6): `spec-first-build`, `calibration-battery`,
  `systematic-debugging`, `production-readiness-review`, `agent-sop-generator`,
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
namespaced, e.g. `vibekit-code:systematic-debugging`). Force one directly with
`/vibekit-code:production-readiness-review`.

> Note: this marketplace uses relative plugin paths (`./plugins/...`), which resolve
> only when users add it via **Git** (GitHub/GitLab/git URL). Adding it by a raw link
> to `marketplace.json` will not work — keep it on a git host.

---

## For the publisher: put it on GitHub

Everything is already filled in (owner: Andreas Demetriou, MIT 2026,
repo `androsland/vibekit`). No edits needed — just create the repo named **vibekit**
and push.

1. Optionally validate first:
   ```
   claude plugin validate .
   ```
2. From this folder (its contents become the repo root):
   ```
   git init
   git add .
   git commit -m "vibekit marketplace: vibekit-code + vibekit-ops"
   git branch -M main
   git remote add origin https://github.com/androsland/vibekit.git
   git push -u origin main
   ```
3. Make the repo **public** so others can install without auth.
4. Share the install commands from the section above.

### Shipping updates

Edit files, bump the `version` in the relevant `plugin.json` (if the version string
doesn't change, existing users won't get the update), commit, and push. Users refresh:

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
