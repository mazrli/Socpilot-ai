@'
# SOCPilot Architecture

## Current Lab Architecture

```text
User Browser
  |
  v
Open WebUI / SOCPilot
  |
  v
Ollama API
  |
  v
Local AI Model
```

## Components

### Ubuntu Server

The base operating system for the local AI lab.

### Ollama

Runs local language models and exposes a local API.

### Open WebUI

Provides the browser-based user interface.

### Local Models

Used for chat, summarization and basic security analysis.

## Network

Current local access:

```text
Client Device
  -> http://SERVER-IP:3000
    -> Open WebUI
      -> Ollama
```

Default ports:

| Service | Port | Purpose |
|---|---:|---|
| SSH | 22 | Remote server management |
| Open WebUI | 3000 | Web interface |
| Ollama | 11434 | Local model API |

## Security Boundary

The current lab is intended for local network use only.

```text
Local Network Only
No public internet exposure
No port forwarding
```

## Current Flow

```text
Input:
Security log or SOC-related question

Process:
Open WebUI sends the prompt to Ollama.
Ollama runs the local model.
The model generates an analysis.

Output:
Structured summary, indicators, risk level and recommended next steps.
```

## Future Architecture

```text
Browser
  |
  v
Reverse Proxy / Authentication
  |
  v
SOCPilot Web UI
  |
  v
AI Gateway
  |
  +--> Ollama / vLLM
  |
  +--> RAG Service
          |
          +--> Vector Database
          +--> Internal Documentation
          +--> Threat Intelligence
```

## Future Improvements

- authentication hardening
- Tailscale access for private remote use
- RAG with internal documentation
- structured prompt templates
- log ingestion pipeline
- integration with SIEM exports
'@ | Set-Content -Encoding UTF8 docs\architecture.md