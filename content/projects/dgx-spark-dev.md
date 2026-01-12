---
title: "DGX Spark Development Cookbook"
date: 2025-11-22
tags: ["DGX", "NVIDIA", "Linux", "AI", "DevOps", "ML-Agents"]
categories: ["Projects"]
summary: "Comprehensive guide for setting up and developing on NVIDIA DGX Spark with ARM64 architecture."
---

## Hardware Overview

### DGX Spark (Grace Blackwell)

- **Architecture**: `ARM64` (AArch64)
  - **Key Point**: All software (Clash, Anaconda, Unity packages) must use `arm64` or `aarch64` versions, NOT `amd64/x86_64`
- **Unified Memory**: CPU and GPU share 128GB memory
  - `nvidia-smi` won't show separate VRAM usage (displays "Not Supported")
  - Use `htop` to check total system memory
- **Headless Mode**: No monitor/keyboard, pure remote network control

### Development Machine Setup

- **Mac**: Primary development, connected via Tailscale
- **Windows (Alienware)**: Backup for Linux ARM64 packaging compatibility

## Network & Proxy Configuration

### Tailscale (Network Tunnel)

Solves AP isolation issues common in school/hotspot networks by assigning virtual IPs starting with `100.x.y.z`.

### Linux Command Line Proxy (Clash/Mihomo)

**Core Program**: [Mihomo (Clash Meta)](https://github.com/MetaCubeX/mihomo) Linux ARM64 version

**Start Command**: `./clash -d .` (reads `config.yaml` and `Country.mmdb` from current directory)

**Temporary Proxy** (current terminal session):

```bash
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
export all_proxy="socks5://127.0.0.1:7890"
```

**Service-Level Proxy** (for background services like Ollama):

```bash
# Edit service configuration
sudo systemctl edit ollama.service

# Add these lines
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"

# Restart service
sudo systemctl restart ollama
```

## Remote Development (VS Code SSH)

### Connection Setup

Use VS Code's **Remote - SSH** extension with Tailscale IP.

### SSH Config Example

```ssh-config
Host dgx
    HostName 100.x.y.z  # Tailscale IP
    User your_username
    # IdentityFile ~/.ssh/your_private_key
```

### File Transfer

Drag and drop directly in VS Code's file explorer sidebar - more intuitive than `scp`.

### Prevent Auto-Sleep

```bash
# Disable auto suspend
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

## AI Model Deployment (Ollama)

### Installation

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh
```

### Model Management

```bash
# Run model (auto-downloads if not present)
ollama run llama3.1:70b

# List local models
ollama list

# Remove model
ollama rm <model_name>
```

**Note**: Large model downloads support resume - if interrupted, re-run the same command to continue.

## Unity ML-Agents Training

### Environment Setup

```bash
# Create conda environment
conda create -n mlagents python=3.10
conda activate mlagents

# Install ML-Agents
pip3 install torch
pip3 install mlagents
```

### Unity Build Settings

- **Scripting Backend**: IL2CPP (required, Mono doesn't support ARM64)
- **Target Architecture**: ARM64
- **Target Platform**: Linux Server
- **Render Pipeline**: 3D (Built-in) or Dedicated Server recommended

### Training Commands

```bash
# Grant execute permission
chmod +x ./Your_Build_Name.x86_64

# Start training
mlagents-learn config/your_config.yaml --env=./Your_Build_Name.x86_64 --no-graphics

# Resume training
mlagents-learn config/your_config.yaml --run-id=experiment_01 --resume
```

## Linux Survival Commands

| Command | Description |
|---------|-------------|
| `chmod +x <file>` | Grant execute permission |
| `nvidia-smi` | Check GPU status |
| `htop` | Interactive process viewer |
| `watch -n 1 <cmd>` | Run command every 1 second |
| `find ~ -name "keyword"` | Find files in home directory |
| `sudo systemctl restart <service>` | Restart a service |
| `Ctrl + C` | Stop foreground process |

## System Monitoring

```bash
# GPU status
nvidia-smi
watch -n 1 nvidia-smi

# System resources
htop
free -h
df -h
```

## Process Management

```bash
# Run in background
nohup python train.py > output.log 2>&1 &

# List background jobs
jobs -l
ps aux | grep python

# Kill process
kill -9 <PID>
```

## File Operations

```bash
# Find files
find . -name "*.py" -type f

# Transfer files
scp local_file user@dgx:/remote/path/
rsync -avz ./data/ user@dgx:/remote/data/
```

---

**Last Updated**: 2025-11-22
