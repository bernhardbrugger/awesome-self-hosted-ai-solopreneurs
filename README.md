# Awesome Self-Hosted AI Stack for Solopreneurs [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated, honest list of self-hosted AI tools for solopreneurs and small
> teams who want to run their own AI infrastructure instead of routing
> everything through US SaaS APIs — with an explicit GDPR / EU-hosting lens.

Most "awesome self-hosted AI" lists are technology round-ups: they tell you
*what exists*, not *what happens to your data*. This list is built the other
way around. Every entry answers two questions: **can I actually deploy this
on a normal Hetzner/OVH/Scaleway VPS**, and **does it quietly phone home to a
US cloud**? Entries that fail the second question are still listed — honesty
beats completeness — but they carry a ⚠️ warning explaining exactly why.

**Who this is for:** a freelancer, consultant, or 2–10 person team in the EU
who needs an internal chatbot, a document/RAG pipeline, or an automation
layer, and would rather self-host on a €20/month VPS than sign up for five
different US SaaS tools with unclear data-processing terms.

**Not legal advice.** The DSGVO / Compliance Basics section below summarizes
publicly available guidance from EU data protection authorities. It is not a
substitute for advice from a lawyer or your local Datenschutzbeauftragte(r).

## Contents

- [Chat & Interface Layer](#chat--interface-layer)
- [Inference Engines (Model Serving)](#inference-engines-model-serving)
- [RAG & Vector Search](#rag--vector-search)
- [Workflow & Agent Automation](#workflow--agent-automation)
- [Document Processing & OCR](#document-processing--ocr)
- [Observability & Monitoring](#observability--monitoring)
- [Auth & Access Management](#auth--access-management)
- [Infrastructure & Deployment Templates](#infrastructure--deployment-templates)
- [DSGVO / Compliance Basics](#dsgvo--compliance-basics) ⚠️
- [Starter Stacks](#starter-stacks)

## Chat & Interface Layer

- [LibreChat](https://github.com/danny-avila/LibreChat) - Self-hosted, multi-provider ChatGPT-style interface with Agents, MCP support, and secure multi-user auth built in. MIT-licensed, no subscription gates.
- [Open WebUI](https://github.com/open-webui/open-webui) - Polished, fast-moving self-hosted chat UI for Ollama and any OpenAI-compatible backend. ⚠️ Since v0.6.6 the license adds a branding-retention clause for deployments with 50+ aggregate users; solopreneur-scale internal use is unaffected and may fully rebrand.

## Inference Engines (Model Serving)

- [Ollama](https://github.com/ollama/ollama) - The easiest way to run open-weight models locally; MIT-licensed, no telemetry of prompt content when run locally. ⚠️ Binds to all interfaces with no auth by default — a January 2026 internet scan found 175,000+ publicly reachable Ollama instances. Never expose port 11434 directly; put it behind a reverse proxy with authentication (see Auth & Access Management below).
- [vLLM](https://github.com/vllm-project/vllm) - High-throughput, OpenAI-API-compatible serving engine, Apache-2.0. The default choice once you have a GPU box and need real concurrency instead of single-user local inference.
- [llama.cpp](https://github.com/ggml-org/llama.cpp) - MIT-licensed, dependency-light C/C++ inference engine that runs quantized models on CPU-only VPS instances — the pragmatic option when a GPU box isn't in the budget yet.
- [Hugging Face Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) - ⚠️ Versions ≥1.0 ship under the HFOIL 1.0 license (not OSI-approved; reselling as a hosted service needs a license grant from Hugging Face), and the project entered maintenance mode on 2025-12-11. Hugging Face itself now recommends vLLM or SGLang for new deployments — listed here only for teams already running it.

## RAG & Vector Search

- [Qdrant](https://github.com/qdrant/qdrant) - Apache-2.0, Rust-based vector database with a small memory footprint — comfortably self-hosts on a low-spec VPS alongside your other services.
- [Weaviate](https://github.com/weaviate/weaviate) - BSD-3-Clause vector database with built-in hybrid search and a GraphQL/REST API.
- [pgvector](https://github.com/pgvector/pgvector) - PostgreSQL-licensed extension that adds vector similarity search directly to PostgreSQL. If you already run PostgreSQL for your app, this is the lowest-new-infrastructure way into RAG.

## Workflow & Agent Automation

- [n8n](https://github.com/n8n-io/n8n) - 400+ integrations, visual workflow builder, strong AI-agent nodes. ⚠️ Licensed under the Sustainable Use License (source-available, not OSI-approved): free to self-host for internal/non-commercial use, but reselling it as a hosted service to third-party customers is explicitly not covered and requires a commercial Enterprise license.
- [Node-RED](https://github.com/node-red/node-red) - Apache-2.0, fully permissive flow-based automation tool. Weaker SaaS/CRM integration library than n8n, but a good fit if your automation leans toward devices, MQTT, or simple internal glue logic with zero licensing caveats.

## Document Processing & OCR

- [Docling](https://github.com/docling-project/docling) - MIT-licensed (IBM Research, now hosted at the Linux Foundation's LF AI & Data). Converts PDFs, Office files, HTML, and scanned images into clean Markdown/JSON — built specifically to feed RAG pipelines.
- [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF) - MPL-2.0. Adds a searchable OCR text layer to scanned PDFs using Tesseract; the standard building block behind most self-hosted document pipelines, including Paperless-ngx below.
- [Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) - GPL-3.0 full document management system (ingest, OCR, tag, full-text search) with a REST API that exposes extracted document text — useful as the document substrate you point a RAG/embedding worker at, not just a scanner archive.

## Observability & Monitoring

- [Langfuse](https://github.com/langfuse/langfuse) - Core tracing, evaluations, prompt management, and the playground are MIT-licensed with no usage limits when self-hosted. ⚠️ Enterprise modules (SCIM, audit logging, data-retention policies) require a commercial license even in a self-hosted deployment. Also offers an EU cloud region (AWS eu-west-1) with a DPA if you decide not to self-host.
- [Opik](https://github.com/comet-ml/opik) - Apache-2.0 LLM observability and evaluation platform from Comet; fully permissive, no gated features.
- [Arize Phoenix](https://github.com/Arize-ai/phoenix) - ⚠️ Elastic License 2.0 (source-available, not OSI-approved: you may self-host it, but may not resell it as a hosted service). Self-hosting itself has zero feature gates, and it's built on OpenTelemetry.

## Auth & Access Management

- [Keycloak](https://github.com/keycloak/keycloak) - Apache-2.0, the mature choice for full OIDC/SAML/LDAP identity federation. Steeper learning curve than the alternatives below, but the most battle-tested if you need to federate against existing enterprise directories later.
- [Authentik](https://github.com/goauthentik/authentik) - MIT-licensed core with a modern, flow-based visual configuration UI — noticeably faster to get productive with as a solo operator than Keycloak. Paid Enterprise edition only adds RADIUS support and SLAs.
- [Authelia](https://github.com/authelia/authelia) - Apache-2.0 lightweight forward-auth and 2FA portal that sits in front of a Traefik or Nginx reverse proxy — the simplest way to put real authentication in front of Ollama, Qdrant's dashboard, or any of the tools above that ship with no auth of their own.

## Infrastructure & Deployment Templates

- [Coolify](https://github.com/coollabsio/coolify) - Apache-2.0 self-hosted PaaS with 280+ one-click service templates (including several tools on this list). Installs on a single Hetzner CAX11/CCX13 box in minutes and gives you a dashboard for deploys, backups, and SSL instead of hand-rolled Compose files.
- [Dokploy](https://github.com/Dokploy/dokploy) - ⚠️ Open-core, dual-licensed (open-source core + a separate proprietary license file for enterprise features), and still pre-1.0. Lighter-weight than Coolify (lower idle RAM, Docker Swarm + Traefik under the hood) if you want the simplest possible one-app-per-VPS setup.
- [terraform-provider-hcloud](https://github.com/hetznercloud/terraform-provider-hcloud) - MPL-2.0 official Hetzner Cloud Terraform provider — reproducible, version-controlled VPS provisioning instead of manually clicking through the Hetzner console.
- [OVH Terraform provider](https://github.com/ovh/terraform-provider-ovh) - Official OVHcloud Terraform provider. Note OVH's API surface is split across several products, so non-trivial setups (object storage, load balancers) often need the OpenStack or AWS-compatible provider alongside it.

## DSGVO / Compliance Basics

⚠️ **Not legal advice** — this is a plain-language summary of publicly
available guidance for orientation, not a compliance certification. Talk to
a lawyer or your Datenschutzbeauftragte(r) for an actual assessment of your
setup.

- Self-hosting doesn't reduce your compliance burden, it relocates it. The
  German federal data protection authority (BfDI), in guidance dated
  22 December 2025, states plainly that a self-hosted setup drops the need
  for an Art. 28 DSGVO data-processing agreement with an external vendor —
  but only because *you* are now the sole controller, which means every
  obligation a DPA would normally cover has to be documented internally
  instead.
- Data minimization: only process the personal data your use case actually
  needs, anonymize or pseudonymize fine-tuning/RAG source data where
  feasible, and define and enforce concrete deletion periods rather than
  keeping everything indefinitely.
- Transparency: be able to explain, on request, what data a given tool
  processed and why (Art. 15 access requests), and document model/tool
  configuration and data flows as you build the stack, not after the fact.
- Technical and organizational measures under Art. 32 DSGVO, at minimum:
  access control in front of every tool that doesn't ship its own (see the
  Auth & Access Management section above), encryption in transit and at
  rest, audit logging, and periodic security review. Several tools in this
  list, including Ollama, Qdrant, and Coolify's dashboard, have no
  authentication by default — that's an operator responsibility, not a bug.
- EU AI Act timeline: obligations for providers of general-purpose AI models
  (GPAI) have applied since 2 August 2025; transparency obligations under
  Art. 50, such as disclosing AI-generated content, apply from 2 August
  2026; high-risk system obligations were pushed back to 2 December 2027 via
  the "AI Omnibus" simplification package. Self-hosting an open-weight model
  generally makes *you* the deployer/operator rather than the GPAI provider
  — but check whether your specific use case falls under an Annex III
  high-risk category regardless.
- Watch for hidden third-country transfers: the biggest GDPR risk in a
  "self-hosted" stack usually isn't the core tool, it's an overlooked
  telemetry call, crash reporter, or a "cloud" tier bundled into an
  otherwise self-hostable product. Every ⚠️ in this list flags exactly that
  kind of risk where one was found.

Further reading (not fetched in full for this list, referenced for your own
research): [BfDI "Handreichung KI", 22 December 2025](https://www.bfdi.bund.de/SharedDocs/Downloads/DE/DokumenteBfDI/Dokumente-allg/2025/Handreichung-KI.pdf?__blob=publicationFile&v=1) ·
[HmbBfDI on LLMs and personal data](https://datenschutz-hamburg.de/fileadmin/user_upload/HmbBfDI/Datenschutz/Informationen/240715_Diskussionspapier_HmbBfDI_KI_Modelle.pdf) ·
[Bitkom: Datenschutzkonformer Umgang mit personenbezogenen Daten in KI-Modellen](https://www.bitkom.org/Bitkom/Publikationen/Datenschutzkonformer-Umgang-mit-personenbezogenen-Daten-in-KI-Modellen)

## Starter Stacks

- [n8n self-hosted AI starter kit](https://github.com/n8n-io/self-hosted-ai-starter-kit) - Official n8n project (Apache-2.0, 15k★). Docker Compose combining n8n + Ollama + Qdrant + PostgreSQL — the fastest path from zero to a working local RAG/automation workflow.
- [hwdsl2/self-hosted-ai-stack](https://github.com/hwdsl2/self-hosted-ai-stack) - MIT, smaller and newer (142★) but broader: Ollama, LiteLLM, AnythingLLM, Whisper/WhisperLive, Kokoro TTS, Docling, and an MCP Gateway on top of PostgreSQL+pgvector. Worth a look once you need voice or a model-routing gateway on top of the basics.
- This repo's own Quick Start (in `quickstart/`, see `quickstart/README.md`) - Ollama + Open WebUI + Qdrant in a minimal 3-service Docker Compose file, meant as a first deployable step on a fresh EU VPS.

## Related Lists

- [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted) - The canonical, much broader list of self-hostable software; not AI-specific.
- [awesome-private-ai](https://github.com/tdi/awesome-private-ai) - Broader private/on-prem AI tooling (fine-tuning, quantization, agents); no GDPR/EU-hosting framing.
- [awesome-self-hosted-ai](https://github.com/frechdi/awesome-self-hosted-ai) - Similar self-hosted-AI scope with VPS-provider benchmarks; no systematic GDPR/EU framing.
- [awesome-llm-services](https://github.com/av/awesome-llm-services) - Narrower list focused specifically on self-hostable LLM inference services.
- [awesome-gdpr](https://github.com/erichard/awesome-gdpr) - Curated GDPR-compliant SaaS tools for websites; general-purpose, not AI or self-hosting specific.
