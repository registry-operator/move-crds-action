# Move CRDs Action

A GitHub Action for moving Custom Resource Definitions (CRDs) between repositories. This action handles copying CRD files from a source location to a target repository and can create or update pull requests to manage these changes.

## Usage

To use this action, include it in your GitHub workflow as shown below:

```yaml
name: move-crds-action

on:
  push:
    branches:
      - "main"

permissions:
  contents: write
  pull-requests: write

jobs:
  move-crds-action:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - run: |
        git config --global user.name "GitHub Actions"
        git config --global user.email "41898282+github-actions[bot]@users.noreply.github.com"
    - uses: ./
      with:
        targetRepository: 'shanduur/move-crds-action'
        targetChart: 'test/chart'
        targetChartVersion: '1.0.0'
        apiToken: '${{ secrets.GITHUB_TOKEN }}'
        files: 'test/static/*'
```

## Inputs

- `targetRepository`: The target repository in which to move the CRD files. The format should be "owner/repo" (e.g., "myuser/myrepo"). (Required)

- `targetChart`: The name of the target chart in the target repository where the CRDs should be added or updated. (Required)

- `targetChartVersion`: The version of the target chart in the target repository. The CRDs will be added to the chart at this version. (Required)

- `files`: A glob pattern to specify the CRD files to be moved. The pattern should match the files to be copied from the source location to the target repository. (Required)

- `apiToken`: The GitHub API token for authentication and API access. By default, uses the `GITHUB_TOKEN` secret provided by the GitHub workflow context. (Required)

- `title`: The title for the pull request when copying CRDs to the target repository. This title will be used for the PR creation or update process. (Optional, default: `'chore(crds): copied CRDs'`)

- `commitMessage`: The commit message to use when copying CRDs to the target repository. This message will be used when creating the commit in the target repository. (Optional, default: `'chore(crds): copied CRDs'`)

- `author`: The name of the author to use when creating the pull request. If not provided, the default GitHub Actions bot will be used. (Optional, default: `'actions'`)

## How It Works

The action runs a bash script that:

1. Copies the specified CRD files from the source repository to the target repository.
2. Creates or updates a pull request with the specified title, commit message, and author in the target repository.

## License

This project is licensed under [The Unlicense](LICENSE). Please see the license file for more information.
