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
    uses: karloscodes/workflows/.github/workflows/vulncheck.yml@v1.0.0
```

Dependabot moves the tag in each repo, so a bad change never reaches every repo at once.

## Workflows

| Workflow | What it does |
|---|---|
| `vulncheck.yml` | Once a day, scans what users run: the code, the lock files, and the published images. A red run emails you. |

## Where jobs run

A public repo uses GitHub's runners, never hq. A private repo sets the repository variable `RUNS_ON`, so its scheduled jobs run on hq. The code of a pull request never runs on hq.

## License

MIT
