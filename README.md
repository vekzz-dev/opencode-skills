# opencode-skills

A collection of AI agent skills for [OpenCode](https://github.com/opencode-ai/opencode) — structured instructions that guide AI agents through specific workflows.

## Skills

| Skill | Description |
|-------|-------------|
| [api-design](api-design/) | REST API design: resource naming, versioning, error handling, pagination, HATEOAS, OpenAPI |
| [changelog-maintenance](changelog-maintenance/) | Semantic versioning, changelogs, and release notes |
| [create-readme](create-readme/) | Generate comprehensive README files for any project type |
| [database-design](database-design/) | Database modeling, normalization, indexing, migrations, query optimization |
| [docker](docker/) | Multi-stage builds, docker-compose, security, image optimization |
| [git-commit](git-commit/) | Conventional commits with intelligent staging and message generation |
| [java-junit](java-junit/) | JUnit 5 best practices: parameterized tests, assertions, mocking |
| [java-springboot](java-springboot/) | Spring Boot production patterns: DI, REST, security, caching |
| [java-springboot-testing](java-springboot-testing/) | Test slices, MockMvcTester, Testcontainers, AssertJ |
| [latex](latex/) | Compile-safe LaTeX academic documents: articles, theses, essays, guides |
| [ui-components](ui-components/) | Preline UI, HyperUI, Flowbite — component libraries for Thymeleaf + HTMX |
| [update-readme](update-readme/) | Detect and update outdated README content |
| [web-mvc](web-mvc/) | Thymeleaf + HTMX + Alpine.js — server-side web UIs without React |

## Structure

Each skill follows this layout:

```
skill-name/
├── SKILL.md           # Main instructions (frontmatter + workflow)
└── references/        # Optional supplementary docs
    └── *.md
```

## Installation

**Recommended** — use [slap-skills](https://github.com/vekzz-dev/slap-skills) to install, update, and manage individual skills on demand:

```bash
# Install a single skill
slap install latex

# Install multiple at once
slap install docker api-design web-mvc

# List available skills
slap search

# Update all installed skills
slap update
```

`slap-skills` is a CLI tool that downloads skills directly from this repo to your OpenCode skills directory, keeps them updated, and lets you cherry-pick only what you need — no cloning the entire collection.

---

You can also install manually:

```bash
# Clone the whole collection
git clone https://github.com/vekzz-dev/opencode-skills.git ~/.config/opencode/skills/opencode-skills

# Or copy specific skills
cp -r latex ~/.config/opencode/skills/
```

## Usage

Skills are loaded automatically based on context triggers defined in each `SKILL.md` frontmatter. You can also reference them explicitly when prompting your agent.

## License

[MIT](LICENSE)
