# Getting Started

> **Pre-release:** no supported xCopilot binary or installer has been published. Do not use unofficial installation commands or artifacts.

When the first supported release is available, this guide will provide the complete end-user path:

1. Select a versioned release from this project's GitHub Releases page.
2. Download the installer/package, signed checksum manifest, signature, SBOM, and provenance statement.
3. Verify the manifest signature, expected publisher, and package SHA-256 before execution.
4. Run the platform installer and explicitly approve any optional local runtime or model download.
5. Start xCopilot, complete the local-only first-run setup, and verify the Engine is bound to loopback.
6. Select a free-model-eligible local or hosted provider and run the built-in connection check.
7. Open a project and review the permission, privacy, and resource limits before the first task.

Exact supported operating systems, hardware requirements, commands, package names, verification keys, and screenshots will be generated and tested as part of the release process. Until then, any claimed installer or binary should be treated as unofficial.

## Private Source Preview

Developers who already have authorized access to `xCopilot Source` can run the Windows source launcher:

```powershell
.\start-xcopilot.bat
```

It builds the source, starts the loopback-only Engine and Web app, opens a one-use authenticated browser URL, and opens the repository CLI. Keep the launcher window open while using either client.

In the Web app:

1. enter the repository path and optionally a session title;
2. create or select a durable chat session;
3. choose an explicitly prepared local model when one is available;
4. send one prompt and watch the bounded response stream;
5. use **Stop active response** to cancel the exact current operation; and
6. use **Replay history** after a reload or interruption to restore ordered durable messages without duplicates.

The Web app remembers only the selected session identity in same-origin browser storage. Authentication remains in an `HttpOnly`, `SameSite=Strict` local Engine session, and browser mutations require the Engine-issued CSRF value. No third-party scripts, fonts, or hosted Web assets are loaded.

When no approved prepared model is configured, session creation, selection, and history replay remain available in both clients. Prompt submission stays disabled and xCopilot does not silently download, invent, or select a paid/unknown deployment. Local connection and model-readiness checks remain available under **Local readiness diagnostics**.

### Safe local model artifacts

The Web **Model management** panel separates assessment, transfer, and selection:

1. enter an exact Hugging Face `namespace/repository`, 40-character revision, and GGUF file path;
2. choose **Preview exact model** without downloading the model body;
3. review the displayed revision, license, public/gated state, exact byte size, SHA-256, and eligibility evidence;
4. choose **Approve and pull exact artifact** only when every fact matches your intended artifact;
5. use **Cancel transfer** to stop the exact active download and clean partial staging; and
6. after a verified install, choose **Set active** explicitly.

Pull never selects a model automatically. **Refresh** rechecks the pinned metadata and disables stale approval or active selection when the revision, file, license, availability, size, or hash changes or cannot be confirmed.

The equivalent CLI workflow is deliberately two-step so the preview can be reviewed before approval:

```powershell
node .\cli\dist\index.js models pull `
  --repository Qwen/Qwen2.5-Coder-0.5B-Instruct-GGUF `
  --revision ebb2015119c907b064c512bf053e945850b5875f `
  --file qwen2.5-coder-0.5b-instruct-q4_k_m.gguf

node .\cli\dist\index.js models pull --id <model-id> --approve
node .\cli\dist\index.js models set --id <model-id>
node .\cli\dist\index.js models list
node .\cli\dist\index.js models refresh --id <model-id>
```

Press `Ctrl+C` during an approved CLI pull to request cancellation. xCopilot accepts only a currently verified, public, free-license-eligible, single-file GGUF artifact with an exact publisher SHA-256 and bounded size. It refuses unsafe formats, unverified or gated/private metadata, redirects outside its HTTPS allowlist, size/hash/content mismatches, insufficient disk headroom, and partial installations.

An active artifact is a trusted catalog selection, not a running chat deployment. Chat remains unavailable until an approved local runtime exposes a compatible prepared deployment through the Engine.

### Register and control a local runtime

The Web **Runtime management** panel keeps discovery, registration, enablement, and process/model residency separate:

1. choose managed llama.cpp, dedicated Ollama, or diagnostic loopback;
2. enter the exact local path or loopback endpoint and assign a deployment role;
3. choose **Discover** and review trust, ownership, model identity, warnings, and configuration/discovery digests;
4. choose **Register** only when the preview matches the intended runtime;
5. explicitly enable the registration; and
6. use only the actions allowed by its ownership.

A managed llama.cpp registration can be started and stopped by xCopilot. A dedicated Ollama registration can load or unload the approved model but xCopilot never starts or stops the user-owned daemon. A generic loopback registration is diagnostic-only and cannot become an automatic chat route.

Equivalent CLI inspection and lifecycle commands are:

```powershell
node .\cli\dist\index.js runtimes list
node .\cli\dist\index.js runtimes status <runtime-id>
node .\cli\dist\index.js runtimes enable <runtime-id>
node .\cli\dist\index.js runtimes start <runtime-id>
node .\cli\dist\index.js runtimes stop <runtime-id>
node .\cli\dist\index.js runtimes disable <runtime-id>
```

Preview and register managed llama.cpp with the same exact arguments:

```powershell
$runtimeArgs = @(
  "--kind", "managed_llama_cpp",
  "--role", "chat_edit",
  "--runtime-dir", "C:\path\to\verified-llama.cpp",
  "--backend", "cpu",
  "--acceleration", "x64",
  "--device-pool", "system-memory",
  "--context-window", "2048",
  "--max-concurrent", "1",
  "--threads", "4"
)
node .\cli\dist\index.js runtimes discover @runtimeArgs
node .\cli\dist\index.js runtimes register @runtimeArgs
```

For the pinned source-preview Ollama profile, first start and verify Ollama yourself, then preview/register its exact loopback endpoint and local blob:

```powershell
node .\cli\dist\index.js runtimes register `
  --kind ollama_service `
  --role chat_edit `
  --endpoint http://127.0.0.1:11434 `
  --profile ollama-xcopilot-readiness-q4 `
  --artifact-path C:\path\to\exact\ollama-blob `
  --max-concurrent 1

node .\cli\dist\index.js runtimes load <runtime-id>
node .\cli\dist\index.js runtimes unload <runtime-id>
```

Registration never silently enables or starts a runtime. Only enabled, ready, non-diagnostic deployments are offered to chat. Pending actions, stale model bindings, offline services, unexpected managed-process exits, identity mismatches, and unresolved cleanup are shown explicitly and removed from routing.

### Check whether a model can run locally

The Web **Can I run it locally?** panel loads a redacted local hardware profile and uses the same catalog and runtime registrations shown elsewhere in the app:

1. select the exact cataloged model artifact;
2. choose all registered runtimes or one exact runtime;
3. choose Code completion, Interactive chat, Agentic edit, Code/PR review, or bounded RLM repository analysis;
4. optionally override context tokens, output tokens, or concurrency; and
5. choose **Assess local fit**.

The result shows compatibility, memory and disk fit, broad static response-time ranges, confidence, evidence, and suggested constraints. The assessment does not download the artifact, start or load a runtime, run a prompt, send hardware data away from the device, or claim that faster inference means better coding quality.

When a managed llama.cpp registration is currently bound to another GGUF, xCopilot can still estimate the selected artifact but marks runtime reconfiguration as required and will not call it **Recommended**. Ollama assessments require the exact verified artifact identity.

Equivalent CLI commands are:

```powershell
node .\cli\dist\index.js doctor --hardware
node .\cli\dist\index.js models can-run <model-id>
node .\cli\dist\index.js models can-run <model-id> --runtime <runtime-id> --workload interactive-chat-v1 --context 8192
node .\cli\dist\index.js runtimes compare <model-id> --workload rlm-repository-analysis-v1
```

Use `--json` when another local tool needs the same structured result. The first source-preview release provides static estimates only; optional measured benchmarks remain planned.

### Inspect local capacity and queued work

The Web **Capacity & quota** panel reads the same Resource Scheduler state used to admit local runtimes. For each configured pool it shows:

- total scheduler capacity, reserved capacity, and currently available memory, CPU threads, sequences, and concurrency;
- resident runtime registrations and their current reservation state;
- exact active leases and FIFO queue entries;
- the next known capacity return, labeled **Estimated**; and
- an explicit statement that local resources have no quota reset.

Use **Refresh capacity** after starting, loading, stopping, or unloading a runtime. A missing release time is shown as `unknown`; xCopilot does not invent a countdown.

Ollama and other external local runtimes share the system-memory capacity budget by default, so the dashboard does not count the same host RAM or CPU twice. A separate external pool appears only when an independent capacity budget has been explicitly configured.

Equivalent CLI commands are:

```powershell
node .\cli\dist\index.js capacity status
node .\cli\dist\index.js capacity explain <runtime-or-deployment-id>
```

Add `--json` for the same bounded API response. Local `localResetAt` is always `null`; memory and execution slots return only when the corresponding lease is released.

### Safe file context and one-file changes

To include a repository file in one model request, put the directive on its own prompt line:

```text
@file src/example.ts
@file src/example.ts#L10-L40
```

The Engine reads the file from the durable session's repository, labels it as untrusted context with no authorization, and keeps the prompt itself unchanged in history.

The Web **Safe file context and diff** panel can load one permitted text file, edit exact replacement text, and create a durable diff without writing the worktree. Review the diff, then choose **Approve exact proposal** or **Reject proposal**. **Undo latest applicable edit** restores the exact prior bytes only when the file still matches the applied replacement.

Equivalent CLI commands are:

```text
/file src/example.ts#L10-L40
/propose src/example.ts => "exact replacement text\n"
/diff
/approve
/reject
/undo
```

Approval and undo refuse to overwrite a file changed since the proposal or apply step. Absolute/traversing paths, ignored/generated/vendor files, credential-like paths or content, binaries, invalid UTF-8, oversized files, links/junctions/reparse points, and uncertain path identity are refused explicitly.

After a source build, the command equivalents are:

```powershell
node .\cli\dist\index.js serve
node .\cli\dist\index.js
node .\cli\dist\index.js ask "Summarize this repository"
```

Interactive CLI and Web prompts require an approved prepared local deployment. The current developer validation uses the small Qwen2.5 Coder 0.5B Q4_K_M model as a compatible local test artifact; Qwen3 1.7B, TinyLlama, GPT-2 Small, and other small models still require a verified compatible runtime/model profile before they can be selected.
