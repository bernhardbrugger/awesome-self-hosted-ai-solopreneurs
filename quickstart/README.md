# Quick Start: Ollama + Open WebUI + Qdrant

A minimal, 3-service self-hosted AI stack meant as the first thing you
deploy on a fresh EU VPS (Hetzner CAX11/CX22, OVH, Scaleway — anything with
≥4 GB RAM for a small model; add a GPU instance later if you need it).

It combines:

- **[Ollama](https://github.com/ollama/ollama)** — runs the LLM.
- **[Open WebUI](https://github.com/open-webui/open-webui)** — the chat
  interface, with its built-in RAG wired to Qdrant instead of its default
  local Chroma store.
- **[Qdrant](https://github.com/qdrant/qdrant)** — vector store, ready for
  when you outgrow the built-in RAG and want to build your own pipeline
  against it directly.

## Before you start

⚠️ **This compose file has not been run against a live Docker Engine as
part of preparing this repository** — the environment used to write it had
no Docker available. The image names, environment variables, and port
mappings are taken directly from each project's own documentation/source
(Ollama, Open WebUI, and Qdrant's official Compose examples) and cross
-checked, but **you should still treat this as unverified until you run it
yourself** and open an issue if something doesn't match current upstream
behavior.

## Deploy

```bash
cp .env.example .env
# edit .env: set WEBUI_SECRET_KEY to the output of `openssl rand -hex 32`

docker compose up -d
```

Then open `http://<your-server-ip>:3000` (or better: put it behind a
reverse proxy with TLS — see [Coolify](../README.md#infrastructure--deployment-templates)
or Traefik/Caddy — and use the domain instead of the raw IP/port).

Pull a model once the stack is up:

```bash
docker compose exec ollama ollama pull llama3.2
```

Then in Open WebUI's admin settings, confirm under **Documents** that the
vector database shows as Qdrant (it reads `VECTOR_DB`/`QDRANT_URI` from the
compose file automatically — no UI configuration needed).

## Security notes (see also the main [DSGVO / Compliance Basics](../README.md#️-dsgvo--compliance-basics) section)

- Only `open-webui`'s port is published. `ollama` and `qdrant` are reachable
  solely over the internal Docker network — do not add `ports:` entries for
  them on a public-facing server.
- Set a real `WEBUI_SECRET_KEY`; the compose file will refuse to start
  without one (`:?` in the environment value enforces this).
- Enable Open WebUI's own auth (on by default) and consider putting
  [Authelia](../README.md#auth--access-management) in front of it for 2FA
  once you have real users.
- Back up the three named volumes (`ollama_data`, `qdrant_data`,
  `open_webui_data`) — they hold your models, vectors, and chat history/user
  accounts respectively.

## Next steps

- Swap `llama3.2` for whatever open-weight model fits your RAM/VRAM budget.
- Point a document pipeline ([Docling](../README.md#document-processing--ocr)
  or [Paperless-ngx](../README.md#document-processing--ocr)) at the same
  Qdrant instance to build real RAG over your own documents instead of
  relying on Open WebUI's built-in per-chat file upload.
- Once this is stable, look at the [Starter Stacks](../README.md#starter-stacks)
  section for a fuller setup that adds workflow automation (n8n) or
  additional modalities (voice, embeddings gateway).
