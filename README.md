# `runestack/rune-setup-action`

Install the [Rune](https://github.com/runestack/rune) CLI on a GitHub
Actions runner.

This is the install-only primitive — for the common deploy case, use
[`runestack/rune-cast-action`](https://github.com/runestack/rune-cast-action) which
wraps `rune-setup-action` + `rune login` + `rune cast` in a single step.

## Usage

```yaml
- uses: runestack/rune-setup-action@v1
  with:
    version: v0.0.1-dev.26   # or "latest" (default)

- run: rune get services -n stg
  env:
    # Either pre-login in another step, or set these so `rune` has a
    # context. The setup action does NOT log you in.
    RUNE_SERVER: ${{ vars.RUNED_HOST }}
    RUNE_TOKEN:  ${{ secrets.RUNE_TOKEN }}
```

## Inputs

| Input          | Default              | Description                                                  |
|----------------|----------------------|--------------------------------------------------------------|
| `version`      | `latest`             | Tag from `runestack/rune` (e.g. `v0.0.1-dev.26`).            |
| `github-token` | `${{ github.token }}`| Token used for the version lookup and asset download.        |

## Outputs

| Output    | Description                                                            |
|-----------|------------------------------------------------------------------------|
| `version` | The resolved Rune CLI version that was installed.                      |
| `path`    | Absolute directory containing `rune` (also added to `PATH`).           |

## Platforms

The action downloads the matching `rune-cli_<os>_<arch>.tar.gz` asset
from the requested release. Supported runners:

- `ubuntu-latest`, `ubuntu-22.04`, `ubuntu-20.04` (linux/amd64, linux/arm64)
- `macos-latest`, `macos-13`, `macos-14` (darwin/amd64, darwin/arm64)

The binary is cached at `$RUNNER_TOOL_CACHE/rune/<version>/<os>_<arch>/`
so subsequent steps and matrix jobs in the same workflow reuse it.

## License

Apache-2.0 — see [LICENSE](LICENSE). Matches the licence of [Rune](https://github.com/runestack/rune) itself.
