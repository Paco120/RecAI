# Setup Guide for RecAI

This guide covers installation and configuration for all RecAI subprojects.

## Table of Contents

- [System Requirements](#system-requirements)
- [Installation](#installation)
- [API Configuration](#api-configuration)
- [Subproject-Specific Setup](#subproject-specific-setup)
- [Troubleshooting](#troubleshooting)

## System Requirements

- **Python:** 3.9 or higher
- **OS:** Linux, macOS, or Windows
- **Memory:** 8GB+ recommended
- **GPU:** Optional but recommended for some models

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/RecAI.git
cd RecAI
```

### 2. Create Virtual Environment

**Linux/macOS:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Windows (Command Prompt):**
```cmd
python -m venv venv
venv\Scripts\activate.bat
```

### 3. Upgrade pip

```bash
pip install --upgrade pip setuptools wheel
```

## API Configuration

### OpenAI API Setup

1. Get an API key from [OpenAI Platform](https://platform.openai.com/api-keys)

2. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

3. Edit `.env` and add your API key:
   ```
   OPENAI_API_KEY=sk-...
   OPENAI_API_TYPE=open_ai
   ```

### Azure OpenAI Setup

1. Set up Azure OpenAI Service in Azure Portal

2. Edit `.env`:
   ```
   OPENAI_API_TYPE=azure
   OPENAI_API_BASE=https://your-resource.openai.azure.com/
   OPENAI_API_VERSION=2023-03-15-preview
   OPENAI_API_KEY=your-azure-api-key
   ```

### Local Model Setup (Vicuna)

1. Install FastChat:
   ```bash
   pip install fschat
   ```

2. Download Vicuna weights:
   ```bash
   git clone https://huggingface.co/lmsys/vicuna-7b-v1.5
   ```

3. Start the local server:
   ```bash
   # Terminal 1: Controller
   python -m fastchat.serve.controller
   
   # Terminal 2: Model Worker
   python -m fastchat.serve.model_worker --model-path path-to-vicuna
   
   # Terminal 3: API Server
   python -m fastchat.serve.openai_api_server --host localhost --port 8000
   ```

4. Update `.env`:
   ```
   OPENAI_API_TYPE=open_ai
   OPENAI_API_BASE=http://localhost:8000/v1
   OPENAI_API_KEY=EMPTY
   ```

## Subproject-Specific Setup

### InteRecAgent

```bash
cd InteRecAgent
pip install -r requirements.txt

# Download data resources from:
# - Google Drive: https://drive.google.com/file/d/1nSw2cuoi_WEOnHRg_eIyLWGBAjHdelsg/view?usp=drive_link
# - RecDrive: https://rec.ustc.edu.cn/share/baa4d930-48e1-11ee-b20c-3fee0ba82bbd

# Extract and place in resources/
# Your structure should look like:
# - InteRecAgent/resources/game/
# - InteRecAgent/resources/movie/
# - InteRecAgent/resources/beauty_product/

# Run the app
DOMAIN=game python app.py --engine gpt-4
```

### RecLM-gen

```bash
cd RecLM-gen
pip install -r requirements.txt

# Training example
python train.py --config config.yaml
```

### RecLM-emb

```bash
cd RecLM-emb
pip install -r requirements.txt

# Run embeddings
python embed.py --data data.json --output embeddings.npy
```

### RecLM-eval

```bash
cd RecLM-eval
pip install -r requirements.txt

# Evaluate models
python evaluate.py --model_name gpt-4
```

### Knowledge_Plugin

```bash
cd Knowledge_Plugin
pip install -r requirements.txt

# Use knowledge plugin
python main.py --knowledge_source knowledge.json
```

### RecExplainer

```bash
cd RecExplainer
pip install -r requirements.txt

# Generate explanations
python explain.py --model recommender_model --samples test_data.json
```

### RecLM-uni

```bash
cd RecLM-uni
pip install -r requirements.txt

# Run unified model
python run.py --config config.yaml
```

## Troubleshooting

### Import Errors

**Problem:** `ModuleNotFoundError: No module named 'langchain'`

**Solution:**
```bash
pip install langchain openai
```

### API Key Issues

**Problem:** `AuthenticationError` or `unauthorized`

**Solution:**
1. Verify API key in `.env`
2. Check API key has required permissions
3. For Azure, verify base URL and API version

### CUDA/GPU Issues

**Problem:** GPU not detected or CUDA errors

**Solution:**
```bash
# Use CPU-only versions
pip install torch-cpu
# Or reinstall with CPU variant
pip install torch==2.0.0 --index-url https://download.pytorch.org/whl/cpu
```

### Memory Issues

**Problem:** `MemoryError` or out-of-memory errors

**Solution:**
- Reduce batch size
- Use gradient checkpointing
- Use CPU instead of GPU
- Process smaller datasets

### Network Issues

**Problem:** Timeout errors with API calls

**Solution:**
```bash
# Increase timeout in .env or code
# Export timeout variable
export OPENAI_REQUEST_TIMEOUT=60
```

### Platform-Specific Issues

**Windows:**
- Use `python` instead of `python3`
- Use `.\venv\Scripts\activate.ps1` for PS
- Use `venv\Scripts\activate.bat` for CMD

**macOS:**
- May need to install Xcode tools: `xcode-select --install`

**Linux:**
- Ensure Python dev libraries are installed: `sudo apt install python3-dev`

## Getting Help

- Check [FAQ](./FAQ.md)
- Review [Issues](https://github.com/YOUR-USERNAME/RecAI/issues)
- Check project [Documentation](./README.md)
- Read research [Papers](./README.md#citation)
