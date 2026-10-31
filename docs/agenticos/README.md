# Agenticos for HG680P

Agenticos bundles an ARM64 Cortex-A53 llama-server binary and Qwen2.5-1.5B-Instruct Q4_K_M model in an Armbian image for board `s905x`.

## Build

Run `.github/workflows/build-agenticos.yml` on branch `agenticos`. Defaults: Debian Trixie minimal (official Odroid N2 source image), `kernel_usage=stable`, `armbian_kernel=6.12.y_6.18.y`, `auto_kernel=true`, `armbian_fstype=ext4`, `armbian_size=512/4096` MiB, `builder_name=Agenticos`, and `armbian_files=agenticos-files`. If an official Trixie image is unavailable, provide an official Noble-or-newer image through `armbian_url`. Older base releases are rejected.

The workflow is copied from the existing official-image workflow. It runs the checked-out fork's local composite action. All original repository files remain unchanged. Binary and model downloads exist only on the runner and inside release images. No new secrets are required; release upload uses the ephemeral built-in GITHUB_TOKEN.

The Ubuntu 24.04 runner uses the repository's matching dependency list and cross toolchain. CPU flags remain `-mcpu=cortex-a53` and `GGML_NATIVE=OFF`. The workflow checks the model URL before compilation, repairs executable permissions and systemd links, and rejects release images at or above 1.9 billion bytes. If this occurs, change only the overlay model URL to the user-provided Qwen2.5-0.5B-Instruct Q4_K_M URL, verify HTTP 200, and rebuild. Maximum three repair attempts per error category.

## First boot

No compilation, package installation, or model download runs on the device. First boot checks bundled components, creates the workspace and log directories, and starts llama-server on 127.0.0.1:8080. Run `agenticos "cek status sistem"`. Agent actions are logged to `/var/log/agenticos/actions.jsonl`.

The original overlay workflow snippet is retained in `workflow-steps.yml` for provenance; the executable build workflow is authoritative. Documentation is outside the system overlay.

## Validation limits

A successful Actions build does not prove physical HG680P boot, eMMC installation, available RAM at context 2048, or inference speed. Confirm these on the device before considering the image validated for hardware use. Actual kernel patch versions and compressed image sizes are recorded by each build, not inferred from the selected series.
