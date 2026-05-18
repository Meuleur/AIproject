---
description: Avance d'un cran sur le projet du jour (analyse → implémentation → commit → push)
allowed-tools: Bash(git:*), Bash(gh:*), Bash(python:*), Bash(pip:*), Bash(pytest:*), Bash(npm:*), Bash(node:*), Read, Write, Edit, Glob, Grep, WebFetch
---

# Commande /go — contribution quotidienne

Tu es chargé de faire **avancer d'un cran** mes projets d'IA. Une seule règle d'or : **chaque commit doit avoir une vraie valeur ajoutée**. Pas de commit vide, pas de "fix typo" pour la forme, pas de fichier généré pour rien.

## 1. État des lieux (max 60 sec)

Exécute en parallèle :

```bash
git status
git log --oneline -10
git branch --show-current
```

Puis lis `ROADMAP.md` à la racine. Repère :
- Le **projet actif** (premier projet non terminé)
- La **prochaine tâche non cochée** (`- [ ]`) de ce projet

S'il n'y a pas de ROADMAP.md, ou si tous les projets sont terminés, **arrête-toi** et demande-moi quoi faire.

## 2. Choix de la tâche

Prends la **prochaine tâche non cochée** du projet actif. **N'en prends qu'UNE seule** — l'objectif est un commit par jour, pas une session de 4h.

Si la tâche te semble trop grosse pour tenir dans une seule session honnête (>~150 lignes de code utiles), découpe-la en sous-tâches dans le ROADMAP et ne fais que la première.

## 3. Implémentation

- Code propre, lisible, commenté en français là où c'est utile
- **Toujours** au moins un test unitaire si tu ajoutes une fonction (pytest pour Python, vitest/jest pour JS)
- Si tu ajoutes une dépendance, mets-la dans `requirements.txt` / `package.json`
- Si le projet n'existe pas encore physiquement, crée son dossier `projects/<nom-du-projet>/` avec un `README.md` minimal qui explique son but
- Pas de code "à compléter plus tard", pas de TODO en pagaille — termine ce que tu commences

## 4. Vérification avant commit

```bash
# Lance les tests si présents
pytest projects/<nom-du-projet>/ -q 2>/dev/null || true
```

Si des tests échouent à cause de ton changement : **corrige avant de commiter**. Si tu n'arrives pas à corriger, n'invente rien — reverte et raconte-moi pourquoi.

## 5. Commit + push

Convention de commit : **Conventional Commits en anglais**.

```bash
git add -A
git commit -m "feat(<projet>): <résumé court de l'apport>" -m "<2-3 lignes de contexte si utile>"
git push
```

Exemples valides :
- `feat(sentiment-analyzer): add tokenizer with French stopwords`
- `test(rag-chatbot): cover empty-context edge case`
- `refactor(image-classifier): extract data augmentation into separate module`
- `docs(agent-framework): document the tool registry API`

**Ne commit jamais** :
- des fichiers `.env`, des clés API, des modèles `.pt`/`.h5` lourds
- des artefacts générés (`__pycache__/`, `node_modules/`, `.DS_Store`)
- du code mort ou commenté "au cas où"

Assure-toi qu'un `.gitignore` raisonnable existe ; sinon ajoute-le dans le même commit.

## 6. Mise à jour du ROADMAP

Coche la tâche que tu viens de finir dans `ROADMAP.md` (`- [x]`) et inclus cette mise à jour dans le **même commit**.

Si tu as découvert en chemin des sous-tâches utiles, ajoute-les sous la tâche courante.

## 7. Compte-rendu

Termine ta réponse par un bloc court de la forme :

```
Projet     : <nom>
Tache      : <ce que tu as fait>
Commit     : <hash court> — <message>
Tests      : <X passed / Y failed / aucun>
Suivant    : <prochaine tache du roadmap>
```

C'est tout. Pas d'enrobage marketing, pas de "j'espère que ça aide". Du concret.
