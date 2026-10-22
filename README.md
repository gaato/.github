# gaato/.github

Reusable GitHub Actions workflows shared by [gaato](https://github.com/gaato)'s repositories.

## MoonBit canary

[`moonbit-canary.yml`](.github/workflows/moonbit-canary.yml) runs a repository's gates on the
`latest` and `nightly` [ghcr.io/gaato/moonbit](https://github.com/gaato/moonbit-docker) images,
so a toolchain change shows up before the pinned CI moves to it.

```yaml
name: Canary

on:
  schedule:
    - cron: "17 21 * * *"
  workflow_dispatch:

permissions:
  contents: read

jobs:
  canary:
    uses: gaato/.github/.github/workflows/moonbit-canary.yml@main
    with:
      gates: scripts/gates.sh
```

| Input | Default | |
|---|---|---|
| `gates` | (required) | Command run from the repository root. Use the same entrypoint as CI, so the two cannot drift apart. |
| `setup` | `""` | Command run before the gates, e.g. `apt-get update && apt-get install -y --no-install-recommends jq`. |
| `uv` | `false` | Install [uv](https://docs.astral.sh/uv/). |
| `runs-on` | `ubuntu-24.04` | Runner label. |
| `timeout-minutes` | `40` | Per channel. |

Node.js 24 is already in the images. `moon update` runs before `setup`.

## License

[Blue Oak Model License 1.0.0](LICENSE.md)
