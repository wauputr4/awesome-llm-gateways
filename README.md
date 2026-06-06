# Awesome LLM Gateways

[![Awesome](https://awesome.re/badge-flat2.svg)](https://awesome.re)
[![License: CC0-1.0](https://img.shields.io/badge/license-CC0--1.0-lightgrey.svg)](LICENSE)

A curated comparison of **open-source LLM gateways**, **AI gateways**, **model routers**, and **provider proxies**.

**LLM gateways** sit between applications and model providers. They usually handle **provider routing**, **retries/fallbacks**, **observability**, **budgets**, **access control**, **guardrails**, and **OpenAI-compatible API translation**.

<a id="how-to-use-this-list"></a>

## 🚀 How to Use This List

- **Start with [Quick Comparison](#quick-comparison)** for a fast scan of fit, strengths, and trade-offs.
- **Use [Choosing a Gateway](#choosing-a-gateway)** when you already know the capability you need.
- **Read [Evaluation Criteria](#evaluation-criteria)** before testing a gateway in production.
- **Check each upstream repository** for current license, maturity, deployment docs, and provider behavior before adopting it.

<a id="scope"></a>

## 🎯 Scope

This list prioritizes public repositories that act as **gateway**, **proxy**, **router**, or **model-access infrastructure** rather than generic SDKs or end-user AI applications.

<details>
<summary>🔎 Related terms and search keywords</summary>

AI gateway, model gateway, inference gateway, agent gateway, agentic AI gateway, agentic data-plane gateway, bidirectional AI gateway, spec-first AI gateway, OpenAPI-to-MCP gateway, MCP gateway, A2A gateway, LLM proxy, AI reverse proxy, model router, semantic router, prompt-routing AI gateway, adaptive-routing LLM gateway, zero-trust LLM gateway, coding-tool AI router, OpenAI-compatible proxy, Anthropic-compatible proxy, OpenAI Responses API proxy, OpenTelemetry LLM gateway, observability-first AI gateway, provider passthrough gateway, streaming-optimized AI gateway, semantic-cache AI gateway, dashboard-backed LLM gateway, key-management LLM gateway, API-key-rotation LLM gateway, quota-management AI gateway, budget-aware AI router, spend-cap LLM gateway, Python/FastAPI LLM proxy, importable LLM gateway, LLM API redistribution gateway, and API format-conversion gateway.

</details>

## 📚 Contents

- [🚀 How to Use This List](#how-to-use-this-list)
- [🎯 Scope](#scope)
- [⚡ Quick Comparison](#quick-comparison)
- [🧩 Projects](#projects)
- [🧭 Choosing a Gateway](#choosing-a-gateway)
- [✅ Evaluation Criteria](#evaluation-criteria)
- [🤝 Contributing](#contributing)

<a id="quick-comparison"></a>

## ⚡ Quick Comparison

Use this table for **first-pass filtering**. The detailed project notes below add **context**, **caveats**, and **adoption checks**.

| Project | Repo | Language | License | Best Fit | Main Strengths | Trade-offs |
| --- | --- | --- | --- | --- | --- | --- |
| LiteLLM | [BerriAI/litellm](https://github.com/BerriAI/litellm) | Python | Other | Teams that want broad provider coverage and OpenAI-compatible routing quickly. | 100+ provider support, cost tracking, load balancing, logging, guardrails, active ecosystem. | Python proxy can be less attractive for very low-latency edge gateway use cases; license should be reviewed before commercial embedding. |
| Portkey Gateway | [Portkey-AI/gateway](https://github.com/Portkey-AI/gateway) | TypeScript | MIT | Product teams that want gateway plus guardrails and model routing. | Friendly API, many model integrations, guardrails, model router, permissive license. | Some ecosystem value is tied to Portkey's broader platform; self-hosting depth should be checked for each feature. |
| Helicone AI Gateway | [Helicone/ai-gateway](https://github.com/Helicone/ai-gateway) | Rust | GPL-3.0 | Teams that want a lightweight OpenAI-compatible gateway with routing, rate limits, cache, and observability hooks. | Rust implementation, 100+ model/provider positioning, smart routing, fallbacks, rate limits, caching, Docker/self-hosting, and Helicone/OpenTelemetry observability. | License metadata should be reviewed because the repository sidebar and README text differ; some observability value is tied to the Helicone ecosystem. |
| OmniRoute | [diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute) | TypeScript | MIT | Developers and teams that want a local or hosted AI router for many providers and coding tools through one endpoint. | OpenAI-compatible APIs, broad provider catalog, multiple routing strategies, automatic fallback, prompt compression, format translation, MCP/A2A integrations, and desktop/PWA options. | Very broad product surface with marketing-heavy claims; production maturity, security model, and provider behavior should be validated carefully. |
| A3M Router | [Das-rebel/a3m-router](https://github.com/Das-rebel/a3m-router) | TypeScript | MIT | Teams that want parallel multi-LLM execution with confidence scoring and a small footprint. | Parallel ensemble routing, 47+ providers, semantic cache, circuit breaker, guardrails, and RouteLLM-style classification signals. | Newer project with a smaller community; benchmark, cost-savings, and routing-accuracy claims should be validated on your own workloads. |
| NadirClaw | [NadirRouter/NadirClaw](https://github.com/NadirRouter/NadirClaw) | Python | MIT | Developers who want a local OpenAI/Anthropic-compatible router that sends simple prompts to cheaper or local models. | Prompt-complexity routing, coding-tool compatibility, OpenAI and Anthropic API surfaces, fallback chains, streaming, cost tracking, budgets, caching, dashboard, and Docker support. | Cost-savings claims and classifier accuracy should be validated on real workloads; local-first routing is less suited to teams that need centralized multi-tenant governance out of the box. |
| Agentgateway | [agentgateway/agentgateway](https://github.com/agentgateway/agentgateway) | Rust | Apache-2.0 | Platform teams that need agentic AI traffic governance across LLM, MCP, and A2A flows. | OpenAI-compatible LLM routing, MCP and A2A gateway support, budget/spend controls, prompt enrichment, load balancing, failover, guardrails, auth/RBAC, OpenTelemetry, and Kubernetes options. | Broader agentic-proxy scope than a narrow model gateway; teams should validate LLM provider behavior and operational maturity against their own agent/tool traffic. |
| Plano | [katanemo/plano](https://github.com/katanemo/plano) | Rust | Apache-2.0 | Teams building agentic applications that want an out-of-process proxy/data plane for orchestration, LLM routing, safety, and traces. | Envoy-rooted Rust proxy, OpenAI-compatible agent endpoints, semantic model aliases/preferences, guardrail filter chains, OpenTelemetry traces/metrics, YAML configuration, and hosted/local routing-model options. | Broader agentic-app platform than a minimal provider proxy; teams should validate routing-model dependency, local model setup, and production operations for their deployment. |
| Barbacane | [barbacane-dev/barbacane](https://github.com/barbacane-dev/barbacane) | Rust | AGPL-3.0 | API/platform teams that want one spec-first gateway for outbound LLM calls and inbound MCP tool exposure. | OpenAI-compatible LLM dispatcher, OpenAI/Anthropic/Ollama provider fallback, OpenAPI-to-MCP exposure, token limits, prompt/response guards, Prometheus metrics, OpenTelemetry traces, and WASM plugin model. | AGPL/commercial licensing needs review; broader API gateway architecture may be more than teams need for a simple provider proxy. |
| Envoy AI Gateway | [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway) | Go | Apache-2.0 | Kubernetes/cloud-native teams already aligned with Envoy Gateway. | Built on Envoy Gateway, strong infrastructure pedigree, Kubernetes-native direction. | Younger project than general API gateways; feature surface may be narrower for app-level LLMOps needs. |
| Higress | [higress-group/higress](https://github.com/higress-group/higress) | Go | Apache-2.0 | Cloud-native API gateway users that want AI gateway capability in one gateway. | AI-native API gateway, Envoy-based, API gateway plus AI routing patterns. | Operational model is closer to full API gateway infrastructure than lightweight app proxy. |
| Inference Gateway | [inference-gateway/inference-gateway](https://github.com/inference-gateway/inference-gateway) | Go | MIT | Teams that want a self-hosted, lightweight gateway for multiple hosted and local providers. | Unified proxy for providers such as OpenAI, Ollama, Groq, Cohere, Anthropic, Cloudflare, and DeepSeek; streaming, MCP, OpenTelemetry, Docker, and Kubernetes support. | Routing model is environment/configuration driven; teams needing advanced policy, spend governance, or full API management may need surrounding tooling. |
| GoModel | [ENTERPILOT/GoModel](https://github.com/ENTERPILOT/GoModel) | Go | MIT | Teams that want a lightweight Go gateway with OpenAI-compatible APIs, provider passthrough, and built-in usage visibility. | Supports many hosted and local providers, streaming, Responses API, embeddings, files, batches, observability, guardrails, cost tracking, Docker, and config/env-based setup. | Newer project with a fast-changing provider matrix; advanced enterprise policy and governance depth should be validated against the exact deployment. |
| Proxify | [poixeai/proxify](https://github.com/poixeai/proxify) | TypeScript / Go | MIT | Teams that want a self-hosted AI API reverse proxy with simple path-based upstream routing and streaming optimizations. | Route-prefix forwarding for OpenAI, Claude, Gemini, DeepSeek, Azure, and other HTTP APIs; stream smoothing, heartbeat keepalive, tail acceleration, hot-reloaded routes, Docker, and dashboard. | More reverse proxy than full LLMOps gateway; advanced model policy, budget controls, and provider-native format conversion need external tooling or custom routing rules. |
| LLM0 Gateway | [mrmushfiq/llm0-gateway](https://github.com/mrmushfiq/llm0-gateway) | Go | MIT | Teams that want a self-hosted Go gateway with failover, semantic caching, and spend controls. | OpenAI-compatible chat endpoint, OpenAI/Anthropic/Gemini/Ollama routing, exact and semantic caching, streaming, per-key rate limits, per-customer spend caps, cost tracking, Redis/Postgres, and Docker Compose. | Very new project with a small community; provider coverage is narrower than broad aggregators and semantic caching adds Postgres/pgvector plus embedding-service operations. |
| pLLM | [andreimerfu/pllm](https://github.com/andreimerfu/pllm) | Go | MIT | Teams that want a Go OpenAI-compatible gateway with route-based model orchestration, failover, and Kubernetes deployment options. | Virtual model routes, priority/latency/weighted/random routing, automatic failover, multi-key load balancing, caching, budgets, Prometheus metrics, OpenTelemetry, Docker, and Helm support. | Newer project with ambitious production claims; teams should validate provider adapters, dashboard/admin APIs, and Redis-backed routing behavior against real traffic. |
| New API | [QuantumNous/new-api](https://github.com/QuantumNous/new-api) | Go | AGPL-3.0 | Teams that need a self-hosted AI model hub for key management, quota controls, and cross-format gateway APIs. | OpenAI-compatible, Claude-compatible, Gemini-compatible, Responses, realtime, image/audio/embedding/rerank interfaces, weighted routing, retries, user-level rate limits, Docker, and web UI. | AGPL licensing must be reviewed for hosted/commercial deployments; broad model and billing surface adds more operational policy decisions than a minimal proxy. |
| One API | [songquanpeng/one-api](https://github.com/songquanpeng/one-api) | JavaScript / Go | MIT | Teams that want a self-hosted OpenAI-format API management and key redistribution gateway with broad provider channels. | OpenAI-format access to many providers, channel management, load balancing, token quotas, user groups, model mapping, retries, Docker images, web UI, and multi-node deployment guidance. | GitHub language metadata is frontend-heavy; default credentials and public-service compliance warnings require careful deployment hardening, and format conversion is centered on OpenAI-style APIs. |
| Squirrel | [mylxsw/llm-gateway](https://github.com/mylxsw/llm-gateway) | Python | MIT* | Teams that want a Python/FastAPI LLM gateway with a dashboard, protocol conversion, and cost-aware routing. | OpenAI, OpenAI Responses, and Anthropic-compatible APIs, rule-based routing, cost/priority/weight load balancing, retries, failover, request logging, cost analytics, Docker, SQLite/PostgreSQL, and Next.js dashboard. | GitHub license metadata is not populated even though the README badge says MIT; newer project with dashboard and storage dependencies, so teams should validate data retention, auth, and production hardening. |
| LM-Proxy | [Nayjest/lm-proxy](https://github.com/Nayjest/lm-proxy) | Python | MIT | Developers that want a small OpenAI-compatible Python/FastAPI proxy usable as a library or standalone service. | Provider-agnostic OpenAI-format endpoint, model-pattern routing, streaming, virtual API keys, group access controls, TOML/YAML/JSON/Python config, and Google/Anthropic/OpenAI/local inference support. | Smaller project and narrower operational surface than full LLM gateway platforms; advanced governance, dashboards, and production hardening need surrounding tooling or custom extensions. |
| LLM API Key Proxy | [Mirrowel/LLM-API-Key-Proxy](https://github.com/Mirrowel/LLM-API-Key-Proxy) | Python | Unknown | Teams that want an OpenAI/Anthropic-compatible proxy with provider translation, API-key rotation, and failover. | FastAPI proxy, OpenAI and Anthropic endpoints, Gemini/OpenAI/Anthropic/LiteLLM-backed providers, intelligent key rotation, load balancing, resilience library, and Docker deployment. | GitHub license metadata is not populated; provider translation and key-rotation behavior should be validated carefully before sharing credentials or using it in multi-tenant production. |
| LLM Gateway | [theopenco/llmgateway](https://github.com/theopenco/llmgateway) | TypeScript | AGPL-3.0 / Enterprise | Teams that want a self-hosted or hosted OpenAI-compatible gateway with dashboard analytics and key management. | Multi-provider routing, centralized provider keys, usage/cost analytics, performance monitoring, Docker self-hosting, and dashboard/playground apps. | Dual-license model needs review; the monorepo includes several apps and backing services, so it is heavier than a minimal proxy. |
| Routerly | [Inebrio/Routerly](https://github.com/Inebrio/Routerly) | TypeScript | AGPL-3.0 | Teams that want a self-hosted OpenAI/Anthropic-compatible router with cost controls and project isolation. | Multi-policy routing, native OpenAI and Anthropic API compatibility, local/provider model targets, cost tracking, budget enforcement, project tokens, CLI, dashboard, and Docker support. | AGPL licensing needs review; newer project with a smaller community, and LLM-powered routing claims should be validated against real traffic. |
| OpenZiti LLM Gateway | [openziti/llm-gateway](https://github.com/openziti/llm-gateway) | Go | Apache-2.0 | Teams that need OpenAI-compatible routing across hosted and private inference backends, especially behind NAT or private networks. | Single Go binary, semantic routing, Anthropic translation, virtual API keys, OpenTelemetry metrics, multi-endpoint load balancing, and zrok/OpenZiti zero-trust connectivity. | Early-stage project with a smaller community; provider coverage is narrower than broad aggregation gateways and strongest when its zero-trust networking model is needed. |
| Kong Gateway | [Kong/kong](https://github.com/Kong/kong) | Lua | Apache-2.0 | Enterprises that already need API management and want AI gateway features in the same stack. | Mature API gateway, plugin ecosystem, AI gateway and MCP-related features, strong operations story. | Heavier than purpose-built LLM proxies; some advanced workflows may depend on Kong ecosystem/product choices. |
| Apache APISIX | [apache/apisix](https://github.com/apache/apisix) | Lua | Apache-2.0 | Teams that want a cloud-native API gateway with AI gateway capabilities. | Mature Apache project, API management, Kubernetes ingress, plugins, AI gateway direction. | More general-purpose API gateway than LLM-specific proxy; AI workflows may need plugin/configuration work. |
| Bifrost | [maximhq/bifrost](https://github.com/maximhq/bifrost) | Go | Apache-2.0 | Teams optimizing for low-overhead model routing and gateway performance. | Go implementation, model routing, load balancing, guardrails, observability and cost-oriented features. | Younger than established API gateways; benchmark and feature claims should be verified in your workload. |
| Traceloop Hub | [traceloop/hub](https://github.com/traceloop/hub) | Rust | Apache-2.0 | Teams that want a high-performance OpenAI-compatible gateway with tracing and metrics built in. | Rust gateway, OpenAI-compatible API, OpenTelemetry tracing, Prometheus metrics, YAML mode, PostgreSQL-backed management mode, and Kubernetes assets. | Provider coverage is narrower than broad aggregators; database mode adds PostgreSQL and management API operations. |
| TensorZero | [tensorzero/tensorzero](https://github.com/tensorzero/tensorzero) | Rust | Apache-2.0 | Teams that want an LLM gateway tied to observability, evaluation, optimization, and experimentation. | Rust gateway, model access layer, feedback/evaluation loop, experimentation-oriented LLMOps platform. | Broader platform than a standalone proxy; teams should confirm they want the surrounding LLMOps workflow, not only request routing. |
| CoderPlan | [coderplan.ai](https://coderplan.ai) | — | Proprietary | Developers who want a managed LLM API gateway for Claude Code, Cursor, Codex CLI, and Gemini CLI without self-hosting. | OpenAI-compatible API, pay-per-use, Claude/GPT/Gemini/DeepSeek models, Hong Kong/Singapore edge nodes, free credits for new users. | Hosted service (not open-source); best fit for developers who want zero-config API access rather than self-hosted gateway infrastructure. |
| FerryAPI | [ferryapi.io](https://www.ferryapi.io/) | — | Proprietary | Developers and teams that want a managed OpenAI-compatible gateway for production apps. | OpenAI-compatible API, public docs, prepaid usage-based billing, developer API key management, and lower-cost model access positioning. | Hosted service (not open-source); teams should validate pricing, provider behavior, data handling, and service terms before adoption. |

<a id="projects"></a>

## 🧩 Projects

Each entry follows the same shape: **repository links first**, then a short factual summary, practical **pros**, and adoption **caveats**.

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

### Helicone AI Gateway

- GitHub: [Helicone/ai-gateway](https://github.com/Helicone/ai-gateway)
- Website/docs: [docs.helicone.ai/ai-gateway](https://docs.helicone.ai/gateway)
- Language: Rust
- License: GPL-3.0

Helicone AI Gateway is a Rust-based OpenAI-compatible gateway for routing requests across many LLM providers. It focuses on a lightweight proxy path with smart routing, load balancing, rate limits, caching, fallbacks, self-hosting, and observability through Helicone and OpenTelemetry.

**Pros**

- Rust implementation with a self-hostable gateway package.
- OpenAI-compatible client path for many providers and models.
- Supports routing strategies, provider load balancing, fallbacks, rate limits, and cache configuration.
- Useful when gateway traffic should connect directly to observability, monitoring, and debugging workflows.

**Cons**

- GitHub license metadata currently shows GPL-3.0 while the README license text references Apache License, so teams should verify the effective license before adoption.
- Some observability and management workflows are naturally strongest when used with the broader Helicone platform.
- Provider breadth, latency, and cache claims should be validated against the exact self-hosted or cloud-hosted deployment.

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

### A3M Router

- GitHub: [Das-rebel/a3m-router](https://github.com/Das-rebel/a3m-router)
- npm: [adaptive-memory-multi-model-router](https://www.npmjs.com/package/adaptive-memory-multi-model-router)
- Language: TypeScript
- License: MIT

A3M Router is an open-source TypeScript LLM router and AI gateway that can run providers in parallel, score responses, and return the best answer with transparent routing context. It routes across many provider targets using RouteLLM-style classification signals to balance cost, capability, and response quality.

**Pros**

- MIT licensed.
- Parallel multi-LLM execution and ensemble-style response scoring are useful when answer quality matters more than a single sequential fallback path.
- OpenAI-compatible proxy surface can fit existing SDKs and coding-tool workflows.
- Lightweight TypeScript package with semantic cache, circuit breaker, prompt-injection guardrail patterns, budget enforcement, and episodic-memory positioning.

**Cons**

- Newer project with a smaller community and emerging production track record.
- Published cost-savings, benchmark, and routing-accuracy claims should be validated against real traffic before adoption.
- TypeScript/Node.js-focused project, so teams standardized on Python, Go, Rust, or Kubernetes-native gateways may prefer other options.

### NadirClaw

- GitHub: [NadirRouter/NadirClaw](https://github.com/NadirRouter/NadirClaw)
- Website: [getnadir.com](https://getnadir.com)
- Language: Python
- License: MIT

NadirClaw is a local-first LLM router and cost optimizer for coding tools and OpenAI-compatible clients. It classifies prompts by complexity, routes simple requests to cheaper or local models, keeps complex work on stronger models, and exposes OpenAI-compatible chat completions plus an Anthropic-compatible messages endpoint.

**Pros**

- MIT licensed.
- Strong fit for Claude Code, Codex, Cursor, Continue, OpenClaw, and other tools that can point at an OpenAI-compatible base URL.
- Includes prompt-complexity routing, fallback chains, streaming, session persistence, context-window filtering, cost tracking, budget controls, caching, Prometheus metrics, OpenTelemetry tracing, a dashboard, and Docker deployment.
- Local deployment keeps provider keys under the operator's control and can route directly to Gemini, OpenAI, Anthropic, Ollama, and LiteLLM-supported providers.

**Cons**

- Cost-savings and classifier-accuracy claims should be measured against the team's own prompts, providers, and quality thresholds.
- Local-first design is useful for individuals and small teams, but centralized team governance is positioned more strongly in the hosted/pro version.
- Broader feature surface and coding-tool focus may be unnecessary for teams that only need a simple provider reverse proxy.

### Agentgateway

- GitHub: [agentgateway/agentgateway](https://github.com/agentgateway/agentgateway)
- Website/docs: [agentgateway.dev](https://agentgateway.dev)
- Language: Rust
- License: Apache-2.0

Agentgateway is a Rust-based agentic AI proxy for LLM, MCP, and A2A traffic. Its LLM gateway path routes traffic to providers such as OpenAI, Anthropic, Gemini, and Bedrock through a unified OpenAI-compatible API, while adding budget and spend controls, prompt enrichment, load balancing, failover, guardrails, authentication, RBAC, and OpenTelemetry-based observability.

**Pros**

- Apache-2.0 licensed.
- Strong fit for teams that need one gateway layer for agent-to-LLM, agent-to-tool, and agent-to-agent communication.
- Includes LLM gateway, MCP gateway, A2A gateway, guardrails, auth, rate limiting, TLS, and OpenTelemetry features.
- Supports both standalone quickstart and Kubernetes/Gateway API deployment paths.

**Cons**

- Broader agentic-proxy scope may be unnecessary for teams that only need a small OpenAI-compatible provider proxy.
- Project is moving quickly, so exact LLM provider behavior, controller maturity, and policy workflows should be validated before production adoption.
- MCP/A2A and agent governance features add concepts and operations beyond traditional model routing.

### Plano

- GitHub: [katanemo/plano](https://github.com/katanemo/plano)
- Website/docs: [planoai.dev](https://planoai.dev)
- Language: Rust
- License: Apache-2.0

Plano is a Rust-based AI-native proxy server and data plane for agentic applications. It moves agent orchestration, LLM routing, guardrail filter chains, and OpenTelemetry-based traces and metrics into an out-of-process layer configured with YAML, with OpenAI-compatible agent endpoints and model routing by provider model name, semantic alias, or preferences.

**Pros**

- Apache-2.0 licensed public repository with active development and clear AI gateway/proxy positioning.
- Built around Envoy-style proxy infrastructure and agentic traffic patterns rather than only SDK-level provider calls.
- Supports declarative agent routing, model-provider configuration, filter chains for moderation/safety hooks, and automatic observability.
- Useful when agent services should stay framework-agnostic while sharing routing, safety, and tracing infrastructure.

**Cons**

- Broader agentic-app platform than a narrow OpenAI-compatible provider proxy, so it may be unnecessary for simple key aggregation.
- Some routing examples depend on Plano-hosted or local purpose-built routing models; teams should validate that dependency and local model setup before production use.
- Agent orchestration, filters, and tracing introduce more operational concepts than a minimal reverse proxy.

### Barbacane

- GitHub: [barbacane-dev/barbacane](https://github.com/barbacane-dev/barbacane)
- Website/docs: [barbacane.dev](https://barbacane.dev)
- Language: Rust
- License: AGPL-3.0

Barbacane is a Rust-based bidirectional AI gateway for teams that want one spec-first layer for outbound LLM calls and inbound agent tool exposure. Its AI proxy dispatcher exposes an OpenAI-compatible API surface for providers such as OpenAI, Anthropic, and Ollama, while its OpenAPI extensions can expose existing API operations as MCP tools with the same auth, validation, rate-limit, and observability middleware.

**Pros**

- Public Rust repository with clear AI gateway, OpenAPI, AsyncAPI, and MCP positioning.
- Combines outbound LLM routing with inbound OpenAPI-to-MCP exposure, which is useful for teams governing both app-to-model and agent-to-tool traffic.
- Supports provider fallback, policy-driven targets, prompt and response guards, token limits, cost metrics, Prometheus, OpenTelemetry, and WASM plugin extensibility.
- Spec-first design can reduce drift between API contracts, gateway policy, and agent-visible tools.

**Cons**

- AGPL-3.0/commercial licensing should be reviewed before hosted or embedded commercial deployment.
- Broader API gateway and control/data-plane architecture can be heavier than a minimal OpenAI-compatible proxy.
- Teams should validate provider translation, MCP exposure boundaries, and fail-closed guardrail behavior against their exact OpenAPI specs.

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

### Proxify

- GitHub: [poixeai/proxify](https://github.com/poixeai/proxify)
- Website/demo: [proxify.poixe.com](https://proxify.poixe.com)
- Language: TypeScript / Go
- License: MIT

Proxify is a self-hosted AI API reverse proxy gateway. It routes provider traffic through path prefixes such as `/openai`, `/gemini`, `/claude`, and `/deepseek`, keeps request parameters and upstream API keys compatible with existing clients, and adds LLM streaming optimizations such as smoothing, heartbeat keepalive, and tail acceleration.

**Pros**

- MIT licensed.
- Practical for teams that mainly need one self-hosted reverse proxy entry point in front of multiple AI API upstreams.
- Supports route configuration with hot reload, model-field rewriting, Docker deployment, token or IP-based access controls, and a dashboard.
- Streaming-specific behavior can help with long-running SSE responses and idle timeout handling.

**Cons**

- More of a configurable reverse proxy than a full model-routing or LLMOps gateway.
- Advanced budget enforcement, tenant isolation, provider health scoring, and observability need surrounding tooling or custom extensions.
- API compatibility depends on upstream route configuration, so teams should test each provider path and model rewrite rule directly.

### LLM0 Gateway

- GitHub: [mrmushfiq/llm0-gateway](https://github.com/mrmushfiq/llm0-gateway)
- Website/docs: [llm0.ai](https://www.llm0.ai/)
- Language: Go
- License: MIT

LLM0 Gateway is a self-hosted Go gateway that exposes an OpenAI-compatible chat-completions endpoint across OpenAI, Anthropic, Gemini, and local Ollama backends. It focuses on cloud/local failover, exact and semantic caching, streaming, rate limits, per-customer spend caps, and request cost tracking.

**Pros**

- MIT licensed.
- Single Go gateway with Docker Compose support and OpenAI-compatible client integration.
- Supports provider failover modes across cloud and local Ollama targets.
- Includes exact cache, semantic cache, per-key rate limits, customer/project spend controls, usage headers, and request logging.
- Useful when cost controls and cache behavior are first-class requirements in a small self-hosted gateway.

**Cons**

- Very new project with a small public community, so production maturity and long-term maintenance should be validated carefully.
- Provider coverage is narrower than broad aggregators such as LiteLLM or Portkey Gateway.
- Semantic caching depends on Postgres/pgvector and an embedding service, which adds operational moving parts beyond a minimal reverse proxy.
- Current support is centered on chat-completions style traffic rather than a full cross-format model API surface.

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

### One API

- GitHub: [songquanpeng/one-api](https://github.com/songquanpeng/one-api)
- Language: JavaScript / Go
- License: MIT

One API is a self-hosted LLM API management and redistribution gateway for exposing many model providers through a standard OpenAI-style API surface. It supports provider channel management, load balancing, streaming, token and quota controls, user groups, model mapping, retry behavior, Docker images, multi-node deployment guidance, and a web management UI.

**Pros**

- MIT licensed.
- Mature public repository with a large community and active updates.
- Supports many hosted and local model channels, including OpenAI, Azure OpenAI, Anthropic Claude, Gemini, Mistral, Groq, Ollama, Cohere, DeepSeek, Cloudflare Workers AI, xAI, and several China-market providers.
- Includes practical gateway administration features such as token expiry, quotas, allowed IP ranges, allowed model access, user/channel groups, model mapping, quota details, management API access, and Docker deployment.
- Useful when a team needs one self-hosted control panel for internal key distribution and OpenAI-format provider access.

**Cons**

- GitHub primary-language metadata currently reports JavaScript, while the project also ships a Go backend and single-binary deployment path; teams should inspect the stack directly.
- The README warns about default root credentials and regional public-service compliance, so production deployments need careful hardening and policy review.
- API unification is centered on OpenAI-style access; teams needing deep native Claude, Gemini, Responses API, or bidirectional format conversion should validate exact compatibility.
- Broad user, quota, channel, and UI management features make it heavier than a minimal reverse proxy.

### Squirrel

- GitHub: [mylxsw/llm-gateway](https://github.com/mylxsw/llm-gateway)
- Language: Python
- License: MIT according to README badge; GitHub license metadata is not populated

Squirrel is a Python/FastAPI LLM gateway for unifying access to OpenAI, Anthropic, and compatible providers. It exposes OpenAI-compatible, OpenAI Responses-compatible, and Anthropic-compatible APIs, with protocol conversion, model mapping, rule-based routing, provider failover, request logging, cost analytics, and a Next.js management dashboard.

**Pros**

- Public repository with clear gateway/proxy positioning and recent activity.
- Supports OpenAI Chat, OpenAI Responses, Anthropic Messages, streaming, embeddings, audio, and image-style API paths.
- Includes routing strategies for rule, priority, weight, and cost-aware provider selection.
- Dashboard covers providers, model mapping, API keys, logs, and usage/cost analytics.
- Docker deployment supports a simple SQLite path and a PostgreSQL-backed production-style path.

**Cons**

- GitHub does not currently expose license metadata for the repo, so teams should verify the effective MIT license before adoption.
- Python backend plus Next.js dashboard is heavier than a minimal single-binary gateway.
- Full request/response logging can be useful for debugging but needs careful retention, redaction, and privacy configuration in production.
- Newer project with a smaller ecosystem than LiteLLM, Portkey Gateway, or established API gateways.

### LM-Proxy

- GitHub: [Nayjest/lm-proxy](https://github.com/Nayjest/lm-proxy)
- Language: Python
- License: MIT

LM-Proxy is a lightweight Python/FastAPI HTTP proxy that exposes an OpenAI-compatible endpoint across providers such as OpenAI, Anthropic, Google AI, Google Vertex AI, and local PyTorch-based inference. It can run as a standalone service or be imported as a Python library, with model-pattern routing, streaming, virtual API key checks, access groups, and TOML/YAML/JSON/Python configuration.

**Pros**

- MIT licensed.
- Small Python/FastAPI gateway with a familiar OpenAI-compatible client surface.
- Supports provider-agnostic routing by model pattern, streaming responses, virtual API keys, group-based access controls, and environment-backed upstream keys.
- Useful when teams want a simple configurable proxy that can also be embedded in Python applications.

**Cons**

- Smaller public project than broad gateway platforms such as LiteLLM, Portkey Gateway, or New API, so production maturity should be validated carefully.
- Provider coverage and routing policy depth are narrower than larger multi-provider gateways.
- Does not center dashboards, budget enforcement, or enterprise governance; teams needing those workflows may need additional tooling.

### LLM API Key Proxy

- GitHub: [Mirrowel/LLM-API-Key-Proxy](https://github.com/Mirrowel/LLM-API-Key-Proxy)
- Language: Python
- License: Unknown

LLM API Key Proxy is a Python/FastAPI proxy and resilience library that exposes OpenAI-compatible and Anthropic-compatible endpoints across configured providers. It focuses on one endpoint for multiple LLM providers, provider translation, intelligent API-key rotation, load balancing, failover, and compatibility with clients that support custom OpenAI or Anthropic base URLs.

**Pros**

- Public repository with clear universal LLM gateway and proxy positioning.
- OpenAI and Anthropic-compatible API surfaces can reduce client changes for tools that support custom base URLs.
- Includes provider translation, key rotation, failover, load balancing, and a reusable resilience library.
- Docker-based deployment path is useful for local or small self-hosted setups.

**Cons**

- GitHub does not currently expose license metadata for the repo, so teams should verify the effective license before adoption.
- Key rotation across many upstream credentials can become a security and policy risk without careful secret management and audit controls.
- More focused on proxy resilience than broader gateway governance; budgets, tenant isolation, dashboards, and enterprise policy may require additional tooling.

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

### Routerly

- GitHub: [Inebrio/Routerly](https://github.com/Inebrio/Routerly)
- Website/docs: [routerly.ai](https://www.routerly.ai/)
- Language: TypeScript
- License: AGPL-3.0

Routerly is a self-hosted LLM gateway for routing requests across providers such as OpenAI, Anthropic, Gemini, Mistral, Ollama, xAI, Cohere, and custom HTTP backends. It exposes OpenAI-compatible and Anthropic-compatible APIs, with project-scoped tokens, routing policies, cost tracking, budget enforcement, a CLI, dashboard, and Docker deployment path.

**Pros**

- Native OpenAI and Anthropic API compatibility is useful for teams that do not want every client forced through one API shape.
- Multi-policy routing covers cost, health, performance, capability, context, budget, rate-limit, fairness, and optional LLM-assisted selection.
- Project isolation, budget limits, and usage reporting are built into the gateway rather than delegated entirely to external tooling.
- Runs self-hosted without requiring PostgreSQL or Redis for the core gateway path.

**Cons**

- AGPL-3.0 licensed, so hosted, modified, or commercial deployment plans need license review.
- Smaller and newer than established gateway projects, with fewer third-party production references.
- LLM-assisted routing can add another model call and policy failure mode; teams should benchmark deterministic and LLM-powered routing separately.
- Provider coverage is narrower than broad aggregators such as LiteLLM, Portkey Gateway, or New API.

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

### CoderPlan

- Website: [coderplan.ai](https://coderplan.ai)
- Language: Not applicable for the hosted service
- License: Proprietary

CoderPlan is a managed LLM API gateway for developers who want one hosted endpoint for Claude Code, Cursor, Codex CLI, Gemini CLI, and similar coding tools without running their own gateway infrastructure.

**Pros**

- OpenAI-compatible API surface for common coding tools and CLI workflows.
- Provides access to Claude, GPT, Gemini, DeepSeek, and related model options through a pay-per-use service.
- Hosted deployment can be useful when zero-config onboarding matters more than self-hosted control.

**Cons**

- Proprietary hosted service rather than an open-source repository, so teams should validate pricing, data handling, and service terms before adoption.
- Less suitable for teams that require self-hosting, source-code review, or full infrastructure control.
- Regional edge-node and model-availability claims should be verified against the current service documentation.

### FerryAPI

- Website: [ferryapi.io](https://www.ferryapi.io/)
- Docs: [ferryapi.io/docs](https://www.ferryapi.io/docs)
- Language: Not applicable for the hosted service
- License: Proprietary

FerryAPI is a managed OpenAI-compatible AI API gateway for production applications. It focuses on hosted gateway access, prepaid usage-based billing, developer API key management, and lower-cost model access rather than self-hosted open-source infrastructure.

**Pros**

- OpenAI-compatible API surface for applications that already use OpenAI-style clients.
- Public documentation and developer setup path.
- Hosted deployment can reduce self-hosting overhead for teams that prefer managed gateway infrastructure.
- Developer API key management and prepaid usage-based billing can be useful for production access control and spend planning.

**Cons**

- Proprietary hosted service rather than an open-source repository, so teams cannot self-host or audit source code from this listing.
- Pricing, provider behavior, model availability, data handling, and service terms should be validated directly before production adoption.
- Less suitable for teams that need Kubernetes-native deployment, private-network routing, or full infrastructure ownership.

<a id="choosing-a-gateway"></a>

## 🧭 Choosing a Gateway

These are **starting points**, not final recommendations. Validate each candidate against your **traffic pattern**, **compliance needs**, and **deployment model**.

| If you need... | Start with |
| --- | --- |
| Maximum provider breadth and OpenAI-compatible routing | LiteLLM |
| Guardrails and app-team-friendly model routing | Portkey Gateway |
| Lightweight Rust gateway with observability, caching, and rate limits | Helicone AI Gateway |
| A local or hosted AI router for many coding tools and provider accounts | OmniRoute |
| Parallel multi-LLM execution with confidence scoring | A3M Router |
| Local prompt-complexity routing for coding tools and cost control | NadirClaw |
| Agentic AI traffic governance across LLM, MCP, and A2A flows | Agentgateway |
| Agentic application data plane with orchestration, guardrails, LLM routing, and traces | Plano |
| Spec-first outbound LLM routing plus inbound OpenAPI-to-MCP tool exposure | Barbacane |
| Kubernetes-native AI traffic management on Envoy | Envoy AI Gateway |
| API gateway plus AI gateway in a cloud-native stack | Higress |
| Lightweight self-hosted proxy for hosted and local providers | Inference Gateway |
| Lightweight Go gateway with OpenAI-compatible APIs, provider passthrough, and cost visibility | GoModel |
| Self-hosted AI API reverse proxy with route-prefix forwarding and streaming optimization | Proxify |
| Self-hosted Go gateway with semantic caching, failover, and spend caps | LLM0 Gateway |
| Go gateway with virtual model routes, failover, and Kubernetes deployment options | pLLM |
| Self-hosted model hub with key/quota management and cross-format APIs | New API |
| Self-hosted OpenAI-format API management and key redistribution gateway | One API |
| Python/FastAPI gateway with dashboard analytics and protocol conversion | Squirrel |
| Small importable or standalone Python/FastAPI OpenAI-compatible proxy | LM-Proxy |
| OpenAI/Anthropic-compatible proxy with API-key rotation and failover | LLM API Key Proxy |
| Dashboard-backed OpenAI-compatible gateway with key management and usage analytics | LLM Gateway |
| Self-hosted OpenAI/Anthropic-compatible routing with project budgets and token isolation | Routerly |
| OpenAI-compatible routing to private inference backends across NAT or private networks | OpenZiti LLM Gateway |
| Enterprise API management plus AI gateway features | Kong Gateway |
| Apache API gateway maturity with AI gateway direction | Apache APISIX |
| Low-overhead Go model routing and gateway performance | Bifrost |
| OpenAI-compatible gateway traffic with built-in tracing and Prometheus metrics | Traceloop Hub |
| Gateway traffic connected to evaluation and experimentation loops | TensorZero |
| Managed coding-tool gateway without self-hosting | CoderPlan |
| Managed OpenAI-compatible gateway for production apps | FerryAPI |

<a id="evaluation-criteria"></a>

## ✅ Evaluation Criteria

Before adopting a gateway, compare candidates across these practical dimensions:

- Provider coverage and OpenAI-compatible API support.
- API format conversion needs, such as OpenAI-compatible, Claude-compatible, Gemini-compatible, or Responses API translation.
- Routing, fallback, retry, and load-balancing behavior.
- Prompt classification, semantic routing, and quality/cost policy behavior.
- Streaming support and latency overhead.
- Authentication, key management, API-key rotation, budget controls, and tenant isolation.
- Logging, tracing, metrics, and cost attribution.
- Guardrails, prompt/message policies, PII handling, and auditability.
- Spec-first gateway needs, including whether OpenAPI/AsyncAPI contracts should drive model routes, agent-visible tools, and policy enforcement.
- Agent orchestration needs, including whether routing should happen between agent services as well as between model providers.
- Deployment model: library, sidecar, proxy, Kubernetes gateway, or full API gateway.
- License and commercial-use constraints.
- Whether GitHub license metadata, README badges, and checked-in license files agree.

<a id="contributing"></a>

## 🤝 Contributing

Contributions are welcome. Please keep entries **factual**, **comparable**, and **easy to scan**:

- Link to the GitHub repository.
- Include license, primary language, and deployment model when known.
- Describe strengths and trade-offs, not only marketing claims.
- Prefer open-source projects with public code.
- Add comparison notes when a project overlaps with existing entries.
- Hosted/SaaS gateways that serve similar routing and access functions are also welcome, noted as such.

<a id="license"></a>

## 📄 License

This list is released under [CC0-1.0](LICENSE).
