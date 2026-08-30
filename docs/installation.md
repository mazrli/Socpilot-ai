@'
# SOCPilot Installation Notes

This document describes the current lab installation.

## Operating System

- Ubuntu Server
- Local network access
- SSH enabled

## Base Packages

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl wget git htop net-tools unzip nano
```

## Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Check service status:

```bash
systemctl status ollama
```

## Pull Local Models

```bash
ollama pull gemma:2b
ollama pull llama3.2:1b
```

List models:

```bash
ollama list
```

Test model:

```bash
ollama run gemma:2b
```

## Test Ollama API

```bash
curl http://localhost:11434/api/tags
```

## Install Docker

```bash
sudo apt install -y docker.io docker-compose-v2
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```

Log out and log in again after adding the user to the docker group.

## Run Open WebUI

```bash
docker run -d \
  --name open-webui \
  --restart always \
  -p 3000:8080 \
  -e WEBUI_NAME="SOCPilot" \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  ghcr.io/open-webui/open-webui:main
```

## Access Web UI

```text
http://SERVER-IP:3000
```

## Useful Commands

```bash
docker ps
docker restart open-webui
docker logs open-webui --tail 100

ollama list
systemctl status ollama
sudo systemctl restart ollama
```
'@ | Set-Content -Encoding UTF8 docs\installation.md