# Local AI Voice Assistant  
### (llama.cpp + SillyTavern + GPT-SoVITS-V2)

---

## Overview

This project implements a **fully local AI conversational system** combining:

- Local LLM inference using `llama.cpp`
- Web-based chat interface via `SillyTavern`
- Real-time voice output using `GPT-SoVITS-V2`

The entire system runs **offline**, supports **GPU acceleration**, and demonstrates a modular multi-service AI architecture.

---

## Features

- Local LLM inference (Meta-Llama-3.1-8B-Instruct-Q4_K_M)
- CUDA GPU acceleration
- SillyTavern web interface integration
- Real-time AI voice generation
- Fully offline operation
- Modular service architecture

---

## Tech Stack

| Component | Technology |
|------------|-------------|
| LLM Runtime | llama.cpp |
| UI | SillyTavern |
| Voice Synthesis | GPT-SoVITS v2 |
| GPU Acceleration | CUDA |
| Languages | Python, C++ |

---

## System Architecture

```
User Input → SillyTavern UI → llama.cpp Server → GPT-SoVITS → Audio Output
```

Each component runs independently with its own environment.

---

# Setup Guide

---

## STEP 1 — Install CUDA (GPU Support)

### Verify Installation

Run:

```bash
nvidia-smi
nvcc --version
```

If CUDA is missing, download:

https://developer.nvidia.com/cuda-downloads

Select:

- Windows
- x86_64
- exe (local)

During installation choose:

✔ CUDA Toolkit  
✔ NVCC  
✔ CUDA Runtime  
✔ Visual Studio Integration  

(Optional: Nsight tools)

---

## Create a new root folder and do the rest of the steps inside it.

## STEP 2 — Build llama.cpp with GPU Support

### Clone llama GIT repository

```bash
git clone https://github.com/ggml-org/llama.cpp.git

```

### Clean Previous Build If Available

Delete:

```
\LLama\llama.cpp\build
```

---

### Rebuild with CUDA Enabled

Open:

**x64 Native Tools Command Prompt for VS 2022**

Run:

```bash
cd C:\Users\xxx\...\LLama\llama.cpp
mkdir build
cd build
cmake .. -G "Visual Studio 17 2022" -A x64 -DLLAMA_CUDA=ON
cmake --build . --config Release
```

---

## STEP 3 — Run llama.cpp Server

```bash
.\llama-server.exe ^
--model "C:\Users\xxx\...\LLama\llama.cpp\models\Meta-Llama-3.1-8B-Instruct-Q4_K_M.gguf" ^
--port 8080 ^
--ctx-size 2048 ^
--n-gpu-layers 999 ^
--flash-attn auto
```

Server runs at:

```
http://127.0.0.1:8080/
```

---

## STEP 4 — Setup SillyTavern

### Clone SilyTavern GIT repository

```bash
git clone https://github.com/SillyTavern/SillyTavern.git

```

Inside the SillyTavern folder
Run:

```bash
start.bat
```

Configure API in API connections:

- API: Text Completion  
- API Type: llama.cpp  
- API URL: `http://127.0.0.1:8080/`
- llama.cpp Model: `Meta-Llama-3.1-8B-Instruct-Q4_K_M.gguf`

Click **Connect**.

---

## STEP 5 — Install GPT-SoVITS-V2 (Voice Synthesis)

### Clone GPT-SoVITS-V2 GIT repository

```bash
git clone https://github.com/v3ucn/GPT-SoVITS-V2.git

```

### Create New Virtual Environment Inside GPT-SoVITS-V2 Folder

New:
```bash
python -m venv sovits-env-2
sovits-env-2\Scripts\activate
```

---

### Install Dependencies

Make sure to download and place the requirements_locked_final.txt file inside GPT-SoVITS-V2 folder.
This will download all the version locked dependencies that are required to get GPT-SoVITS-V2 working.

```bash
pip install -r requirements_locked_final.txt
```

---

### Important Fix (Required)

Edit file:

```
GPT-SoVITS-V2\sovits2-venv\Lib\site-packages\LangSegment\__init__.py
```

Remove the following lines if present:

```
setLangfilters
getLangfilters
```

---

### Run GPT-SoVITS-V2 API

```bash
python api_v2.py
```

Default port:

```
9872
```

---


# Running the Full System

Start services in this order:

---

### 1️⃣ Start llama.cpp Server

```bash
.\llama-server.exe ^
--model "C:\Users\xxx\...\LLama\llama.cpp\models\Meta-Llama-3.1-8B-Instruct-Q4_K_M.gguf" ^
--port 8080 ^
--ctx-size 2048 ^
--n-gpu-layers 999 ^
--flash-attn auto
```

---

### 2️⃣ Start GPT-SoVITS API

```bash
python api_v2.py
```

---

### 3️⃣ Launch SillyTavern

```bash
start.bat
```

System is now fully operational.

---

## Virtual Environment Structure

| Component | Environment |
|------------|-------------|
| llama.cpp | Native C++ build |
| GPT-SoVITS | Python virtual environment |
| SillyTavern | Node.js runtime |

---

# Future Improvements (TODO)

- [x] **High Priority:**
  - [x] User guide.
  - [ ] Automated startup script.

- [ ] **Features**
  - [ ] Add real-time speech-to-text using Whisper
  - [ ] Implement QDRANT vector databse for long-term memory
  - [ ] Reduce voice response latency and implement voice stream by chunks
  - [ ] Optimize GPU memory usage for multi-model execution
  - [ ] Experiment with MoE concept to have specialized personalities with dynamic switching

---

## Credits
Special thanks to the following projects:

### Forked repositories
- [GPT-SoVITS-V2](https://github.com/v3ucn/GPT-SoVITS-V2)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [SillyTavern](https://github.com/SillyTavern/SillyTavern)

### Pretrained Models
- [Chinese Speech Pretrain](https://github.com/TencentGameMate/chinese_speech_pretrain)
- [Chinese-Roberta-WWM-Ext-Large](https://huggingface.co/hfl/chinese-roberta-wwm-ext-large)
- [Meta-Llama-3.1-8B-Instruct-Q4_K_M-GGUF](https://huggingface.co/joshnader/Meta-Llama-3.1-8B-Instruct-Q4_K_M-GGUF)

## 📚 Learning Outcomes

This project demonstrates:

- Local LLM deployment
- GPU-accelerated inference setup
- Multi-service AI system integration
- Real-time AI voice pipeline development
- Virtual environment management
- Version locks to reduce dependecy mismatch

---

## Project Status

**Active Development**

---
