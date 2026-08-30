# SOCPilot Security Notes

SOCPilot is a local AI lab environment for testing AI-assisted security analysis. The system should not be exposed directly to the internet.

The project is designed for learning, testing and technical validation of local AI-assisted log analysis, SOC-style workflows and private data analysis. It is not a production SOC platform and should not be used with real sensitive data unless proper security controls, governance and review processes are implemented.

## Security Purpose

The security purpose of SOCPilot is to validate a local-first AI approach.

The key idea is:

```text
Keep data local.
Use AI as decision support.
Let humans verify the result.
```

SOCPilot should help analyze logs, summarize events and support investigation workflows without sending prompts, logs or documents to external AI providers.

## Current Security Principles

SOCPilot follows a local-first and controlled-use approach.

Current principles:

- Local network access only
- No direct public internet exposure
- No public port forwarding
- No external AI API required
- No autonomous execution of actions
- Human review remains required
- Test data only
- Synthetic or anonymized logs only
- No real sensitive logs in the public repository
- No secrets, credentials, certificates or internal system details in GitHub

## Local-First Model

The main security value of SOCPilot is that AI processing happens locally.

```text
User
  -> Open WebUI / SOCPilot
    -> Ollama
      -> Local AI Model
```

This means that prompts, logs and test data are processed on the local machine instead of being sent to external AI providers.

However, local processing does not remove the need for proper security controls.

Local AI still requires:

- data classification
- access control
- careful handling of logs
- human validation of AI output
- protection of the host system
- secure administration
- auditability if real data is introduced later

## Do Not Publish

The following data must not be committed to GitHub:

- real production logs
- real internal IP addresses
- usernames from real environments
- internal domain names
- passwords
- API keys
- SSH keys
- private keys
- certificates
- `.env` files
- database connection strings
- screenshots containing sensitive information
- customer or employee data
- incident details from real environments
- security findings from internal systems

Only synthetic or anonymized data should be used in the public repository.

## Test Data Rules

Allowed test data:

- synthetic logs
- anonymized logs
- fake usernames
- fake hostnames
- fake IP addresses
- generated sample events
- lab-only test data

Not allowed:

- real production logs
- real authentication logs
- real firewall logs
- real SIEM exports
- real incident data
- internal hostnames
- internal usernames
- internal system paths
- real security alerts

## AI Usage Boundary

The AI model should only provide analysis and recommendations.

The model may be used to:

- summarize logs
- identify suspicious indicators
- suggest possible attack patterns
- estimate risk level
- suggest investigation steps
- explain missing context
- help structure an investigation
- support learning and documentation

The AI model must not be used to directly:

- execute commands
- modify systems
- change firewall rules
- create accounts
- disable accounts
- change permissions
- delete logs
- update security tools
- perform automated incident response actions

Any operational action must be reviewed and executed by a human or by an approved automation workflow.

## Human Review Requirement

AI-generated output must always be reviewed critically.

The model can produce:

- incomplete analysis
- incorrect conclusions
- misleading recommendations
- hallucinated explanations
- false positives
- false negatives

SOCPilot should therefore be treated as decision support, not as an authority.

Recommended principle:

```text
AI suggests.
Human verifies.
Approved process executes.
```

## Network Exposure

The current lab should only be available on the local network.

Default ports:

| Service | Port | Purpose |
|---|---:|---|
| SSH | 22 | Remote server management |
| Open WebUI | 3000 | Web interface |
| Ollama | 11434 | Local model API |

Do not expose these ports directly to the internet.

Avoid:

```text
Router port forwarding
Public access to port 3000
Public access to port 11434
Public access to SSH without hardening
```

For remote private access, use a secure private network approach such as:

- Tailscale
- WireGuard VPN
- Zero Trust access gateway

## SSH Security

Recommended future SSH hardening:

- use SSH keys instead of passwords
- disable password login
- disable root login
- restrict SSH access to trusted IP ranges
- use firewall rules
- keep the system updated
- monitor failed login attempts

Example future controls:

```text
PasswordAuthentication no
PermitRootLogin no
```

These controls should only be applied after SSH key-based access has been configured and tested.

## Open WebUI Security

Open WebUI should be used carefully.

Recommended controls:

- keep signup disabled unless needed
- use a strong admin password
- avoid public exposure
- restrict access to trusted devices
- review users regularly
- keep the container updated
- do not paste sensitive production data into public demos

For a personal lab, signup should normally remain disabled.

## Ollama Security

Ollama should not be exposed directly to the internet.

The Ollama API is intended to be consumed by Open WebUI or trusted local services.

Avoid:

```text
Public access to http://SERVER-IP:11434
```

Preferred model:

```text
User
  -> Open WebUI
    -> Ollama
      -> Local model
```

Not preferred:

```text
Internet
  -> Ollama API directly
```

## Private Data Analysis Boundary

SOCPilot may be used to test analysis of private or sensitive data in a controlled lab environment.

The goal is to evaluate whether local AI models can help analyze private logs, notes or documents while keeping processing local.

Possible future use cases:

- internal log analysis
- private documentation review
- incident note summarization
- operational data analysis
- support case classification
- local knowledge search
- future RAG against internal documents

Private data should still be handled carefully. Local processing reduces external exposure, but it does not remove the need for data classification, access control, anonymization, auditability and human review.

## Prompt Injection Risks

Future RAG or document-based workflows must consider prompt injection risks.

Possible risks:

- malicious text inside documents
- instructions hidden in logs or files
- model ignoring system instructions
- model leaking unrelated context
- model producing unsafe recommendations

Future mitigations:

- source validation
- prompt templates
- output constraints
- human review
- citation of sources
- separation between data and instructions
- logging of model interactions

## RAG Security Considerations

If RAG is added later, the following controls should be considered:

- index only approved documents
- classify documents before ingestion
- avoid indexing secrets
- avoid indexing production credentials
- restrict access by role
- log document access
- show sources in answers
- validate answers against source material
- remove outdated or incorrect documents

RAG should not be added before the basic lab workflow is stable.

## Recommended Future Controls

Recommended future controls include:

- SSH key-based access
- disabled SSH password login
- firewall rules
- Tailscale or VPN for private remote access
- authentication hardening for Open WebUI
- audit logging
- data classification before using real logs
- prompt injection testing
- backup of configuration
- update process for containers and models
- role-based access if multiple users are added
- separation between lab and production networks

## Operational Boundary

SOCPilot should not be connected directly to production systems in the current phase.

Current allowed workflow:

```text
Synthetic or anonymized log
  -> SOCPilot analysis
  -> Human review
  -> Manual learning or documentation
```

Future controlled workflow:

```text
Approved exported logs
  -> anonymization
  -> local AI analysis
  -> human review
  -> documented recommendation
```

Not allowed in the current lab:

```text
Production system
  -> direct AI action
  -> automatic remediation
```

## Incident Response Boundary

SOCPilot may support incident investigation by helping with:

- summarization
- pattern recognition
- triage support
- investigation checklists
- suggested next steps

It must not independently:

- block IP addresses
- disable users
- isolate hosts
- modify firewall rules
- update SIEM rules
- close incidents
- trigger playbooks without approval

## Repository Safety Checklist

Before pushing changes to GitHub, check:

- no real logs
- no passwords
- no API keys
- no private keys
- no certificates
- no internal IP addresses
- no internal hostnames
- no screenshots with sensitive data
- no `.env` files
- no customer or employee information

Suggested command before commit:

```bash
git status
```

Review all changed files before pushing.

## Summary

SOCPilot is a local AI security analysis lab.

The security model is based on:

- local processing
- synthetic test data
- no direct internet exposure
- no autonomous actions
- human review
- controlled experimentation

The project should remain a safe lab environment until stronger controls, governance and validation are implemented.