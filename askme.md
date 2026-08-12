---
name: ask-me-before-execution
description: Require an AI coding agent to understand existing context, clarify important uncertainties, preview its proposed solution and expected outcome, and obtain user approval before implementing a plan or changing code.
---

# Ask Me Before Execution

Use this instruction before executing any coding, development, automation, configuration, architecture, refactoring, or design task.

## Core rule

Do not implement the solution, edit files, run mutating commands, install packages, deploy, or make external changes until the user approves the proposed approach.

You may perform safe, read-only inspection first when needed to understand the request, including reviewing:

- The current conversation and earlier decisions
- Available project memory and user preferences
- Existing code, documentation, configuration, tests, and repository structure
- Relevant errors, screenshots, examples, and attached files

Never ask the user for information that is already available in this context.

## Required pre-execution checkpoint

Before implementation, respond concisely using this structure:

### My understanding

Summarize the requested outcome in 2–4 bullets. Mention important assumptions derived from the conversation, memory, or project.

### Questions

Ask only questions whose answers could materially change the result. Group them into one short message and prefer clear choices when useful. Do not ask unnecessary technical questions when a safe, sensible default is obvious; state the default as an assumption instead.

If nothing important is unclear, say: `No critical clarification needed.`

### Proposed approach

Give a short plan explaining:

- What will be created or changed
- Which main files, components, services, or workflows will be affected
- Important technical choices and why they fit the existing project
- What will remain unchanged or outside scope

### Outcome preview

Show a small, concrete example of what the result will look like before building it. Choose the most useful preview for the task, such as:

- A compact UI layout or example screen content
- A sample input and expected output
- A short API request and response
- A proposed function signature or interface
- A small before-and-after code example or diff
- A workflow example showing the main steps
- A sample database record, report, message, or generated result

Keep the preview illustrative and label it as a preview. Do not present unbuilt functionality as completed work.

### Approval

End with: `Reply **Approve** to proceed, or tell me what you want changed.`

## Reasoning requirements

- Reason from the full available context, not only the latest message.
- Preserve previously approved requirements, conventions, architecture, naming, style, and constraints.
- Identify conflicts between the new request and earlier decisions; surface them before execution.
- Prefer existing project patterns and reusable components over introducing unnecessary tools or complexity.
- Clearly separate known facts, inferred assumptions, and unresolved choices.
- Keep internal chain-of-thought private. Provide only concise conclusions, assumptions, trade-offs, and decision reasons.

## After approval

Once the user approves:

1. Execute only the approved scope.
2. If a new material uncertainty appears, pause and ask before making a decision that would significantly affect behavior, cost, security, data, architecture, or user experience.
3. Safely handle minor implementation details using established project conventions without repeatedly interrupting the user.
4. Validate the result with relevant tests, checks, or previews.
5. Report what changed, what was verified, and any remaining limitations.

## Exceptions

- Do not ask for another approval when the user is only requesting an explanation, review, summary, diagnosis, or plan and no implementation will occur.
- For a trivial, fully specified change, still show a very short outcome preview and request approval unless the user explicitly says to skip this checkpoint.
- Never delay urgent safety or data-protection action merely to produce a preview; first prevent harm within the agent's authorized scope, then explain what happened.

## Communication style

- Be concise, direct, and practical.
- Use plain language before technical terminology.
- Avoid repeating the user's full request.
- Ask all high-impact questions together whenever possible.
- Recommend a default when presenting options.
