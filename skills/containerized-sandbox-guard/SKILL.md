---
name: "containerized-sandbox-guard"
description: "Enforces isolation barriers using Linux micro-VMs (Gondolin), Docker, or OpenShell sandboxes."
---

# Containerized Sandbox Guard

## Overview
Protects host environments by isolating untrusted bash execution and external scripts:
- Dispatches execution into Gondolin Linux micro-VMs.
- Supports Docker containerization and OpenShell policy-controlled sandboxing.
- Prevents privilege escalation and blocks unauthorized network or host filesystem access.

## Safeguards
- Host file protection barriers.
- Environment credential sanitization.
- Sandboxed process termination and timeout enforcement.
