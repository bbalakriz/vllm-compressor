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
```
pip install guidellm
guidellm benchmark   --target "http://localhost:8000"   --rate-type sweep   --max-seconds 30   --data "prompt_tokens=256,output_tokens=128"
```
```
pip install lm-eval
lm-eval   --model vllm   --model_args "pretrained=balakriz/DeepSeek-R1-0528-Qwen3-8B-W4A16-11jul,max_model_len=1000"   --tasks sst2   --device cuda   --batch_size 1   --output_path ./results/cola_summary   --log_samples
```
