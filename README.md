# AIproject — centre de contrôle de mon portfolio ML/LLM

Repo "central" qui orchestre une dizaine de projets de fine-tuning, d'implémentations from-scratch et d'algos ML, chacun dans son **propre repo GitHub**.

Le plan complet est dans [`ROADMAP.md`](./ROADMAP.md).

## Projets

| Projet | Repo dédié | Focus | Statut |
|---|---|---|---|
| `sentiment-analyzer` | (legacy, ici) | NLP FR + LoRA | en cours |
| `lora-lab` | [Meuleur/lora-lab](https://github.com/Meuleur/lora-lab) | LoRA/QLoRA bench | à venir |
| `preference-tuning` | [Meuleur/preference-tuning](https://github.com/Meuleur/preference-tuning) | DPO/ORPO | à venir |
| `nano-llm-fr` | [Meuleur/nano-llm-fr](https://github.com/Meuleur/nano-llm-fr) | mini-GPT from scratch | à venir |
| `distillation-pipeline` | [Meuleur/distillation-pipeline](https://github.com/Meuleur/distillation-pipeline) | teacher → student | à venir |
| `quantization-bench` | [Meuleur/quantization-bench](https://github.com/Meuleur/quantization-bench) | GPTQ/AWQ/GGUF | à venir |
| `mini-evals` | [Meuleur/mini-evals](https://github.com/Meuleur/mini-evals) | framework d'eval | à venir |
| `transformer-from-scratch` | [Meuleur/transformer-from-scratch](https://github.com/Meuleur/transformer-from-scratch) | attention, blocks, RoPE | à venir |
| `tokenizer-bpe-from-scratch` | [Meuleur/tokenizer-bpe-from-scratch](https://github.com/Meuleur/tokenizer-bpe-from-scratch) | BPE pédagogique | à venir |
| `decoding-strategies` | [Meuleur/decoding-strategies](https://github.com/Meuleur/decoding-strategies) | KV cache, sampling | à venir |
| `mini-moe` | [Meuleur/mini-moe](https://github.com/Meuleur/mini-moe) | Mixture of Experts | à venir |
| `ml-from-scratch` | [Meuleur/ml-from-scratch](https://github.com/Meuleur/ml-from-scratch) | 15+ algos numpy | à venir |
| `llm-eval-cli` | [Meuleur/llm-eval-cli](https://github.com/Meuleur/llm-eval-cli) | CLI d'eval | à venir |
| `dataset-dedup` | [Meuleur/dataset-dedup](https://github.com/Meuleur/dataset-dedup) | MinHash + LSH | à venir |
| `tokenizer-compare` | [Meuleur/tokenizer-compare](https://github.com/Meuleur/tokenizer-compare) | comparateur de tokenizers | à venir |
| `inference-bench` | [Meuleur/inference-bench](https://github.com/Meuleur/inference-bench) | tokens/s, VRAM | à venir |
| `model-card-generator` | [Meuleur/model-card-generator](https://github.com/Meuleur/model-card-generator) | model cards HF-ready | à venir |

> Les liens sont en place mais les repos n'existent pas encore — ils sont créés à la volée par `/go` quand on arrive sur le projet correspondant.

## Workflow

Voir [`SETUP.md`](./SETUP.md) pour l'install. En bref :

```bash
cd ~/Desktop/Dev/ai-projects-starter
claude
> /go
```

`/go` crée le repo manquant, le clone, fait 5 tâches de la roadmap, commit chacune dans le bon repo, met à jour le ROADMAP ici, push tout.

## Stack

Python 3.11+, PyTorch, Hugging Face (transformers/peft/trl), bitsandbytes, llama.cpp, FastAPI.
