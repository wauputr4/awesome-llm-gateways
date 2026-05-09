# Awesome LLM Gateways

A curated list of open-source LLM gateways, AI gateways, and model-routing proxies.

LLM gateways sit between applications and model providers. They usually handle provider routing, retries, fallbacks, observability, budgets, access control, guardrails, and OpenAI-compatible API translation.

Related terms include AI gateway, model gateway, inference gateway, LLM proxy, model router, and OpenAI-compatible proxy.

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
| Envoy AI Gateway | [envoyproxy/ai-gateway](https://github.com/envoyproxy/ai-gateway) | Go | Apache-2.0 | Kubernetes/cloud-native teams already aligned with Envoy Gateway. | Built on Envoy Gateway, strong infrastructure pedigree, Kubernetes-native direction. | Younger project than general API gateways; feature surface may be narrower for app-level LLMOps needs. |
| Higress | [higress-group/higress](https://github.com/higress-group/higress) | Go | Apache-2.0 | Cloud-native API gateway users that want AI gateway capability in one gateway. | AI-native API gateway, Envoy-based, API gateway plus AI routing patterns. | Operational model is closer to full API gateway infrastructure than lightweight app proxy. |
| Inference Gateway | [inference-gateway/inference-gateway](https://github.com/inference-gateway/inference-gateway) | Go | MIT | Teams that want a self-hosted, lightweight gateway for multiple hosted and local providers. | Unified proxy for providers such as OpenAI, Ollama, Groq, Cohere, Anthropic, Cloudflare, and DeepSeek; streaming, MCP, OpenTelemetry, Docker, and Kubernetes support. | Routing model is environment/configuration driven; teams needing advanced policy, spend governance, or full API management may need surrounding tooling. |
| Kong Gateway | [Kong/kong](https://github.com/Kong/kong) | Lua | Apache-2.0 | Enterprises that already need API management and want AI gateway features in the same stack. | Mature API gateway, plugin ecosystem, AI gateway and MCP-related features, strong operations story. | Heavier than purpose-built LLM proxies; some advanced workflows may depend on Kong ecosystem/product choices. |
| Apache APISIX | [apache/apisix](https://github.com/apache/apisix) | Lua | Apache-2.0 | Teams that want a cloud-native API gateway with AI gateway capabilities. | Mature Apache project, API management, Kubernetes ingress, plugins, AI gateway direction. | More general-purpose API gateway than LLM-specific proxy; AI workflows may need plugin/configuration work. |
| Bifrost | [maximhq/bifrost](https://github.com/maximhq/bifrost) | Go | Apache-2.0 | Teams optimizing for low-overhead model routing and gateway performance. | Go implementation, model routing, load balancing, guardrails, observability and cost-oriented features. | Younger than established API gateways; benchmark and feature claims should be verified in your workload. |
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
| Kubernetes-native AI traffic management on Envoy | Envoy AI Gateway |
| API gateway plus AI gateway in a cloud-native stack | Higress |
| Lightweight self-hosted proxy for hosted and local providers | Inference Gateway |
| Enterprise API management plus AI gateway features | Kong Gateway |
| Apache API gateway maturity with AI gateway direction | Apache APISIX |
| Low-overhead Go model routing and gateway performance | Bifrost |
| Gateway traffic connected to evaluation and experimentation loops | TensorZero |

## Evaluation Criteria

When comparing LLM gateways, check:

- Provider coverage and OpenAI-compatible API support.
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
