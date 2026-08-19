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
| `xcopilot models list` | Lists exact cataloged model previews, transfer state, installation state, and active selection. |
| `xcopilot models refresh [--id <model-id>]` | Rechecks one or all pinned previews and invalidates stale eligibility or selection. |
| `xcopilot models pull ...` | Previews an exact Hugging Face GGUF; `--approve` explicitly starts the verified pull. |
| `xcopilot models set --id <model-id>` | Selects one currently valid installed artifact without starting a runtime. |
| `/model` | Shows configured deployments; `/model <id>` selects one for later turns. |
| `/file <path>[#Lx-Ly]` | Reads one permitted repository text file as bounded untrusted context. |
| `/propose <path> => <text>` | Creates a durable one-file diff without writing the worktree. |
| `/diff [proposal-id]` | Displays the latest or exact durable proposal and state. |
| `/approve [proposal-id]` | Revalidates and applies the exact proposal after an explicit decision. |
| `/reject [proposal-id]` | Durably rejects the proposal without changing the file. |
| `/undo [proposal-id]` | Restores exact prior bytes when the applied file has not changed. |
| `/clear` | Starts a fresh conversation without deleting prior durable history. |
| `/help` | Shows interactive commands. |
| `/exit` | Closes the CLI. |
| `Ctrl+C` | Cancels the exact active generation; a second interrupt aborts the client request. |

The CLI never calls a model runtime directly. It authenticates to the local Engine, uses bounded newline-delimited streaming, and records the user message, selected model, final assistant message, terminal status, and available token/timing metrics. If no approved deployment is configured, the REPL stays open and reports that state; it does not fall back to a paid or unknown model.

## Current Source-Preview Web Chat

The locally hosted Web client opens only through the Engine's one-use browser bootstrap URL. It does not connect to a model runtime directly or load third-party Web assets.

| Control | Behavior |
| --- | --- |
| **Create durable session** | Creates a repository chat from an explicit local path and optional title. |
| **Sessions** | Selects an existing durable conversation shared with the CLI/API. |
| **Replay history** | Reloads ordered durable events without duplicating messages. |
| **Prepared local model** | Selects only a deployment already approved and exposed by the Engine. |
| **Send one prompt** | Streams one bounded response and reports terminal token/timing metrics when available. |
| **Stop active response** | Cancels the exact active or reload-recovered operation. |
| **Local readiness diagnostics** | Checks the Engine connection, live events, and prepared model readiness. |

Reloading the page restores the selected durable session. If a generation start is durable but its terminal event is still pending, the Web client identifies that exact operation, blocks a conflicting prompt, and allows replay or cancellation. Authentication failures, replay failures, no-session, no-model, interrupted-stream, and cancellation failures are shown explicitly rather than presented as successful responses.

The Web interface includes semantic landmarks and headings, associated form labels, visible keyboard focus, disabled/busy states, a concise live status region, and readable errors. Incremental model tokens are not repeatedly announced through the conversation live region.

## Safe Model Artifact Management

Model management is available only through the authenticated local Engine. The CLI and Web client do not write downloads into the open repository and do not call Hugging Face directly.

Every candidate begins with a metadata-only preview bound to one exact Hugging Face repository, immutable commit revision, and repository-relative GGUF file. Before any model bytes transfer, xCopilot displays and persists:

- normalized repository, revision, and file identity;
- declared license and free-eligibility decision;
- public, gated, private, and disabled state;
- exact non-zero byte size;
- publisher SHA-256 and storage evidence; and
- the reasons behind the eligibility decision.

An explicit approval is valid only for that exact preview digest. The Engine streams the approved body with bounded memory into private staging, validates every approved HTTPS redirect, checks disk headroom and content length, refuses byte overruns, verifies GGUF magic, exact size, and SHA-256, flushes the file, and atomically exposes only a complete verified installation. Absolute local paths are never returned to clients.

Use **Cancel transfer** in the Web panel or `Ctrl+C` while the CLI is waiting for a pull. Cancellation aborts the exact operation, removes partial staging, and records a non-success terminal state. Startup recovery also removes orphan staging and marks interrupted work explicitly rather than assuming installation succeeded.

Use **Refresh catalog** or `xcopilot models refresh` before relying on older metadata. Changed or unconfirmed revision, file, license, gated/private/disabled state, size, or hash invalidates prior approval and clears active eligibility. Installed bytes may remain in private storage, but they cannot be selected until a current safe preview is approved and installed. At most one currently valid installed artifact can be active, and pull never activates it automatically.

Model-artifact selection and chat deployment selection are separate. `xcopilot models set` records the approved installed artifact; it does not launch llama.cpp, Ollama, or another runtime. Chat still requires a compatible prepared deployment exposed by the Engine.

## Safe File Workflow

xCopilot clients never read or write repository files directly. Every operation goes through the authenticated local Engine and the durable session's canonical repository.

For model context, use a standalone `@file <relative-path>` prompt line. Add `#L<start>-L<end>` to select a bounded line range. Up to four directives and 64 KiB of combined context are accepted per request. Repository text is always marked as untrusted data and cannot change policy or approve an action.

A single-file transaction follows these steps:

1. load one permitted UTF-8 text file;
2. edit the exact replacement text;
3. create and review a durable unified diff while the worktree remains unchanged;
4. explicitly approve or reject the proposal; and
5. optionally undo an applied proposal while the replacement still matches exactly.

Approval rechecks the repository, path, ignore/credential/binary/size/link policy, current file identity, and base hash. A changed target returns a conflict rather than overwriting user work. The replacement is staged, flushed, atomically installed, reopened, and verified. Interrupted apply/undo transitions are reconciled before the Engine becomes ready; unknown or partial target bytes stop readiness instead of being guessed.

The safe-file policy refuses absolute or traversing paths, mount crossings, `.gitignore`/`.xcopilotignore` matches, generated/vendor defaults, credential-like paths/content, binary or invalid UTF-8/control-heavy data, oversized files, symlinks, junctions, reparse points, and path-identity races. There is no broad ignore override in this preview.

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
