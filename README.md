# Awesome LLM Gateways

A curated list of open-source LLM gateways, AI gateways, and model-routing proxies.

LLM gateways sit between applications and model providers. They usually handle provider routing, retries, fallbacks, observability, budgets, access control, guardrails, and OpenAI-compatible API translation.

Related terms include AI gateway, model gateway, inference gateway, LLM proxy, model router, semantic router, adaptive-routing LLM gateway, zero-trust LLM gateway, coding-tool AI router, OpenAI-compatible proxy, OpenAI Responses API proxy, OpenTelemetry LLM gateway, provider passthrough gateway, dashboard-backed LLM gateway, and API format-conversion gateway.

This list prioritizes public repositories that act as gateway, proxy, router, or model-access infrastructure rather than generic SDKs or end-user AI applications.

## Contents

- [Quick Comparison](#quick-comparison)
- [Projects](#projects)
- [Choosing a Gateway](#choosing-a-gateway)
- [Evaluation Criteria](#evaluation-criteria)
- [Contributing](#contributing)

## Quick Comparison

| Project | Repo | Language | License | Best Fit | Main Strengths | Trade-offs |
| --- | --- | --- | --- | --- | --- | --- |
| LiteLLM | [BerriAI/litellm](https://github.com/BerriAI/litellm) | Python | Other | Teams that want broad provider coverage and OpenAI-compatible routing quickly. | 100+ provider support, cost tracking, load balancing, logging, guardrails, active ecosystem. | Python proxy can be less attractive for very low-latency edge gateway use cases; license should be reviewed before commercial embedding. |
| Portkey Gateway | [Portkey-AI/gateway](https://github.com/Portkey-AI/gateway) | TypeScript | MIT | Product teams that want gateway plus guardrails and model routing. | Friendly API, many model integrations, guardrails, model router, permissive license. | Some ecosystem value is tied to Portkey's broader platform; self-hosting depth should be checked for each feature. |
| OmniRoute | [diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute) | TypeScript | MIT | Developers and teams that want a local or hosted AI router for many providers and coding tools through one endpoint. | OpenAI-compatible APIs, broad provider catalog, multiple routing strategies, automatic fallback, prompt compression, format translation, MCP/A2A integrations, and desktop/PWA options. | Very broad product surface with marketing-heavy claims; production maturity, security model, and provider behavior should be validated carefully. |
| Envoy AI Gateway | [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway) | Go | Apache-2.0 | Kubernetes/cloud-native teams already aligned with Envoy Gateway. | Built on Envoy Gateway, strong infrastructure pedigree, Kubernetes-native direction. | Younger project than general API gateways; feature surface may be narrower for app-level LLMOps needs. |
| Higress | [higress-group/higress](https://github.com/higress-group/higress) | Go | Apache-2.0 | Cloud-native API gateway users that want AI gateway capability in one gateway. | AI-native API gateway, Envoy-based, API gateway plus AI routing patterns. | Operational model is closer to full API gateway infrastructure than lightweight app proxy. |
| Inference Gateway | [inference-gateway/inference-gateway](https://github.com/inference-gateway/inference-gateway) | Go | MIT | Teams that want a self-hosted, lightweight gateway for multiple hosted and local providers. | Unified proxy for providers such as OpenAI, Ollama, Groq, Cohere, Anthropic, Cloudflare, and DeepSeek; streaming, MCP, OpenTelemetry, Docker, and Kubernetes support. | Routing model is environment/configuration driven; teams needing advanced policy, spend governance, or full API management may need surrounding tooling. |
| GoModel | [ENTERPILOT/GoModel](https://github.com/ENTERPILOT/GoModel) | Go | MIT | Teams that want a lightweight Go gateway with OpenAI-compatible APIs, provider passthrough, and built-in usage visibility. | Supports many hosted and local providers, streaming, Responses API, embeddings, files, batches, observability, guardrails, cost tracking, Docker, and config/env-based setup. | Newer project with a fast-changing provider matrix; advanced enterprise policy and governance depth should be validated against the exact deployment. |
| pLLM | [andreimerfu/pllm](https://github.com/andreimerfu/pllm) | Go | MIT | Teams that want a Go OpenAI-compatible gateway with route-based model orchestration, failover, and Kubernetes deployment options. | Virtual model routes, priority/latency/weighted/random routing, automatic failover, multi-key load balancing, caching, budgets, Prometheus metrics, OpenTelemetry, Docker, and Helm support. | Newer project with ambitious production claims; teams should validate provider adapters, dashboard/admin APIs, and Redis-backed routing behavior against real traffic. |
| New API | [QuantumNous/new-api](https://github.com/QuantumNous/new-api) | Go | AGPL-3.0 | Teams that need a self-hosted AI model hub for key management, quota controls, and cross-format gateway APIs. | OpenAI-compatible, Claude-compatible, Gemini-compatible, Responses, realtime, image/audio/embedding/rerank interfaces, weighted routing, retries, user-level rate limits, Docker, and web UI. | AGPL licensing must be reviewed for hosted/commercial deployments; broad model and billing surface adds more operational policy decisions than a minimal proxy. |
| LLM Gateway | [theopenco/llmgateway](https://github.com/theopenco/llmgateway) | TypeScript | AGPL-3.0 / Enterprise | Teams that want a self-hosted or hosted OpenAI-compatible gateway with dashboard analytics and key management. | Multi-provider routing, centralized provider keys, usage/cost analytics, performance monitoring, Docker self-hosting, and dashboard/playground apps. | Dual-license model needs review; the monorepo includes several apps and backing services, so it is heavier than a minimal proxy. |
| OpenZiti LLM Gateway | [openziti/llm-gateway](https://github.com/openziti/llm-gateway) | Go | Apache-2.0 | Teams that need OpenAI-compatible routing across hosted and private inference backends, especially behind NAT or private networks. | Single Go binary, semantic routing, Anthropic translation, virtual API keys, OpenTelemetry metrics, multi-endpoint load balancing, and zrok/OpenZiti zero-trust connectivity. | Early-stage project with a smaller community; provider coverage is narrower than broad aggregation gateways and strongest when its zero-trust networking model is needed. |
| Kong Gateway | [Kong/kong](https://github.com/Kong/kong) | Lua | Apache-2.0 | Enterprises that already need API management and want AI gateway features in the same stack. | Mature API gateway, plugin ecosystem, AI gateway and MCP-related features, strong operations story. | Heavier than purpose-built LLM proxies; some advanced workflows may depend on Kong ecosystem/product choices. |
| Apache APISIX | [apache/apisix](https://github.com/apache/apisix) | Lua | Apache-2.0 | Teams that want a cloud-native API gateway with AI gateway capabilities. | Mature Apache project, API management, Kubernetes ingress, plugins, AI gateway direction. | More general-purpose API gateway than LLM-specific proxy; AI workflows may need plugin/configuration work. |
| Bifrost | [maximhq/bifrost](https://github.com/maximhq/bifrost) | Go | Apache-2.0 | Teams optimizing for low-overhead model routing and gateway performance. | Go implementation, model routing, load balancing, guardrails, observability and cost-oriented features. | Younger than established API gateways; benchmark and feature claims should be verified in your workload. |
| Traceloop Hub | [traceloop/hub](https://github.com/traceloop/hub) | Rust | Apache-2.0 | Teams that want a high-performance OpenAI-compatible gateway with tracing and metrics built in. | Rust gateway, OpenAI-compatible API, OpenTelemetry tracing, Prometheus metrics, YAML mode, PostgreSQL-backed management mode, and Kubernetes assets. | Provider coverage is narrower than broad aggregators; database mode adds PostgreSQL and management API operations. |
| TensorZero | [tensorzero/tensorzero](https://github.com/tensorzero/tensorzero) | Rust | Apache-2.0 | Teams that want an LLM gateway tied to observability, evaluation, optimization, and experimentation. | Rust gateway, model access layer, feedback/evaluation loop, experimentation-oriented LLMOps platform. | Broader platform than a standalone proxy; teams should confirm they want the surrounding LLMOps workflow, not only request routing. |

## Projects

### LiteLLM

- GitHub: [BerriAI/litellm](https://github.com/BerriAI/litellm)
- Website/docs: [docs.litellm.ai](https://docs.litellm.ai/docs/)
- Language: Python

LiteLLM is a Python SDK and proxy server for calling many LLM providers through OpenAI-compatible or native formats. It is a strong default when provider breadth and quick integration matter.

**Pros**

- Broad provider coverage across OpenAI, Anthropic, Gemini, Bedrock, Azure, Vertex AI, Cohere, Hugging Face, vLLM, and more.
- Useful operational features: cost tracking, load balancing, logging, and guardrails.
- Large community and fast-moving ecosystem.

**Cons**

- Runtime and deployment model may not fit teams that standardize on Go, Envoy, or Kubernetes-native gateway control planes.
- License metadata should be reviewed before using it as an embedded commercial dependency.

### Portkey Gateway

- GitHub: [Portkey-AI/gateway](https://github.com/Portkey-AI/gateway)
- Website: [portkey.ai/features/ai-gateway](https://portkey.ai/features/ai-gateway)
- Language: TypeScript
- License: MIT

Portkey Gateway focuses on AI gateway routing, guardrails, and model access through a developer-friendly API.

**Pros**

- MIT licensed.
- Strong focus on model routing and guardrail workflows.
- Good fit for application teams that want gateway functionality without adopting a full API gateway stack.

**Cons**

- Some capabilities may be most useful alongside Portkey's hosted or broader platform components.
- Teams should verify self-hosted behavior for the exact guardrails, observability, and routing features they need.

### OmniRoute

- GitHub: [diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute)
- Website: [omniroute.online](https://omniroute.online)
- Language: TypeScript
- License: MIT

OmniRoute is a TypeScript AI gateway and router that exposes OpenAI-compatible endpoints for many providers, coding agents, and multimodal APIs. It combines provider routing, automatic fallback, prompt compression, format translation, MCP/A2A integrations, and app surfaces such as web, desktop, and PWA.

**Pros**

- MIT licensed.
- Broad provider and tool focus, including OpenAI-compatible clients, Claude Code, Codex, Gemini CLI, Cursor, Cline, OpenClaw, and other coding tools.
- Supports multiple routing strategies, auto-fallback, prompt compression, format translation, chat/responses/embeddings/images/audio-related APIs, MCP server tooling, and A2A protocol integration.
- Useful when one local or hosted endpoint should sit in front of many AI subscriptions, API keys, and low-cost or free provider options.

**Cons**

- The project has a very broad surface area, so teams should validate the specific gateway path they need instead of assuming every advertised workflow is production-ready.
- README and positioning are marketing-heavy; claims around provider count, compression savings, and tool compatibility should be checked against real workloads.
- More application-like than minimal gateway infrastructure, which may be unnecessary for teams that only need a small reverse proxy or Kubernetes-native gateway.

### Envoy AI Gateway

- GitHub: [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway)
- Website/docs: [aigateway.envoyproxy.io](https://aigateway.envoyproxy.io)
- Language: Go
- License: Apache-2.0

Envoy AI Gateway is built on Envoy Gateway and targets unified access to generative AI services.

**Pros**

- Natural fit for Kubernetes and Envoy Gateway environments.
- Apache-2.0 licensed.
- Good direction for platform teams that want AI traffic managed like other infrastructure traffic.

**Cons**

- Younger and more infrastructure-oriented than some app-layer LLM gateway projects.
- May require more Kubernetes/gateway expertise than lightweight proxies.

### Higress

- GitHub: [higress-group/higress](https://github.com/higress-group/higress)
- Website: [higress.ai](https://higress.ai)
- Language: Go
- License: Apache-2.0

Higress is an AI-native API gateway with cloud-native and Envoy-based roots.

**Pros**

- Combines general API gateway concerns with AI gateway direction.
- Apache-2.0 licensed.
- Useful when AI traffic should share policy, routing, and operations with existing API gateway traffic.

**Cons**

- More infrastructure-heavy than a simple LLM provider proxy.
- Best fit depends on whether the team wants to operate a full API gateway.

### Inference Gateway

- GitHub: [inference-gateway/inference-gateway](https://github.com/inference-gateway/inference-gateway)
- Language: Go
- License: MIT

Inference Gateway is a self-hosted proxy server for accessing multiple language model APIs through a unified interface. It supports hosted providers and local options, with features such as streaming responses, MCP integration, OpenTelemetry, Docker deployment, and Kubernetes deployment examples.

**Pros**

- MIT licensed.
- Go implementation with a lightweight self-hosted deployment model.
- Supports multiple provider targets, including OpenAI, Ollama, Groq, Cohere, Anthropic, Cloudflare, and DeepSeek.
- Includes practical production-facing features such as streaming, TLS/timeouts, OpenTelemetry, Docker, and Kubernetes support.

**Cons**

- Advanced governance, budget management, and policy workflows appear less central than in larger API gateway or LLMOps platforms.
- Provider routing is primarily configuration and model-name driven, so teams should validate it against their desired routing and fallback behavior.

### GoModel

- GitHub: [ENTERPILOT/GoModel](https://github.com/ENTERPILOT/GoModel)
- Website/docs: [gomodel.enterpilot.io](https://gomodel.enterpilot.io/)
- Language: Go
- License: MIT

GoModel is a Go-based AI gateway that exposes OpenAI-compatible endpoints across hosted and local providers such as OpenAI, Anthropic, Gemini, DeepSeek, Groq, OpenRouter, xAI, Azure OpenAI, Ollama, vLLM, and others. It also includes provider-native passthrough, observability, guardrails, cost tracking, Docker deployment, and configuration through environment variables or YAML.

**Pros**

- MIT licensed.
- Lightweight Go gateway with Docker and source-based deployment options.
- Supports chat completions, Responses API, embeddings, model listing, streaming, files, batches, and provider passthrough patterns.
- Useful when teams want gateway behavior plus built-in usage, token, and cost visibility without adopting a full API gateway stack.

**Cons**

- Newer project with a fast-moving provider and endpoint matrix, so exact provider behavior should be validated before production use.
- Enterprise-grade tenant isolation, budget enforcement, and policy workflows may need surrounding platform controls.
- Configuration-first routing is practical for simple deployments but may be less expressive than dedicated policy engines or Kubernetes-native gateway control planes.

### pLLM

- GitHub: [andreimerfu/pllm](https://github.com/andreimerfu/pllm)
- Language: Go
- License: MIT

pLLM is a Go-based LLM gateway that exposes an OpenAI-compatible API across providers such as OpenAI, Anthropic, Azure OpenAI, AWS Bedrock, Google Vertex AI, Grok, and Cohere. Its main abstraction is a virtual route that maps application model names to real provider models with configurable routing strategies, failover chains, multi-key load balancing, caching, budget controls, and observability.

**Pros**

- MIT licensed.
- Go implementation with Docker Compose and Helm deployment paths.
- Supports virtual model routes with priority, least-latency, weighted round-robin, and random routing strategies.
- Includes automatic failover, circuit-breaker-style health tracking, multi-key load balancing, rate limits, budget controls, Prometheus metrics, OpenTelemetry tracing, and an admin dashboard.
- Useful when teams want OpenAI-compatible client compatibility plus runtime route management and Kubernetes-friendly deployment.

**Cons**

- Newer project with a smaller track record than mature gateway stacks, so production maturity should be validated carefully.
- Some routing behavior depends on backing services such as Redis for distributed latency tracking, which adds operational state beyond a minimal proxy.
- Provider adapter compatibility, dashboard workflows, and enterprise-style access controls should be tested against the exact deployment and traffic shape.

### New API

- GitHub: [QuantumNous/new-api](https://github.com/QuantumNous/new-api)
- Website/docs: [docs.newapi.pro](https://docs.newapi.pro/en/docs)
- Language: Go
- License: AGPL-3.0

New API is a self-hosted LLM gateway and AI asset management system for aggregating authorized model providers, managing API keys and quotas, and exposing multiple model API formats. It supports OpenAI-compatible, OpenAI Responses, Claude Messages, Gemini, embeddings, rerank, image, audio, and realtime-style interfaces, with Docker-based deployment and a management UI.

**Pros**

- Go implementation with Docker and Docker Compose deployment paths.
- Supports key management, token grouping, quota/accounting workflows, user-level model rate limits, weighted routing, and automatic retry on failure.
- Useful when teams need one internal model hub that can convert between OpenAI-compatible, Claude-compatible, and Gemini-compatible formats.

**Cons**

- AGPL-3.0 licensed, so hosted, modified, or commercial deployment plans need careful license review.
- Broad gateway, billing, user-management, and UI surface can be more operationally complex than a small provider proxy.
- Some format-conversion paths are documented as partial or in development, so exact API compatibility should be tested before relying on them.

### LLM Gateway

- GitHub: [theopenco/llmgateway](https://github.com/theopenco/llmgateway)
- Website: [llmgateway.io](https://llmgateway.io)
- Language: TypeScript
- License: AGPL-3.0 core / Enterprise features

LLM Gateway is a TypeScript API gateway for routing LLM requests through a unified OpenAI-compatible interface. It supports hosted and self-hosted deployment, centralizes provider API keys, and includes dashboard-oriented usage, cost, and performance analytics.

**Pros**

- Public repository with active development and clear AI gateway positioning.
- Supports routing requests to multiple LLM providers through one OpenAI-compatible interface.
- Includes centralized provider key management, token/cost tracking, performance monitoring, dashboard apps, and Docker-based self-hosting.
- Useful when teams want a gateway plus operational visibility rather than only a thin request proxy.

**Cons**

- Core is AGPL-3.0 while enterprise features use a separate commercial license, so deployment and modification plans need license review.
- Monorepo includes UI, playground, API, gateway, docs, admin, database, and shared packages, which adds more operational surface than small single-binary gateways.
- Some advanced billing, retention, organization, and provider-key features are enterprise-scoped, so self-hosted feature boundaries should be checked before adoption.

### OpenZiti LLM Gateway

- GitHub: [openziti/llm-gateway](https://github.com/openziti/llm-gateway)
- Language: Go
- License: Apache-2.0

OpenZiti LLM Gateway is an OpenAI-compatible proxy for routing requests across OpenAI, Anthropic, and OpenAI-compatible backends such as Ollama, vLLM, llama-server, and SGLang. Its distinctive angle is connecting gateways and private inference backends through zrok/OpenZiti so teams can route across NAT, private networks, or cloud boundaries without exposing backend ports directly.

**Pros**

- Apache-2.0 licensed.
- Single Go binary with YAML configuration.
- Supports model-prefix routing, optional semantic routing, Anthropic translation, streaming, virtual API keys, metrics, and multi-endpoint load balancing.
- Useful for private GPU servers or mixed hosted/local inference backends where network exposure and identity-based access matter.

**Cons**

- Smaller and newer than established gateway projects, so production maturity should be validated carefully.
- Provider coverage is narrower than broad aggregation gateways such as LiteLLM or Portkey Gateway.
- The strongest differentiator is tied to zrok/OpenZiti networking; teams that only need basic provider aggregation may find that extra model unnecessary.

### Kong Gateway

- GitHub: [Kong/kong](https://github.com/Kong/kong)
- Website: [konghq.com](https://konghq.com/install/)
- Language: Lua
- License: Apache-2.0

Kong is a mature API gateway that now includes AI gateway and LLM gateway features in its broader gateway ecosystem.

**Pros**

- Battle-tested API gateway foundation.
- Strong plugin and operations ecosystem.
- Good fit for enterprise API management, security, policy, and governance needs.

**Cons**

- Heavier than a purpose-built LLM proxy.
- Some AI workflows may require adopting broader Kong-specific patterns or products.

### Apache APISIX

- GitHub: [apache/apisix](https://github.com/apache/apisix)
- Website: [apisix.apache.org](https://apisix.apache.org/)
- Language: Lua
- License: Apache-2.0

Apache APISIX is a cloud-native API gateway that also positions itself for AI gateway use cases. It is useful when AI traffic should be governed through a mature API gateway with plugins, ingress support, and API management patterns.

**Pros**

- Mature Apache project with strong API gateway foundations.
- Apache-2.0 licensed.
- Good fit for teams that already need API management, ingress, routing, and plugin-based policy enforcement.

**Cons**

- Not as narrowly focused on LLM provider abstraction as LiteLLM or Portkey.
- AI-specific workflows may require more gateway/plugin configuration and operations expertise.

### Bifrost

- GitHub: [maximhq/bifrost](https://github.com/maximhq/bifrost)
- Website: [getmaxim.ai/bifrost](https://www.getmaxim.ai/bifrost)
- Language: Go
- License: Apache-2.0

Bifrost is a Go-based AI gateway focused on model routing, load balancing, guardrails, observability, cost controls, and low-overhead serving.

**Pros**

- Go runtime is attractive for gateway performance and low-overhead deployment.
- Apache-2.0 licensed.
- Focused on practical LLM gateway concerns: routing, load balancing, guardrails, model support, and token/cost management.

**Cons**

- Younger than mature API gateway stacks such as Kong or APISIX.
- Published performance and feature claims should be validated against your deployment shape, providers, and traffic patterns.

### Traceloop Hub

- GitHub: [traceloop/hub](https://github.com/traceloop/hub)
- Website/docs: [traceloop.com/docs/hub](https://www.traceloop.com/docs/hub)
- Language: Rust
- License: Apache-2.0

Traceloop Hub is a Rust-based LLM gateway that exposes an OpenAI-compatible API for multiple providers while centralizing tracing and metrics. It supports simple YAML configuration as well as a PostgreSQL-backed database mode with a management API for dynamic provider, model, and pipeline configuration.

**Pros**

- Apache-2.0 licensed.
- Rust implementation with async runtime and a gateway-focused deployment model.
- Built-in OpenTelemetry tracing, Prometheus metrics, health checks, Docker, Helm, and Kubernetes deployment assets.
- Supports providers such as OpenAI, Anthropic, Azure OpenAI, Google Vertex AI, and AWS Bedrock.
- Useful when gateway observability and operational telemetry are first-class requirements.

**Cons**

- Provider coverage is narrower than broad aggregation gateways such as LiteLLM, Portkey Gateway, or New API.
- Database mode introduces PostgreSQL, migrations, and a separate management API surface.
- Teams should validate pipeline behavior, hot reload, and provider compatibility against their production traffic patterns.

### TensorZero

- GitHub: [tensorzero/tensorzero](https://github.com/tensorzero/tensorzero)
- Website: [tensorzero.com](https://tensorzero.com)
- Language: Rust
- License: Apache-2.0

TensorZero is an open-source LLMOps platform that includes an LLM gateway alongside observability, evaluation, optimization, and experimentation workflows. It fits teams that want model access to connect directly with feedback loops and systematic prompt/model improvement.

**Pros**

- Apache-2.0 licensed.
- Rust implementation is attractive for teams that want a compiled gateway layer.
- Connects gateway traffic with evaluation, experimentation, and optimization workflows.

**Cons**

- Broader than a narrow provider proxy, so adoption may involve more workflow and data-model decisions.
- Best fit depends on whether the team wants TensorZero's LLMOps platform concepts, not only routing and fallback behavior.

## Choosing a Gateway

| If you need... | Start with |
| --- | --- |
| Maximum provider breadth and OpenAI-compatible routing | LiteLLM |
| Guardrails and app-team-friendly model routing | Portkey Gateway |
| A local or hosted AI router for many coding tools and provider accounts | OmniRoute |
| Kubernetes-native AI traffic management on Envoy | Envoy AI Gateway |
| API gateway plus AI gateway in a cloud-native stack | Higress |
| Lightweight self-hosted proxy for hosted and local providers | Inference Gateway |
| Lightweight Go gateway with OpenAI-compatible APIs, provider passthrough, and cost visibility | GoModel |
| Go gateway with virtual model routes, failover, and Kubernetes deployment options | pLLM |
| Self-hosted model hub with key/quota management and cross-format APIs | New API |
| Dashboard-backed OpenAI-compatible gateway with key management and usage analytics | LLM Gateway |
| OpenAI-compatible routing to private inference backends across NAT or private networks | OpenZiti LLM Gateway |
| Enterprise API management plus AI gateway features | Kong Gateway |
| Apache API gateway maturity with AI gateway direction | Apache APISIX |
| Low-overhead Go model routing and gateway performance | Bifrost |
| OpenAI-compatible gateway traffic with built-in tracing and Prometheus metrics | Traceloop Hub |
| Gateway traffic connected to evaluation and experimentation loops | TensorZero |

## Evaluation Criteria

When comparing LLM gateways, check:

- Provider coverage and OpenAI-compatible API support.
- API format conversion needs, such as OpenAI-compatible, Claude-compatible, Gemini-compatible, or Responses API translation.
- Routing, fallback, retry, and load-balancing behavior.
- Streaming support and latency overhead.
- Authentication, key management, budget controls, and tenant isolation.
- Logging, tracing, metrics, and cost attribution.
- Guardrails, prompt/message policies, PII handling, and auditability.
- Deployment model: library, sidecar, proxy, Kubernetes gateway, or full API gateway.
- License and commercial-use constraints.

## Contributing

Contributions are welcome. Please keep entries factual and useful:

- Link to the GitHub repository.
- Include license, primary language, and deployment model when known.
- Describe strengths and trade-offs, not only marketing claims.
- Prefer open-source projects with public code.
- Add comparison notes when a project overlaps with existing entries.

## License

This list is released under [CC0-1.0](LICENSE).
