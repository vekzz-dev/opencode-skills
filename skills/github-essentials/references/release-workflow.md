# Tag-driven releases

## Why the tag is the trigger

The tag is the only immutable artifact in Git. A branch moves; a tag does not. Making `push: tags` the trigger means the version number exists exactly once, in one place, and the workflow that publishes it is deterministic.

Alternatives are worse in specific, predictable ways:

| Trigger | Failure mode |
|---|---|
| `workflow_dispatch` with a version input | The typed version can disagree with the tag or with the manifest. Two sources of truth, no check |
| `release: published` | A human step happens first, so a release can exist without notes or without the workflow ever running |
| `push` to the default branch | Publishes on every merge. Version becomes meaningless |

## Why the tag is validated twice

`on: push: tags: ['v*.*.*']` is a glob, not a schema. It matches `vfoo.bar.baz`. The workflow therefore re-checks two things in bash:

1. The tag matches `^vMAJOR.MINOR.PATCH(-PRERELEASE)?$`.
2. The tagged commit is an ancestor of the default branch.

The second check is the one that prevents real incidents. Tags pushed from a feature branch or from a local experiment publish code nobody reviewed. `git merge-base --is-ancestor "$GITHUB_SHA" FETCH_HEAD` fails the job in that case.

`fetch-depth: 0` is required for the ancestor check. With the default shallow clone the merge base cannot be computed.

## Why `--verify-tag`

`gh release create v1.2.3` will create the tag itself if it does not exist, using whatever the local HEAD is. In a workflow that means a release pointing at an unrelated commit. `--verify-tag` aborts instead.

## Why no registry publishing

Publishing to npm, PyPI, or Maven Central adds a real security surface: trusted publishing must be configured in the registry, the package name must be proven, and long-lived tokens must be replaced by OIDC (`id-token: write`). That is a separate decision per ecosystem, not a default.

When publishing is requested, add it as its own job after the release, with:

```yaml
permissions:
  contents: write
  id-token: write
```

and no static token in `env`.

## Prereleases

Any tag containing `-` (for example `v1.2.0-rc.1`) is published with `--prerelease`, which keeps it out of "Latest". The release notes are generated from the commits and merged pull requests since the previous tag.

## Generated notes are not a changelog

`--generate-notes` builds the release body from pull request titles and commit messages. It is a release page, not a curated document. If the repository keeps a `CHANGELOG.md`, maintain it with `changelog-maintenance` and link to it from the release body.

## First release

```bash
git tag -a v1.0.0 -m "v1.0.0"
git push origin v1.0.0
```

The workflow needs `contents: write`. Repositories with restricted default permissions must also enable it under Settings → Actions → General → Workflow permissions.
