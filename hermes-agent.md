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
  
     






