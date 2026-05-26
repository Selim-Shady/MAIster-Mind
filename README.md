# MAIster-Mind V1
*By Selim Boukhari* — [LinkedIn](https://www.linkedin.com/in/selim-boukhari-6356b949/)

MAIster-Mind is an AI-assisted development framework that orchestrates opencode agents to turn a specification (`need.md`) into working code. It goes through planning, step-by-step implementation, and automatic verification — all within your context window limits.

> **Requires [opencode](https://opencode.ai)** — install it first.

## Quick start

Pick a preset folder and copy its contents to your project root:

- `MAIsterMind-code` — You want the AI to write new code
- `MAIsterMind-test` — You want the AI to write tests for existing code

Each preset includes:

- `.agents/` — Skills that teach the AI how to behave
- `.opencode/` — Agent permissions and optional model override
- `need.md` — Your specification — fill this in

## Custom skills

You can add your own skills or modify the existing ones. Each skill is a folder under `.agents/skills/<name>/SKILL.md`.

> ⚠️ **Keep these two skills unchanged:** `plan` and `plan-to-blackboard`. They are the orchestration engine.
> When you add a new skill, register it in the `STRICT SKILLS DICTIONARY` inside `plan-to-blackboard/SKILL.md`.

## Writing `need.md`

This is the most important file. Describe exactly what you want the AI to build. Be precise, structured, and leave no room for interpretation.

**Good example:**
> Create a CLI tool that reads a `config.json` file, validates its schema, and prints "OK" or a list of errors. Use `argparse` for CLI parsing and `jsonschema` for validation. Output should be plain text, no dependencies beyond the standard library.

**Bad example:**
> Make a config checker.

## What the tool does

1. Launches opencode
2. Generates a multi-step plan
3. Creates a "blackboard" — a checklist, AI guidelines (using chosen skills), and verification criteria
4. Lets you review / modify the plan
5. Lets you switch to a different model for the implementation phase
6. Implements one step per agent instance, keeping the context window small
7. Auto-verifies compliance with the blackboard after each step
8. Produces a final quality analysis
