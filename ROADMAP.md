# ROADMAP — Portfolio ML / LLM fine-tuning

> **Repo de contrôle** : ce fichier vit dans `Meuleur/AIproject`. Il orchestre tous mes projets ML.
> **Architecture** : un repo GitHub par projet (multi-repo). Les commits sont distribués sur des dizaines de repos publics.
>
> **GitHub user** : `Meuleur`
> **Racine locale** : `~/Desktop/Dev/`

## Convention multi-repo

Chaque projet a un bloc de métadonnées suivi de ses tâches :

```
### Projet N — <slug>
- **Repo**   : Meuleur/<slug>
- **Local**  : ~/Desktop/Dev/<slug>
- **Statut** : not started | in progress | done

- [ ] tâche 1
- [ ] tâche 2
```

Le slug est exactement le nom du repo GitHub. Quand `/go` rencontre un projet `not started`, il crée le repo GitHub (`gh repo create Meuleur/<slug> --public --clone`) et le clone à côté du repo de contrôle.

---

## Legacy (à terminer ou migrer)

### Projet 0 — `sentiment-analyzer`
- **Repo**   : Meuleur/sentiment-analyzer
- **Local**  : ~/Desktop/Dev/sentiment-analyzer
- **Statut** : in progress

- [x] Bootstrap (structure dossier + README + smoke test)
- [x] Migrer le projet vers `Meuleur/sentiment-analyzer` (extraire `projects/sentiment-analyzer/` → nouveau repo)
- [x] Loader Allociné review subset depuis HuggingFace + test
- [x] Preprocessing FR (nettoyage, normalisation accents)
- [x] Tokenisation BPE Hugging Face
- [x] Baseline TF-IDF + LogReg + métriques
- [ ] Fine-tuning Camembert-base avec `transformers.Trainer` <!-- needs GPU -->
- [ ] LoRA sur Camembert via peft <!-- needs GPU -->
- [ ] QLoRA (bnb-4bit) sur Camembert <!-- needs GPU -->
- [ ] BENCHMARK.md (4 approches comparées) <!-- needs GPU (dépend des runs) -->
- [x] CLI d'inférence

---

## Projets phares — fine-tuning de petits LM

### Projet 1 — `lora-lab`
- **Repo**   : Meuleur/lora-lab
- **Local**  : ~/Desktop/Dev/lora-lab
- **Statut** : in progress

- [x] Bootstrap repo (README, requirements.txt, structure src/tests/runs)
- [x] Loader unifié de datasets instruct (Alpaca-FR, OpenAssistant-FR)
- [x] Script paramétrable de fine-tuning LoRA avec `trl.SFTTrainer`
- [x] Run LoRA sur TinyLlama-1.1B (r=8, alpha=16) (#1)
- [x] Run LoRA sur Qwen2.5-0.5B (100 steps MPS sur M1 Pro, voir lora-lab/BENCHMARK.md)
- [ ] Run LoRA sur Qwen2.5-1.5B (#2) <!-- MPS OK (lent) -->
- [ ] Run LoRA sur Phi-3-mini (#3) <!-- MPS OK (lent) -->
- [ ] Variante QLoRA (bnb-4bit) sur les 4 modèles (#4) <!-- needs CUDA (bnb) -->
- [ ] Sweep r ∈ {4, 8, 16, 32} (#5) <!-- MPS OK (lent) -->
- [ ] Sweep alpha ∈ {8, 16, 32, 64} (#6) <!-- MPS OK (lent) -->
- [x] LoRA sur attention only vs attention + MLP (#7)
- [x] Merge LoRA → modèle standalone + sauvegarde
- [x] Export GGUF (llama.cpp) (#8)
- [x] BENCHMARK.md complet (#9) (3 runs réels : Qwen0.5B attn-only, Qwen0.5B attn+MLP, TinyLlama 1.1B)
- [x] README de recommandations pratiques (#10)

### Projet 2 — `preference-tuning`
- **Repo**   : Meuleur/preference-tuning
- **Local**  : ~/Desktop/Dev/preference-tuning
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Loader de dataset de préférences (Anthropic HH ou Argilla DPO-mix-FR)
- [ ] Format chosen/rejected
- [ ] DPO via `trl.DPOTrainer` sur Qwen2.5-0.5B-SFT
- [ ] Évaluation win-rate vs modèle SFT
- [ ] ORPO via `trl.ORPOTrainer`
- [ ] DPO vs ORPO (qualité, temps, VRAM)
- [ ] Variante KTO
- [ ] DPO + LoRA vs DPO full
- [ ] BENCHMARK.md + README

### Projet 3 — `nano-llm-fr`
- **Repo**   : Meuleur/nano-llm-fr
- **Local**  : ~/Desktop/Dev/nano-llm-fr
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Tokenizer BPE entraîné from scratch sur sous-corpus FR
- [ ] Multi-head attention from scratch (sans `nn.MultiheadAttention`)
- [ ] Transformer block (pre-norm, FFN, residuals)
- [ ] Embeddings + positional sinusoïdal
- [ ] Modèle complet ~10M params + test shapes
- [ ] DataLoader streaming sur Wikipedia FR
- [ ] Training loop (mixed precision, grad accumulation, checkpoints)
- [ ] Run d'entraînement (~100M tokens) + log courbes
- [ ] Sampling : greedy, top-k, top-p, temperature
- [ ] Variante RoPE
- [ ] Variante GQA
- [ ] Variante SwiGLU
- [ ] Notebook `analysis.ipynb` (visualisation attention heads)
- [ ] README + courbes

### Projet 4 — `distillation-pipeline`
- **Repo**   : Meuleur/distillation-pipeline
- **Local**  : ~/Desktop/Dev/distillation-pipeline
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Setup teacher (Mistral-7B-Instruct) + student (Qwen2.5-0.5B)
- [ ] Loss KL + cross-entropy hard
- [ ] Temperature scaling
- [ ] Training loop mixed precision
- [ ] Évaluation student vs SFT-classique vs teacher
- [ ] Variante top-k distillation (économie VRAM)
- [ ] BENCHMARK.md + README

### Projet 5 — `quantization-bench`
- **Repo**   : Meuleur/quantization-bench
- **Local**  : ~/Desktop/Dev/quantization-bench
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Modèle de réf : Qwen2.5-1.5B-Instruct fp16
- [ ] Quantize bnb-8bit + bnb-4bit
- [ ] Quantize GPTQ-4bit (optimum)
- [ ] Quantize AWQ-4bit
- [ ] Convert GGUF (Q4_K_M, Q5_K_M, Q8_0)
- [ ] Mesures : taille, VRAM peak, tokens/s, perplexity wikitext-fr
- [ ] Évaluation qualitative 20 prompts
- [ ] BENCHMARK.md
- [ ] README

### Projet 6 — `mini-evals`
- **Repo**   : Meuleur/mini-evals
- **Local**  : ~/Desktop/Dev/mini-evals
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Architecture : Task / Runner / Reporter
- [ ] Task MMLU-mini (100 questions)
- [ ] Task ARC-easy-mini
- [ ] Task HellaSwag-mini
- [ ] Task French-QA-mini (50 questions custom)
- [ ] Task HumanEval-mini (10 problèmes code)
- [ ] Runner avec batching + resume
- [ ] Reporter JSON + Markdown
- [ ] Vue comparative multi-modèles
- [ ] CLI
- [ ] README

---

## Implémentations from-scratch

### Projet 7 — `transformer-from-scratch`
- **Repo**   : Meuleur/transformer-from-scratch
- **Local**  : ~/Desktop/Dev/transformer-from-scratch
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Scaled dot-product attention + test
- [ ] Multi-head attention + test
- [ ] FFN position-wise + test
- [ ] Layer norm manuel + test d'équivalence avec `nn.LayerNorm`
- [ ] Residual + dropout
- [ ] Encoder block
- [ ] Decoder block (masked attention + cross attention)
- [ ] Positional sinusoïdal
- [ ] RoPE
- [ ] ALiBi
- [ ] Tests d'équivalence numérique avec HF
- [ ] Mini-training tâche jouet (reverse string)
- [ ] README pédagogique avec équations

### Projet 8 — `tokenizer-bpe-from-scratch`
- **Repo**   : Meuleur/tokenizer-bpe-from-scratch
- **Local**  : ~/Desktop/Dev/tokenizer-bpe-from-scratch
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] BPE training (count pairs, merge, vocab)
- [ ] Encode / decode
- [ ] Comparaison output avec `tokenizers.ByteLevelBPETokenizer`
- [ ] Variante byte-level
- [ ] CLI d'entraînement
- [ ] README

### Projet 9 — `decoding-strategies`
- **Repo**   : Meuleur/decoding-strategies
- **Local**  : ~/Desktop/Dev/decoding-strategies
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] KV cache manuel
- [ ] Greedy
- [ ] Beam search (k=2, 4, 8)
- [ ] Top-k
- [ ] Top-p (nucleus)
- [ ] Temperature
- [ ] Min-p
- [ ] Speculative decoding
- [ ] Bench vitesse avec/sans KV cache
- [ ] README

### Projet 10 — `mini-moe`
- **Repo**   : Meuleur/mini-moe
- **Local**  : ~/Desktop/Dev/mini-moe
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Router top-2
- [ ] Experts FFN
- [ ] Load balancing loss
- [ ] Intégration dans mini-transformer
- [ ] Comparaison MoE vs dense même budget
- [ ] README

---

## ML classique from-scratch

### Projet 11 — `ml-from-scratch`
- **Repo**   : Meuleur/ml-from-scratch
- **Local**  : ~/Desktop/Dev/ml-from-scratch
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] linear_regression.py (normal eq + GD) + test
- [ ] logistic_regression.py (binaire + softmax) + test
- [ ] knn.py (+ KD-tree) + test
- [ ] k_means.py (Lloyd + k-means++) + test
- [ ] gmm.py (EM) + test
- [ ] pca.py (eig + SVD) + test
- [ ] decision_tree.py (ID3 + CART) + test
- [ ] random_forest.py (bagging) + test
- [ ] gradient_boosting.py (GBM régression) + test
- [ ] naive_bayes.py (gaussien + multinomial) + test
- [ ] svm.py (SMO + kernels) + test
- [ ] neural_net.py (MLP backprop numpy) + test
- [ ] hmm.py (Forward, Viterbi, Baum-Welch) + test
- [ ] lda.py + test
- [ ] tsne.py + test
- [ ] optimizers.py (SGD, Adam, AdamW, etc.) + comparaisons
- [ ] notebook comparison.ipynb

---

## Outils CLI

### Projet 12 — `llm-eval-cli`
- **Repo**   : Meuleur/llm-eval-cli
- **Local**  : ~/Desktop/Dev/llm-eval-cli
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Wrapper léger autour de `lm-evaluation-harness`
- [ ] Config YAML
- [ ] Export Markdown
- [ ] Comparaison multi-modèles

### Projet 13 — `dataset-dedup`
- **Repo**   : Meuleur/dataset-dedup
- **Local**  : ~/Desktop/Dev/dataset-dedup
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Dédup exacte par hash
- [ ] MinHash + LSH
- [ ] CLI

### Projet 14 — `tokenizer-compare`
- **Repo**   : Meuleur/tokenizer-compare
- **Local**  : ~/Desktop/Dev/tokenizer-compare
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Compare N tokenizers sur même texte
- [ ] Métriques : nb tokens, bytes/token, longest
- [ ] Visualisation HTML

### Projet 15 — `inference-bench`
- **Repo**   : Meuleur/inference-bench
- **Local**  : ~/Desktop/Dev/inference-bench
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Bench tokens/s, TTFT, VRAM
- [ ] Backends : transformers, vLLM (subprocess), llama.cpp
- [ ] Sortie CSV

### Projet 16 — `model-card-generator`
- **Repo**   : Meuleur/model-card-generator
- **Local**  : ~/Desktop/Dev/model-card-generator
- **Statut** : not started

- [ ] Bootstrap repo
- [ ] Lit run de fine-tuning + génère README HF-ready
- [ ] Inclut metrics, dataset, hyperparams, hardware

---

## Convention de qualité (rappel)

- Une coche `[x]` = tâche **réellement terminée + commit + push** sur le bon repo
- Pas de commit cosmétique
- Tests pour toute fonction non triviale
- Pas de secrets, pas de modèles lourds (Git LFS si vraiment besoin)
- Conventional Commits en anglais
