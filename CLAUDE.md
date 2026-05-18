# Contexte du repo — pour Claude Code

Ce repo est un monorepo de projets d'IA construits incrémentalement.
Chaque projet vit dans `projects/<nom-du-projet>/`. Le plan global est dans `ROADMAP.md`.

## Comment je bosse

- Mon workflow quotidien : je lance `claude` à la racine, je tape `/go`, et tu fais avancer le projet actif d'un cran.
- Un commit = une vraie contribution. Pas de remplissage cosmétique du graphe GitHub.
- Je préfère la **qualité** à la quantité. Mieux vaut zéro commit aujourd'hui qu'un commit bidon.

## Conventions techniques

**Python** (par défaut pour les projets ML) :
- Version 3.11+
- Formatter : `black` (line length 100)
- Imports : `isort`
- Linting : `ruff`
- Tests : `pytest`
- Type hints partout où c'est raisonnable
- Docstrings style Google ou NumPy, en français OK

**JavaScript / TypeScript** (pour les API et frontends légers) :
- Préférer TypeScript dès qu'il y a >50 lignes
- Tests : `vitest`
- Formatter : `prettier`

**Structure type d'un projet** :
```
projects/<nom>/
  README.md          # but, install, usage
  requirements.txt   # ou pyproject.toml
  src/               # code source
  tests/             # tests pytest
  data/              # ignoré par git si lourd
  notebooks/         # exploration (optionnel)
```

## Conventions Git

- Branche principale : `main`
- Messages : **Conventional Commits en anglais**
  - `feat(<projet>): ...` pour une nouvelle feature
  - `fix(<projet>): ...` pour un bug
  - `refactor(<projet>): ...`, `docs(<projet>): ...`, `test(<projet>): ...`, `chore: ...`
- Un commit = un changement cohérent. Pas de commit-fourre-tout.

## Choses à **ne jamais** faire

- Commit des secrets / clés API / fichiers `.env`
- Commit de modèles lourds (`*.pt`, `*.h5`, `*.bin` > 50 Mo) → utiliser Git LFS si vraiment besoin, sinon documenter comment télécharger
- Commit de notebooks avec outputs lourds → `jupyter nbconvert --clear-output` avant
- Pusher sur `main` sans avoir tourné les tests
- Inventer des résultats de benchmark sans avoir lancé le code
- Laisser du code mort, des TODOs vagues, ou des `print()` de debug

## Quand tu ne sais pas

- **Pose-moi la question.** Mieux vaut une question que un mauvais choix de stack.
- Si je ne suis pas dispo et qu'il faut trancher, choisis l'option la plus **standard** et **bien documentée** dans l'écosystème, pas la plus exotique.

## Stack préférée par domaine

- NLP : Hugging Face Transformers, sentence-transformers, spaCy
- Vision : PyTorch + torchvision, timm
- Series temporelles : statsmodels, prophet, neuralforecast
- Vector DB : chromadb (local), pgvector (prod)
- LLM local : Ollama, llama.cpp
- API : FastAPI
- Frontend léger : Streamlit ou Gradio (pour les démos), sinon HTML+htmx
