---
description: Mode multi-repo — crée/ouvre le repo du projet actif, enchaîne 5 tâches avec commit par tâche, push à la fin
allowed-tools: Bash(git:*), Bash(gh:*), Bash(cd:*), Bash(python:*), Bash(python3:*), Bash(pip:*), Bash(pip3:*), Bash(pytest:*), Bash(npm:*), Bash(node:*), Bash(npx:*), Bash(ls:*), Bash(cat:*), Bash(mkdir:*), Bash(touch:*), Bash(cp:*), Bash(mv:*), Bash(find:*), Bash(echo:*), Bash(pwd), Bash(test:*), Bash(grep:*), Read, Write, Edit, Glob, Grep, WebFetch
---

# Commande /go — mode multi-repo

Tu travailles avec un **monorepo de contrôle** (ce repo, `Meuleur/AIproject`, qui contient `ROADMAP.md`) et **un repo GitHub dédié par projet** (ex : `Meuleur/lora-lab`, `Meuleur/nano-llm-fr`). Tous les repos vivent en parallèle dans `~/Desktop/Dev/`.

Une session `/go` enchaîne **5 tâches**, fait **un commit par tâche dans le bon repo**, met à jour le ROADMAP dans le repo de contrôle, et push tout.

## 0. Sanity check (max 10 sec)

```bash
gh auth status 2>&1 | head -5    # gh authentifié ?
git config user.name             # identité git ok ?
pwd                              # on est bien dans ~/Desktop/Dev/ai-projects-starter ?
```

Si `gh` n'est pas authentifié → **stop**, dis-moi de lancer `gh auth login`.
Si l'identité git n'est pas configurée → **stop**, dis-moi `git config user.email "mathis34400@gmail.com"`.

## 1. Lecture du ROADMAP

```bash
cat ROADMAP.md | head -100
```

Repère le **premier projet** dont le statut est `in progress` ou `not started` (dans l'ordre du fichier) ET qui a encore des tâches `- [ ]`.

Note ses métadonnées :
- **slug** (nom du repo, ex: `lora-lab`)
- **chemin local** (ex: `~/Desktop/Dev/lora-lab`)
- **statut**
- **5 premières tâches non cochées**

## 2. Préparer le repo de projet (idempotent)

### Cas A — Le projet est `Meuleur/AIproject` lui-même (sentiment-analyzer legacy)

Tu travailles directement dans le repo courant, dans `projects/sentiment-analyzer/`. Pas de création de repo.

### Cas B — C'est un projet dédié, premier passage

```bash
SLUG=<slug>
LOCAL="$HOME/Desktop/Dev/$SLUG"

if [ ! -d "$LOCAL" ]; then
  # Vérifie si le repo GitHub existe déjà
  if gh repo view "Meuleur/$SLUG" >/dev/null 2>&1; then
    # Existe déjà → clone
    gh repo clone "Meuleur/$SLUG" "$LOCAL"
  else
    # N'existe pas → crée puis clone (public, sans README initial)
    gh repo create "Meuleur/$SLUG" --public --description "$SLUG — part of Meuleur/AIproject portfolio" --clone --add-readme=false || \
    gh repo create "Meuleur/$SLUG" --public --description "$SLUG — part of Meuleur/AIproject portfolio"
    # Si --clone n'a pas créé le dossier (cas vieilles versions de gh), clone manuellement
    if [ ! -d "$LOCAL" ]; then
      cd "$HOME/Desktop/Dev"
      gh repo clone "Meuleur/$SLUG"
    fi
  fi
fi
```

Si le dossier local est vide (vient d'être créé), initialise une structure minimale **avant** la première tâche :

```bash
cd "$LOCAL"
[ -f README.md ] || cat > README.md << 'EOF'
# <slug>

<une-phrase-sur-le-projet>

Part of [Meuleur/AIproject](https://github.com/Meuleur/AIproject).
EOF
[ -f .gitignore ] || cp "$HOME/Desktop/Dev/ai-projects-starter/.gitignore" .gitignore
mkdir -p src tests
[ -f requirements.txt ] || touch requirements.txt
```

Et commit ce bootstrap **en premier** (compte comme la première tâche "Bootstrap repo") :

```bash
git add -A
git commit -m "chore: bootstrap <slug> project structure"
git push -u origin main 2>/dev/null || git push -u origin master 2>/dev/null || (git branch -M main && git push -u origin main)
```

### Cas C — Le repo existe déjà localement

```bash
cd "$LOCAL"
git pull --rebase --autostash 2>&1 | tail -5
```

## 3. Boucle d'exécution (5 tâches max)

Pour chaque tâche, dans l'ordre du ROADMAP, **dans le repo de projet** (pas le repo de contrôle) :

### a. Implémente

- Code propre, lisible
- Test pytest pour toute fonction non triviale
- Ajoute les deps à `requirements.txt` (ne `pip install` que si tu en as besoin pour vérifier le code, et si ça prend < 30 sec)
- Pas de TODO vague, termine

### b. Vérifie

```bash
[ -f pytest.ini ] || [ -d tests ] && pytest -q 2>&1 | tail -10 || true
```

Tests rouges à cause de ton changement → corrige avant de commiter.

### c. Commit dans le repo de projet

```bash
git add -A
git commit -m "<type>(<slug>): <résumé court>"
```

Conventional Commits, anglais. `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`.

### d. Coche la tâche dans le ROADMAP DU REPO DE CONTRÔLE

```bash
cd "$HOME/Desktop/Dev/ai-projects-starter"
# Remplace `- [ ] <description>` par `- [x] <description>` (avec Edit tool, plus précis)
cd "$LOCAL"
```

Tu ne commits pas le ROADMAP à chaque tâche — tu fais un seul commit ROADMAP groupé à la fin (étape 4).

### e. Tâche suivante

Pas de récap, pas de pause. Tu enchaînes.

## 4. Finalisation

### a. Push le repo de projet

```bash
cd "$LOCAL"
git push
```

Si rebase nécessaire : `git pull --rebase && git push`.

### b. Commit + push le ROADMAP mis à jour dans le repo de contrôle

```bash
cd "$HOME/Desktop/Dev/ai-projects-starter"
git add ROADMAP.md
git commit -m "chore(roadmap): mark N tasks done in <slug>"
git push
```

### c. Mise à jour du statut

Si toutes les tâches du projet sont cochées, change son **statut** dans ROADMAP.md à `done` (dans le même commit que ci-dessus).
Si tu viens de démarrer un projet, change son statut de `not started` à `in progress`.

## 5. Règles non négociables

- **Aucun commit vide ou cosmétique** : si une "tâche" est triviale au point de ne rien changer, saute-la et coche-la quand même.
- **Aucun secret commité** (`.env`, `*.key`, modèles `> 50MB`).
- **Pas de force push**.
- **Pas de `gh repo delete` ni `git push --force`**.
- Si une tâche nécessite un GPU et que tu n'en as pas accès : marque-la `<!-- needs GPU -->` dans le ROADMAP, saute-la, passe à la suivante.

## 6. Compte-rendu final

Une seule fois, à la toute fin :

```
Session     : <date>
Projet      : <slug> (https://github.com/Meuleur/<slug>)
Tâches      : <N> terminées
Commits     :
  - <hash> <message>
  - <hash> <message>
  ...
Tests       : <X passed / Y failed / aucun>
Push        : OK projet + OK contrôle
Prochaine   : <prochaine tâche du roadmap>
```

C'est tout. Pas de blabla.
