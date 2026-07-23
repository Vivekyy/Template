# Agentic Coding Template

A minimal repository template for projects developed with coding agents.
It provides shared instructions and reusable skills for Claude Code and Codex, consistent Markdown checks, and pull-request guardrails for agent-authored changes.

## What is included

- A single set of repository instructions in `AGENTS.md`, shared with Claude Code through `CLAUDE.md`.
- A canonical `.agents/skills/` directory exposed to both Claude Code and Codex.
- TypeScript-based Danger rules that protect maintainer-owned files and inline guarded regions from agent-authored changes.
- Markdown formatting checks powered by [rumdl](https://github.com/rvben/rumdl).
- GitHub Actions workflows for Markdown validation and agent-guard enforcement.

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/) 24
- npm

### Set up the repository

1. Create a repository from this template or clone it locally.
2. Install the development dependencies:

   ```sh
   npm ci
   ```

3. Update the project name and metadata in `package.json`.
4. Add your application code, tests, and project-specific scripts.
5. Replace the general guidance in `AGENTS.md` with any additional instructions agents need to work safely in your codebase.

## Commands

| Command                         | Purpose                                      |
| ------------------------------- | -------------------------------------------- |
| `npm run typecheck`             | Type-check the Danger configuration          |
| `npm run format:markdown`       | Format Markdown files with rumdl             |
| `npm run format:markdown:check` | Check Markdown formatting without rewriting  |
| `npm run danger:ci`             | Run agent-guard checks in a pull request      |

`danger:ci` expects the pull-request environment and a `GITHUB_TOKEN`; it normally runs through GitHub Actions rather than on a developer workstation.

## Shared agent skills

`.agents/skills/` is the source of truth for reusable workflows.
The `.claude/skills` and `.codex/skills` paths are symlinks to that directory, so a skill only needs to be maintained once.

Add each skill under its own directory:

```text
.agents/skills/
└── example-skill/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

`SKILL.md` contains the workflow instructions.
The `agents/openai.yaml` file supplies the Codex display name, short description, and default prompt.

## Agent guardrails

The Danger workflow runs on pull requests targeting `main`.
When a pull request is identified as agent-authored, it:

- rejects changes to paths listed in `PROTECTED_PATHS` in `dangerfile.ts`;
- rejects changes between comment-only `agent-guard:off` and `agent-guard:on` markers;
- warns reviewers when guard policy files or inline guard markers change.

A pull request is considered agent-authored when the PR author, a commit author, or a `Co-authored-by` trailer identifies a coding agent.
Agents contributing code should identify themselves in at least one commit so these checks run consistently.

Changes to guard rules should be made as separately reviewed policy changes, not as part of the implementation they would permit.

## Repository layout

```text
.
├── .agents/                  # Canonical shared agent workflows
├── .claude/skills            # Claude Code compatibility symlink
├── .codex/skills             # Codex compatibility symlink
├── .github/workflows/        # Markdown and Danger CI
├── AGENTS.md                 # Repository-wide agent instructions
├── CLAUDE.md                 # Claude Code entry point
├── dangerfile.ts             # Agent guard policy
├── package.json              # Tooling and npm scripts
└── tsconfig.json             # TypeScript configuration
```

## License

This template is available under the [MIT License](LICENSE).
