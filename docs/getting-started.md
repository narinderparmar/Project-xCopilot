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
