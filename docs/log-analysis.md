# SOCPilot Log Analysis

## Purpose

This document describes the log analysis use case in SOCPilot.

The goal is to test how locally hosted AI models can assist with basic SOC-style log review without sending log data to external AI services.

SOCPilot is used as a local lab environment for analyzing synthetic or anonymized logs.

## Input

The input is a log sample.

Examples:

- SSH authentication logs
- NGINX access logs
- NGINX error logs
- ModSecurity alerts
- HAProxy logs
- Windows Event Logs

The first test case uses SSH authentication logs.

## Process

The log sample is pasted into SOCPilot together with a structured analysis prompt.

The prompt asks the model to:

1. summarize the log events
2. identify suspicious indicators
3. detect possible attack patterns
4. estimate risk level
5. recommend investigation steps
6. describe missing context

## Output

The expected output is a structured SOC-style analysis.

Example output structure:

1. Executive summary
2. Suspicious indicators
3. Possible attack pattern
4. Risk level
5. Recommended actions
6. What should be checked next
7. Missing information

## First Test Case: SSH Authentication Logs

Example log:

```text
Aug 29 21:10:02 server sshd[1021]: Failed password for invalid user admin from 185.10.20.33 port 51222 ssh2
Aug 29 21:10:05 server sshd[1022]: Failed password for invalid user root from 185.10.20.33 port 51223 ssh2
Aug 29 21:10:09 server sshd[1023]: Failed password for testuser from 185.10.20.33 port 51224 ssh2
Aug 29 21:12:40 server sshd[1044]: Accepted password for testuser from 192.168.1.20 port 50110 ssh2
```

## Analysis Prompt

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

Logs:
[PASTE LOGS HERE]
```

## Expected Analysis Goals

The model should help identify:

- repeated failed login attempts
- login attempts against common usernames such as admin or root
- possible brute-force behavior
- suspicious source IP patterns
- successful login after failed attempts
- missing investigation context
- recommended next investigation steps

## Example Questions for SOCPilot

```text
What suspicious indicators exist in these logs?
```

```text
Is this behavior consistent with brute-force activity?
```

```text
What should a SOC analyst check next?
```

```text
What information is missing for a complete investigation?
```

## Security Notes

Only synthetic or anonymized logs should be used.

Do not commit real production logs, internal IP addresses, real usernames, hostnames, internal domains, passwords, API keys, SSH keys or certificates to the repository.

## Lab Boundary

SOCPilot is used for analysis and decision support only.

The AI model should not execute commands, modify systems, change firewall rules, create accounts, change permissions or perform automated incident response actions.

Operational actions must always be reviewed and executed by a human or by an approved automation workflow.

## Future Log Sources

Future log analysis tests may include:

- NGINX access logs
- NGINX error logs
- ModSecurity alerts
- HAProxy logs
- Windows Event Logs
- firewall logs
- VPN authentication logs
- SIEM-exported events

## Future Improvements

Possible future improvements:

- structured prompt templates
- log parsing scripts
- anonymization before analysis
- RAG against internal documentation
- threat intelligence enrichment
- severity scoring
- exportable investigation summaries
- comparison between different local models

## Summary

The log analysis use case is the first practical test area for SOCPilot.

The goal is to evaluate whether local AI can provide useful SOC-style analysis from log samples while keeping all data local.