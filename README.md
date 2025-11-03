# LMCache: Cache Inference with vLLM

🚀 **AI Inference Optimization with LMCache and vLLM**

This project demonstrates the integration of LMCache with vLLM to accelerate language model inference while reducing memory usage.

## 📋 Features

- ✅ **Token caching** with LMCache
- ✅ **Multi-model support** (TinyLlama, Phi-3, Qwen, etc.)
- ✅ **Memory optimization** GPU/CPU
- ✅ **OpenAI-compatible REST API**
- ✅ **Automatic quantization**

## 📚 Documentation

- [LMCache Official Documentation](https://lmcache.ai/)
- [LMCache Quickstart Guide](https://docs.lmcache.ai/getting_started/quickstart.html)
- Hugging Face CLI


# Installation

## Create Python env &install LMCache Python Package
```bash
uv venv --python 3.12
source .venv/bin/activate
uv pip install lmcache vllm

```

## Hugginface CLI installation

```bash
# Install the Hugging face fackage
uv pip install huggingface_hub

# create new token in Hf ( settings/new token) and paste after typyg the following command line
hf auth login
```

## LMCache USage

see Documentation (https://docs.lmcache.ai/getting_started/quickstart.html)
Open two Linux sessions:
In the first session - Launch the server:
```bash
# With TinyLlama 1.1B 
LMCACHE_CHUNK_SIZE=8 vllm serve TinyLlama/TinyLlama-1.1B-Chat-v1.0 --port 8000 \
  --kv-transfer-config '{"kv_connector":"LMCacheConnectorV1", "kv_role":"kv_both"}' \
  --gpu-memory-utilization 0.7

# Or with Phi-3 Mini
LMCACHE_CHUNK_SIZE=8 vllm serve microsoft/Phi-3-mini-4k-instruct --port 8000 \
  --kv-transfer-config '{"kv_connector":"LMCacheConnectorV1", "kv_role":"kv_both"}'

# Or with Llama-3.1-8B and quantization
LMCACHE_CHUNK_SIZE=8 vllm serve meta-llama/Llama-3.1-8B-Instruct --port 8000 \
  --kv-transfer-config '{"kv_connector":"LMCacheConnectorV1", "kv_role":"kv_both"}' \
  --gpu-memory-utilization 0.7 \
  --quantization bitsandbytes

# Or with Llama-3.1-8B and quantization with reduced memory (requires Meta access)
LMCACHE_CHUNK_SIZE=8 vllm serve meta-llama/Llama-3.1-8B-Instruct --port 8000 \
  --kv-transfer-config '{"kv_connector":"LMCacheConnectorV1", "kv_role":"kv_both"}' \
  --gpu-memory-utilization 0.6 \
  --quantization awq

# Or with Qwen2.5 7B 
LMCACHE_CHUNK_SIZE=8 vllm serve Qwen/Qwen2.5-7B-Instruct --port 8000 \
  --kv-transfer-config '{"kv_connector":"LMCacheConnectorV1", "kv_role":"kv_both"}' \
  --gpu-memory-utilization 0.7 \
  --quantization bitsandbytes

```
n the second session - Test the API:
```bash
 curl http://localhost:8000/v1/completions   -H "Content-Type: application/json"   -d '{
    "model": "TinyLlama/TinyLlama-1.1B-Chat-v1.0",
    "prompt": "Hello, my name is olivier",
    "max_tokens": 50
  }'
```

## 🐛 Troubleshooting
Clear Memory

```bash
pkill -f python
```
Check GPU Memory
```bash
nvidia-smi
```

WSL2 Specific
```bash
Restart WSL if needed
wsl --shutdown
```

## 📊 Supported Models
a lot from Huggingface
Model	Size	GPU Memory	Status
TinyLlama	1.1B	~2GB	✅ Tested
Qwen2.5 1.5B	1.5B	~3GB	✅ Recommended
Qwen2.5 7B	7B	~7GB	✅ Recommended

## 🔧 API Documentation

Interactive API documentation available at:
```bash
 http://localhost:8000/docs
 ```