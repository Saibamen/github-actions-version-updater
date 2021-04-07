# GitHub Action Version Updater

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/saadmk11/github-action-upgrade?style=flat-square)](https://github.com/saadmk11/github-action-upgrade/releases/latest)
[![GitHub](https://img.shields.io/github/license/saadmk11/github-action-upgrade?style=flat-square)](https://github.com/saadmk11/github-action-upgrade/blob/main/LICENSE)
[![GitHub Marketplace](https://img.shields.io/badge/Get%20It-on%20Marketplace-orange?style=flat-square)](https://github.com/marketplace/actions/github-action-upgrade)
[![GitHub stars](https://img.shields.io/github/stars/saadmk11/github-action-upgrade?color=success&style=flat-square)](https://github.com/saadmk11/github-action-upgrade/stargazers)

## What is GitHub Action Version Updater?

**GitHub Action Version Updater** is GitHub Action that is used to **update other GitHub Actions** in a Repository
and create a **pull request** with the updates. It is an automated dependency updater similar to GitHub's **Dependabot**, 
but for GitHub Actions.

## How Does It Work:

* GitHub Action Version Updater first goes through all the **workflows**
  in a repository and **checks for updates** for each of the action used in those workflows.

* If an update is found and if that action is **not ignored** then the workflows are updated
  with the **latest release** of the action being used.

* If at least one update is found a new branch is created and pushed to GitHub

* Then a pull request is created with that branch

## Usage:

We recommend running this action on a [`schedule`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#schedule) 
event or a [`workflow_dispatch`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_dispatch) event.

To integrate `GitHub Action Version Updater` on your repository, create a `YAML`  file 
inside `.github/workflows/` directory (`.github/workflows/updater.yaml`):

```yaml
name: GitHub Action Version Updater

# Controls when the action will run. 
on:
  # can be used to run workflow manually
  workflow_dispatch:
  schedule:
    # Run on every Sunday
    - cron:  '0 0 * * 0'

jobs:
  build:
    runs-on: ubuntu-latest
  
    steps:
      - uses: actions/checkout@v2
        with:
          # Access token with `workflow` scope is required
          token: ${{ secrets.WORKFLOW_SECRET }}

      - name: Run GitHub Action Updater
        uses: saadmk11/github-action-upgrade@main
        with:
          # Optional, This will be used to configure git
          # defaults to `github-actions[bot]` if not provided.
          committer_username: 'test'
          committer_email: 'test@test.com'
          # Access token with `workflow` scope is required
          token: ${{ secrets.WORKFLOW_SECRET }}
          # Do not upgrade these actions (Optional)
          ignore: '["actions/checkout@v2", "actions/cache@v2"]'
```

### Important Note:

GitHub does not allow updating workflow files inside an action workflow run.
The token generated by GitHub in every workflow (`${{secrets.GITHUB_TOKEN}}`) does not have
permission to update a workflow. That's why you need to create a [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)
with **repo** and **workflow** scope and pass it to the action.

To know more about how to pass a secret to GitHub actions you can [Read The Docs](https://docs.github.com/en/actions/reference/encrypted-secrets)

# License

The code in this project is released under the [MIT License](LICENSE).
