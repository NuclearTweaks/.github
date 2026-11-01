# NuclearTweaks/.github

Org-shared CI for all NuclearTweaks tweak repos.

- `.github/workflows/tweak-ci.yml` — reusable workflow called by every tweak repo:
  push main → build **rootless device** deb → open release PR; merged `release/*` PR → tag + GitHub Release (deb asset) + `repository_dispatch` to `NuclearTweaks.github.io` (APT repo rebuild).
- `.github/actions/setup-theos` — composite action: theos + submodules + hash-agile ldid (`LDID_BIN`).

## Per-tweak wiring (done automatically by `meta` repo `tools/new-tweak.sh`)
1. repo `.github/workflows/build.yml` calls this workflow with `secrets: inherit`.
2. repo secret `NUCLEAR_BOT_TOKEN` (repo-scoped PAT) so `publish` can dispatch the APT repo (GITHUB_TOKEN cannot cross repos).
3. repo topic `nucleartweaks-tweak` — that is what marks a repo as a tweak for the APT rebuilder.

Simulator debs are intentionally never built on Actions — local loop only (`meta → tools/sim-test.sh`).
