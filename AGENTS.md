# Repository Guidelines

## Project Structure & Module Organization
The repo is intentionally lightweight: `01-system-prompts/` houses persona definitions for every AI teammate (PM, architect, backend, frontend, QA, reviewer, supervisor), while `02-workflow-templates/` stores reusable collaboration templates such as `project-kickoff.md` and the canonical `task-breakdown.json`. Keep each prompt self-contained and reference siblings via relative paths so downstream tools like team coordinators can assemble flows reliably. Use `README.md` for high-level onboarding context and reserve any experimental drafts for `README_back.md`.

## Build, Test, and Development Commands
- `tree -L 2` gives a quick structure check before and after edits; run it from repo root when documenting layout changes.
- `npx markdownlint "**/*.md"` enforces consistent Markdown spacing, headings, and list styles across every agent prompt.
- `python -m json.tool 02-workflow-templates/task-breakdown.json` validates workflow JSON after edits; replace the path with any new template you add.

## Coding Style & Naming Conventions
Stick to Markdown with ATX headings and keep sections concise (≤120 words) so instructions fit within LLM context limits. Roles follow snake_case filenames (`backend_developer.md`) and use level-2 headings for capabilities, workflows, and guardrails. Highlight literal commands in fenced blocks and rely on ASCII punctuation. For workflow templates, prefer deterministic keys in lowerCamelCase and describe required/optional fields inline.

## Testing Guidelines
Treat linting as the primary safety net: Markdown lint plus JSON formatting must pass before opening a PR. When adding runnable examples (e.g., CLI snippets), execute them locally and paste sanitized outputs so QA agents can replay steps verbatim. For prompt logic, stage manual “tabletop tests”: feed the updated prompt to your preferred LLM and capture any regressions in tone or scope in the PR description.

## Commit & Pull Request Guidelines
Recent history shows short Chinese summaries (`git log` → “提交我的项目”); improve clarity by using imperative, descriptive English or bilingual messages such as `feat: add qa-agent defect triage checklist / 新增缺陷排查清单`. Each PR should link to the relevant template or persona, list validation commands, and include screenshots or LLM transcripts when behavior changes. Request at least one reviewer familiar with the affected role to keep the virtual team aligned.
