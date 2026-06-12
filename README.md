# pi — My AI Coding Sidekick

This is my personal, minimal pi setup. pi is an AI coding agent I use to help write code, refactor stuff, debug, and generally get things done faster.

---

## What's Where

```
~/.pi/
├── agent/                  # All the pi config lives here
│   ├── AGENTS.md           # Rules pi follows when working
│   ├── settings.json       # Settings — model, theme, installed packages
│   ├── mcp.json            # Connects to MCP servers (context-mode)
│   ├── keybindings.json    # Custom keyboard shortcuts for the TUI
│   ├── skills/             # Instructions pi uses for specific tasks
│   ├── extensions/         # Extra features (like subagents)
│   ├── sessions/           # Session state
│   └── npm/                # Packages that extend pi
├── context-mode/           # Context-mode data (knowledge base, sessions)
├── README.md               # This file
└── .gitignore
```

---

## What's Installed

**Packages that make pi actually useful:**

| Package               | What it does                                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `context-mode`        | Keeps a searchable knowledge base so pi remembers stuff between sessions. Also has sandbox tools for running code without cluttering the conversation. |
| `pi-lens`             | Code analysis — catches type errors, lint issues, security problems as I work.                                                                         |
| `pi-subagents`        | Lets pi spin up sub-agents to work on multiple things at once (chains, parallel tasks, etc.).                                                          |
| `pi-tasks`            | Task tracking for multi-step work — a todo list pi can manage.                                                                                         |
| `pi-mcp-adapter`      | Hooks pi up to MCP servers (the protocol behind context-mode).                                                                                         |
| `pi-markdown-preview` | Renders Markdown/LaTeX to PDF, HTML, or images.                                                                                                        |
| `pi-powerline-footer` | A nicer status bar in the TUI.                                                                                                                         |
| `ask-user-question`   | Lets pi ask me structured questions instead of guessing what I want.                                                                                   |

---

## Skills — How pi Knows What to Do

Skills are basically playbooks. When pi sees a certain kind of task, it loads the relevant skill so it knows how to handle it properly.

| Skill                            | When pi uses it                                                     |
| -------------------------------- | ------------------------------------------------------------------- |
| `brainstorming`                  | Before building something — explores requirements and design first  |
| `conventional-commit`            | Generating commit messages the right way                            |
| `dispatching-parallel-agents`    | When there's multiple independent things to do at once              |
| `executing-plans`                | Following a written implementation plan step by step                |
| `finishing-a-development-branch` | Wrapping up a branch — merge, PR, cleanup                           |
| `frontend-design`                | Building UI that doesn't look like a template                       |
| `generate-commit`                | Reading staged changes and suggesting a commit message              |
| `grill-with-docs`                | Stress-testing plans against the project's own docs and terminology |
| `handoff`                        | Compressing a conversation so another agent can pick it up          |
| `karpathy-guidelines`            | Not overcomplicating things, making surgical changes, testing first |
| `react-best-practices`           | React perf tips from Vercel's engineering team                      |
| `receiving-code-review`          | Handling feedback without blindly accepting bad suggestions         |
| `requesting-code-review`         | Double-checking work before committing                              |
| `subagent-driven-development`    | Using subagents to parallelize implementation                       |
| `systematic-debugging`           | Actually figuring out what's broken before fixing it                |
| `test-driven-development`        | Tests first, then code                                              |
| `using-git-worktrees`            | Isolating work without switching branches                           |
| `using-superpowers`              | Finding and loading the right skill for the job                     |
| `vercel-composition-patterns`    | Building composable React components                                |
| `verification-before-completion` | Running checks before declaring something done                      |
| `writing-plans`                  | Planning multi-step work before touching code                       |
| `writing-skills`                 | Creating and editing skills                                         |

## Extensions

- **`subagent/`** — The big one. Lets pi run multiple agents in parallel, chain them together, fan out work, and isolate changes in worktrees so nothing steps on itself.

---

## Context-Mode

The knowledge base pi uses to remember things. It indexes docs, code, and session data so pi can search it later. Key features:

- **Persistent search** — Indexed content sticks around across sessions (FTS5 under the hood)
- **Sandbox execution** — Run JS, Python, Shell, Rust etc. without dumping raw output into the conversation
- **Batch processing** — Fire off a bunch of commands, auto-index the results, ask questions about them
- **Session memory** — Auto-captures decisions, errors, plans, blockers so pi doesn't forget what happened

---

## The Ground Rules (from AGENTS.md)

The one-pager that keeps pi from going off the rails:

1. **Think first** — Don't guess, state assumptions, ask if unsure
2. **Keep it simple** — Minimum code that works, nothing extra
3. **Surgical changes** — Touch only what needs changing, don't refactor nearby stuff for fun
4. **Prove it works** — Define how you'll verify, loop until confirmed

---

## Quick Start

If I were setting this up fresh:

```bash
npm install -g @earendil-works/pi-coding-agent
```

Then tweak `agent/settings.json` to pick a model, theme, and install whatever packages are needed.
