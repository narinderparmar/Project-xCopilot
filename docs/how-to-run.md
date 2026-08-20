# How to Run xCopilot (Hands-on)

> Applies to the private source preview. There is no supported public installer yet — do not distribute or run unofficial packages.

This guide walks through a first-hand experience of both the locally hosted Web UI and the CLI on Windows, using the launcher included in `xCopilot Source`.

## Prerequisites

- Windows 10/11 with PowerShell 5.1 or newer.
- Node.js **v24.19.0** and npm **11.17.0** available on `PATH`. If they are not on `PATH`, set `XCOPILOT_NODE_HOME` to their folder before launching.
- A free TCP port `43120` on the loopback interface. Override with `XCOPILOT_LAUNCHER_PORT` if it is in use.
- Authorized clone of the `xCopilot Source` repository.

xCopilot binds only to `127.0.0.1` and does not send telemetry.

## 1. Start both UIs with one command

From an ordinary PowerShell window:

```powershell
cd "C:\Source Code\The Blue Room\xCopilot\xCopilot Source"
.\start-xcopilot.bat
```

On first run the launcher installs exact dependencies and builds the Engine, CLI, and Web assets. It then:

1. starts the loopback-only Engine on `http://127.0.0.1:43120`;
2. opens your default browser at a one-use bootstrap URL that establishes an `HttpOnly` browser session;
3. opens a second window titled **xCopilot CLI** already authenticated against the same Engine.

Keep the launcher window open while using either client.

Other launcher actions:

```powershell
.\start-xcopilot.bat check   # verify toolchain and build only
.\start-xcopilot.bat stop    # stop the launcher-managed Engine
```

## 2. Try the Web UI (opens automatically)

1. Enter the **repository path** you want the session to reference. The source folder itself is a valid choice for a first run.
2. Optionally set a session title, then choose **Create session**.
3. Explore the right-hand panels: **Local readiness diagnostics**, **Model management**, **Runtime management**, **Can I run it locally?**, **Capacity & quota**, and **Safe file context and diff**.
4. In the model selector, look for **Automatic** (managed llama.cpp when strictly eligible) or entries labelled **explicit only** (such as Ollama).
5. Type a prompt in the composer and choose **Send**.
6. Use **Stop active response** to cancel the exact current operation, and **Replay history** after a reload to restore ordered durable messages without duplicates.

If the composer is disabled, no ready `chat_edit` deployment is available yet. Follow section 4 to register one, or continue exploring the read-only panels — session creation, selection, and history replay all work without a model.

## 3. Try the CLI (opens in a second window)

The launcher CLI is already pointed at the same Engine and bearer file. Useful REPL commands:

```text
help
/session new
/session list
Summarize this repository
/model automatic
/model <deployment-id>
/file src\example.ts
/file src\example.ts#L10-L40
/propose src\example.ts => "exact replacement text\n"
/diff
/approve
/reject
/undo
```

Press **Ctrl+C** to cancel the exact active operation without leaving the REPL.

One-shot queries from any PowerShell window that has already run the launcher:

```powershell
node .\cli\dist\index.js ask "Summarize this repository"
node .\cli\dist\index.js ask --model <deployment-id> "Use this explicit-only local model"
```

## 4. Give xCopilot a real model (optional but needed for chat)

The smallest verified artifact is Qwen2.5 Coder 0.5B Q4_K_M (~470 MB). The two-step pull is deliberate so you can review the preview before approving the download.

```powershell
# Preview the exact artifact
node .\cli\dist\index.js models pull `
  --repository Qwen/Qwen2.5-Coder-0.5B-Instruct-GGUF `
  --revision ebb2015119c907b064c512bf053e945850b5875f `
  --file qwen2.5-coder-0.5b-instruct-q4_k_m.gguf

# Approve and select
node .\cli\dist\index.js models pull --id <model-id> --approve
node .\cli\dist\index.js models set  --id <model-id>
node .\cli\dist\index.js models list
```

Register and start managed llama.cpp against a verified runtime folder:

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
node .\cli\dist\index.js runtimes enable <runtime-id>
node .\cli\dist\index.js runtimes start  <runtime-id>
```

Once the Runtime Manager reports the deployment as ready, the Web selector will show it as **Automatic** and both UIs will accept prompts.

Dedicated Ollama can be added the same way but stays **explicit only**; xCopilot never treats it as a silent fallback.

## 5. Quick sanity checks (no chat required)

```powershell
node .\cli\dist\index.js doctor --hardware
node .\cli\dist\index.js capacity status
node .\cli\dist\index.js runtimes list
node .\cli\dist\index.js models list
```

## 6. Stop cleanly

- Close the launcher window (it stops the Engine on exit), or run `.\start-xcopilot.bat stop`.
- The launcher-managed Engine PID and bearer file live under `%LOCALAPPDATA%\xCopilot\launcher`.

## Troubleshooting

- **`Node.js 24.19.0 and npm 11.17.0 were not found`** — install those exact versions or set `XCOPILOT_NODE_HOME`.
- **`TCP port 43120 is already in use`** — pick a free port with `$env:XCOPILOT_LAUNCHER_PORT = "43121"` before running the launcher.
- **Browser tab shows an auth error** — the bootstrap URL is single-use; run `.\start-xcopilot.bat stop` and start again.
- **Prompt box stays disabled** — no runtime has a ready `chat_edit` deployment; complete section 4.
- **Engine did not become ready within 60 seconds** — inspect `%LOCALAPPDATA%\xCopilot\launcher\engine.stdout.log` and `engine.stderr.log`.
