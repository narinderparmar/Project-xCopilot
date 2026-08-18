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

It builds the source, starts the loopback-only Engine and Web app, and opens the repository CLI. The CLI remains usable for `/help`, `/model`, and session setup when no model is configured, but it does not silently download or invent a deployment.

After a source build, the command equivalents are:

```powershell
node .\cli\dist\index.js serve
node .\cli\dist\index.js
node .\cli\dist\index.js ask "Summarize this repository"
```

Interactive prompts require an approved prepared local deployment. The current developer validation uses the small Qwen2.5 Coder 0.5B Q4_K_M model as a compatible local fallback; Qwen3 1.7B, TinyLlama, GPT-2 Small, and other small models still require a verified compatible runtime/model profile before they can be selected.
