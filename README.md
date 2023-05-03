# Sensitive Codepaths Check Action

This GitHub action checks for changes in specified codepaths within a pull request. It can be used to monitor sensitive or critical parts of your codebase, and notify you when changes are detected.

## Inputs

| Input Name | Description                        | Required | Default Value |
|------------|------------------------------------|----------|---------------|
| token      | The GitHub token for authentication| Yes      |               |

## Usage

1. First, create a `sensitive-codepaths.yml` file in the root directory of your repository, and list the codepaths you want to monitor:

```yaml
# sensitive-codepaths.yml
codepaths:
  - './middleware/auth.js'
  - './path/to/another/file.js'
  - './path/to/some/directory'
```

2. Add the following snippet to your workflow file (e.g., .github/workflows/check_changes.yml):

```yaml
- uses: wickett/sensitive-codepaths@v1
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Example

```yaml
name: Check changes in specified codepaths

on:
  pull_request:
    types:
      - opened
      - synchronize

jobs:
  check_changes:
    runs-on: ubuntu-latest
    steps:
      - uses: wickett/sensitive-codepaths@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
```