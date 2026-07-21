# 🛠️ Quick Jenkins Test Environment (with Docker-in-Docker Support)

A lightweight, pre-configured Jenkins setup designed for local testing, CI/CD pipeline experimentation, and automated deployments using Docker-outside-of-Docker (DooD).


## 📂 Project Structure

```text
├── Dockerfile          # Custom Jenkins image with pre-installed Docker CLI
└── docker-compose.yml  # Local orchestration with Docker socket mapping
```



## 📋 Configuration Files

### 1. Custom Jenkins `Dockerfile`

This image extends the official Jenkins LTS image (JDK 25) and installs the `docker-ce-cli`, allowing Jenkins to execute Docker commands on the host machine.

```dockerfile
FROM jenkins/jenkins:latest-jdk25

USER root

RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y docker-ce-cli

USER root
```

### 2. Docker Compose (`docker-compose.yml`)

Maps port `8082` for the web UI and mounts the host's Docker socket (`/var/run/docker.sock`) so Jenkins can build and run sibling containers.

```yaml
services:
  jenkins:
    build: .
    container_name: jenkins
    restart: unless-stopped
    ports:
      - "8082:8080"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock

volumes:
  jenkins_home:
```

## 🚀 Quick Start Guide

### Step 1: Clone or Place Files

Ensure both `Dockerfile` and `docker-compose.yml` are in the same directory.

### Step 2: Build and Start Jenkins

Run the following command in your terminal to build the custom image and start the container in the background:

```bash
docker compose up --build -d
```

### Step 3: Access Jenkins

1. Open your browser and navigate to: **`http://localhost:8082`**
2. Retrieve the initial administrator password from the container logs:
```bash
docker logs jenkins

```
3. Copy the password from the terminal, paste it into the setup wizard, install the suggested plugins, and create your admin user.