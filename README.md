# Blast Radius

**Blast Radius** is a read-only Claude Code plugin that maps the likely impact of a code change **before implementation starts**.

It explores an unfamiliar or complex repository and returns a compact change-impact report covering:

- files and modules likely to change
- dependency chains and wiring
- API, schema, persistence, event, configuration, and serialization contracts
- relevant tests and missing coverage
- compatibility, migration, state, security, concurrency, and performance risks
- a safe implementation order

The plugin is intentionally conservative: its analysis agent only receives Claude Code's `Read`, `Grep`, and `Glob` tools. It cannot edit files or run shell commands.

## Install from the Garb-in-PacK marketplace

Add the marketplace:

```text
/plugin marketplace add Garb-in-PacK/blast-radius
```

Install the plugin:

```text
/plugin install blast-radius@garb-in-pack
```

Reload if Claude Code asks you to:

```text
/reload-plugins
```

## Usage

```text
/blast-radius:analyze "add Redis caching to the user profile endpoint"
```

More examples:

```text
/blast-radius:analyze "rename CustomerId to AccountId across the public API"

/blast-radius:analyze "move order processing from synchronous HTTP to a background queue"

/blast-radius:analyze "replace the legacy XML serializer without breaking stored data"
```

## What the report contains

A typical result includes:

1. **Change surface** — Direct / Likely / Watch files and modules
2. **Dependency chain** — the important path through the codebase
3. **Contracts and compatibility** — interfaces that must stay compatible or migrate safely
4. **Tests** — existing coverage to run or update and the smallest missing tests
5. **Risk register** — severity, repository evidence, and mitigation
6. **Safe implementation order** — a reversible, compatibility-preserving sequence
7. **Unknowns** — only questions the repository itself could not answer

The report ends with an overall **Blast radius** and **Confidence** assessment.

## Why this exists

Small-looking changes in mature codebases often cross architectural boundaries. A normal implementation prompt can discover those dependencies only after edits have started. Blast Radius makes reconnaissance a separate, reviewable step.

It is particularly useful for:

- legacy code
- unfamiliar repositories
- cross-service refactors
- schema and serialization changes
- dependency upgrades
- caching or state changes
- API evolution
- risky production fixes

## Read-only design

The skill runs in a forked subagent context using the bundled `blast-radius-researcher` agent.

That agent is restricted to:

```text
Read
Grep
Glob
```

No `Write`, `Edit`, `Bash`, or other mutation-capable tools are provided to the agent.

## Local development

Clone the repository and load the plugin directly:

```bash
claude --plugin-dir .
```

Then run:

```text
/blast-radius:analyze "your proposed change"
```

After changing plugin files:

```text
/reload-plugins
```

Validate before publishing a release:

```bash
claude plugin validate .
claude plugin validate . --strict
```

## Repository structure

```text
blast-radius/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── agents/
│   └── blast-radius-researcher.md
├── skills/
│   └── analyze/
│       └── SKILL.md
├── CHANGELOG.md
├── LICENSE
└── README.md
```

## Author

[Garb-in-PacK](https://github.com/Garb-in-PacK)

## License

MIT
