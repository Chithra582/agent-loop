# Agent Explainability & Transparency Report

- **Agent Name:** pi-agent-harness
- **OpenGAP Specification:** 0.1.0
- **Agent ID:** pi-agent-harness-agent
- **Domain:** Developer Tools / Autonomous Coding Agent Harness & Tool Calling Runtime
- **Passport Validation Tier:** Tier-1 Certified Autonomous Agent

---

## 1. Overview & Architectural Purpose

The **Pi Agent Harness** (`pi-agent-harness`) is a modular, extensible autonomous coding agent runtime designed for high-velocity software engineering tasks. Built on a monorepo architecture encompassing `@earendil-works/pi-coding-agent`, `@earendil-works/pi-agent-core`, `@earendil-works/pi-ai`, and `@earendil-works/pi-durable`, the agent pairs an interactive terminal differential rendering user interface with durable execution state management, pluggable tool dispatching, and multi-provider LLM routing.

The harness acts as a dependable pair programmer: reading workspace files, computing precise edits, executing test suites in sandboxes, and maintaining transactional logs. Every operation is deterministic, auditable, and subject to human oversight.

---

## 2. How the Agent Decides (Decision-Making Logic)

### 2.1 Autonomous Tool Execution & Interactive Cycle
- **Decision:** Determines whether to read files, execute shell commands, patch codebase files, or query the developer for clarification.
- **Rules:**
  - Evaluates developer intent from the interactive prompt and maps target outcomes to registered tool schemas.
  - Prioritizes read-only inspection (file viewing, directory listing) before attempting write operations or commands.
  - Requires pre-computation and presentation of unified diffs prior to executing file modifications.

### 2.2 Provider Selection & Model Invocation
- **Decision:** Selects optimal LLM backends (Anthropic Claude, OpenAI GPT, Google Gemini, or local models) and parameter configurations.
- **Rules:**
  - Selects models matching task complexity (reasoning-intensive refactoring vs. lightweight formatting).
  - Automatically triggers fallback failover routing when primary endpoints experience rate limits or network degradation.
  - Normalizes multi-provider message schemas into uniform internal representation to ensure consistent tool calling.

### 2.3 Durable State Journaling & Checkpointing
- **Decision:** Determines when to create atomic transaction checkpoints for active conversation turns and pending tasks.
- **Rules:**
  - Writes state records after each tool invocation result and user message ingestion.
  - Preserves full rollback markers enabling session resumption across terminal restarts or system interrupts.
  - Restricts journal persistence to local encrypted storage without telemetry exfiltration.

### 2.4 Sandbox Isolation & Boundary Protection
- **Decision:** Decides execution isolation levels (direct host, Gondolin micro-VM, Docker container, or OpenShell sandbox) for bash commands.
- **Rules:**
  - Classifies shell commands based on potential risk profile (destructive filesystem operations, network requests, unknown scripts).
  - Mandates micro-VM or container encapsulation whenever sandboxing configuration is enabled.
  - Rejects commands attempting to escalate host privileges or read host environment credentials.

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
| :--- | :--- | :--- | :--- |
| Terminal User Prompts & Instructions | Interactive TUI / CLI STDIN | Instructs agent on tasks and development goals | Local memory buffer, never exfiltrated |
| Local Workspace Filesystem | Project repository & disk workspace | Reads source files and generates unified diffs | Processed in-place with diff preview |
| LLM API Responses & Streaming Tokens | Multi-provider AI endpoints (Anthropic, OpenAI, Google) | Core model reasoning and tool calling | HTTPS TLS encrypted, adheres to API privacy |
| Durable State Journal & Checkpoints | Local JSON/SQLite session storage | Preserves session context across restarts | Stored on local disk, never broadcast |

---

## 4. Known Limitations & Failure Modes

1. **Destructive Command Execution Risks:**
   - *Limitation:* Autonomous execution of terminal commands may inadvertently delete or overwrite files.
   - *Mitigation:* Sandbox boundary isolation, user confirmation prompts for dangerous operations, and diff pre-checks.

2. **Context Window Saturation:**
   - *Limitation:* Large file trees and extensive debugging logs may exhaust LLM context limits.
   - *Mitigation:* Dynamic prompt truncation, compaction algorithms, and sliding window memory management.

3. **Model Hallucination & Syntax Errors:**
   - *Limitation:* Language models may suggest nonexistent package APIs or invalid syntax.
   - *Mitigation:* Vitest/TypeScript compiler test runs, iterative lint passes, and interactive feedback loops.

4. **API Rate Limiting & Provider Outages:**
   - *Limitation:* Upstream provider throttling or token quota exhaustion halts autonomous loops.
   - *Mitigation:* Provider fallback routing, exponential backoff retries, and local LLM runtime switching.

---

## 5. Verification, Safety & Human Oversight

- **Human-in-the-Loop Interactivity:** The differential TUI provides users real-time approval control over tool invocations, shell executions, and file edits.
- **Durable Rollback:** All operations are tracked in a transaction log, enabling rollback of errant file edits and recovery to previous checkpoints.
- **Strict Sandboxing:** Micro-VM (Gondolin), Docker, and OpenShell isolation prevent unauthorized network access or host system compromise.
- **Credential Isolation:** API keys and environment secrets are kept strictly out of chat context and LLM prompt tokens.
