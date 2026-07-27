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

Copy the skill folders to your OpenCode skills directory:

```bash
# User-level skills
cp -r api-design changelog-maintenance create-readme database-design docker git-commit java-junit \
  java-springboot java-springboot-testing latex ui-components update-readme web-mvc \
  ~/.config/opencode/skills/
```

Or clone directly:

```bash
git clone https://github.com/vekzz-dev/opencode-skills.git ~/.config/opencode/skills/opencode-skills
```

Or install individual skills with [slap-skills](https://github.com/vekzz-dev/slap-skills):

```bash
slap install latex
```

## Usage

Skills are loaded automatically based on context triggers defined in each `SKILL.md` frontmatter. You can also reference them explicitly when prompting your agent.

## License

[MIT](LICENSE)
