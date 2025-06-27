In a machine with nvidia cuda access, run the following. 

Note the params `--userns=keep-id:uid`, `max_model_len`, `gpu_memory_utilization` in the podman run

```
mkdir -p rhaiis-cache/{flashinfer,huggingface,torch,vllm}
chmod a+rwX rhaiis-cache

export HF_TOKEN=<<your_token>>
podman run --rm -it --device nvidia.com/gpu=all --userns=keep-id:uid=1001 \
        --security-opt=label=disable --shm-size=4GB -p 8000:8000 --env "HUGGING_FACE_HUB_TOKEN=$HF_TOKEN" \
        --env "HF_HUB_OFFLINE=0" --env=VLLM_NO_USAGE_STATS=1 -v ./rhaiis-cache:/opt/app-root/src/.cache \
        registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 --model balakriz/DeepSeek-R1-0528-Qwen3-8B-W4A16-G128
        --max_model_len=2048 --gpu_memory_utilization=0.9
```
