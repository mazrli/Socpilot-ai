
# SOCPilot

SOCPilot is a local AI lab for SOC support, security log analysis and incident response experimentation.

The project explores how local AI models can support security analysts by summarizing logs, identifying suspicious patterns and providing structured investigation guidance without sending data to external AI services.

## Purpose

The purpose of SOCPilot is to test how local AI can be used as decision support within security operations.

The current focus is on:

- security log analysis
- SOC support workflows
- incident summarization
- suspicious pattern detection
- local AI-assisted investigation
- future RAG against internal documentation and threat intelligence

SOCPilot is currently a lab project, not a production security platform.

## Current Stack

- Ubuntu Server
- Docker
- Ollama
- Open WebUI
- Local AI models
- Local network access only

## Current Models

The first tested models are:

- `gemma:2b`
- `llama3.2:1b`

Additional models may be tested later depending on hardware capacity.

## Architecture

```text
Browser
  -> Open WebUI / SOCPilot
    -> Ollama
      -> Local AI Model
```

The AI model runs locally. No prompts, logs or documents are sent to external AI providers.

## Initial Use Case

The first use case is manual analysis of SSH authentication logs.

Example prompt:

```text
Analyze these SSH logs as a SOC analyst.

Return:
1. Summary
2. Suspicious indicators
3. Risk level
4. Recommended actions
5. What should be checked next
```

## Planned Use Cases

- SSH log analysis
- NGINX access and error log analysis
- ModSecurity alert analysis
- HAProxy log review
- Windows Event Log summarization
- Threat intelligence enrichment
- RAG against internal security documentation

## Security Principles

SOCPilot follows a local-first approach:

- no external AI API required
- no direct internet exposure
- no autonomous execution of actions
- AI is used only as decision support
- human review remains required
- logs and prompts should be handled according to data classification

## Current Status

The lab environment is operational with:

- Ubuntu Server installed
- Ollama running as a service
- local models downloaded
- Open WebUI configured as the user interface
- SOCPilot name configured in the UI

## Next Steps

- test analysis against more log formats
- document installation steps
- add example prompts
- evaluate stronger multilingual models
- test RAG with local documentation
- define safe workflows for SOC-style investigations

## Disclaimer

SOCPilot is an experimental lab project. It is not intended to replace security analysts, SIEM platforms or incident response processes. The purpose is to evaluate how local AI can support security work in a controlled environment.
