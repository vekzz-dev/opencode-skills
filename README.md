# opencode-skills

A collection of AI agent skills for [OpenCode](https://github.com/opencode-ai/opencode) — structured instructions that guide AI agents through specific workflows.

## Skills

| Skill | Description |
|-------|-------------|
| [changelog-maintenance](changelog-maintenance/) | Semantic versioning, changelogs, and release notes |
| [create-readme](create-readme/) | Generate comprehensive README files for any project type |
| [git-commit](git-commit/) | Conventional commits with intelligent staging and message generation |
| [java-junit](java-junit/) | JUnit 5 best practices: parameterized tests, assertions, mocking |
| [java-springboot](java-springboot/) | Spring Boot production patterns: DI, REST, security, caching |
| [spring-boot-testing](spring-boot-testing/) | Test slices, MockMvcTester, Testcontainers, AssertJ |
| [update-readme](update-readme/) | Detect and update outdated README content |

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
cp -r changelog-maintenance create-readme git-commit java-junit java-springboot spring-boot-testing update-readme \
  ~/.config/opencode/skills/
```

Or clone directly:

```bash
git clone https://github.com/vekzz-dev/opencode-skills.git ~/.config/opencode/skills/opencode-skills
```

## Usage

Skills are loaded automatically based on context triggers defined in each `SKILL.md` frontmatter. You can also reference them explicitly when prompting your agent.

## License

[MIT](LICENSE)
