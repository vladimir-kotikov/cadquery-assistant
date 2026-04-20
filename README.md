# cadquery-llm-skill

A skill that helps LLMs write correct, idiomatic [CadQuery](https://cadquery.readthedocs.io) code.

LLMs regularly get CadQuery wrong - defaulting to Boolean/CSG thinking instead of
CadQuery's BRep-native approach, misusing selectors, building in global coordinates,
and reaching for `.union()` / `.cut()` when a workplane operation would be cleaner.
This skill gives Claude Code the context it needs to do better.

## What's Included

- **`SKILL.md`** - dense quick-reference: BRep mindset, both APIs, selector cheat sheet, critical anti-patterns
- **`concepts/`** - deep-dive files on BRep vs CSG, workplanes, selectors, and the Free Function API
- **`patterns/`** - verified common patterns and anti-patterns with explanations
- **`examples/`** - annotated runnable scripts
- **`docs/`** - local snapshot of the CadQuery RST documentation for offline grepping

## Installation

Clone the repo somewhere accessible on your machine:

```bash
git clone https://github.com/jmwright/cadquery-llm-skill.git ~/cadquery-llm-skill
```

## Usage

### Option 1 - Reference in a prompt (one-off)

Prefix any CadQuery prompt with `@` to load the skill file:

```
@~/cadquery-llm-skill/SKILL.md

Write a CadQuery script that creates a flanged pipe fitting.
```

Claude Code will read `SKILL.md` before responding. For deeper questions, reference
specific concept files:

```
@~/cadquery-llm-skill/SKILL.md
@~/cadquery-llm-skill/concepts/selectors.md

Why is my selector not picking the right face?
```

### Option 2 - Add to a project's CLAUDE.md (persistent)

Add the following to your project's `CLAUDE.md` file so Claude Code loads the skill
automatically for every CadQuery task in that project:

```markdown
## CadQuery Skill

This project uses CadQuery. Before writing any CadQuery code, read:
@~/cadquery-llm-skill/SKILL.md

For deeper reference:
- Selectors: @~/cadquery-llm-skill/concepts/selectors.md
- Workplanes: @~/cadquery-llm-skill/concepts/workplanes.md
- Free Function API: @~/cadquery-llm-skill/concepts/free-function-api.md
- Anti-patterns: @~/cadquery-llm-skill/patterns/anti-patterns.md

When unsure about a method, grep the local docs:
grep -rn "method_name" ~/cadquery-llm-skill/docs/
```

Adjust the path to match where you cloned the repo.

### Option 3 - Global CLAUDE.md

To enable the skill for all projects, add the same block to `~/.claude/CLAUDE.md`.

## Grepping the Docs

The `docs/` directory is a local snapshot of the CadQuery RST documentation.
Use it to look up method signatures, selector behaviour, and API details without
a network request:

```bash
# Find all mentions of a method
grep -rn "\.shell\(" ~/cadquery-llm-skill/docs/

# Look up selector syntax
grep -n "NearestToPointSelector\|StringSyntaxSelector" ~/cadquery-llm-skill/docs/selectors.rst

# Check the Free Function API docs
grep -n "def loft\|def cap\|def fill" ~/cadquery-llm-skill/docs/free-func.rst
```

Claude Code can run these same greps when it needs to verify API details.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). All code examples must be verified before submission.
