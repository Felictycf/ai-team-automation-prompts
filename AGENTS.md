# Repository Guidelines

## Project Structure & Module Organization
This repo stores reusable AI agent system prompts and workflow templates. System prompts live in `01-system-prompts/` (architect, code reviewer, PM, supervisor). Process-driven materials reside in `02-workflow-templates/`, mixing Markdown walkthroughs and JSON schemas. Keep the numeric prefixes (`01-`, `02-`) so folders stay ordered chronologically; name new files descriptively, e.g., `01-system-prompts/security-analyst.md`. Reference `README.md` for the high-level catalog when adding new content.

## Build, Test, and Development Commands
Even though the project is documentation-first, treat edits like code:
- `rg -n "##" 01-system-prompts` quickly inspects headings before editing.
- `npx markdownlint "**/*.md"` (install `markdownlint-cli` once) enforces spacing, heading depth, and checklist style.
- `jq empty 02-workflow-templates/task-breakdown.json` validates JSON schemas before committing.

## Coding Style & Naming Conventions
Write Markdown with a single `#` title, sentence-case `##` sections, and bold callouts that mirror existing prompts. Prefer ordered steps for workflows and `[ ]` checklists for actionable items. Keep prose concise and directive. JSON artifacts are two-space indented, with property order matching the schema in `02-workflow-templates/task-breakdown.json`; maintain camelCase keys for nested objects and snake_style for enum values only when already present. Filenames should read `<role>-agent.md` or `<topic>-template.md`.

## Testing Guidelines
Before opening a PR, render new Markdown locally (VS Code preview) and scan for broken lists or spacing regressions. Run `npx markdownlint` to flag heading skips, then spot-read for inclusive language and redundant bullets. For JSON updates, run `jq empty <file>` plus a sample instantiation that passes through your preferred schema validator to ensure constraints such as regex patterns still hold. When altering workflows, include at least one example checklist you ran through manually to confirm the sequence feels actionable.

## Commit & Pull Request Guidelines
Commits follow the imperative, capitalized style already in history (`Create Architect Agent system prompt...`, `Add Supervisor Guidelines...`). Keep each commit scoped to one prompt or workflow tweak. Pull requests should summarize the user scenario, link any tracking issue, list affected files, and paste representative excerpt(s) so reviewers can skim without opening every file. If the change modifies instructions agents will execute, describe the expected behavioral impact and call out any reviewer checklists that need re-running.
