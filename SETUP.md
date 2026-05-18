# Setup — `/go` quotidien avec Claude Code

Objectif : créer un repo GitHub où, chaque jour, tu lances `claude` dans le dossier, tu tapes `/go`, et Claude Code fait avancer un projet IA d'un cran, commit, push. **Vraies contributions, pas de remplissage cosmétique.**

---

## Ce que tu vas obtenir

Un repo `ai-projects/` (nom au choix) qui contient :

```
ai-projects/
├── .claude/
│   └── commands/
│       └── go.md          # la slash command /go
├── projects/              # les projets IA, créés au fur et à mesure
├── CLAUDE.md              # briefing permanent pour Claude
├── ROADMAP.md             # la liste des projets et tâches
├── .gitignore
└── README.md              # description publique du repo
```

---

## Étape 1 — Prérequis (une seule fois)

1. **Git** + un compte GitHub.
2. **GitHub CLI** (pratique mais pas obligatoire) :
   ```bash
   brew install gh   # macOS
   gh auth login
   ```
3. **Claude Code** installé :
   ```bash
   npm install -g @anthropic-ai/claude-code
   claude --version
   ```
   Première exécution : `claude` te guidera pour te connecter à ton compte Anthropic.

4. **Python 3.11+** et **node 20+** disponibles dans ton PATH (tu en auras besoin pour les projets).

---

## Étape 2 — Créer le repo

```bash
# Choisis où vivront tes projets
cd ~/Code   # ou ailleurs

# Crée le dossier
mkdir ai-projects && cd ai-projects

# Init git
git init -b main

# Crée le repo distant (public, pour que ça compte dans tes contribs GitHub)
gh repo create ai-projects --public --source=. --remote=origin
```

Si tu ne veux pas utiliser `gh`, crée le repo manuellement sur github.com puis :

```bash
git remote add origin git@github.com:<ton-user>/ai-projects.git
```

---

## Étape 3 — Déposer les fichiers fournis

Depuis le dossier où sont les fichiers livrés ici, copie-les à la racine du repo :

```bash
# Adapte le chemin source selon où tu as téléchargé les fichiers
SRC=~/Downloads/cowork-output

cp "$SRC/CLAUDE.md"    .
cp "$SRC/ROADMAP.md"   .
cp "$SRC/.gitignore"   .
cp "$SRC/README.md"    .

# La slash command : le dossier est livré sous "claude-config/" ;
# il faut le renommer en ".claude/" (le point devant compte)
mkdir -p .claude
cp -R "$SRC/claude-config/commands" .claude/
```

Vérifie :

```bash
ls -la
ls -la .claude/commands/
# Tu dois voir go.md
```

---

## Étape 4 — Premier commit

```bash
git add -A
git commit -m "chore: bootstrap repo with ROADMAP and /go workflow"
git push -u origin main
```

C'est ton premier vrai commit. Authentique.

---

## Étape 5 — Configurer git (si pas déjà fait)

Important pour que tes commits comptent bien dans **TES** contribs GitHub :

```bash
git config user.name "Mathis"
git config user.email "mathis34400@gmail.com"
```

> ⚠️ L'email **doit** être celui associé à ton compte GitHub (vérifié dans `Settings → Emails`). Sinon les commits apparaissent comme « unknown » et ne comptent pas dans le graphe de contribs.

---

## Étape 6 — Premier `/go`

```bash
cd ~/Code/ai-projects
claude
```

Dans la session :

```
/go
```

Claude va :
1. Lire le ROADMAP
2. Voir que la première tâche est « Bootstrap `sentiment-analyzer` »
3. Créer le dossier, le README, la structure
4. Lancer les tests (aucun à ce stade)
5. Cocher la tâche dans le ROADMAP
6. Commit + push

Tu vois apparaître un commit dans `gh repo view --web` et **un carré vert dans ton graphe** dans la journée.

---

## Étape 7 — Quotidien

Chaque jour :

```bash
cd ~/Code/ai-projects
claude
> /go
```

C'est tout. ~5 minutes. Au bout de quelques semaines, tu auras :
- Un graphe de contribs rempli **avec de vraies contributions**
- Un ou plusieurs projets IA qui avancent réellement
- Des README, des benchmarks, des tests — du contenu que tu peux montrer en entretien

---

## Astuces

### Alias terminal pour aller encore plus vite

Dans ton `~/.zshrc` ou `~/.bashrc` :

```bash
alias go='cd ~/Code/ai-projects && claude'
```

Puis dans le terminal : `go` puis `/go` dans Claude Code.

### Si tu veux automatiser à 100% (cron)

Tu peux faire tourner Claude Code en mode non-interactif :

```bash
cd ~/Code/ai-projects && claude -p "/go"
```

Et le mettre dans un cron à 8h chaque matin. **Mais attention** : sans toi pour valider, tu n'as plus le contrôle qualité. Je te recommande de garder la version manuelle au moins les premières semaines, le temps de calibrer.

### Que faire si `/go` se plante ?

- Lis le message d'erreur, corrige le souci avec Claude (`pourquoi ça a échoué ?`)
- Le ROADMAP n'est cochée qu'**après** un push réussi, donc tu ne perds rien

### Que faire si une journée tu n'as pas le temps ?

Rien. Ne force pas. Pas de commit > faux commit. Le but c'est de construire un vrai portfolio, pas de tricher avec un graphe vert.

---

## Pour aller plus loin

- Ajouter un workflow GitHub Actions qui lance `pytest` à chaque push (`feat(ci): add pytest workflow` sera un commit utile pour `/go`)
- Activer les **GitHub Pages** sur ce repo pour publier les README en site web
- Quand un projet est mature, le **split** dans son propre repo public (plus visible qu'enfoui dans un monorepo)
