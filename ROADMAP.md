# ROADMAP — Projets IA

> Un monorepo de projets IA construits incrémentalement. Chaque tâche non cochée représente environ une session journaliere de `/go`. Les projets sont rangés du plus simple au plus avancé : on termine (ou stabilise) un projet avant d'attaquer le suivant.

---

## Projet 1 — `sentiment-analyzer` (analyseur de sentiments FR)

Classifier des phrases françaises en positif / négatif / neutre, du baseline jusqu'à un modèle transformer fine-tuné.

- [ ] Bootstrap : créer `projects/sentiment-analyzer/`, README, structure de dossiers (`data/`, `src/`, `tests/`), `requirements.txt` minimal
- [ ] Charger un dataset français (Allociné review subset ou équivalent) — script `src/load_data.py` + test
- [ ] Préprocessing : nettoyage texte, normalisation accents, tokenisation simple
- [ ] Baseline TF-IDF + Logistic Regression — `src/baseline.py` + métriques (accuracy, F1)
- [ ] Sauvegarde / chargement du modèle baseline (pickle ou joblib)
- [ ] CLI simple : `python -m src.predict "j'ai adoré ce film"` → label
- [ ] Ajouter un word embedding moyen (fastText FR) comme deuxième baseline
- [ ] Fine-tuning d'un modèle Hugging Face (camembert-base) sur le même dataset
- [ ] Comparer les 3 approches dans un `BENCHMARK.md` (tableaux, courbes)
- [ ] Wrapper FastAPI : POST `/predict` → JSON
- [ ] Dockerfile + test que le conteneur démarre
- [ ] README final avec exemples d'usage, limites, idées d'amélioration

## Projet 2 — `rag-chatbot` (chatbot RAG sur tes propres docs)

Un chatbot qui répond à partir d'une base de documents (PDF, MD, TXT). Architecture RAG classique.

- [ ] Bootstrap `projects/rag-chatbot/` + README + structure
- [ ] Loader de documents (PDF via pypdf, MD, TXT) → liste de chunks
- [ ] Chunking : sliding window avec overlap, paramétrable
- [ ] Embeddings : `sentence-transformers` (modèle multilingue) + cache local
- [ ] Vector store : chromadb en local, persisté sur disque
- [ ] Recherche top-k avec score de similarité
- [ ] Prompt template pour grounding (réponds uniquement à partir du contexte fourni)
- [ ] Wrapper LLM : interface abstraite + implémentation Ollama (local, gratuit)
- [ ] CLI interactive : `python -m src.chat`
- [ ] Évaluation simple : 10 Q/R préparées + comparaison sortie/attendu
- [ ] Streaming des réponses dans la CLI
- [ ] Interface Gradio ou Streamlit basique
- [ ] README + démo (screenshot ou GIF)

## Projet 3 — `image-classifier` (vision : classification d'images)

Classifier des images dans plusieurs catégories. Du zéro au transfer learning.

- [ ] Bootstrap `projects/image-classifier/`
- [ ] Dataset : CIFAR-10 ou Fashion-MNIST (téléchargement automatique)
- [ ] Data loader PyTorch + transforms (resize, normalisation)
- [ ] Petit CNN from scratch (3 conv blocks) → train 5 epochs, log loss/accuracy
- [ ] Sauvegarde du modèle + script d'inférence sur une image
- [ ] Data augmentation (flip, rotation, color jitter) — comparer avant/après
- [ ] Transfer learning depuis ResNet18 pré-entraîné
- [ ] Comparer CNN custom vs ResNet18 dans un BENCHMARK.md
- [ ] Grad-CAM pour visualiser ce que le modèle regarde
- [ ] Mini-API FastAPI : POST une image → label + confiance
- [ ] Frontend HTML minimal pour uploader une image et voir le résultat
- [ ] README final

## Projet 4 — `time-series-forecaster` (prédiction de séries temporelles)

Prédire des séries temporelles (cours boursier, météo, ventes) avec plusieurs approches.

- [ ] Bootstrap projet
- [ ] Chargement d'un dataset publi (e.g. yfinance pour le BTC ou un dataset Kaggle météo)
- [ ] EDA : décomposition tendance/saisonnalité, autocorrélation, visualisations
- [ ] Baseline naïve (valeur précédente, moyenne mobile)
- [ ] ARIMA / SARIMA via statsmodels
- [ ] LSTM PyTorch
- [ ] Prophet de Meta
- [ ] N-BEATS ou Temporal Fusion Transformer
- [ ] Comparatif des 5 approches sur les mêmes métriques (MAE, RMSE, MAPE)
- [ ] Backtest walk-forward
- [ ] Notebook explicatif `analysis.ipynb`
- [ ] README

## Projet 5 — `mini-agent-framework` (framework d'agents minimaliste)

Un framework d'agents LLM en moins de 1000 lignes : tool use, mémoire, planification simple.

- [ ] Bootstrap projet
- [ ] Interface `Tool` (description, schéma d'args, exécution)
- [ ] Registry de tools + résolution par nom
- [ ] Tools de base : `read_file`, `write_file`, `run_python`, `web_search` (stub d'abord)
- [ ] Boucle agent : LLM call → parse tool call → execute → feedback
- [ ] Mémoire courte (historique de la conversation) + truncation par tokens
- [ ] Mémoire longue (vector store, réutilise le code de rag-chatbot)
- [ ] Planification : décomposition d'une tâche en sous-tâches avant exécution
- [ ] Tracing : log structuré de chaque étape pour debug
- [ ] CLI : `python -m src.agent "écris un fichier qui calcule Fibonacci"`
- [ ] Tests d'intégration sur 3 tâches simples
- [ ] Doc d'archi dans `ARCHITECTURE.md`
- [ ] README + exemples

## Projet 6 — `llm-fine-tuner` (fine-tuning LLM avec LoRA)

Pipeline complet pour fine-tuner un petit LLM avec LoRA/QLoRA sur un dataset custom.

- [ ] Bootstrap projet
- [ ] Génération d'un petit dataset instruct (Q/R en français, JSON-lines)
- [ ] Loader du dataset + split train/eval
- [ ] Tokenisation + formatage en chat template
- [ ] Setup LoRA via peft sur un modèle 1B-3B (TinyLlama, Phi-3-mini)
- [ ] Boucle de training avec `accelerate` ou `trl`
- [ ] Évaluation : perplexity + 5 prompts qualitatifs
- [ ] Export et merge des poids LoRA → modèle final
- [ ] Quantization GGUF pour inférence locale (llama.cpp)
- [ ] Script d'inférence simple
- [ ] README + benchmarks before/after

---

## Projets « bonus » (quand les 6 premiers sont stables)

- `recommender-system` — recos films/livres, collaborative filtering puis hybride
- `audio-classifier` — classifier des sons (ESC-50), spectrogrammes + CNN
- `gan-toy` — DCGAN sur MNIST pour comprendre les GANs
- `diffusion-from-scratch` — implémenter un mini-DDPM pédagogique
- `multi-modal-search` — recherche texte ↔ image avec CLIP
- `reinforcement-learning-cartpole` — DQN puis PPO sur CartPole / LunarLander

---

## Convention

- Une coche `[x]` = tâche réellement terminée et committée
- On ne saute pas de tâche sans raison — si une étape devient inutile, on la supprime explicitement avec un commit
- Si une tâche est trop grosse, on la **scinde** en sous-tâches sous la tâche d'origine
