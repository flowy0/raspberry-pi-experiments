# Hermes Agent



After watching a few youtube videos on Hermes agent, i decided to try it out on 1 of my rpi5. 

### My RPI5 setup.
Raspberry Pi 5, 8GB Ram, 512 NVME SSD.

# Trials and Tribulations of Small Models on Raspberry Pi 5 8GB

The difficulty only started on Day 2. 


## Day 1
 - Very smooth setup, tried out Kimi K2.5 Cloud model on default settings.
 - Managed to setup a few cron jobs to backup config files, a few news alerts.

## Day 2
 - Ran out of credits for Kimi K2.5
 - Started using openrouter free models, (eg qwen3.6 plus)
 - Started running some experiments on which small models to use (ollama) 

## Day 3
 - Openrouter took qwen3.6 plus off the free tier.
 -  Experimentation of various small models continued (ollama on proxmox vm, minipc)
 -  Ollama(Docker) on the rpi5 with small models were fairly slow or timed out
 - Also tried Ollama(Docker) on the minipc, but it introduce too much lag, the experiments stopped there.
 - Small Models tested:
   - Gemma4:e2b
   - qwen3.5:4b
   - qwen3.5:9b
   - qwen2.5:3b


## Day 4
 - Reset all the configs and restarted again
 - Subscribed to MiniMax Token Starter Plan
 - Continued to research on even smaller models.
 - Smaller Models tested:
  - qwen3.5:0.8b
  - gemma4:e2b 
- Hit the issue with ollama not supporting qwen3.5 architecture and alternative quantized gemma4

## Day 5
 - Used Kimi Code to configure llama.cpp on the RPI5, instead of using ollama.
 - This was required as i wanted to use qwen3.5:0.8b which is a new architecture, not supported on ollama 0.20.5rc, this also added some speed boosts.
 - Did some benchmarking between ollama and llama.cpp for the small models
 - Small Models tested:
   - qwen3.5:0.8b
   - qwen2.5:0.5b
   - gemma4:e2b

**Full Speed Ranking (LLaMA.cpp, Pi 5):**

| Model | Tok/s | TTFT (ms) | Total (s) |
|-------|-------|-----------|-----------|
| qwen2.5:0.5B (32K) | **31.3** | 970 | 8.2 |
| qwen2.5:0.5B (16K) | **30.6** | 1474 | 7.5 |
| qwen2.5:0.5B (8K) | **29.5** | 1453 | 8.2 |
| qwen3.5:0.8B-thinking (8K) | 14.6 | 1520 | 19.5 |
| qwen3.5:2b (8K) | 7.8 | 2401 | 30.1 |
| gemma-4-e2b:8k | 6.5 | 4440 | 43.8 |

**Key findings:**
- **qwen2.5:0.5B is fastest** (~30 tok/s) regardless of context size — the 32K variant has best TTFT (970ms) and throughput
- **qwen3.5:2b is 4x slower** than 0.5B (7.8 vs 30 tok/s) — significant speed cost for quality
- **gemma-4-e2b** still broken on complex prompts, slowest overall
- **Quality winner**: qwen3.5:2b — rich coherent outputs, tool use (Python code), proper multi-paragraph answers

**Recommendation for trading advisor:**
- Speed priority → `qwen2.5:0.5B-32K` (31 tok/s, fast TTFT)
- Quality priority → `qwen3.5:2b` (7.8 tok/s, much richer reasoning)
  
     






