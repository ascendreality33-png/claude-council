# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Project Is

Claude Council is a multi-agent framework where multiple Claude agents with distinct roles collaborate to evaluate ideas, plans, and proposals from multiple perspectives simultaneously.

## Agent Architecture

Custom agents live in `.claude/agents/`. Each agent is a Markdown file with YAML frontmatter defining its `name`, `description`, `model`, `color`, and `memory` scope.

Each agent has its own persistent memory system at `.claude/agent-memory/<agent-name>/`, which follows the same file-based memory format (individual `.md` files per memory + a `MEMORY.md` index). Memory is project-scoped and versioned alongside the agents.

## Current Council Members

- **optimist-maximalist** (`green`, `sonnet`) — Maps the highest realistic potential of an idea. Structured output: core restatement → best-case trajectory → preconditions → critical path → hidden potential → honest caveats.
- **devils-advocate** (`red`, `sonnet`) — Stress-tests ideas with grounded skepticism. Structured output: core assumption → where it breaks → what's underestimated → conditions required → resilient reframe.

## Adding New Agents

Create a new `.md` file in `.claude/agents/` with the required frontmatter and system prompt. If the agent should have persistent memory, reference its memory path (`.claude/agent-memory/<name>/`) in the system prompt and initialize a `MEMORY.md` there.

## Council Session Pattern

The orchestrating Claude instance (main conversation) decides which council member(s) to invoke via the `Agent` tool based on the nature of the input. Agents can be run in parallel for balanced multi-perspective analysis or sequentially when one perspective should inform the next.
