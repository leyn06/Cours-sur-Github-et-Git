# 📚 Cours Complet : Comment fonctionne GitHub

## Table des matières
1. [Introduction](#introduction)
2. [Concepts fondamentaux](#concepts-fondamentaux)
3. [Installation et configuration](#installation-et-configuration)
4. [Premiers pas](#premiers-pas)
5. [Workflow de base](#workflow-de-base)
6. [Branches et merge](#branches-et-merge)
7. [Collaboration](#collaboration)
8. [Bonnes pratiques](#bonnes-pratiques)
9. [Dépannage](#dépannage)

---

## Introduction

GitHub est une plateforme de gestion de versions basée sur **Git**. Elle permet aux développeurs de :
- Stocker et versionner leur code
- Collaborer avec d'autres développeurs
- Suivre l'historique des modifications
- Organiser des projets
- Découvrir et contribuer à des projets open-source

### Pourquoi GitHub ?
- **Contrôle de version** : Retrouvez n'importe quelle version de votre code
- **Collaboration** : Travaillez en équipe sans surcharger vos fichiers
- **Traçabilité** : Chaque changement est documenté
- **Communauté** : Partagez vos projets avec le monde

---

## Concepts fondamentaux

### 🏠 Repository (Dépôt)
Un repository est un dossier qui contient :
- Tous les fichiers de votre projet
- L'historique complet des modifications
- Les configurations du projet

```
mon-projet/
├── .git/                 # Dossier caché avec l'historique
├── src/
│   └── main.js
├── README.md
└── .gitignore
```

### 💾 Commit
Un commit est un "snapshot" (cliché) de votre code à un moment donné. Chaque commit :
- A un identifiant unique (hash SHA-1)
- Contient les changements effectués
- Inclut un message descriptif
- Enregistre l'auteur et la date

**Exemple** :
```
commit a1b2c3d4e5f6g7h8i9j0
Author: Alice <alice@example.com>
Date: Thu Aug 15 10:30:00 2026

    Ajouter la fonction de authentification
    
    - Ajout de la vérification du mot de passe
    - Intégration JWT
```

### 🌿 Branch (Branche)
Une branche est une ligne de développement indépendante.

```
main (principale)
 ├─ feature/login
 ├─ feature/database
 └─ bugfix/header
```

**Avantages** :
- Développez sans affecter le code principal
- Testez vos nouvelles idées
- Laissez une trace de votre travail

### 📥 Pull Request (PR)
Une PR est une demande de fusion : "Je propose de fusionner ma branche dans main".

Elle permet de :
- Reviewer le code avant la fusion
- Discuter des changements
- Exécuter des tests automatiques
- Avoir un historique des décisions

### 🔀 Merge
La fusion combine une branche dans une autre.

```
Avant merge:
feature/login → [3 commits]
main          → [10 commits]

Après merge:
main → [13 commits] (inclut les 3 de feature/login)
```

---

## Installation et configuration

### Installer Git

**Windows** :
```bash
# Télécharger depuis https://git-scm.com
# Ou avec Chocolatey
choco install git
```

**macOS** :
```bash
# Avec Homebrew
brew install git

# Ou avec les Xcode Command Line Tools
xcode-select --install
```

**Linux** :
```bash
# Debian/Ubuntu
sudo apt-get install git

# Fedora
sudo dnf install git
```

### Configuration initiale

```bash
# Définir votre nom et email
git config --global user.name "Votre Nom"
git config --global user.email "votre@email.com"

# Vérifier la configuration
git config --global --list

# (Optionnel) Éditeur par défaut
git config --global core.editor "code"
```

### Générer une clé SSH (recommandé)

```bash
# Générer une paire de clés
ssh-keygen -t ed25519 -C "votre@email.com"

# Ajouter à l'agent SSH
ssh-add ~/.ssh/id_ed25519

# Afficher la clé publique (à ajouter sur GitHub)
cat ~/.ssh/id_ed25519.pub
```

---

## Premiers pas

### 1️⃣ Créer un repository local

```bash
# Option A : Créer un nouveau dossier
mkdir mon-projet
cd mon-projet
git init

# Option B : Cloner un repository existant
git clone https://github.com/utilisateur/mon-projet.git
cd mon-projet
```

### 2️⃣ Vérifier l'état du repository

```bash
git status
```

**Résultat** :
```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

### 3️⃣ Voir l'historique

```bash
git log

# Format court
git log --oneline

# Avec le graphique des branches
git log --oneline --graph --all
```

---

## Workflow de base

### Le cycle : Modifier → Stage → Commit

```
┌─────────────┐
│ Fichier     │
│ modifié     │
└──────┬──────┘
       │ git add
       ▼
┌─────────────┐
│ Staged      │ (prêt à commiter)
│ (Index)     │
└──────┬──────┘
       │ git commit
       ▼
┌─────────────┐
│ Committed   │ (sauvegardé)
│ (Historique)│
└─────────────┘
```

### Étape 1 : Modifier des fichiers

```bash
# Créer/modifier des fichiers
echo "console.log('Hello');" > app.js
```

### Étape 2 : Stage les changements

```bash
# Ajouter un fichier spécifique
git add app.js

# Ajouter tous les fichiers
git add .

# Ajouter avec interaction
git add -p
```

### Étape 3 : Commit

```bash
# Commit avec un message
git commit -m "Ajouter le fichier principal"

# Commit avec un message long
git commit -m "Ajouter le fichier principal" -m "Ceci contient la logique centrale de l'application"

# Modifier le dernier commit (dangereux !)
git commit --amend
```

### Exemple complet

```bash
# 1. Créer des fichiers
echo "console.log('Hello');" > app.js
echo "*.log" > .gitignore

# 2. Vérifier le statut
git status
# On branch main
# Untracked files:
#   app.js
#   .gitignore

# 3. Ajouter les fichiers
git add app.js .gitignore

# 4. Vérifier à nouveau
git status
# Changes to be committed:
#   new file:   app.js
#   new file:   .gitignore

# 5. Commiter
git commit -m "Initialiser le projet avec fichier principal"

# 6. Vérifier le log
git log --oneline
# a1b2c3d (HEAD -> main) Initialiser le projet avec fichier principal
```

---

## Branches et merge

### Pourquoi utiliser des branches ?

Imaginez deux développeurs :
- **Alice** travaille sur la authentification
- **Bob** travaille sur la base de données

Sans branches → **Chaos !** 🔥  
Avec branches → **Ordre !** ✨

### Créer et basculer de branche

```bash
# Créer une nouvelle branche
git branch feature/login

# Basculer vers cette branche
git checkout feature/login

# Raccourci : créer et basculer
git checkout -b feature/login

# Ou avec la version récente de Git
git switch -c feature/login

# Lister les branches
git branch -a

# Supprimer une branche
git branch -d feature/login
```

### Workflow avec branches

```bash
# 1. Créer une branche pour une feature
git checkout -b feature/login

# 2. Faire des modifications et commits
echo "function login() {}" > login.js
git add login.js
git commit -m "Ajouter la fonction login"

# 3. Retourner à main
git checkout main

# 4. Fusionner la branche
git merge feature/login

# 5. Nettoyer
git branch -d feature/login
```

### Résoudre les conflits

Un conflit survient quand deux branches modifient la même ligne.

**Exemple de conflit** :
```javascript
<<<<<<< HEAD
function login() {
  // Version de main
  console.log("Login v1");
}
=======
function login() {
  // Version de feature/login
  console.log("Login v2");
  console.log("Nouvelle version");
}
>>>>>>> feature/login
```

**Résolution** :
```bash
# 1. Éditer le fichier et choisir la bonne version
# 2. Ajouter le fichier résolu
git add login.js

# 3. Compléter le merge
git commit -m "Résoudre le conflit de login.js"
```

---

## Collaboration

### Synchroniser avec GitHub

```bash
# Cloner un repository
git clone https://github.com/utilisateur/projet.git

# Ajouter un repository distant
git remote add origin https://github.com/utilisateur/projet.git

# Lister les remotes
git remote -v

# Récupérer les changements (sans fusionner)
git fetch origin

# Récupérer et fusionner (= fetch + merge)
git pull origin main

# Envoyer vos commits
git push origin main
```

### Workflow collaboratif complet

```bash
# 1. Cloner le projet
git clone https://github.com/alice/mon-app.git
cd mon-app

# 2. Créer votre branche de feature
git checkout -b feature/nouvelle-page

# 3. Faire vos changements
echo "<h1>Nouvelle page</h1>" > page.html
git add page.html
git commit -m "Créer la nouvelle page"

# 4. Envoyer la branche
git push origin feature/nouvelle-page

# 5. Créer une Pull Request sur GitHub (via l'interface)

# 6. Après approval, supprimer la branche locale
git checkout main
git branch -d feature/nouvelle-page

# 7. Mettre à jour main localement
git pull origin main
```

### Travailler avec des forks

```bash
# 1. Forker le repository (via GitHub)

# 2. Cloner votre fork
git clone https://github.com/votre-utilisateur/projet-forked.git

# 3. Ajouter le repo original comme "upstream"
git remote add upstream https://github.com/alice/projet.git

# 4. Récupérer les mises à jour du projet original
git fetch upstream

# 5. Créer une branche basée sur upstream
git checkout -b bugfix upstream/main

# 6. Faire vos changements et pusher vers votre fork
git push origin bugfix

# 7. Créer une Pull Request vers le repo original
```

---

## Bonnes pratiques

### 1️⃣ Messages de commit clairs

❌ **Mauvais** :
```
git commit -m "fix"
git commit -m "update"
git commit -m "asdf"
```

✅ **Bon** :
```
git commit -m "Corriger le bug de connexion sur Safari"
git commit -m "Refactoriser la gestion des erreurs"
git commit -m "Ajouter les tests unitaires pour UserService"
```

**Format recommandé** :
```
[TYPE] Courte description (50 caractères max)

Description détaillée si nécessaire.
Peut contenir plusieurs lignes.

Closes #123 (fermer une issue)
```

**Types courants** :
- `feat:` Une nouvelle fonctionnalité
- `fix:` Correction d'un bug
- `docs:` Documentation
- `style:` Formatage du code
- `refactor:` Refactorisation
- `test:` Ajout de tests
- `chore:` Maintenance

### 2️⃣ Commits atomiques

Chaque commit doit représenter **une seule unité logique**.

❌ **Mauvais** : Un seul commit avec 10 choses différentes  
✅ **Bon** : 5 commits petits et focalisés

```bash
# Bon workflow :
git add src/login.js
git commit -m "Ajouter la fonction login"

git add tests/login.test.js
git commit -m "Ajouter les tests pour login"

git add docs/API.md
git commit -m "Documenter l'endpoint login"
```

### 3️⃣ Le fichier .gitignore

Ignorez les fichiers inutiles :

```
# .gitignore
node_modules/
.env
.DS_Store
*.log
dist/
build/
.idea/
*.swp
```

### 4️⃣ Utiliser des branches par feature

```
main (stable)
├── feature/login (développement)
├── feature/api (développement)
├── bugfix/header (bugfix)
└── release/v1.0 (release)
```

### 5️⃣ Pull Requests avec description

```markdown
## Description
Ajoute la fonction de connexion utilisateur.

## Type de changement
- [x] Nouvelle fonctionnalité
- [ ] Correction de bug
- [ ] Breaking change

## Tests effectués
- [x] Tests unitaires
- [x] Tests d'intégration
- [ ] Tests e2e

## Screenshots (si applicable)
![Login page](url)

## Checklist
- [x] Mon code suit le style du projet
- [x] J'ai commenté mon code
- [x] J'ai ajouté/mis à jour la documentation
- [x] Mes changements ne génèrent pas d'erreurs
```

---

## Dépannage

### ❌ "Changes not staged for commit"

```bash
# Vous avez modifié des fichiers mais pas stagé
git status

# Solutions :
git add .          # Stager tout
git add fichier    # Stager un fichier spécifique
```

### ❌ "Your branch is ahead of 'origin/main' by 3 commits"

```bash
# Vous avez des commits non pushés
git push origin main
```

### ❌ "Merge conflict"

```bash
# Conflit lors d'un merge
# 1. Éditer les fichiers avec <<<<<<< >>>>>
# 2. Résoudre manuellement
git add fichiers-resolus
git commit -m "Résoudre les conflits"
```

### ❌ "Detached HEAD"

```bash
# Vous êtes sur un commit au lieu d'une branche
git checkout main     # Retourner à une branche
```

### ❌ Annuler le dernier commit (non pushé)

```bash
# Soft : garder les changements
git reset --soft HEAD~1

# Mixed : défaire l'ajout mais garder les fichiers (défaut)
git reset --mixed HEAD~1

# Hard : tout annuler (dangereux !)
git reset --hard HEAD~1
```

### ❌ Récupérer un commit supprimé

```bash
# Trouver tous les commits récents
git reflog

# Revenir à un commit
git checkout a1b2c3d
```

---

## Commandes utiles à connaître

| Commande | Description |
|----------|-------------|
| `git init` | Initialiser un repo |
| `git clone URL` | Cloner un repo |
| `git status` | Voir l'état actuel |
| `git add FICHIER` | Stager un fichier |
| `git commit -m "MSG"` | Créer un commit |
| `git push REMOTE BRANCHE` | Envoyer les commits |
| `git pull REMOTE BRANCHE` | Récupérer et fusionner |
| `git fetch REMOTE` | Récupérer sans fusionner |
| `git branch -a` | Lister les branches |
| `git checkout -b BRANCHE` | Créer et basculer |
| `git merge BRANCHE` | Fusionner une branche |
| `git log --oneline` | Voir l'historique |
| `git diff` | Voir les différences |
| `git stash` | Sauvegarder temporaire |
| `git tag v1.0.0` | Créer une version |

---

## Ressources supplémentaires

- 📖 [Documentation Git officielle](https://git-scm.com/doc)
- 🐱 [GitHub Docs](https://docs.github.com)
- 🎮 [Learn Git Branching (interactif)](https://learngitbranching.js.org/)
- 📚 [Pro Git Book](https://git-scm.com/book)

---

**Bravo ! Vous maîtrisez maintenant les bases de GitHub ! 🚀**

## Vous pouvez trouvez des exercices en rapport avec ce cours [ici](https://github.com/leyn06/Exercice-Github-et-Git).

**Cours Ecrit Par Leyn_13**