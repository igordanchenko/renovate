# renovate

Shareable [Renovate](https://docs.renovatebot.com/) config presets.

## Usage

Add one of the presets to `renovate.json` in the consuming repo:

**Libraries** — `dependencies` keep semver ranges, `devDependencies` are
pinned, `peerDependencies` are widened:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>igordanchenko/renovate:lib"
  ]
}
```

**Apps** — everything is pinned to exact versions:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>igordanchenko/renovate:app"
  ]
}
```

## Automerge prerequisites

Renovate automerges non-major updates once status checks pass, but **it will
also automerge when a repo has no status checks at all** — make sure every
consuming repo runs meaningful CI on pull requests. Recommended GitHub setup:

- branch protection (or a ruleset) on the default branch with required status
  checks
- "Allow auto-merge" enabled in the repo settings (Renovate uses GitHub's
  native auto-merge by default)
