# Project xCopilot

**Status: pre-release implementation. No supported binary or installer has been released.**

Project xCopilot is the planned public distribution name for xCopilot: a local-first coding assistant with a Node.js/TypeScript CLI and authenticated locally hosted Web UI. It is designed to use local models by default and to reject paid/metered model inference.

This repository is the canonical home for end-user documentation and future release artifacts. Private implementation source, developer setup, tests, and engineering evidence remain in the separate `xCopilot Source` repository.

The private source preview now includes durable local sessions, the interactive CLI (`xcopilot`, `xcopilot ask`, and `xcopilot serve`), an authenticated locally hosted Web chat, safe local model-artifact preview/pull/selection, a durable local Runtime Manager, deterministic local-only model routing, a static local-model Feasibility Advisor, safe repository-file context, and one-file diff approval/undo. Both clients use the same Engine session history, model-artifact catalog, ownership-gated runtime state, redacted hardware/workload assessments, automatic managed llama.cpp route, explicit-only Ollama selection, streaming, exact-operation cancellation, and file-safety policy. This does not change the release warning: no artifact in this repository is currently supported for installation.

## User Documentation

- [Documentation index](docs/README.md)
- [Getting started](docs/getting-started.md)
- [User manual](docs/user-manual.md)
- [Release artifact policy](releases/README.md)

## Planned Capabilities

- Repository-aware chat and approved single/multi-file edits.
- Safe agent/tool workflow with plans, transactional undo, structured approvals, and isolated Git worktrees.
- Dedicated local/PR code-review mode.
- Bounded Recursive Language Model (RLM) handling for oversized inputs.
- MCP client and local MCP Lab for inspect/lint/health/diff/test before enablement.
- **Can I run it locally?** calculator for model/quantization fit and workload-based static response-time ranges; optional measured local benchmarks remain planned.
- Tiered multi-runtime local inference: managed llama.cpp, dedicated Ollama service support, profiled local APIs, role-specific deployments, and central RAM/VRAM/concurrency scheduling.
- Capacity & Quota dashboard showing active/queued workload, local model resources, hosted free-model consumption/reservations, exact next reset or rolling recovery, and future remote-runner capacity.
- Measured prompt/cache/context efficiency through deterministic manifests, local context bills, cache evidence, and quality-preserving regression budgets.
- Future privacy-first durable project/user memory with provenance, explicit forget/redact/rollback, and evaluated reversible skill learning.
- Future local stdio ACP editor interoperability, Plugin SDK/registry, ToolRadar reference plugin, execution sandbox, disabled local scheduler, four bounded local/remote placement modes, private cross-device continuity, and first-party IDE clients.

## Source and License Status

- Core implementation source is private and is **not currently open source**.
- This repository does not contain the private implementation source.
- A future release may provide signed binaries, PowerShell installers, manifests, SBOM/provenance, and documentation from this public distribution project through GitHub Releases.
- The intended use is non-commercial, but the complete product license, third-party/model notices, distribution rights, and signing identity are not finalized. No public artifact should be distributed until those release blockers are resolved.

## Installation Safety

There is no valid installation command yet. Future Windows installation will:

1. download a PowerShell bootstrap from a **versioned GitHub Release** to disk;
2. verify the expected Authenticode publisher;
3. verify a signed checksum manifest and package hash;
4. execute only after verification;
5. ask separately before installing any optional runtime or downloading a model.

Mutable-branch `irm .../main/install.ps1 | iex` instructions will not be published.

## Planning Documents

- [xCopilot Concept repository](https://github.com/narinderparmar/xCopilot-Concept)
- [Concept](https://github.com/narinderparmar/xCopilot-Concept/blob/main/CONCEPT.md)
- [SDLC document index](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/README.md)
- [Deep document review](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/11-document-review.md)
- [Local Model Feasibility Advisor](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/12-local-model-feasibility-advisor.md)
- [Multi-Runtime Local and On-Device Inference](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/13-multi-runtime-local-inference.md)
- [Durable Memory and Reversible Learning](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/14-durable-memory-lifecycle.md)
- [Local Background Scheduler](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/15-local-background-scheduler.md)
- [Execution Sandbox Strategy](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/16-execution-sandbox-strategy.md)
- [Context Efficiency and Observability](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/17-context-efficiency-observability.md)
- [Execution Placement and Elastic Parallelism](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/18-execution-placement-parallelism.md)
- [Capacity and Quota Dashboard](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/19-capacity-quota-dashboard.md)
- [Implementation Readiness and Action Plan](https://github.com/narinderparmar/xCopilot-Concept/blob/main/docs/20-implementation-readiness-action-plan.md)

## Helpful Evaluation Sources

- [GitHub Copilot documentation](https://docs.github.com/en/copilot)
- [OpenCode](https://opencode.ai/)
- [`awesome-opensource-ai` Autonomous Coding Agents](https://github.com/alvinreal/awesome-opensource-ai#autonomous-coding-agents)
- [Ollama documentation](https://docs.ollama.com/)
- [llama.cpp](https://github.com/ggml-org/llama.cpp), [MLC-LLM](https://github.com/mlc-ai/mlc-llm), [WebLLM](https://github.com/mlc-ai/web-llm), and [LiteRT-LM](https://github.com/google-ai-edge/LiteRT-LM)
- [Hugging Face JavaScript](https://huggingface.co/docs/huggingface.js/index)
- [Recursive Language Models paper](https://arxiv.org/abs/2512.24601)
- [Letta Code](https://github.com/letta-ai/letta-code), [jcode](https://github.com/1jehuang/jcode), and [Prime Agent](https://github.com/PrimeIntellect-ai/prime-agent)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Agent Client Protocol](https://agentclientprotocol.com/)
- [NVIDIA OpenShell](https://github.com/NVIDIA/OpenShell), [Background Agents](https://github.com/ColeMurray/background-agents), [Open SWE](https://github.com/langchain-ai/open-swe), and [firstmate](https://github.com/kunchenguid/firstmate)
- [MCP.Directory](https://mcp.directory/) and [MCP Playground](https://www.mcpplayground.tech/)
- [ToolRadar documentation](https://toolradar.com/docs)
- [IndiaAI](https://indiaai.gov.in/) and [AIKosh](https://aikosh.indiaai.gov.in/)

Discovery catalogs and rankings are not cost, license, availability, or security authorities. Exact model artifacts/endpoints must be independently verified and disabled when evidence is stale.
