# Agenticos Hybrid — multi-device agent platform

Agenticos is a modular, hardware-adaptive agent platform. The first image target is the HG680P (S905X, 2 GB RAM), but the agent runtime is designed for ARM64 boards, Linux PCs and servers.

## Architecture and capabilities

The following capabilities are **planned targets**, not claims of completed implementation:

- **Core:** planner, executor, evaluator, persistent tasks, bounded loops and audit trail.
- **Brains:** local llama.cpp inference, HTTPS cloud providers, explicit routing and fallback policy.
- **Knowledge:** persistent SQLite memory, FTS5 session search, reflection and memory curation.
- **Skills:** versioned skill registry, skill discovery, test-before-promotion, rollback and safe skill improvement.
- **Tools:** filesystem, terminal, HTTP, MCP and device-specific adapters with least privilege and explicit approvals.
- **Automation:** scheduler, notifications, resumable jobs and rate/resource budgets.
- **Delegation:** optional subagents, concurrency limits and isolated execution.
- **Interfaces:** CLI, API, dashboard and optional messaging gateways.
- **Operations:** structured logs, health checks, update validation, recovery and rollback.

These concepts are informed by [Hermes Agent](https://github.com/NousResearch/hermes-agent) (MIT). Do not copy upstream code without preserving license and attribution.

## Hardware profiles

| Profile | Intended devices | Default runtime |
| --- | --- | --- |
| lite | HG680P and other 2 GB ARM64 devices | Single agent, compact local model, SQLite, remote delegation |
| standard | ARM64 SBCs with more RAM | Local/cloud routing, scheduler, more skills |
| full | PCs and servers | Parallel subagents, larger models, full gateway |

Hardware profiles are **resource budgets**, not feature forks. Skills and memory formats should remain compatible across profiles. No autonomous escalation of privileges is permitted.

## Image build requirements

1. Build the binary and download the model **during image creation**, not first boot.
2. Verify the model's GGUF magic and the executable's target architecture.
3. Stage the overlay in the image root filesystem and validate systemd paths.
4. Produce reproducible build artifacts and a manifest of versions and hashes.
5. Test agent tools, approval denial, workspace traversal and recovery.
6. Boot-test each supported device image before calling it ready.

## Security and operational rules

- Treat prompts, model output, retrieved content and skills as untrusted.
- Require explicit authorization for writes, network side effects and privileged actions.
- Separate agent code, skills, workspace data, memory and secrets.
- Do not commit API keys or permit unattended privilege escalation.
- New or self-modified skills must pass tests and approval before activation.
- Keep backups and rollback points for updates.

## Current status

The lightweight Python agent, local llama-server configuration, firstboot validation and an Armbian build workflow exist. The full Hermes-inspired capabilities, build success, model inference and hardware boot are **not yet verified**. Do not merge the development PR or flash the image as a finished Agenticos release.
