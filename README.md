# workflows

[![CI](https://github.com/karloscodes/workflows/actions/workflows/ci.yml/badge.svg)](https://github.com/karloscodes/workflows/actions/workflows/ci.yml) [![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

Reusable GitHub Actions workflows for every repo of [karloscodes](https://github.com/karloscodes). A change here is one commit, not one pull request per repo.

## Install

There is nothing to install. A repo calls a workflow from its own file.

## Use

A repo keeps a small file with its own name and schedule, and calls the workflow at `main`:

```yaml
name: Vulncheck

on:
  schedule:
    - cron: "23 5 * * *"
  workflow_dispatch:

permissions:
  contents: read
  packages: read

jobs:
  scan:
    uses: karloscodes/workflows/.github/workflows/vulncheck.yml@main
```

A commit here reaches every repo on its next run, so a revert fixes every repo at once.

**Test a change first.** Push it to a branch, point one public and one private repo at it (`@my-branch`), and run them. Then merge to `main`.

## Workflows

| Workflow | Used by | What it does |
|---|---|---|
| `gitleaks.yml` | every repo's CI | Scans the full history for secrets. |
| `merge-security-updates.yml` | every repo | Merges a Dependabot security or patch update when CI passed. |
| `claude.yml` | every repo | Answers `@claude` from owners, members, and collaborators. |
| `vulncheck.yml` | Go repos, Rails apps | Once a day, scans what users run: the code, the lock files, and the published images. |
| `go-security.yml` | Go repos | gosec and govulncheck. Inputs: `go-version`, `prepare`, `gosec-flags`, `check-dockerfile`. |
| `release-train.yml` | Go images | Weekly candidates and stable releases, the fast lane, and the rebuild lane. |
| `install-paths.yml` | Go images | The three install paths with Chasen. |
| `install-line.yml` | Go images | The live install line. Input: `url`. |
| `security-release.yml` | Go CLIs | Tags a patch when a security update merges. |
| `ci-astro.yml` | Astro sites | Build, gitleaks, and `test`. |
| `nightly-rebuild.yml` | Astro sites | Saves the download counts, which rebuilds the site. |
| `security-rails.yml` | Rails apps | The check that security updates shipped, and the weekly advisory scan. |

A repo keeps what is its own: its tests, its build, and its schedule. `release.yml` of a product, a CLI, or an SDK stays in the repo: each one builds something else, and npm and RubyGems tie trusted publishing to the workflow file.

## Development

A commit to `main` reaches every repo on its next run, and nothing merges by itself here. Run the test above on a branch first, and CI checks every branch with actionlint and gitleaks.

## Where jobs run

A public repo uses GitHub's runners, never hq. A private repo sets the repository variable `RUNS_ON`, so its scheduled jobs from the default branch run on hq. A run by hand on another branch stays on GitHub's runners. The code of a pull request never runs on hq, so CI and `claude.yml` always use GitHub's runners.

## License

MIT
