# Dynamic Badges

Use dynamic badges that update automatically without manual maintenance.

## What Are Dynamic Badges?

Badges that fetch live data and update automatically.

## Dynamic Badge Services

### Shields.io

Most popular, free badge service.

```markdown
[![GitHub stars](https://img.shields.io/github/stars/USER/REPO)](URL)
```

### Badgen.net

Fast, simple badges.

```markdown
[![Downloads](https://badgen.net/npm/dm/package)](URL)
```

### Cody Lindsey

Alternative shields-style badges.

## GitHub Stats Badges

### Stars

```markdown
[![Stars](https://img.shields.io/github/stars/USER/REPO?style=flat)](https://github.com/USER/REPO/stargazers)
```

### Forks

```markdown
[![Forks](https://img.shields.io/github/forks/USER/REPO?style=flat)](https://github.com/USER/REPO/fork)
```

### Downloads

```markdown
npm:
[![npm](https://img.shields.io/npm/dm/PACKAGE)](https://www.npmjs.com/package/PACKAGE)

PyPI:
[![PyPI - Downloads](https://img.shields.io/pypi/dm/PACKAGE)](https://pypi.org/project/PACKAGE/)
```

### Last Commit

```markdown
[![Last commit](https://img.shields.io/github/last-commit/USER/REPO)](https://github.com/USER/REPO/commits)
```

### Commit Activity

```markdown
[![Commit activity](https://img.shields.io/github/commit-activity/w/USER/REPO)](https://github.com/USER/REPO/graphs/commit-activity)
```

### Contributors

```markdown
[![Contributors](https://img.shields.io/github/contributors/USER/REPO)](https://github.com/USER/REPO/graphs/contributors)
```

## Package Manager Badges

### npm Version

```markdown
[![npm version](https://img.shields.io/npm/v/PACKAGE)](https://www.npmjs.com/package/PACKAGE)
```

### npm Downloads (weekly)

```markdown
[![npm](https://img.shields.io/npm/dw/PACKAGE)](https://www.npmjs.com/package/PACKAGE)
```

### PyPI Version

```markdown
[![PyPI version](https://img.shields.io/pypi/v/PACKAGE)](https://pypi.org/project/PACKAGE/)
```

### PyPI Python Versions

```markdown
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/PACKAGE)](https://pypi.org/project/PACKAGE/)
```

### Maven Central

```markdown
[![Maven Central](https://img.shields.io/maven-central/v/GRPID:ARTIFACTID)](https://central.sonatype.com/artifact/GRPID:ARTIFACTID)
```

## CI/CD Badges

### GitHub Actions

```markdown
[![Build](https://github.com/USER/REPO/actions/workflows/WORKFLOW/badge.svg)](https://github.com/USER/REPO/actions)
```

### CircleCI

```markdown
[![CircleCI](https://dl.circleci.com/status-badge/img/gh/USER/REPO/tree/main.svg)](https://app.circleci.com/pipelines/github/USER/REPO)
```

### Travis CI

```markdown
[![Travis CI](https://travis-ci.org/USER/REPO.svg?branch=main)](https://travis-ci.org/USER/REPO)
```

## Code Quality Badges

### Codecov

```markdown
[![codecov](https://codecov.io/gh/USER/REPO/branch/main/graph/badge.svg)](https://app.codecov.io/gh/USER/REPO)
```

### CodeQL

```markdown
[![CodeQL](https://github.com/USER/REPO/actions/workflows/codeql/badge.svg)](https://github.com/USER/REPO/actions)
```

### Code Climate

```markdown
[![Code Climate](https://img.shields.io/codeclimate/maintainability/USER/REPO)](https://codeclimate.com/github/USER/REPO)
```

### Snyk

```markdown
[![Snyk Vulnerabilities](https://img.shields.io/snyk/vulnerabilities/github/USER/REPO)](https://security.snyk.io/vuln/SNYK-JS-PACKAGE)
```

## Custom Dynamic Badges

### Using shields.io API

```markdown
[![Custom](https://img.shields.io/badge/dynamic/json?url=YOUR_JSON_URL&label=Label&color=color)](URL)
```

### Example: Build Size

```markdown
[![Bundle Size](https://img.shields.io/bundlephobia/min/PACKAGE)](https://bundlephobia.com/package/PACKAGE)
```

### Example: Dependency Count

```markdown
[![Dependencies](https://img.shields.io/librariesio/dependent-repos/npm/PACKAGE)](https://libraries.io/npm/PACKAGE)
```

### Example: Pull Requests

```markdown
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)](http://makeapullrequest.com)
```

## Dynamic Badge Examples

### Complete Example

```markdown
# My Project

[![npm version](https://img.shields.io/npm/v/my-package)](https://www.npmjs.com/package/my-package)
[![npm downloads](https://img.shields.io/npm/dm/my-package)](https://www.npmjs.com/package/my-package)
[![License](https://img.shields.io/npm/l/my-package)](LICENSE)
[![Build](https://github.com/user/my-project/actions/workflows/main.yml/badge.svg)](https://github.com/user/my-project/actions)
[![Stars](https://img.shields.io/github/stars/user/my-project)](https://github.com/user/my-project/stargazers)

> Description...
```

### Template Variables

Many services support variables:

```markdown
[![Stars](https://img.shields.io/github/stars/${{ github.repository }})](URL)
```

## Auto-Updating Badges

### Badge Parameters

```markdown
<!-- Refresh every 4 hours -->
[![npm](https://img.shields.io/npm/v/package?color=blue&style=flat)](URL)

<!-- Query parameters -->
https://img.shields.io/npm/v/package?color=blue&label=Version
```

### Refresh Rates

| Service | Auto-refresh |
|---------|---------------|
| shields.io | ~4 hours |
| badgen.net | ~5 minutes |
| gh-badges | On GitHub event |

## Creating Custom Badges

### shields.io Custom

```markdown
https://img.shields.io/badge/dynamic/json?url=URL&label=Label&color=color&query=$.field
```

### Badge in GitHub Actions

```yaml
- name: Update badge
  run: |
    # Update badge with new data
    BADGE_DATA=$(echo "$STATS" | jq -r '.count')
    sed -i "s/\d+ stars/$BADGE_DATA stars/" README.md
```

## Best Practices

1. **Don't overdo** - 3-5 badges at top
2. **Link to real pages** - Make badges clickable
3. **Use consistent style** - Same style across badges
4. **Test badges work** - Verify URLs before committing
5. **Consider performance** - Too many dynamic badges slow load

## Validation

```bash
# Check all badge URLs
grep -oP 'https://img\.shields\.io[^")]+' README.md | while read url; do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
  echo "$status - $url"
done
```
