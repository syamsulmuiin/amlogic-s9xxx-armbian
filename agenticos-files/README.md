# Agenticos Hybrid

Agenticos is an experimental ARM64 agent runtime for the HG680P (S905X, 2 GB RAM), integrated as an overlay into an Armbian image.

## Architecture

- **Local brain:** `llama-server` with a bundled GGUF model.
- **Optional remote brain:** an explicitly configured OpenAI-compatible HTTPS endpoint. Cloud credentials must not be committed.
- **Agent runtime:** bounded tool loop, restricted workspace and explicit approval for file writes.
- **First boot:** validate preinstalled binary/model; do not compile or download at boot.

## Readiness criteria

1. Build workflow produces an image containing the executable `llama-server` and non-empty GGUF model.
2. Image is validated for ARM64 compatibility and correct systemd service paths.
3. Agent passes unit tests for tool restrictions, workspace traversal and approval rejection.
4. Boot and inference are tested on the actual HG680P before claiming production readiness.

## Security

Use the local model by default. Never enable unattended writes without an explicit authorization design. Avoid running the agent as root, and keep secrets outside Git.

## Status

Foundation work in progress. Image build and physical-device validation are not yet complete.
