@'
# SOCPilot Security Notes

SOCPilot is a local lab environment and should not be exposed directly to the internet.

## Current Security Principles

- Local network access only
- No public port forwarding
- No external AI API required
- No autonomous execution of actions
- Human review remains required
- Test data only
- No real sensitive logs in the public repository

## Do Not Publish

The following data must not be committed to GitHub:

- real IP addresses from internal systems
- usernames from real environments
- internal domain names
- passwords
- API keys
- SSH keys
- certificates
- production logs
- screenshots containing sensitive information

## Recommended Future Controls

- SSH key-based access
- disable password login for SSH
- firewall rules
- Tailscale for private remote access
- authentication hardening for Open WebUI
- audit logging
- data classification before using real logs
- prompt injection testing for future RAG use cases

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
'@ | Set-Content -Encoding UTF8 docs\security.md