---
title: Install Kubernetes Ubuntu
date: 2024-05-29 20:17 -500
categories: [setup,kubernetes,ubuntu]
tags: [setup,kubernetes,ubuntu]
---

# Install Kubernetes Ubuntu

## Server Detail

- Ubuntu 22.04
- node:
  - kube.local0
  - kube.local1
  - kube.local2

### Set /etc/hosts

```text
10.0.0.11 kube.local0
10.0.0.12 kube.local1
10.0.0.13 kube.local3
```

### Setup CRI

```bash
apt update; apt upgrade; apt autoremove (you may reboot the server)
apt install ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt update
apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
nano /etc/docker/daemon.json
```

```json /etc/docker/daemon.json
{
"insecure-registries": ["registry.blackeye.id"],
"exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts": {
   "max-size": "100m"
   },
"storage-driver": "overlay2"
}
```

```bash
systemctl enable docker
systemctl restart docker
```
