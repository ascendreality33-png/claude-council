# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Project Is

Claude Council is a multi-agent framework where multiple Claude agents with distinct roles collaborate to evaluate ideas, plans, and proposals from multiple perspectives simultaneously.

## Agent Architecture

Custom agents live in `.claude/agents/`. Each agent is a Markdown file with YAML frontmatter defining its `name`, `description`, `model`, `color`, and `memory` scope.

Each agent has its own persistent memory system at `.claude/agent-memory/<agent-name>/`, which follows the same file-based memory format (individual `.md` files per memory + a `MEMORY.md` index). Memory is project-scoped and versioned alongside the agents.

## Current Council Members

- **optimist-maximalist** (`green`, `sonnet`) — Maps the highest realistic potential of an idea.
- **devils-advocate** (`red`, `sonnet`) — Stress-tests ideas with grounded skepticism.

New agents added to `.claude/agents/` automatically join future council gatherings — no manual updates required.

## Adding New Agents

Create a new `.md` file in `.claude/agents/` with the required frontmatter and system prompt. If the agent should have persistent memory, reference its memory path (`.claude/agent-memory/<name>/`) in the system prompt and initialize a `MEMORY.md` there.

---

## Council Deliberation Protocol

### Trigger

When the user's message begins with **"agents gather"** followed by an idea or proposal, execute the full council deliberation protocol below. Do not abbreviate or skip steps.

### Orchestration Steps

**You (the main Claude instance) are the orchestrator.** Subagents cannot spawn subagents, so you must handle all coordination directly.

1. **Discover agents** — Use the Glob tool to list all `.md` files in `.claude/agents/`. Each file is a council member. Read any agent file you haven't seen this session to understand their role.

2. **Launch all agents in parallel** — Use the Agent tool to invoke every discovered agent simultaneously. Pass the following as the prompt to each:

   ```
   COUNCIL SESSION: [paste the user's idea/proposal verbatim]

   Structure your response exactly as follows:
   **Initial Reaction:** [1–2 sentence gut response]
   **Key Considerations:** [bulleted analysis, as many as warranted]
   **Critical Insights:** [the non-obvious things — what others will likely miss]
   **Confidence:** [X/10] — [one sentence justifying this level]
   ```

3. **Collect all responses** — Wait for every agent to complete before proceeding.

4. **Write to shared_reasoning.md** — Append a new session block to `shared_reasoning.md` using the format defined below. Create the file if it does not exist.

5. **Synthesize** — After writing the log, provide a synthesis in the main conversation: areas of agreement, key tensions between agents, and the 1–2 questions the council has surfaced that most need to be resolved before acting.

### shared_reasoning.md Format

Each session is appended as a new block. Never overwrite existing sessions.

```markdown
---

## Session: YYYY-MM-DD HH:MM
### Proposal: [idea verbatim]

### [Agent Name]
**Initial Reaction:** ...
**Key Considerations:**
- ...
**Critical Insights:** ...
**Confidence:** X/10 — ...

### [Agent Name]
...

### Synthesis
[Key agreements, tensions, and open questions]

---
```

The file header (written once, on first creation):

```markdown
# Council Deliberation Log

This file is the persistent record of all council sessions in this project.
Each session is appended below in chronological order.
```
