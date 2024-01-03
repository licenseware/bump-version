# Bump Version

This GitHub Action allows you to bump the patch version of your semantic
versioned project.

## Usage

### Periodic releases at midnight

```yaml
name: ci

on:
  schedule:
    - cron: "30 1 */1 * *"

jobs:
  scheduled-release:
    if: github.event_name == 'schedule'
    permissions:
      contents: write  # required for git tag
    runs-on: ubuntu-latest
    steps:
      - name: Bump version
        uses: licenseware/bump-version@v1
        with:
          release: "true"  # Publish GitHub release (truthy values: true, yes, 1)
          tag: "true"  # Tag and push to repository (truthy values: true, yes, 1)
          token: ${{ secrets.GITHUB_TOKEN }}  # required for GitHub release
```

### Bump with no side-effect

```yaml
name: ci

on:
  push:
    tags:
      - v*

jobs:
  bump-version:
    runs-on: ubuntu-latest
    steps:
      - name: Bump version
        id: bump
        uses: licenseware/bump-version@v1
      - name: Get the new version
        run: echo "The new version is ${{ steps.bump.outputs.version }}"
```

## How it works

Here's a table of input and their output when running this Action:

| Input | Output |
| --- | --- |
| v1 | v1.0.1 |
| v1.2 | v1.2.1 |
| v1.2.3 | v1.2.4 |
