# Awesome LLM Gateways [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> The LLM proxy layer you can actually run yourself.

An LLM gateway is now default infra: one endpoint in front of every model provider, doing key management, fallback, load balancing, routing, caching, and spend limits so your app doesn't have to. But most of the "gateway" conversation points at hosted products (OpenRouter, Cloudflare AI Gateway, Vercel AI Gateway) where your API keys and every prompt flow through someone else's servers.

This list is the opposite. Every gateway, router, and cache here is **open-source** and **self-hostable**. You run the proxy, your keys and traffic never leave your infrastructure, and nothing on the list quietly dies when a vendor sunsets a plan. Where a project is genuinely open but paywalls its production hardening (SSO, budgets, HA, guardrails), it's flagged $\textcolor{red}{\textsf{open-core}}$ so you know before you build on it.

## Contents

- [🔀 All-in-One Gateways](#all-in-one-gateways)
- [🧭 Model Routers](#model-routers)
- [💰 Semantic Caching](#semantic-caching)
- [🔌 Client-Side Routers](#client-side-routers)

---

### What earns a place

Three tests, all required. Miss one and it's off the list, no matter how good the tool is.

| test | means |
|---|---|
| **self-hostable** | you run the gateway yourself, your API keys and request traffic never pass through a vendor's servers |
| **open-source** | a real OSI license you can audit before it sits in your critical path |
| **no lock-in** | OpenAI-compatible or standard interface, so you can pull it out, not dead weight without a paid backend |

**Cut on purpose:** cloud-only gateways you can't self-host (OpenRouter, Cloudflare AI Gateway, Vercel AI Gateway, Not Diamond, Martian, Requesty, Unify, Keywords AI, Eden AI), source-available-but-not-OSI projects (e.g. LangDB/vLLora's Elastic License), and repos that have gone dark (Lunary, TensorZero). Fine tools, just not open + self-hostable, so not here.

---

### Red flags

A note in $\textcolor{red}{\textsf{red}}$ marks friction you'll hit before the tool earns its place, a fact you can verify on the repo page in seconds:

| flag | meaning |
|---|---|
| $\textcolor{red}{\textsf{open-core}}$ | genuinely OSS, but production features (SSO, budgets, HA, guardrails) sit behind a paid tier |
| $\textcolor{red}{\textsf{fiddly setup}}$ | needs Docker plus a database/Redis, or a full Kubernetes cluster, before first use |
| $\textcolor{red}{\textsf{slowing down}}$ | real and usable, but going quiet (acquired, in maintenance mode, or no meaningful commits lately) |
| $\textcolor{red}{\textsf{early}}$ | young or pre-1.0, small maintainer, API still moving |

*Copyleft heads-up: New API and LLM Gateway are AGPL-3.0, free to self-host, but if you modify them and offer them as a network service, the license obligates you to publish your changes.*

---

<a id="all-in-one-gateways"></a>
## 🔀 All-in-One Gateways

Unified proxies that sit in front of many providers behind one OpenAI-compatible endpoint, handling keys, fallback, routing, caching, and spend.

- **[Apache APISIX](https://github.com/apache/apisix)** · Cloud-native API gateway whose built-in ai-proxy plugin routes OpenAI-compatible traffic across LLM providers with rate limiting and logging
- **[Bifrost](https://github.com/maximhq/bifrost)** · Single-binary Go gateway unifying 1,000+ models behind one OpenAI-compatible API with sub-100µs routing overhead · $\textcolor{red}{\textsf{open-core}}$
- **[Envoy AI Gateway](https://github.com/envoyproxy/ai-gateway)** · Kubernetes-native LLM gateway built on Envoy handling multi-provider routing, token rate limiting, and failover via CRDs · $\textcolor{red}{\textsf{fiddly setup}}$
- **[Helicone](https://github.com/Helicone/helicone)** · Drop-in OpenAI-compatible proxy for logging, caching, cost tracking, and rate limiting, with full self-host feature parity · $\textcolor{red}{\textsf{slowing down}}$
- **[Higress](https://github.com/higress-group/higress)** · Istio/Envoy-based API gateway with native AI plugins for multi-provider LLM routing, rate limiting, and MCP hosting
- **[Kong](https://github.com/Kong/kong)** · General-purpose API gateway extended with AI plugins for multi-LLM routing, request/response transformation, and prompt handling · $\textcolor{red}{\textsf{open-core}}$
- **[TrustGate](https://github.com/NeuralTrust/TrustGate)** · Self-hosted Go Agent Gateway (Apache-2.0) for OpenAI-compatible LLM proxy + MCP aggregation, with per-consumer credentials, budgets, and `tools/list` filtering
- **[LiteLLM](https://github.com/BerriAI/litellm)** · OpenAI-compatible proxy for 100+ LLM APIs with virtual keys, spend tracking, budgets, load balancing, and fallback routing · $\textcolor{red}{\textsf{open-core}}$
- **[LLM Gateway](https://github.com/theopenco/llmgateway)** · TypeScript gateway unifying LLM providers behind one API with usage analytics, caching, and provider key management · $\textcolor{red}{\textsf{open-core}}$ · $\textcolor{red}{\textsf{fiddly setup}}$
- **[New API](https://github.com/QuantumNous/new-api)** · Actively maintained one-api fork adding a model marketplace, token billing, and OpenAI/Claude/Gemini-compatible endpoints
- **[Portkey AI Gateway](https://github.com/Portkey-AI/gateway)** · Fast Node/Bun gateway routing to 1,600+ models with retries, load balancing, virtual keys, and caching; runs via npx · $\textcolor{red}{\textsf{open-core}}$

<a id="model-routers"></a>
## 🧭 Model Routers

Tools that pick the best, cheapest, or fastest model (or technique) per request instead of hardcoding one.

- **[NVIDIA NeMo Switchyard](https://github.com/NVIDIA-NeMo/Switchyard)** · Python proxy translating OpenAI/Anthropic APIs and routing via a built-in LLM classifier across vLLM, NIM, or Ollama backends · $\textcolor{red}{\textsf{early}}$
- **[OptiLLM](https://github.com/algorithmicsuperintelligence/optillm)** · OpenAI-compatible inference proxy with 20+ reasoning techniques plus a learned classifier that routes each prompt to the best approach
- **[RouteLLM](https://github.com/lm-sys/RouteLLM)** · LMSYS framework routing queries between a strong and weak model via trained cost-quality classifiers; no commits since August 2024 · $\textcolor{red}{\textsf{slowing down}}$
- **[Semantic Router (Aurelio Labs)](https://github.com/aurelio-labs/semantic-router)** · Lightweight Python decision layer routing prompts to routes, tools, or models by embedding similarity; runs fully offline with local encoders
- **[vLLM Semantic Router](https://github.com/vllm-project/semantic-router)** · Mixture-of-models router pairing BERT classifiers with a proxy to send each prompt to the best-fit self-hosted model · $\textcolor{red}{\textsf{early}}$

<a id="semantic-caching"></a>
## 💰 Semantic Caching

Cache responses by meaning, not exact string match, to cut token spend and latency on repeated or similar prompts.

- **[GPTCache](https://github.com/zilliztech/GPTCache)** · Python library caching LLM responses by embedding similarity; in-memory by default with pluggable FAISS or Milvus backends · $\textcolor{red}{\textsf{slowing down}}$
- **[ModelCache](https://github.com/codefuse-ai/ModelCache)** · Ant Group's semantic cache; ships a zero-config SQLite/FAISS demo, but production setup needs MySQL plus Milvus or Redis · $\textcolor{red}{\textsf{fiddly setup}}$
- **[Semcache](https://github.com/sensoris/semcache)** · Rust semantic-caching proxy for OpenAI-compatible APIs; single container, fully in-memory, no external database to run · $\textcolor{red}{\textsf{open-core}}$

<a id="client-side-routers"></a>
## 🔌 Client-Side Routers

Libraries you embed in your app for one unified interface across providers, the router lives in your process, not a separate proxy.

- **[aisuite](https://github.com/andrewyng/aisuite)** · Andrew Ng's thin unified chat-completions wrapper across ~10 providers through a single OpenAI-style client call
- **[any-llm](https://github.com/mozilla-ai/any-llm)** · Mozilla.ai's unified Python interface calling official provider SDKs directly behind one function, with no hosted dependency
- **[llm](https://github.com/simonw/llm)** · Simon Willison's CLI and Python library for dozens of remote and local models through a plugin system
- **[token.js](https://github.com/token-js/token.js)** · TypeScript SDK exposing 200+ models behind OpenAI's request/response shape, embedded directly in your app · $\textcolor{red}{\textsf{early}}$
- **[Vercel AI SDK](https://github.com/vercel/ai)** · TypeScript toolkit with a unified generateText/streamText API across providers; provider fallback needs middleware, not built-in

---

*Last audited 2026-07-22 · 23 tools · open-source & self-hostable only · [Submit a gateway](contributing.md)*
