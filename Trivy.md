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
