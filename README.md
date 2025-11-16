gensyn-testnet-nodes/
├── README.md
├── rl-swarm/
│   ├── setup.md
│   └── troubleshooting.md
├── blockassist/
│   ├── setup.md
│   └── wsl-guide.md
├── codeassist/
│   ├── setup.md
│   └── configuration.md
└── images/
    ├── rl-swarm/
    ├── blockassist/
    └── codeassist/

**    # Gensyn Testnet Nodes Setup Guide**

**This repository contains comprehensive setup guides for all three Gensyn testnet nodes:**

- **RL-Swarm** - GPU-based reinforcement learning node
- **BlockAssist** - Minecraft AI assistant node (WSL setup)
- **CodeAssist** - Local AI coding agent

## 📋 Prerequisites

### System Requirements
- **RL-Swarm**: NVIDIA GPU (RTX 3090/4090 recommended), Ubuntu
- **BlockAssist**: Windows 10/11 with WSL2, Python 3.10+
- **CodeAssist**: 8GB RAM minimum, Docker, Python 3.8+

### Common Dependencies
- Git
- Python 3.8+
- Node.js & Yarn
- CUDA (for GPU nodes)

## 🚀 Quick Start

Choose your node and follow the dedicated setup guide:

### [RL-Swarm Setup](./rl-swarm/setup.md)
GPU-accelerated reinforcement learning node for the Gensyn network.

### [BlockAssist Setup](./blockassist/setup.md)
Minecraft AI assistant node running on WSL with GUI support.

### [CodeAssist Setup](./codeassist/setup.md)
Fully local AI coding agent using Docker and Ollama.

## 🔧 Common Tools Installation

### Python & Virtual Environment
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv -y
python3 -m venv gensyn-env
source gensyn-env/bin/activate

**
Node.js & Yarn**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g yarn

Git
```bash
sudo apt install git -y

📊 Monitoring & Support

Dashboard: https://dashboard.gensyn.ai/
Documentation: https://docs.gensyn.ai/

🤝 Contributing
Feel free to submit issues and enhancement requests!

📝 License
This project is licensed under the MIT License.

⚠️ Disclaimer
This is a testnet setup guide. Always follow official Gensyn documentation for mainnet deployments.




Now, let me create the individual setup files:

### RL-Swarm Setup (`rl-swarm/setup.md`)

```markdown
# RL-Swarm Node Setup Guide

This guide will help you set up the RL-Swarm GPU node for Gensyn testnet.

## System Requirements
- NVIDIA GPU (RTX 3090/4090 recommended)
- Ubuntu 20.04/22.04
- 75-100 GB disk space
- CUDA compatible drivers

## Installation Steps

### 1. Install System Dependencies
```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip -y

### 2. Install Python & Node.js
```bash
sudo apt install -y python3 python3-pip python3.10-venv
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g yarn

### 3. Clone and Setup RL-Swarm
```bash
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
git pull


### 4. Create Virtual Environment
```bash
screen -S swarm
python3 -m venv .venv
source .venv/bin/activate


### 5. Run RL-Swarm
```bash
./run_rl_swarm.sh

### 6. Setup Ngrok for Access
```bash
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
tar -xvzf ngrok-v3-stable-linux-amd64.tgz
mv ngrok /usr/local/bin/
ngrok config add-authtoken YOUR_AUTH_TOKEN
ngrok http 3000


### 7. Complete Setup ( copy temp folder. Then use this folder everytime swarm-code or block)
```bash
Access the forwarded ngrok URL in your browser
Login with Google account
Follow on-screen prompts in the swarm screen

Important Files
swarm.pem - Your node identity file (BACKUP THIS FILE)


### Screen Management
```bash
screen -r swarm          # Reattach to swarm session
CTRL + A then D          # Detach from screen session
screen -ls               # List all screen sessions


### Troubleshooting

Use CTRL + C to stop and restart with ./run_rl_swarm.sh
Update regularly with git fetch origin && git reset --hard origin/main





