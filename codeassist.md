
### CodeAssist Setup (`codeassist/setup.md`)

# CodeAssist Setup Guide

Fully local AI coding agent using Docker and Ollama.

## System Requirements
- 8GB RAM minimum (16GB recommended)
- 10-20GB free disk space
- Ubuntu 20.04/22.04 or Windows with WSL2
- Docker support

## Installation Steps

### 1. System Update
```bash
sudo apt update && sudo apt upgrade -y
```
2. Install Docker
```
sudo apt install docker.io -y
sudo systemctl enable docker
sudo service docker start
docker --version
```
3. Install Python and PIP
```
sudo apt install python3 python3-pip -y
python3 --version
pip3 --version
```
4. Install UV Package Manager
```
curl -LsSf https://astral.sh/uv/install.sh | sh
uv --version
```
5. Clone and Setup CodeAssist
```
git clone https://github.com/gensyn-ai/codeassist.git
cd codeassist
```
6. Run CodeAssist
```
uv run run.py
```
7. HuggingFace Token
When prompted, enter your HuggingFace token to proceed with the setup.

Features
✅ Write code
✅ Debug code
✅ Generate files
✅ Answer coding questions
✅ Build apps & scripts
✅ Fully private and local
✅ Lightweight and fast

Usage
After setup, CodeAssist runs entirely locally using Docker containers and Ollama models. No internet connection required for normal operation.

Troubleshooting
Ensure Docker is running: sudo service docker status

Check disk space: df -h
Verify Python version: python3 --version
