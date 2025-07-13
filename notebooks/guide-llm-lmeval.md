## Set up vLLM
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
```
```
sudo apt update
sudo apt install build-essential python3.12-dev
python3.12 -m venv ~/venv
source ~/venv/bin/activate
pip install --upgrade pip
pip install vllm
vllm serve balakriz/DeepSeek-R1-0528-Qwen3-8B-W4A16-11jul --max_model_len=1024 --no-multi-step-stream-outputs
```

##Test vLLM
```
curl -X POST http://localhost:8000/v1/completions   -H "Content-Type: application/json"   -d '{
    "model": "balakriz/DeepSeek-R1-0528-Qwen3-8B-W4A16-11jul",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant. **Do not** show your chain‑of‑thought. Only return the final answer."},
      {"role": "user",   "content": "What is machine learning?"}
    ],
    "max_tokens": 800
  }' | jq .
```

## Setup Guidellm and do benchmark on the model
```
pip install guidellm
guidellm benchmark   --target "http://localhost:8000"   --rate-type sweep   --max-seconds 30   --data "prompt_tokens=256,output_tokens=128"
```

## Setup lm-eval and evaluate the model
```
pip install lm-eval
lm-eval   --model vllm   --model_args "pretrained=balakriz/DeepSeek-R1-0528-Qwen3-8B-W4A16-11jul,max_model_len=1000"   --tasks sst2   --device cuda   --batch_size 1   --output_path ./results/cola_summary   --log_samples
```
