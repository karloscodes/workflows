# workflows

Reusable GitHub Actions workflows for every repo of [karloscodes](https://github.com/karloscodes). A change here is one commit, not one pull request per repo.

## Use

A repo keeps a small file with its own name and schedule, and calls the workflow by a version tag:

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
    uses: karloscodes/workflows/.github/workflows/vulncheck.yml@v1.1.0
```

Dependabot moves the tag in each repo, so a bad change never reaches every repo at once.

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

## Where jobs run

A public repo uses GitHub's runners, never hq. A private repo sets the repository variable `RUNS_ON`, so its scheduled jobs from main run on hq. The code of a pull request never runs on hq, so CI and `claude.yml` always use GitHub's runners.

## License

MIT
