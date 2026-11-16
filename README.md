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

- Dependency updates are `chore(deps)`, so they wait for the next `feat` or `fix` release. Security
  updates are `fix(deps)` and release right away.
- Minor, patch and digest updates automerge. Node.js majors and anything a repo flags do not.
- Related packages move in one PR: Node.js, pnpm, GitHub Actions, Hono, OpenTelemetry, Vite,
  Oxlint, Drizzle.

## What `library` does

Production dependencies get `fix(deps)` titles, so updating one releases. Lockfiles refresh weekly, and
majors never automerge.

## Changing a preset

A change here reaches every consumer on its next Renovate run. Put a rule here only when two or
more repos need it. A rule for one repo goes in that repo's `renovate.json`.
