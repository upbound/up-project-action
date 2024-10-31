# up-project-action

This GitHub Action, `up-project-action`, installs the `up-cli` tool, authenticates with upbound using a personal access token, builds the up project, and optionally pushes it to upbound on the `main` branch.

## Features

- **Install up-cli**: Installs `up-cli` from a specified channel and version.
- **Authenticate with upbound**: Logs in to upbound using a personal access token.
- **Build Project**: Builds the up project in the current repository.
- **Conditional Push**: Pushes the up project to upbound only when running on the `main` branch.

## Usage

Add this action to your workflow to automatically build and push up projects on specified branches.

### Example Workflow

```yaml
name: Build and Deploy up Project

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Run upbound Project Action
        uses: upbound/up-project-action@v1
        with:
          channel: 'stable'                # Optional, default is 'stable'
          version: 'current'               # Optional, default is 'current'
          up_token: ${{ secrets.up_TOKEN }}  # Required, upbound API token
          endpoint: 'https://cli.upbound.io' # Optional, default is 'https://cli.upbound.io'
```

## Inputs

| Name       | Description                                                                       | Required | Default                     |
|------------|-----------------------------------------------------------------------------------|----------|-----------------------------|
| `channel`  | Channel for `up-cli` installation (`main`, `stable`, etc.).                       | No       | `stable`                    |
| `version`  | Version of `up-cli` to install (e.g., specific version or `current`).             | No       | `current`                   |
| `up_token` | upbound API token for authentication. This should be stored in GitHub Secrets.    | Yes      |                             |
| `endpoint` | Endpoint for `up-cli` download.                                                   | No       | `https://cli.upbound.io`    |

## Workflow Details

- **Install up-cli**: Downloads and installs `up-cli` based on the specified `channel` and `version`.
- **Authenticate with upbound**: Logs in using the provided `up_token`.
- **Build Project**: Runs `up project build` to build the project.
- **Push Project (main branch only)**: Runs `up project push` only when the workflow is triggered by a push to the `main` branch.

## Notes

- **Permissions**: Make sure to provide the `up_token` as a GitHub Secret to ensure secure authentication.
- **Conditional Push**: The action will push the project only on pushes to the `main` branch, making it suitable for production deployments.

## Author

Created by [upbound](https://www.upbound.io).
