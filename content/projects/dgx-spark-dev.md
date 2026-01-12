---
title: "DGX Spark 开发实践手册"
date: 2025-12-15
draft: false
tags: ["DGX", "NVIDIA", "AI", "Linux", "DevOps", "ML-Agents"]
categories: ["项目经验"]
summary: "在 NVIDIA DGX Spark (Grace Blackwell) 上进行 AI 开发和模型部署的实践经验。"
showToc: true
TocOpen: true
---

## 概述

这是关于在 **NVIDIA DGX Spark (Grace Blackwell)** 上进行开发和部署的实践手册。DGX Spark 是 NVIDIA 面向开发者的 AI 超级计算平台。

---

## 目录

1. 硬件与架构认知
2. 网络与代理配置
3. 远程开发环境
4. AI 大模型部署
5. Unity ML-Agents 训练流程
6. Linux 常用生存指令

---

## 1. 硬件与架构认知

### ARM64 架构注意事项

DGX Spark 使用 **ARM64 (aarch64)** 架构，与常见的 x86_64 有所不同：

- 部分软件包可能没有 ARM 版本
- Docker 镜像需要确认架构兼容性
- 编译时需要指定正确的目标架构

### GPU 资源

- **Blackwell GPU** - NVIDIA 最新一代 AI 加速器
- 支持 FP8、FP16、BF16 等多种精度
- 大显存支持超大模型部署

---

## 2. 网络与代理配置

### 代理设置

```bash
# 设置代理环境变量
export http_proxy="http://proxy:port"
export https_proxy="http://proxy:port"
export no_proxy="localhost,127.0.0.1"

# 永久配置（加入 ~/.bashrc）
echo 'export http_proxy="http://proxy:port"' >> ~/.bashrc
```

### Docker 代理配置

```bash
# 创建 Docker 代理配置
mkdir -p ~/.docker
cat > ~/.docker/config.json << EOF
{
  "proxies": {
    "default": {
      "httpProxy": "http://proxy:port",
      "httpsProxy": "http://proxy:port"
    }
  }
}
EOF
```

---

## 3. 远程开发环境

### VS Code Remote SSH

1. 安装 **Remote - SSH** 扩展
2. 配置 SSH config：

```
Host dgx-spark
    HostName your-dgx-ip
    User your-username
    IdentityFile ~/.ssh/id_rsa
```

3. 连接并开始开发

### JupyterLab

```bash
# 启动 JupyterLab
jupyter lab --ip=0.0.0.0 --port=8888 --no-browser

# 或使用 Docker
docker run -p 8888:8888 -v $(pwd):/workspace jupyter/pytorch-notebook
```

---

## 4. AI 大模型部署

### 使用 vLLM 部署

```bash
# 安装 vLLM
pip install vllm

# 启动模型服务
python -m vllm.entrypoints.openai.api_server \
    --model /path/to/model \
    --host 0.0.0.0 \
    --port 8000
```

### 使用 Ollama

```bash
# 安装 Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 运行模型
ollama run llama2
```

---

## 5. Unity ML-Agents 训练

### 环境准备

```bash
# 创建 conda 环境
conda create -n mlagents python=3.10
conda activate mlagents

# 安装 ML-Agents
pip install mlagents
```

### 训练流程

```bash
# 开始训练
mlagents-learn config/trainer_config.yaml --run-id=experiment_01

# 恢复训练
mlagents-learn config/trainer_config.yaml --run-id=experiment_01 --resume
```

### 配置文件示例

```yaml
behaviors:
  MyAgent:
    trainer_type: ppo
    hyperparameters:
      batch_size: 1024
      buffer_size: 10240
      learning_rate: 3.0e-4
    network_settings:
      normalize: true
      hidden_units: 256
      num_layers: 2
    max_steps: 500000
```

---

## 6. Linux 常用指令

### 系统监控

```bash
# GPU 状态
nvidia-smi
watch -n 1 nvidia-smi

# 系统资源
htop
free -h
df -h
```

### 进程管理

```bash
# 后台运行
nohup python train.py > output.log 2>&1 &

# 查看后台任务
jobs -l
ps aux | grep python

# 终止进程
kill -9 <PID>
```

### 文件操作

```bash
# 查找文件
find . -name "*.py" -type f

# 传输文件
scp local_file user@dgx:/remote/path/
rsync -avz ./data/ user@dgx:/remote/data/
```

---

## 常见问题

### Q: Docker 拉取镜像失败？

A: 检查代理配置，或使用国内镜像源。

### Q: CUDA 版本不匹配？

A: 使用 `nvidia-smi` 确认驱动版本，选择兼容的 CUDA 镜像。

### Q: 训练中断后如何恢复？

A: 使用 `--resume` 参数从 checkpoint 恢复训练。

---

*持续更新中...*
