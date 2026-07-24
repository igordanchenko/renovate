# CLAUDE.md

## What this repo is

Shareable [Renovate](https://docs.renovatebot.com/) config presets — **not an
application**. There is no `package.json`, no build, and no source code; the
deliverables are the JSON preset files at the repo root, consumed by other repos
via `extends`:

- `github>igordanchenko/renovate` → `default.json`
- `github>igordanchenko/renovate:lib` → `lib.json`
- `github>igordanchenko/renovate:app` → `app.json`

## Preset architecture

`default.json` is the base; `lib.json` and `app.json` both `extends` it and are
the two consumer-facing entry points.

- **`default.json`** — the shared behavior both consumer presets inherit
  (scheduling, grouping, automerge policy, digest/action pinning, `0.x`
  handling). Read the file for the current rules; this is where shared changes
  belong.
- **`app.json`** — the "pin everything to exact versions" variant, adding npm
  range rules on top of the defaults.
- **`lib.json`** — the "keep semver ranges" variant. It intentionally adds *no*
  packageRules of its own — its range behavior comes from Renovate's own
  defaults, so tune library behavior in the defaults, not here.

Because `lib`/`app` extend `default`, most edits belong in `default.json`; only
npm range strategy distinguishes the two consumer presets.

## Validating changes

The only check is Renovate's config validator (also what CI runs, in the job
named **`CI`**):

```sh
npx --yes --package renovate -- renovate-config-validator --strict default.json lib.json app.json renovate.json
```

Validation only checks syntax/schema. To prove a `packageRule` actually matches
what you intend, build a control/treatment harness with `platform: local` — the
`renovate-preset-testing` skill documents this, including the pitfall that
global-config `force` *replaces* preset arrays instead of appending.

## Release flow

Releases are automated by semantic-release (`.releaserc.json`,
`conventionalcommits` preset, **no npm plugin** since nothing is published to
npm).

**Commits must follow Conventional Commits** — this drives versioning: `feat` →
minor, `fix` → patch, `BREAKING CHANGE` → major.
