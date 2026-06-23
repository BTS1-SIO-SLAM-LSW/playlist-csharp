# 🚀 TP0 — Mettre en place l'environnement

> **Module :** PlaylistApp (0/4) · **Durée : ~30 min** · Stack : .NET 10 · C# 14

> 🎓 **Concepts associés — à lire EN PREMIER** (explication + auto-évaluation) : [Environnement de développement (Dev Container)](cours/environnement.md)
>
> 👉 **Étape 1 du TP — avant de coder :** lisez cette fiche et réussissez son auto-évaluation. (Le comparatif et le choix y sont aussi détaillés.)

> **Démarche :** partir d'un environnement **décrit** (`.devcontainer`) → comprendre comment il est reconstruit → le faire tourner en **Codespaces** ou en **local**, puis lancer l'application.

> 🔑 **Icônes :** ✍️ à faire · 🎓 concept + auto-évaluation · ✅ valider · 🐳 Docker.

---

> 🏛️ **Enjeu d'architecture** — Un environnement **reproductible** (identique pour tout le monde, fini le « ça marche chez moi ») est la fondation d'un projet sérieux. Le dossier `.devcontainer` **décrit** l'environnement ; Codespaces (cloud) ou VS Code (local) le **recréent à l'identique**.

## 1. Objectifs pédagogiques

| # | Objectif | Vous saurez… |
|---|---|---|
| O1 | Comprendre l'intérêt d'un environnement reproductible | expliquer pourquoi on décrit l'environnement dans le code |
| O2 | Distinguer **image** et **conteneur** (Docker) | dire ce que produit le `.devcontainer` |
| O3 | Lancer l'application | obtenir le menu via `dotnet run` |
| O4 | Choisir sa voie | Codespaces (cloud) **ou** VS Code local |

## 2. La théorie, pas à pas

Un **Dev Container** est un environnement de développement **décrit par du code**, dans le dossier `.devcontainer/`. Deux fichiers clés :

| Fichier | Rôle |
|---|---|
| `.devcontainer/devcontainer.json` | **décrit** l'environnement : image .NET 10, fonctionnalités (Docker, Git), **extensions VS Code** (C# Dev Kit, Docker, SQLite Viewer, REST Client), ports (5000/5001) |
| `.devcontainer/post-create.sh` | **script lancé une fois** après création : installe `dotnet-ef`, restaure NuGet, prépare le dossier de données SQLite |

> 🧠 Détails et auto-évaluation dans la fiche concept : **[Environnement de développement](cours/environnement.md)**.

## 3. Modélisation

### Diagramme d'activité — le cycle de construction

```mermaid
flowchart TD
    A["Ouvrir le projet<br/>(Codespaces ou « Reopen in Container »)"] --> B["Lecture de devcontainer.json"]
    B --> C["Construction du conteneur<br/>(image .NET 10 + fonctionnalités)"]
    C --> D["Exécution de post-create.sh<br/>(dotnet-ef · restore NuGet · dossier data)"]
    D --> E(["✅ Environnement prêt"])
```

> 🗺️ **Lire l'organigramme** : on suit le **sens des flèches** ; chaque **rectangle** est une étape, le **stade final** marque l'environnement prêt.

### Diagramme — un seul descriptif, deux voies

```mermaid
flowchart LR
    DC[".devcontainer/<br/>(environnement décrit)"] --> CS["☁️ GitHub Codespaces"]
    DC --> LOCAL["💻 VS Code local<br/>(Dev Containers)"]
    CS --> APP["✅ .NET 10 · dotnet-ef · Docker · extensions prêts"]
    LOCAL --> APP
```

> 🗺️ **Légende** : le **même** `.devcontainer` produit le **même environnement**, en cloud (Codespaces) comme en local.

## 4. ✍️ Mise en place — pas à pas

### Voie A — GitHub Codespaces (recommandé · zéro installation)

1. Sur votre dépôt : **Code → Codespaces → Create codespace on main**.
2. Patientez ~2 min : VS Code s'ouvre **dans le navigateur**, l'environnement se construit depuis le `.devcontainer`.

✅ **Résultat attendu :** VS Code en ligne, terminal prêt, .NET 10 disponible.

### Voie B — En local avec VS Code

**Prérequis :** [VS Code](https://code.visualstudio.com/), [Docker Desktop](https://www.docker.com/products/docker-desktop/), extension **Dev Containers** (`ms-vscode-remote.remote-containers`).

1. Clonez votre dépôt, ouvrez le dossier dans VS Code.
2. `F1` → *Dev Containers: Reopen in Container*.

✅ **Résultat attendu :** le même environnement qu'en Codespaces, sur votre machine.

> 🐳 Docker Desktop doit être lancé avant d'ouvrir le conteneur.

## 5. ✍️ Lancer et vérifier l'application

```bash
cd PlaylistApp
dotnet run
```

✅ **Résultat attendu :** le menu de l'application s'affiche. Testez-le : `1` liste les chansons, `2` recherche, `0` quitte. **L'environnement fonctionne — vous pouvez passer au TP1.**

## 6. ✅ Validation finale — checklist

- [ ] 🎓 J'ai lu la fiche [Environnement de développement](cours/environnement.md) et réussi son auto-évaluation
- [ ] Mon environnement est prêt (Codespaces **ou** Dev Container local)
- [ ] `dotnet run` affiche le menu de l'application
- [ ] Je sais distinguer une **image** d'un **conteneur**, et à quoi sert `.devcontainer`
- [ ] 🎓 J'ai coché mes missions dans `PROGRESSION.md`

## 7. Dépannage

| Problème | Solution |
|---|---|
| `dotnet : command not found` | Reconstruisez l'environnement : `F1` → *Rebuild Container* (ou recréez le Codespace) |
| Docker ne démarre pas (local) | Lancez **Docker Desktop** avant d'ouvrir le conteneur |
| 1re construction longue | Normal : l'image se télécharge une fois, puis elle est mise en cache |
| Le menu ne s'affiche pas | Êtes-vous bien dans le dossier `PlaylistApp` (`cd PlaylistApp`) ? |

---

➡️ **TP suivant :** [TP1 — Console & POO](PlaylistApp/TP1_GUIDE.md)
🧭 **[Retour au parcours](PARCOURS_TP.md)**
