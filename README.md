# Local AI Orchestration

Docker Compose lab for running a local Ollama engine that can support the
NiteOpsTech / NanoMesh detection and SOAR workflow.

## Purpose

This repo provides the local AI base layer. Other repos can send sanitized
security summaries to the local endpoint for analyst-style review.

It is not required for the parsing, ETL, detection, or SOAR tests to pass.

## Safety Defaults

- Binds Ollama to `127.0.0.1:11434` on the host.
- Stores model data in a named Docker volume.
- Does not expose the model endpoint to the LAN by default.
- Contains no credentials or API keys.

## Validate Compose

```bash
docker compose config
```

## Start

```bash
docker compose up -d
```

## Pull a Model

After the container is running:

```bash
docker exec -it local_ai_engine ollama pull llama3
```

## Smoke Test

```bash
curl http://127.0.0.1:11434/api/tags
```

## Stop

```bash
docker compose down
```

To remove stored model data as well:

```bash
docker compose down -v
```

## NanoMesh Role

This is Level 2 in the NanoMesh support chain:

```text
local Ollama container
-> local_ai_bridge
-> detection summaries
-> SOAR dry-run context
-> autonomous defense matrix
```

## Safety Boundary

Only send sanitized summaries to the model. Do not send real secrets,
credentials, personal records, or production logs.
