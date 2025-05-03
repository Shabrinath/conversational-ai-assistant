# 🧠 Samantha OS1 - Conversational AI Assistant

**Samantha OS1** is an open-source, local-first conversational AI operating system designed to run intelligent agents on your infrastructure. It supports powerful LLM integrations (like OpenAI/Groq/xAI) and allows you to interact using natural language with real-time capabilities.

This guide helps you deploy Samantha OS1 on an **Ubuntu EC2 instance** using **Docker Compose v2+**, and optionally expose it securely over HTTPS using **Ngrok**.

---

## 🛠️ Prerequisites

- AWS EC2 Ubuntu 22.04 or 24.04 instance

- Open ports:

    - TCP `8000` (for web UI)

    - Optional: TCP `22` (for SSH) and `443` (if using HTTPS proxy like Ngrok)

- A valid API key from [Groq](https://console.groq.com/keys), or [OpenAI](https://platform.openai.com/account/api-keys), or [xAI](https://x.ai)

---

## 🚀 Installation Steps

### 1. Update and install Docker

```bash

sudo apt update

sudo apt install -y docker.io

sudo systemctl enable docker

sudo systemctl start docker
```

### 2. Install Docker Compose (v2+ plugin)

```bash

DOCKER_COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep '"tag_name":' | cut -d'"' -f4)

sudo mkdir -p /usr/local/lib/docker/cli-plugins/

sudo curl -SL "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-linux-x86_64" -o /usr/local/lib/docker/cli-plugins/docker-compose

sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

docker compose version
```
### 3. Clone Samantha OS1 repo

```bash

git clone https://github.com/jesuscopado/samantha-os1.git

cd samantha-os1
```
### 4\. Configure API Key

Edit the .env file with your preferred LLM provider's API key:

```bash

vi .env
# Use Azure OpenAI
USE_AZURE='false'

# Azure OpenAI Configuration
AZURE_OPENAI_API_KEY=''
AZURE_OPENAI_URL='' # without https:// nor wss://
AZURE_OPENAI_API_TYPE=''
OPENAI_DEPLOYMENT_NAME_REALTIME=''
OPENAI_API_KEY='xxxx'

# API Keys
TOGETHER_API_KEY='xxxx'
TAVILY_API_KEY='xxxxx'
GROQ_API_KEY='xxxxx'

# Database Configuration
DB_DIALECT='sqlite' # e.g. 'sqlite' 'postgresql'
DB_DATABASE='samantha.db' # if sqlite, path to the database file

```

### 5\. Start the app

```bash
docker compose up -d
```

After a few seconds, Samantha should be available at:
```
http://<your-ec2-public-ip>:8000
```
🌐 Optional: Make It Secure with HTTPS via Ngrok

Install and configure Ngrok to securely expose port 8000 over HTTPS:

```bash
sudo snap install ngrok

ngrok config add-authtoken <your-ngrok-authtoken>

ngrok http 8000

Ngrok will give you a public https:// URL (like https://yourapp.ngrok.io) which you can use to access Samantha securely from anywhere.
```


