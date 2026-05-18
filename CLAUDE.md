# Contexte du repo — pour Claude Code

Ce repo (`Meuleur/AIproject`) est le **centre de contrôle** de mon portfolio ML/LLM.
Il contient :
- `ROADMAP.md` : la liste de tous les projets et tâches
- `.claude/commands/go.md` : la slash command `/go` qui exécute le travail quotidien
- `projects/sentiment-analyzer/` : un projet legacy hébergé dans ce même repo (à migrer)

## Architecture multi-repo

Chaque autre projet vit dans **son propre repo GitHub public** sous le compte `Meuleur` (ex: `Meuleur/lora-lab`, `Meuleur/nano-llm-fr`). Les clones locaux sont dans `~/Desktop/Dev/<slug>/`, en frères de ce repo de contrôle.

Workflow type quand je tape `/go` :

1. Claude lit ce ROADMAP
2. Identifie le projet actif (premier avec tâches non cochées)
3. Si le repo GitHub du projet n'existe pas → `gh repo create Meuleur/<slug> --public --clone`
4. cd dans `~/Desktop/Dev/<slug>/` et fait 5 tâches (1 commit par tâche)
5. Push le projet
6. Revient ici, met à jour le ROADMAP, commit, push

## Identité git

- GitHub user : **Meuleur**
- Email : mathis34400@gmail.com
- Tous les commits doivent partir avec cette identité pour compter dans le graphe de contribs.

## Outils requis (sur la machine)

- `git` (déjà installé)
- `gh` (GitHub CLI, authentifié — `gh auth status` doit dire `Logged in`)
- `python` 3.11+ et `pip`
- `node` 20+ et `npm` (pour Claude Code)

## Conventions techniques

**Python** (par défaut) :
- Version 3.11+
- Format : `black` (line length 100), `isort`
- Lint : `ruff`
- Tests : `pytest`
- Type hints partout où raisonnable
- Docstrings Google/NumPy style, OK en français

**JS/TS** (pour API et frontends légers) :
- TS dès > 50 lignes
- Tests : `vitest`
- Format : `prettier`

**Structure type d'un projet (chaque repo)** :
```
<slug>/
  README.md          # but, install, usage
  requirements.txt   # ou pyproject.toml
  src/               # code source
  tests/             # tests pytest
  data/              # ignoré par git si lourd
  notebooks/         # optionnel
  .gitignore         # copié depuis le repo de contrôle
```

## Conventions Git

- Branche principale : `main`
- **Conventional Commits en anglais** :
  - `feat(<slug>): ...` nouvelle feature
  - `fix(<slug>): ...` bug
  - `refactor(<slug>): ...`, `docs(<slug>): ...`, `test(<slug>): ...`, `chore: ...`, `perf(<slug>): ...`
- Un commit = un changement cohérent
- Le `<slug>` dans le commit = nom du projet/repo

## À ne jamais faire

- Commit de secrets (`.env`, `*.key`, `*.pem`)
- Commit de modèles lourds (`*.pt`, `*.h5`, `*.bin` > 50 MB) → utiliser HF Hub ou Git LFS si vraiment nécessaire
- Notebooks avec gros outputs → `jupyter nbconvert --clear-output` avant
- `git push --force` ou `gh repo delete`
- Code mort, TODOs vagues, `print()` de debug
- Inventer des chiffres de benchmark

## Stack préférée par domaine

- LLM / NLP : Hugging Face Transformers, peft, trl, sentence-transformers, datasets
- Quantization : bitsandbytes, optimum, llama.cpp (GGUF)
- Vision : PyTorch + torchvision, timm
- ML classique : scikit-learn pour la référence, mais on **réimplemente from-scratch** quand c'est pédagogique
- Vector DB : chromadb (local), pgvector (prod)
- LLM local : Ollama, llama.cpp
- API : FastAPI
- Frontend léger : Streamlit ou Gradio

## Quand tu ne sais pas

Pose la question. Mieux vaut une question qu'un mauvais choix de stack. Si je suis indisponible, choisis l'option **standard** et **bien documentée**, pas la plus exotique.

## Performance attendue de /go

- Une session enchaîne **5 tâches** par défaut
- Chaque tâche = un commit dans le repo du projet
- À la fin, un commit dans le repo de contrôle pour cocher les tâches
- 6 push max par session : 5 dans le projet (en fait un seul push final dans le projet) + 1 dans le contrôle
- Si Claude finit un projet entier (toutes les cases cochées), il s'arrête sans démarrer le suivant
