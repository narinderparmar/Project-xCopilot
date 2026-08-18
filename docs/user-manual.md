# xCopilot User Manual

> **Pre-release manual:** this document defines the user-facing manual structure. Sections become supported only when a signed release and its release notes identify them as available.

## Product Surfaces

xCopilot provides a CLI and an authenticated Web interface hosted only by the local Engine. The Engine is the policy authority for model access, project permissions, approvals, resource limits, and audit records.

## Current Source-Preview CLI

| Command | Behavior |
| --- | --- |
| `xcopilot` | Opens a durable conversation for the current repository. |
| `xcopilot ask "<prompt>"` | Creates one durable session and streams one local-model response. |
| `xcopilot serve` | Starts the loopback-only Engine and locally hosted Web app. |
| `/model` | Shows configured deployments; `/model <id>` selects one for later turns. |
| `/clear` | Starts a fresh conversation without deleting prior durable history. |
| `/help` | Shows interactive commands. |
| `/exit` | Closes the CLI. |
| `Ctrl+C` | Cancels the exact active generation; a second interrupt aborts the client request. |

The CLI never calls a model runtime directly. It authenticates to the local Engine, uses bounded newline-delimited streaming, and records the user message, selected model, final assistant message, terminal status, and available token/timing metrics. If no approved deployment is configured, the REPL stays open and reports that state; it does not fall back to a paid or unknown model.

## Manual Contents

1. **Install and update** — verified release download, signature/checksum validation, supported platforms, update, repair, and uninstall.
2. **First run** — local Engine startup, browser bootstrap, project selection, privacy defaults, and optional runtime/model setup.
3. **Models and runtimes** — managed llama.cpp, Ollama and other profiled local runtimes, free hosted-model eligibility, model health, and fallback controls.
4. **Can I run it locally?** — hardware inventory, workload selection, fit assessment, expected response-time range, measured benchmark, and recommendation evidence.
5. **CLI and Web workflows** — chat, plans, file changes, diff review, tests, approvals, cancellation, undo, and session history.
6. **Capacity dashboard** — active and queued workloads, RAM/VRAM use, model capacity, hosted quota consumption, reservations, and time to reset/recovery.
7. **Tools and integrations** — built-in tools, MCP servers, repository worktrees, and later ACP/plugin support.
8. **Security and privacy** — loopback-only access, credentials, project boundaries, logs, telemetry defaults, memory controls, and data deletion.
9. **Troubleshooting** — diagnostics bundle, runtime connectivity, model failures, resource pressure, recovery, and support information.

## Safety Defaults

- Paid or metered inference is rejected unless a future policy explicitly changes the product scope.
- Local execution is preferred and hosted free-model use requires current eligibility evidence.
- File writes, commands, network access, and sensitive operations are subject to policy and approval.
- End-user credentials must not be placed in command-line arguments or diagnostic bundles.
- Release binaries are obtained only from this project's versioned GitHub Releases.

Feature-specific procedures, command references, screenshots, accessibility guidance, and troubleshooting codes will be added alongside the implementation and validated before each release.
