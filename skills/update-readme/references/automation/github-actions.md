# GitHub Actions for README Auto-Update

Set up automated README maintenance with GitHub Actions.

## Auto-Update Workflows

### 1. Update Version Badge

```yaml
# .github/workflows/update-version-badge.yml
name: Update Version Badge

on:
  release:
    types: [published]
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Update badge version
        run: |
          VERSION=${{ github.event.release.tag_name }}
          VERSION=${VERSION#v}  # Remove v prefix
          
          # Update version in README
          sed -i "s/v[0-9]*\.[0-9]*\.[0-9]*/v$VERSION/g" README.md
          
      - name: Commit changes
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "docs: Update version badge"
          file_pattern: README.md
```

### 2. Update Stats Weekly

```yaml
# .github/workflows/readme-stats.yml
name: Update README Stats

on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Get stats
        run: |
          STAR_COUNT=$(gh repo view ${{ github.repository }} --json stargazerCount -q .stargazerCount)
          FORK_COUNT=$(gh repo view ${{ github.repository }} --json forkCount -q .forkCount)
          
          # Update README
          sed -i "s/\[\!\[Stars\]\](\[^(\]\+)/[![Stars](https://img.shields.io\/github\/stars\/${{ github.repository }})]/g" README.md

      - name: Commit
        uses: stefanzweifel/git-auto-commit-action@v5
```

### 3. Self-Updating README

```yaml
# .github/workflows/readme.yml
name: Update README

on:
  schedule:
    - cron: '0 6 * * *'  # Daily at 6am
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          ref: main
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Update stats
        run: |
          # Get latest stats
          STARS=$(gh api repos/${{ github.repository }} --jq .stargazers_count)
          FORKS=$(gh api repos/${{ github.repository }} --jq .forks_count)
          
          # Update README using env vars
          echo "STARS: $STARS"
          echo "FORKS: $FORKS"
          
          # Use刺客 to replace in README
          sed -i "s/\[\!\[Stars\]\](.*stars.*\](*)\)/[![Stars](https://img.shields.io\/github\/stars\/${{ github.repository }}?style=flat)]/g" README.md

      - name: Commit
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "docs: Auto-update README stats"
```

## Complete Auto-Update Template

```yaml
name: README Maintenance

on:
  schedule:
    - cron: '0 0 1 * *'  # Monthly
  release:
    types: [published]
  pull_request:
    paths:
      - 'README.md'
  workflow_dispatch:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Check for issues
        run: |
          # Run outdated detection
          bash .github/scripts/readme-check.sh
          
          # Create issues if found
          if [ -f README_ISSUES.md ]; then
            echo "ISSUES_FOUND=true" >> $GITHUB_ENV
          fi

      - name: Report issues
        if: env.ISSUES_FOUND == 'true'
        run: |
          echo "README needs updates - see README_ISSUES.md"
          cat README_ISSUES.md

  update-badges:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Update version badge
        if: github.event_name == 'release'
        run: |
          VERSION=${{ github.event.release.tag_name }}
          sed -i "s/npm\/v[0-9.]*/npm\/v${VERSION#v}/g" README.md

      - name: Auto-commit
        uses: stefanzweifel/git-auto-commit-action@v5
        with:
          commit_message: "docs: Auto-update README badges"
```

## Scheduled Updates

### Daily Stats Update

```yaml
on:
  schedule:
    - cron: '0 */4 * * *'  # Every 4 hours
```

### Weekly Cleanup

```yaml
on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday
```

### Monthly Review

```yaml
on:
  schedule:
    - cron: '0 0 1 * *'  # First day of month
```

## Environment Variables in README

Use GitHub Secrets:

```yaml
- name: Update with secret
  run: |
    echo "::add-mask::$API_KEY"
    sed -i "s/API_KEY_PLACEHOLDER/$API_KEY/g" README.md
```

## GitHub Token

```yaml
permissions:
  contents: write

steps:
  - uses: actions/checkout@v4
    with:
      token: ${{ secrets.GITHUB_TOKEN }}
```

## Complete Example

```yaml
name: README Auto-Update

on:
  schedule:
    - cron: '0 6 * * *'  # Daily
  release:
    types: [published]

permissions:
  contents: write
  issues: write

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Get repo stats
        id: stats
        run: |
          STARS=$(gh api repos/${{ github.repository }} --jq .stargazers_count)
          echo "stars=$STARS" >> $GITHUB_OUTPUT

      - name: Update README
        run: |
          # Update stars
          sed -i 's/\[\!\[Stars\]\](.*)/[![Stars](https:\/\/img.shields.io\/github\/stars\/${{ github.repository }}?style=flat)]/g' README.md
          
          # Update version if release
          if [ "${{ github.event_name }}" = "release" ]; then
            VERSION=${{ github.event.release.tag_name }}
            sed -i "s/v[0-9.]*/${VERSION#v}/g" README.md
          fi

      - name: Commit
        uses: stefanzweifel/git-autoCommit-action@v5
        with:
          commit_message: "docs: Auto-update README"
```

## Testing Workflows

```bash
# Test locally with act
act workflow_dispatch --verbose

# Dry run
act --dryrun workflow_dispatch
```
