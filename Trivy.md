# Prerequisites

## We need Docker because we will scan a Docker image.
```bash
sudo apt update
sudo apt install docker.io -y
```
## Your User to Docker Group
```bash
sudo usermod -aG docker $USER
```
```bash
newgrp docker
```

## Verify Docker Access
```bash
docker run hello-world
```

# Install Trivy and set up Dockerfile, app.js, and package.json
## Installation
```bash
sudo apt update
sudo apt install wget apt-transport-https gnupg lsb-release -y
```
```bash
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
```
```bash
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
```
```bash
sudo apt update
sudo apt install trivy -y
```
```bash
trivy --version
```
## Create folder
```bash
mkdir trivy-demo
cd trivy-demo
```
### create a Dockerfile
```bash
nano Dockerfile
```
### code (sample Dockerfile which contains issues within container images) - 
```bash
FROM node:14

WORKDIR /app

COPY package*.json ./

RUN apt-get update && apt-get install -y curl

RUN npm install

COPY . .

CMD ["node", "app.js"]
```

docker build -t trivy-demo .

trivy image trivy-demo

trivy image -f json -o report.json trivy-demo

nano Dockerfile.secure

FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

USER node

CMD ["node", "app.js"]

docker build -t trivy-demo-secure -f Dockerfile.secure .

trivy image trivy-demo-secure

