# Sensitive Codepaths Checker Action

This GitHub action checks for changes in specified sensitive codepaths in your repository. It posts a comment on the pull request or commit, indicating whether any sensitive codepaths were impacted.

## Usage

### Pre-requisites

1. Generate a Personal Access Token (PAT) with the `repo` scope by following the instructions in the [GitHub documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
2. Add the generated PAT to your repository's secrets. Go to the repository settings, then click on "Secrets" in the left sidebar. Click on "New repository secret", give it a name like `MY_GITHUB_PAT`, and paste the PAT as the value.
3. Create a `sensitive-codepaths.yml` file in your repository root directory, specifying the sensitive codepaths.

```yaml
# sensitive-codepaths.yml
codepaths:
  - './middleware/auth.js'
  - './path/to/another/file.js'
  - './path/to/some/directory'
```
4. Create a `.github/workflows/check_changes.yml` file in your GitHub repo (you can use the example below as a starting point).

### Example workflow

```yaml
name: Check changes in specified codepaths

on:
  push:
    branches:
      - '**'
  pull_request:
    types:
      - opened
      - synchronize

jobs:
  check_changes:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Sensitive Codepaths Checker
        uses: wickett/sensitive-codepaths@v0.0.1
        env:
          GITHUB_TOKEN: ${{ secrets.MY_GITHUB_PAT }}


## Personal Access Token (PAT)
This action requires a Personal Access Token (PAT) with the repo scope for proper functioning. Follow these steps to set up a PAT:

  1. Generate a Personal Access Token (PAT) with the repo scope by following the instructions in the GitHub documentation.
  2. Add the generated PAT to your repository's secrets. Go to the repository settings, then click on "Secrets" in the left sidebar. Click on "New repository secret", give it a name like MY_GITHUB_PAT, and paste the PAT as the value.
  3. Make sure you the name matches in check_changes.yml

Important: Using a PAT has security implications, as it grants additional access to your account. Make sure to properly protect the PAT and only use it in trusted repositories.

