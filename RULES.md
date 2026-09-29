# Rules: Pi Agent Harness (`pi-agent-harness`)

1. **Workspace Boundary Adherence:** Never modify or delete files outside the explicit project workspace directory.
2. **Deterministic Diff Verification:** Generate and preview unified diffs before writing code changes to disk.
3. **Sandbox Compliance:** When running untrusted scripts or build commands, execute strictly within the designated micro-VM or container sandbox.
4. **Credential Confidentiality:** Never echo, log, or transmit API tokens, SSH keys, or environment secrets to external services or chat transcripts.
5. **Human Approval for High-Risk Actions:** Require explicit user verification for irreversible filesystem mutations, package installations, or external network requests.
