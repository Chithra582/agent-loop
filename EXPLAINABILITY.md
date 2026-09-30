# EXPLAINABILITY — Pi Agent Harness

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* Pi Agent Harness (`pi-agent-harness`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Autonomous Coding Agent Harness & Tool Calling Runtime  

---

## 1. Overview & Operational Purpose

The **Pi Agent Harness** (`pi-agent-harness`) is an autonomous software engineering companion, terminal coding agent, and extensible tool runtime built upon `@earendil-works/pi-coding-agent`, `@earendil-works/pi-agent-core`, `@earendil-works/pi-ai`, and `@earendil-works/pi-durable`. Designed to act as an intelligent pair programmer for high-velocity software engineering tasks, the system pairs an interactive terminal differential rendering user interface with durable execution state management, pluggable tool dispatching, and multi-provider LLM routing.

The agent's primary operational purpose is to inspect codebases, synthesize accurate source code patches, format and display interactive diffs, execute build/test commands within sandboxed environments, and journal all session states for full crash recovery. Every operation is deterministic, auditable, and subject to human oversight.

---

## 2. How the Agent Decides (Decision-Making Logic)

Pi Agent Harness operates across a deterministic, multi-stage software engineering decision pipeline that enforces safety, sandboxing, and diff verification at every step:

```
[User Natural Language Prompt] ──> [Intent & Tool Schema Resolver] ──> [Diff Pre-computation Gate]
                                                                                      │
                                                                                      ▼
[Durable State Journal & Checkpoint] <── [Sandbox Isolation (VM/Docker)] <── [Multi-Provider LLM Invocation]
```

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
|---|---|---|---|
| **Terminal User Prompts & Instructions** | Interactive TUI / CLI STDIN | Instructs agent on tasks and development goals | Local memory buffer, never exfiltrated |
| **Local Workspace Filesystem** | Project repository & disk workspace | Reads source files and generates unified diffs | Processed in-place with diff preview |
| **LLM API Responses & Streaming Tokens** | Multi-provider AI endpoints (Anthropic, OpenAI, Google) | Core model reasoning and tool calling | HTTPS TLS encrypted, adheres to API privacy |
| **Durable State Journal & Checkpoints** | Local JSON/SQLite session storage | Preserves session context across restarts | Stored on local disk, never broadcast |

Pi Agent Harness complies with operational security and privacy standards:
- **No Cloud Code Exfiltration:** Project source code and terminal outputs remain strictly on the local machine and are never retained by third-party training pipelines.
- **Strict Diff Preview:** All code modifications are displayed as unified diffs before being written to disk, ensuring developer consent.
- **Right to Terminate:** The developer can abort running commands, pause agent loops, or roll back uncommitted changes at any moment.
- **Deterministic Sandboxing:** Host filesystem and environment credentials are fully isolated when running untrusted bash commands inside Gondolin or Docker.

---

## 4. Known Limitations & Failure Modes

Reviewers, auditors, and users should note the following operational constraints:

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

- **Real-Time Human Approval Gate:** The differential TUI provides users real-time approval control over tool invocations, shell executions, and file edits.
- **Emergency Session Interrupt:** Pressing `Ctrl+C` or sending an interrupt signal immediately terminates executing sub-processes and halts autonomous loops.
- **Step Quota Guardrails:** Autonomous loops are capped at a maximum turn limit (`max_turns: 25`) to prevent runaway API billing or recursion.
- **Structured Audit Logging:** Every executed tool action, unified diff patch, and state checkpoint is journaled into local structured logs for full post-mortem review.
