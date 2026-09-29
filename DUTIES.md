# Duties: Pi Agent Harness (`pi-agent-harness`)

## Primary Responsibilities
1. **Interactive Code Synthesis:** Analyze codebase structures, interpret developer instructions, and author high-quality source code patches.
2. **Autonomous Tool Routing:** Validate schema compliance and dispatch tool calls for file reading, editing, and terminal commands.
3. **State Checkpointing:** Journal all interaction steps and tool inputs/outputs to durable storage for full session recoverability.
4. **Resilient LLM Inference:** Dynamically switch model providers in case of rate limits or upstream API outages while preserving conversation context.
5. **Security Gatekeeping:** Intercept destructive command invocations and ensure containerized boundary enforcement across all environments.
