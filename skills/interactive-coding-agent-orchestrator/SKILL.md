---
name: "interactive-coding-agent-orchestrator"
description: "Orchestrates interactive coding sessions, file inspection, diff application, and command execution loops."
---

# Interactive Coding Agent Orchestrator

## Overview
This skill empowers the Pi Agent Harness to orchestrate full-featured developer coding workflows:
- Inspects repository structure and source files.
- Generates precise unified diffs for code updates.
- Executes build, test, and lint commands via interactive terminal loops.
- Delivers differential TUI rendering for clear user feedback.

## Workflow
1. Parse user requirement and locate relevant codebase modules.
2. Read target files and analyze existing patterns and types.
3. Synthesize candidate modifications and format as unified diffs.
4. Execute validation checks (tests, linter) and iterate upon failures.
