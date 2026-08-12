# AskMe

**Clarify first. Preview the outcome. Get approval. Then build.**

AskMe is a lightweight instruction file for AI coding agents. It prevents an agent from jumping directly into implementation before it understands what you actually want.

When AskMe is active, the agent must:

1. Review the conversation, project, existing code, and previous decisions.
2. Ask only the clarification questions that could materially affect the result.
3. Present a concise implementation approach.
4. Show a small example or preview of the expected outcome.
5. Wait for your approval before changing code, installing packages, deploying, or performing other write actions.

The reusable instruction is available in [`askme.md`](./askme.md).

## Why AskMe is necessary

AI coding agents are fast, but speed can amplify misunderstanding. A vague request can lead to:

- The wrong feature being built correctly
- Unwanted architecture or dependencies
- Existing conventions being ignored
- Large changes when a small change was expected
- Rework after the result does not match the user's expectations
- Files, configuration, or workflows being changed too early

AskMe adds a short **alignment checkpoint** before implementation. It keeps the human responsible for intent and approval while allowing the agent to handle execution.

It is especially useful for:

- New features
- UI and UX work
- Refactoring
- Architecture decisions
- API and database changes
- Automations and integrations
- Configuration and infrastructure
- Tasks with unclear or incomplete requirements

## How it works

```text
Your request
    ↓
Agent reviews existing context
    ↓
Agent asks important questions
    ↓
Agent shows its approach and an outcome preview
    ↓
You approve or request changes
    ↓
Agent implements and validates the approved scope
```

AskMe permits safe, read-only inspection before approval. This lets the agent examine relevant files, documentation, tests, errors, screenshots, and project structure without modifying anything.

## Quick start

### Option 1: Use it with any coding agent

1. Download or copy [`askme.md`](./askme.md) into your project.
2. Attach or reference it in your coding-agent conversation.
3. Start your request with:

```text
Read askme.md and follow it before implementing this task:

[Describe your task here]
```

If your tool supports file mentions, you can use a command such as:

```text
Read @askme.md and follow it. Add a customer analytics dashboard.
```

This is the simplest and most portable method.

### Option 2: Make it a reusable skill

If your coding tool supports skills, create a skill folder and copy `askme.md` into it as `SKILL.md`.

Example structure:

```text
ask-me-before-execution/
└── SKILL.md
```

Then enable or mention the skill when starting a coding task. The exact skill directory depends on the tool, so follow that tool's current skill-installation documentation.

### Option 3: Add it to project instructions

For persistent behavior, copy the instructions into the project-level rules file used by your coding tool, or tell that file to load `askme.md`.

Common examples include:

| Tool | Common project instruction location |
| --- | --- |
| Claude Code | `CLAUDE.md` or a supported skill folder |
| Codex | `AGENTS.md` or an installed skill |
| Cursor | Project Rules or `.cursor/rules/` |
| Windsurf | Workspace rules |
| Other agents | Their custom-instructions or project-rules file |

Tool behavior and file locations can change. Check your tool's documentation if it does not automatically load the file.

## Example

### User request

```text
Read @askme.md and follow it.

Add social login to my application.
```

### Expected agent response before coding

```markdown
### My understanding

- Add social authentication to the existing login flow.
- Reuse the project's current authentication system and UI patterns.
- Keep email/password login available.

### Questions

1. Which providers should be supported: Google only, or Google and GitHub?
2. Should new social users be created automatically after successful authentication?

### Proposed approach

- Extend the existing authentication service rather than replacing it.
- Add provider buttons to the current login screen.
- Add callback handling and tests for success, cancellation, and failure.
- Leave existing email/password behavior unchanged.

### Outcome preview

Preview:

[ Continue with Google ]
[ Continue with GitHub ]

Successful login → existing user session → current dashboard

### Approval

Reply **Approve** to proceed, or tell me what you want changed.
```

The agent begins implementation only after the user approves the direction.

## Required response format

Before execution, AskMe tells the agent to provide five concise sections:

1. **My understanding** — the intended outcome and important assumptions
2. **Questions** — only decisions that could materially change the result
3. **Proposed approach** — scope, affected areas, and technical direction
4. **Outcome preview** — a small UI, API, workflow, input/output, or code example
5. **Approval** — a clear request to proceed or revise the plan

If no important information is missing, the agent should say:

```text
No critical clarification needed.
```

It must still show the proposed approach and a short outcome preview before implementation.

## Design principles

### Context before questions

The agent should inspect available conversation history, project memory, code, documentation, tests, and previous decisions first. It must not ask the user to repeat information that is already available.

### Important questions only

AskMe is not intended to create a long questionnaire for every task. The agent asks only questions whose answers would meaningfully affect functionality, architecture, security, cost, data, or user experience.

### Preview before implementation

A preview makes an abstract plan concrete. Depending on the task, it may be:

- A compact UI layout
- Sample input and output
- An API request and response
- A proposed interface or function signature
- A short before-and-after example
- A workflow outline
- A representative database record or generated message

### Approval as a hard checkpoint

Until approval is received, the agent must not edit files, install packages, deploy, or make external changes. After approval, it should execute only the agreed scope and validate the result.

## When the checkpoint can be skipped

The approval checkpoint is unnecessary when the user requests only:

- An explanation
- A summary
- A code or design review
- A diagnosis
- A plan with no implementation

For a trivial and fully specified edit, the agent still provides a very short preview unless the user explicitly asks it to skip the checkpoint.

## What AskMe does not do

AskMe is an instruction layer, not a security sandbox or permission system. Its effectiveness depends on the coding agent loading and following the file.

It does not:

- Replace version control, backups, tests, or code review
- Guarantee that an AI-generated plan is correct
- Grant an agent access to files, tools, accounts, or services
- Override higher-priority safety or platform instructions
- Replace human judgment for security-critical or high-risk changes

Use normal engineering safeguards alongside AskMe.

## Contributing

Suggestions and improvements are welcome. If you find a case where the agent asks too many questions, skips an important decision, or provides an unhelpful preview, open an issue or submit a pull request with a practical example.

## Author

Created by [Sarfaraz Ahmed](https://github.com/Sarfaraz021).

If AskMe improves your AI-assisted development workflow, consider starring the repository and sharing it with other developers.
