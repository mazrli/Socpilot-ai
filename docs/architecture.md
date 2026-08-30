# SOCPilot Architecture

## Overview

SOCPilot is a local AI lab designed to test how locally hosted language models can support security log analysis, SOC-style workflows and incident investigation.

The architecture is intentionally simple in the first phase. The goal is to validate the concept before adding more advanced components such as RAG, vector databases, SIEM integration or external access.

SOCPilot is not a production security platform. It is a local proof of concept.

---

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

---

## Current Components

### 1. Ubuntu Server

Ubuntu Server is used as the base operating system for the local AI lab.

Responsibilities:

- host Docker
- run Ollama as a service
- expose Open WebUI on the local network
- allow remote administration over SSH

---

### 2. Docker

Docker is used to run Open WebUI as a container.

Responsibilities:

- isolate Open WebUI from the host system
- simplify deployment
- restart the web interface automatically after reboot

---

### 3. Ollama

Ollama runs the local AI models and exposes a local API.

Responsibilities:

- manage local AI models
- serve models through an HTTP API
- process prompts from Open WebUI
- keep inference local

Default port:

```text
11434
```

---

### 4. Open WebUI / SOCPilot

Open WebUI provides the browser-based interface.

In this lab, the interface is branded as:

```text
SOCPilot
```

Responsibilities:

- provide a web UI for interacting with local models
- connect to Ollama
- allow testing prompts and log analysis workflows
- provide a simple interface for local AI experiments

Default port:

```text
3000
```

---

### 5. Local AI Models

The first tested local models are:

```text
gemma:2b
llama3.2:1b
```

Responsibilities:

- answer security-related prompts
- summarize logs
- identify suspicious patterns
- provide structured analysis output

---

## Network Architecture

Current local access:

```text
Client Device
  |
  | HTTP
  v
http://SERVER-IP:3000
  |
  v
Open WebUI / SOCPilot
  |
  | HTTP
  v
Ollama API :11434
  |
  v
Local AI Model
```

Default ports:

| Service | Port | Purpose |
|---|---:|---|
| SSH | 22 | Remote server management |
| Open WebUI | 3000 | Web interface |
| Ollama | 11434 | Local model API |

The environment is intended for local network use only.

---

## Data Flow

### Input

The user provides a security-related prompt or log sample through the SOCPilot web interface.

Example input:

```text
Analyze these SSH logs as a SOC analyst.
```

---

### Process

```text
1. User submits prompt in browser
2. Open WebUI receives the request
3. Open WebUI sends the prompt to Ollama
4. Ollama sends the prompt to the selected local model
5. The model generates an analysis
6. Ollama returns the response to Open WebUI
7. Open WebUI displays the response to the user
```

---

### Output

The expected output is a structured analysis, for example:

```text
1. Executive summary
2. Suspicious indicators
3. Possible attack pattern
4. Risk level
5. Recommended actions
6. What should be checked next
7. Missing information
```

---

## Initial Use Case Flow

The first use case is SSH authentication log analysis.

```text
Synthetic SSH log sample
  |
  v
SOCPilot prompt
  |
  v
Open WebUI
  |
  v
Ollama
  |
  v
Local model
  |
  v
Structured SOC-style analysis
```

The goal is to test whether a local model can identify basic suspicious patterns such as:

- repeated failed login attempts
- attempts against common usernames
- possible brute-force behavior
- suspicious source IP patterns
- successful login after failed attempts
- missing context needed for investigation

---

## Security Boundary

SOCPilot is designed as a local-first lab.

```text
Local Network Only
No public internet exposure
No port forwarding
No external AI API required
```

The system should not be exposed directly to the internet.

---

## Important Security Rules

The AI model must not have direct access to production systems.

The AI should only provide:

- analysis
- summaries
- recommendations
- investigation support

The AI should not:

- execute commands
- modify systems
- create users
- change firewall rules
- change permissions
- perform automated incident response actions

Any operational action must be reviewed and executed by a human or an approved automation workflow.

---

## Current Architecture Limitations

The current lab does not include:

- authentication hardening
- SIEM integration
- automatic log ingestion
- RAG
- vector database
- role-based access control
- audit logging
- production monitoring
- public remote access

These are future improvements.

---

## Future Architecture

A possible future architecture:

```text
User Browser
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
  |
  +--> SIEM Export / Log Sources
          |
          +--> SSH logs
          +--> NGINX logs
          +--> ModSecurity alerts
          +--> HAProxy logs
          +--> Windows Event Logs
```

---

## Future Components

### AI Gateway

A future gateway layer could control:

- model selection
- rate limits
- authentication
- logging
- prompt templates
- policy enforcement

---

### RAG Service

A future RAG service could allow SOCPilot to answer questions using local documentation.

Possible sources:

- security policies
- incident response procedures
- system documentation
- threat intelligence notes
- internal runbooks

---

### Vector Database

A vector database could be used for semantic search.

Possible options:

- Qdrant
- Chroma
- Weaviate
- PostgreSQL with pgvector

---

### SIEM / Log Integration

Future integrations may include exported or collected logs from:

- SSH
- NGINX
- ModSecurity
- HAProxy
- Windows Event Logs
- SIEM exports

---

## Recommended Next Architecture Step

The next step should not be full automation.

The next step should be:

```text
Manual log input
  -> structured prompt
  -> local AI analysis
  -> human review
```

After that, the project can move toward:

```text
local files
  -> parsing
  -> prompt templates
  -> structured output
  -> optional RAG
```

---

## Target Architecture Principle

SOCPilot should act as decision support, not as an autonomous security system.

```text
AI suggests.
Human verifies.
Approved process executes.
```