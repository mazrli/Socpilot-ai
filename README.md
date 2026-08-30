# SOCPilot

SOCPilot is not a finished security product. It is a local AI lab and proof of concept where I test how locally hosted models can support security log analysis, SOC-style workflows and incident investigation without sending data to external AI services.

The project is focused on learning, testing and technical validation of local AI capabilities in a cybersecurity context.

## Purpose

The purpose of SOCPilot is to explore, test and document how local AI models can be used as decision support in cybersecurity-related tasks.

The current focus is on:

- security log analysis
- SOC support workflows
- incident summarization
- suspicious pattern detection
- local AI-assisted investigation
- prompt testing for incident analysis
- future RAG experiments against documentation and threat intelligence

SOCPilot is currently a lab project, not a production security platform.


## New Lab Goal: Log Analysis

A core goal of SOCPilot is to test how local AI models can support log analysis in a controlled lab environment.

The project will use synthetic or anonymized logs to evaluate whether local models can:

- summarize log events
- identify suspicious indicators
- detect repeated failed login attempts
- recognize possible brute-force behavior
- highlight unusual source IPs
- estimate risk level
- suggest next investigation steps
- explain what additional information is missing

The first log source used in the lab is SSH authentication logs. Future tests may include NGINX, ModSecurity, HAProxy and Windows Event Logs.

## Scope

SOCPilot currently focuses on manual testing using synthetic or anonymized log data.

The first test scenario is SSH authentication log analysis, where the model is asked to summarize events, identify suspicious indicators and suggest what should be checked next.

Future tests may include:

- NGINX access and error logs
- ModSecurity alerts
- HAProxy logs
- Windows Event Logs
- internal security documentation
- threat intelligence enrichment
- RAG-based security knowledge search

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

## Network

Current local access:

```text
Client device
  -> http://SERVER-IP:3000
    -> Open WebUI / SOCPilot
      -> Ollama
        -> Local model
```

Default ports:

| Service | Port | Purpose |
|---|---:|---|
| SSH | 22 | Remote server management |
| Open WebUI | 3000 | Web interface |
| Ollama | 11434 | Local model API |

The environment is intended for local network use only.

## Initial Use Case

The first use case is manual analysis of SSH authentication logs.

Example log data:

```text
Aug 29 21:10:02 server sshd[1021]: Failed password for invalid user admin from 185.10.20.33 port 51222 ssh2
Aug 29 21:10:05 server sshd[1022]: Failed password for invalid user root from 185.10.20.33 port 51223 ssh2
Aug 29 21:10:09 server sshd[1023]: Failed password for testuser from 185.10.20.33 port 51224 ssh2
Aug 29 21:12:40 server sshd[1044]: Accepted password for testuser from 192.168.1.20 port 50110 ssh2
```

Example prompt:

```text
You are a SOC analyst.

Analyze the following security logs.

Return:
1. Executive summary
2. Suspicious indicators
3. Possible attack pattern
4. Risk level: Low / Medium / High
5. Recommended actions
6. What should be checked next
7. What information is missing
```

## Planned Use Cases

- SSH log analysis
- NGINX access and error log analysis
- ModSecurity alert analysis
- HAProxy log review
- Windows Event Log summarization
- threat intelligence enrichment
- RAG against internal security documentation

## Log Analysis Documentation

The log analysis use case is documented here:

- [Log Analysis](docs/log-analysis.md)

## Security and Privacy

SOCPilot follows a local-first approach:

- models run locally
- no external AI API is required
- test data should be synthetic or anonymized
- no production logs should be committed to the repository
- no passwords, keys, certificates or internal system details should be included
- the system should not be exposed directly to the internet

## Security Boundary

The current lab is intended for local network use only.

```text
Local Network Only
No public internet exposure
No port forwarding
```

## Do Not Publish

The following data must not be committed to this repository:

- real production logs
- real internal IP addresses
- usernames from real environments
- internal domain names
- passwords
- API keys
- SSH keys
- certificates
- `.env` files
- screenshots containing sensitive information

## AI Usage Boundary

The AI model should only provide analysis and recommendations.

It should not:

- execute commands
- modify systems
- change firewall rules
- create accounts
- change permissions
- perform automated incident response actions

Any operational action must be reviewed and executed by a human or by an approved automation workflow.

## Limitations

SOCPilot is not designed to replace:

- SOC analysts
- SIEM platforms
- incident response processes
- security monitoring tools
- human decision-making

AI output must be reviewed critically. The model may produce incomplete, incorrect or misleading analysis.

## Current Status

The lab environment is operational with:

- Ubuntu Server installed
- Ollama running as a service
- local models downloaded
- Open WebUI configured as the user interface
- SOCPilot name configured in the UI
- initial SSH log analysis prompt created
- synthetic SSH log sample added

## Next Steps

- test analysis against more log formats
- document installation steps
- add more example prompts
- evaluate stronger multilingual models
- test RAG with local documentation
- define safe workflows for SOC-style investigations
- explore future integration with SIEM exports

## Repository Structure

```text
SOCPilot
├── README.md
├── docs
│   ├── architecture.md
│   ├── installation.md
│   └── security.md
├── examples
│   └── ssh-auth-log-sample.txt
└── prompts
    └── soc-log-analysis.md
```

## Disclaimer

SOCPilot is an experimental project for learning and technical evaluation. It is not intended for production use without further security review, hardening, validation and governance.
