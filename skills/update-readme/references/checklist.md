# README Update Checklist

Use this checklist before every release or monthly review.

## Pre-Update Checklist

### Installation

- [ ] Package manager command works (`npm install`, `pip install`, etc.)
- [ ] Correct package name used
- [ ] Version specified or latest tag works
- [ ] All installation methods tested (npm, yarn, pnpm)
- [ ] Peer dependencies mentioned if needed

### Requirements

- [ ] Node.js/Python/Java version current
- [ ] Requirements match `package.json` engines
- [ ] Requirements match `setup.py` / `pyproject.toml`
- [ ] Requirements match `pom.xml` / `build.gradle`
- [ ] Minimum versions documented

### Usage

- [ ] All code examples run without errors
- [ ] Examples copy-paste and work
- [ ] Output matches documentation
- [ ] Required imports shown
- [ ] Configuration examples work

### API Documentation

- [ ] All public methods documented
- [ ] Signatures match actual code
- [ ] Return types correct
- [ ] Parameters documented
- [ ] Examples for each method
- [ ] Deprecations noted

### Features

- [ ] All features listed
- [ ] No removed features in docs
- [ ] New features added
- [ ] Feature flags documented
- [ ] Roadmap accurate

### Badges

- [ ] Version badge points to current version
- [ ] Build badge passes
- [ ] License badge correct
- [ ] All badges clickable
- [ ] No broken badge URLs
- [ ] Downloads/stars current

### Configuration

- [ ] ENV variables current
- [ ] Config file examples work
- [ ] Default values accurate
- [ ] All options documented

### Links

- [ ] All external links work
- [ ] No broken redirects
- [ ] Documentation links current
- [ ] CHANGELOG linked if exists

### Security

- [ ] No exposed API keys
- [ ] No credentials in examples
- [ ] Security practices documented

### Formatting

- [ ] GFM syntax correct
- [ ] Tables render properly
- [ ] Code blocks syntax highlighted
- [ ] No broken images

## Post-Update Checklist

- [ ] README renders on GitHub
- [ ] Table of contents works (if present)
- [ ] Examples tested in clean environment
- [ ] Spelling checked
- [ ] Previewed in editor

## Release-Specific Checklist

For new releases:

- [ ] Version number updated in README
- [ ] Changelog linked or included
- [ ] Migration guide if breaking
- [ ] Deprecation timeline updated
- [ ] New features highlighted

## Automation Verification

If using auto-updates:

- [ ] GitHub Actions passing
- [ ] Dynamic badges loading
- [ ] Stats updating
- [ ] No CI errors

## Quick Smoke Test

Run this 30-second test:

```bash
# 1. Check version matches
grep -E '"version"' package.json

# 2. Check Node requirement
grep -E '"engines"' package.json

# 3. Check badges exist
grep -c "shields.io" README.md

# 4. Check for outdated patterns
grep -E "Node\.js (1[0-9]|20)" README.md || echo "Check versions"

# 5. Check for TODO/FIXME
grep -E "TODO|FIXME" README.md
```
