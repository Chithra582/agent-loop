---
name: "multi-provider-llm-router"
description: "Routes LLM inference across OpenAI, Anthropic, Google, and local endpoints with fallback failover."
---

# Multi-Provider LLM Router

## Overview
Provides a unified abstraction layer across diverse AI model providers:
- Supports Anthropic Claude, OpenAI GPT, Google Gemini, and Ollama/vLLM endpoints.
- Normalizes streaming tokens, tool calling schemas, and error structures.
- Implements transparent failover when quotas are reached or endpoints experience downtime.

## Capabilities
- Dynamic provider failover.
- Context window monitoring and automatic prompt compaction.
- Structured tool call serialization across all supported backends.
