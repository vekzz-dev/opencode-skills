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
| [frontend-engineering](skills/frontend-engineering/) | Frontend engineering: architecture, components, state, UX, accessibility, testing, performance |
| [git-commit](skills/git-commit/) | Conventional commits with intelligent staging and message generation |
| [java-junit](skills/java-junit/) | JUnit 5 best practices: parameterized tests, assertions, mocking |
| [java-springboot](skills/java-springboot/) | Spring Boot production patterns: DI, REST, security, caching |
| [java-springboot-testing](skills/java-springboot-testing/) | Test slices, MockMvcTester, Testcontainers, AssertJ |
| [latex](skills/latex/) | Compile-safe LaTeX academic documents: articles, theses, essays, guides |
| [solution-design](skills/solution-design/) | Design docs from idea to implementation: PRD, SDD, DBDD, TDD, ADR — scaled to project risk |
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

**Recommended** — use [`skills`](https://github.com/vercel-labs/skills), the open agent skills CLI from Vercel Labs. It installs skills from any git repo into your agent directory and keeps them up to date:

```bash
# 1. See what's available in this repo
npx skills add vekzz-dev/opencode-skills --list

# 2. Install everything globally for OpenCode
npx skills add vekzz-dev/opencode-skills --all -a opencode

# Or pick specific skills
npx skills add vekzz-dev/opencode-skills --skill latex --skill docker -g -a opencode

# 3. Keep them updated
npx skills update
```

Skills are installed by default with symlinks to a single canonical copy, so an update is reflected everywhere. Pass `--copy` if your setup does not support symlinks.

Useful flags:

| Flag           | Description                                                    |
| -------------- | -------------------------------------------------------------- |
| `-g, --global` | Install to `~/.config/opencode/skills/` instead of the project  |
| `-a, --agent`  | Target specific agents (e.g. `opencode`, `claude-code`)        |
| `-s, --skill`  | Install specific skills by name (use `'*'` for all)            |
| `-l, --list`   | List available skills without installing                       |
| `--copy`       | Copy files instead of symlinking                                |
| `-y, --yes`    | Skip all confirmation prompts                                   |
| `--all`        | Install all skills to all agents without prompts                |

`skills` also works with private repositories (it reuses your configured Git, GitHub CLI, or SSH credentials), GitLab, Azure Repos, and local paths.

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
