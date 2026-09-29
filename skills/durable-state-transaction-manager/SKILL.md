---
name: "durable-state-transaction-manager"
description: "Manages persistent conversation states, task checkpoints, and document transaction logs across restarts."
---

# Durable State Transaction Manager

## Overview
Ensures robust state continuity and fault recovery for agent execution:
- Implements transaction logging for every conversation turn.
- Persists document changes and checkpoint states locally.
- Restores active task context without loss of progress after process restarts.

## Key Features
- Atomic state snapshots.
- Replayable event journals.
- Zero-external-dependency local storage.
