# opencode-skills

A collection of AI agent skills for [OpenCode](https://github.com/opencode-ai/opencode) — structured instructions that guide AI agents through specific workflows.

## Skills

| Skill | Description |
|-------|-------------|
| [api-design](skills/api-design/) | REST API design: resource naming, versioning, error handling, pagination, HATEOAS, OpenAPI |
| [changelog-maintenance](skills/changelog-maintenance/) | Semantic versioning, changelogs, and release notes |
| [create-readme](skills/create-readme/) | Generate comprehensive README files for any project type |
| [database-design](skills/database-design/) | Database modeling, normalization, indexing, migrations, query optimization |
| [docker](skills/docker/) | Multi-stage builds, docker-compose, security, image optimization |
| [git-commit](skills/git-commit/) | Conventional commits with intelligent staging and message generation |
| [java-junit](skills/java-junit/) | JUnit 5 best practices: parameterized tests, assertions, mocking |
| [java-springboot](skills/java-springboot/) | Spring Boot production patterns: DI, REST, security, caching |
| [java-springboot-testing](skills/java-springboot-testing/) | Test slices, MockMvcTester, Testcontainers, AssertJ |
| [latex](skills/latex/) | Compile-safe LaTeX academic documents: articles, theses, essays, guides |
| [ui-components](skills/ui-components/) | Preline UI, HyperUI, Flowbite — component libraries for Thymeleaf + HTMX |
| [update-readme](skills/update-readme/) | Detect and update outdated README content |
| [vault-tech-note](skills/vault-tech-note/) | Learning-first tech notes for an Obsidian vault: verified sources, mental models, worked examples |
| [web-mvc](skills/web-mvc/) | Thymeleaf + HTMX + Alpine.js — server-side web UIs without React |

## Structure

Each skill follows this layout:

```
skill-name/
├── SKILL.md           # Main instructions (frontmatter + workflow)
└── references/        # Optional supplementary docs
    └── *.md
```

## Installation

**Recommended** — use [slap-skills](https://github.com/vekzz-dev/slap-skills) to sync skills from this (or any) git repo directly to your OpenCode directory:

```bash
# 1. Install slap-skills
brew tap vekzz-dev/tap
brew install slap-skills

# 2. Add this repo as a source
# First time? Run `slap init` — the wizard asks for the URL and sets everything up.
# Already have sources configured? Use `slap source add`.
slap source add --alias opencode-skills https://github.com/vekzz-dev/opencode-skills

# 3. Pick which skills to install
slap install

# 4. Keep them updated
slap sync
```

`slap-skills` manages skills from any git repo — public or private. Add this repo as a source, select the skills you want, and `slap sync` keeps them updated. No cloning the whole collection, no manual `cp`. It also detects drift, warns on local edits, and survives corrupt manifests.

---

You can also install manually:

```bash
# Clone the whole collection
git clone https://github.com/vekzz-dev/opencode-skills.git ~/.config/opencode/skills/opencode-skills

# Or copy specific skills
cp -r skills/latex ~/.config/opencode/skills/
```

## Usage

Skills are loaded automatically based on context triggers defined in each `SKILL.md` frontmatter. You can also reference them explicitly when prompting your agent.

## License

[MIT](LICENSE)
