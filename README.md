# renovate-config

Shared [Renovate](https://docs.renovatebot.com/config-presets/) presets for my repos. A repo
extends one preset and keeps only the rules that are its own.

| Preset | For | `extends` |
|---|---|---|
| `default` | pnpm/Node apps released with release-please | `github>bojanrajkovic/renovate-config` |
| `library` | npm libraries, where a dependency update should cut a release | `github>bojanrajkovic/renovate-config:library` |

A consumer's `renovate.json` looks like this:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>bojanrajkovic/renovate-config"],
  "packageRules": [
    {
      "description": "A rule only this repo needs",
      "matchPackageNames": ["some-package"],
      "automerge": false
    }
  ]
}
```

Rules in the consumer come after the preset's, so they win on conflict.

## What `default` does

- Starts from `config:best-practices`: image digests are pinned, dev dependencies are pinned to
  exact versions, and lockfiles refresh weekly.
- Commit types follow Renovate's default: production dependencies are `fix(deps)`, everything else
  is `chore(deps)`. A repo that wants updates to wait for the next release overrides it.
- Minor, patch and digest updates automerge. Node.js majors and 0.x minors don't.
- Version ranges keep their style: `bump` raises the floor of a `^1.2.3` range.
- Related packages move in one PR: Node.js, pnpm, GitHub Actions, Hono, OpenTelemetry, Vite,
  Oxlint, Drizzle.

## What `library` does

Production dependencies get `fix(deps)` titles, so updating one releases. Lockfiles refresh weekly, and
majors never automerge.

## Changing a preset

A change here reaches every consumer on its next Renovate run. Put a rule here only when two or
more repos need it. A rule for one repo goes in that repo's `renovate.json`.
