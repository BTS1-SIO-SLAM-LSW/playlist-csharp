# 🚑 Dépannage — PlaylistApp

Cette page est classée **par ce que vous voyez à l'écran**, pas par thème technique.
Repérez votre message d'erreur, appliquez la solution, reprenez votre TP.

> 🎯 **Avant tout : les trois réflexes**
> 1. **Lisez la dernière ligne** du message d'erreur, pas la première. C'est elle qui dit ce qui manque.
> 2. **Vérifiez où vous êtes** : `pwd`. Neuf erreurs sur dix viennent d'un mauvais dossier.
> 3. **Reconstruisez l'environnement** : `F1` → *Dev Containers: Rebuild Container*. Répare la majorité des cas.

---

## 📋 Table des matières

| Vous êtes bloqué sur… | Section |
|---|---|
| Le Codespace ne démarre pas | [1. Codespaces](#1-️-codespaces) |
| Le conteneur local ne démarre pas | [2. Dev Container local](#2--dev-container-local) |
| Une commande `dotnet` échoue | [3. Commandes dotnet](#3-️-commandes-dotnet) |
| Les migrations / la base de données | [4. EF Core et SQLite](#4-️-ef-core-et-sqlite) |
| Swagger, l'API, les ports | [5. API et ports](#5--api-et-ports) |
| Docker et docker compose | [6. Docker](#6--docker) |
| Git, commits, push | [7. Git](#7--git) |

---

## 1. ☁️ Codespaces

### ❌ « Codespace creation failed » / le conteneur ne finit jamais de construire

**Cause la plus fréquente :** votre dépôt a été créé à partir du template **avant** la correction du `.devcontainer` (novembre 2026). L'ancienne version contenait un montage de disque incompatible avec Codespaces.

**Solution :** récupérez la version corrigée dans votre dépôt.

```bash
git remote add upstream https://github.com/ggaillard/playlist-csharp.git
git fetch upstream
git checkout upstream/main -- .devcontainer .gitignore data
git commit -m "Corrige la configuration du Dev Container"
git push
```

Puis **supprimez votre Codespace** et recréez-le : *Code → Codespaces → les trois points → Delete*, puis *Create codespace on main*. Un Codespace ne relit pas le `.devcontainer` sans être recréé.

### ❌ Le Codespace s'ouvre mais `dotnet` est introuvable

Le conteneur a démarré en mode dégradé (« recovery container ») après un échec de construction. Vous êtes dans une image générique, pas dans celle du cours.

**Vérifiez :** `cat /etc/os-release` puis `dotnet --version`. Si `dotnet` répond `command not found`, c'est bien ce cas.

**Solution :** appliquez le correctif ci-dessus, puis recréez le Codespace.

### ❌ « No space left on device »

Vous avez épuisé les 32 Go du Codespace, généralement à cause des images Docker.

```bash
docker system prune -af
dotnet nuget locals all --clear
```

### 💡 Mon Codespace s'arrête tout seul

C'est normal : arrêt automatique après 30 minutes d'inactivité. Vos fichiers sont conservés, il suffit de le relancer. Pensez à **committer** régulièrement : un Codespace supprimé après 30 jours d'inactivité emporte tout ce qui n'a pas été poussé.

---

## 2. 🐳 Dev Container local

### ❌ « Cannot create container » / « error during connect » / « docker daemon is not running »

**Docker Desktop n'est pas lancé.** Ouvrez-le, attendez que l'icône de baleine soit stable, puis relancez `F1` → *Reopen in Container*.

Sous Windows, si Docker Desktop refuse de démarrer : vérifiez que **WSL 2** est installé.

```powershell
wsl --status
wsl --update
```

### ❌ « invalid mount config » / « bind source path does not exist »

Ancienne version du `.devcontainer`, qui exigeait un dossier `data/` inexistant.

**Solution :** appliquez le correctif de la [section 1](#--codespace-creation-failed---le-conteneur-ne-finit-jamais-de-construire), ou créez simplement le dossier manquant avant d'ouvrir le conteneur :

```bash
mkdir -p data
```

### ❌ La construction est interminable la première fois

C'est attendu : **5 à 10 minutes** au premier lancement, le temps de télécharger l'image .NET 10 (environ 1 Go). Les fois suivantes prennent quelques secondes. Ne l'interrompez pas.

### ❌ « permission denied » sur `data/` ou sur un fichier

Droits Unix incorrects, typiquement après un montage créé par root.

```bash
sudo chown -R vscode:vscode /workspaces
```

---

## 3. ⚙️ Commandes dotnet

### ❌ `dotnet : command not found`

Vous n'êtes pas dans le conteneur, ou celui-ci a mal démarré.

**Vérifiez** que la barre verte en bas à gauche de VS Code affiche bien **Dev Container : 🎵 PlaylistApp**. Si ce n'est pas le cas, `F1` → *Reopen in Container*.

### ❌ `dotnet ef : command not found`

L'outil EF Core n'est pas dans le `PATH` de votre terminal actuel.

```bash
export PATH="$PATH:$HOME/.dotnet/tools"
dotnet ef --version
```

Si la commande reste introuvable, installez l'outil :

```bash
dotnet tool install --global dotnet-ef
```

Ouvrir un **nouveau terminal** suffit généralement : le `PATH` n'est lu qu'au démarrage du shell.

### ❌ « MSB1003: Specify a project or solution file »

Vous êtes à la racine du dépôt, où il n'y a aucun projet. Chaque TP a son dossier.

```bash
cd PlaylistApp      # TP1
cd PlaylistAppEF    # TP2
cd PlaylistAppAPI   # TP3 et TP4
dotnet run
```

### ❌ « The process cannot access the file … because it is being used »

Une instance tourne déjà. Arrêtez-la avec `Ctrl+C` dans son terminal, ou :

```bash
pkill -f dotnet
```

### ❌ Erreurs de compilation après avoir modifié le code

Repartez propre :

```bash
dotnet clean
dotnet restore
dotnet build
```

---

## 4. 🗄️ EF Core et SQLite

### ❌ « SQLite Error 1: no such table: Chansons »

La base existe mais les tables n'ont pas été créées : la migration n'a pas été appliquée.

```bash
cd PlaylistAppEF
dotnet ef database update
```

### ❌ « Unable to create an object of type 'PlaylistContext' »

Commande lancée depuis le mauvais dossier. `dotnet ef` doit être exécuté **dans le dossier du projet** qui contient le `DbContext`.

### ❌ « The model backing the context has changed »

Vous avez modifié une entité sans créer la migration correspondante.

```bash
dotnet ef migrations add DecrivezVotreChangement
dotnet ef database update
```

### 🔄 Repartir d'une base vierge

Sans risque : la base est locale à votre poste.

```bash
rm -f data/playlist.db
dotnet ef database update
```

### ❌ « unable to open database file »

Le dossier `data/` n'existe pas.

```bash
mkdir -p data
```

---

## 5. 🌐 API et ports

### ❌ Dans un Codespace, `http://localhost:5000` ne s'ouvre pas

**C'est normal, et ce n'est pas une erreur.** L'application tourne sur une machine distante : `localhost` désigne votre propre ordinateur, pas le Codespace.

**Solution :** onglet **PORTS** en bas de VS Code → ligne du port **5000** → icône 🌐 *Open in Browser*. Ajoutez `/swagger` à l'adresse obtenue.

### ❌ « Address already in use » sur le port 5000

Une API tourne déjà.

```bash
pkill -f dotnet
```

Ou changez de port le temps d'un essai :

```bash
dotnet run --urls http://+:5050
```

### ❌ Swagger affiche « Failed to load API definition »

L'API a démarré mais une erreur est survenue au chargement. **Lisez le terminal** où tourne `dotnet run` : l'exception réelle y est affichée. Le plus souvent, la base n'est pas à jour (voir [section 4](#4-️-ef-core-et-sqlite)).

### ❌ Le port se ferme dès que je ferme l'onglet

Normal : `dotnet run` est lié à son terminal. Laissez-le ouvert pendant vos tests.

---

## 6. 🐋 Docker

> 💡 **À savoir avant de vous acharner :** `docker compose` sert à *observer* la conteneurisation aux TP2 et TP3. Il n'est **jamais obligatoire** pour faire tourner l'application. Si Docker vous bloque, faites votre TP avec `dotnet run` et revenez à Docker ensuite.

### ❌ « docker: command not found » dans le conteneur

La fonctionnalité Docker-in-Docker n'a pas été installée à la construction. `F1` → *Rebuild Container*.

### ❌ « failed to solve: failed to read dockerfile »

Vous n'êtes pas dans le dossier qui contient le `Dockerfile`.

```bash
cd PlaylistAppEF
docker compose up --build
```

### ❌ La construction est très lente en Codespaces

Attendu : Docker-in-Docker recompile tout dans un environnement contraint. Comptez plusieurs minutes. Pour avancer, `dotnet run` fait le même travail en quelques secondes.

---

## 7. 🌿 Git

### ❌ « Updates were rejected because the remote contains work »

Quelqu'un — ou vous, depuis un autre poste — a poussé entre-temps.

```bash
git pull --rebase
git push
```

### ❌ « Please tell me who you are »

```bash
git config --global user.name "Votre Prénom Nom"
git config --global user.email "votre.email@exemple.fr"
```

### ❌ J'ai perdu mes modifications

Regardez d'abord si elles ont été mises de côté :

```bash
git stash list
git stash pop
```

Puis l'historique complet, y compris les commits « perdus » :

```bash
git reflog
```

---

## 🆘 Rien ne fonctionne

**La solution qui répare presque tout**, dans l'ordre :

1. `F1` → *Dev Containers: Rebuild Container* (en local) ou supprimez et recréez le Codespace.
2. Si le problème persiste, appliquez le correctif `.devcontainer` de la [section 1](#1-️-codespaces).
3. Toujours bloqué : ouvrez une **issue** avec le modèle *Question*, en précisant :

| À indiquer | Comment l'obtenir |
|---|---|
| Le TP concerné | TP0 à TP4 |
| Codespaces ou local | — |
| La commande tapée | copier-coller exact |
| Le message d'erreur **complet** | pas seulement la première ligne |
| Votre dossier courant | `pwd` |
| La version de .NET | `dotnet --version` |

> ⚠️ **Ne collez jamais** de mot de passe, de jeton GitHub ni de clé d'API dans une issue. Un dépôt public reste lisible par tous.

---

## 🩺 Auto-diagnostic

Copiez ce bloc dans le terminal. Il vérifie l'environnement en une fois et vous donne l'essentiel à joindre à votre issue.

```bash
echo "--- Environnement ---"
echo "Dossier   : $(pwd)"
echo "Codespace : ${CODESPACES:-non}"
echo "dotnet    : $(dotnet --version 2>&1 | head -1)"
echo "dotnet-ef : $(dotnet ef --version 2>&1 | tail -1)"
echo "docker    : $(docker --version 2>&1 | head -1)"
echo "DB_PATH   : ${DB_PATH:-non défini}"
echo "data/     : $([ -d data ] && echo présent || echo ABSENT)"
echo "base      : $([ -f data/playlist.db ] && echo présente || echo absente)"
echo "branche   : $(git branch --show-current 2>&1)"
```

Sortie attendue : `dotnet` en `10.x`, `data/` présent, et `DB_PATH` défini.
