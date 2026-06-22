# 🎓 Parcours pédagogique PlaylistApp — Sommaire des TP

> 

> 🎯 **Suivez votre progression** dans le [tableau de bord interactif](https://ggaillard.github.io/playlist-csharp) ou dans [PROGRESSION.md](PROGRESSION.md).

> Apprendre C# / .NET 10 en partant d'un exemple fonctionnel et en se l'appropriant par des modifications progressives.

---

## La méthode : 4 étapes, répétées sur chaque TP

Chaque TP applique le **même rituel d'apprentissage**. Vous ne codez jamais à partir d'une page blanche : vous partez d'un exemple qui marche, vous le comprenez, puis vous le faites évoluer.

| Étape | Ce que vous faites | Pourquoi |
|---|---|---|
| **1. Lancer** | Exécuter l'exemple fourni | Voir le résultat attendu avant de toucher au code |
| **2. Comprendre** | Lecture guidée du code, fichier par fichier | Savoir *où* et *pourquoi* avant de modifier |
| **3. Modifier** | 3 paliers : guidé → semi-guidé → autonome | S'approprier en faisant, avec un filet de sécurité dégressif |
| **4. Valider** | Lancer les tests, cocher la checklist | Prouver que ça marche, sans deviner |

### Les 3 paliers de modification

| Palier | Niveau | Ce qu'on vous donne |
|---|---|---|
| 🟢 **Guidé** | Débutant | Objectif + démarche détaillée + indice |
| 🟡 **Semi-guidé** | Intermédiaire | Objectif + grandes étapes + indice |
| 🔴 **Autonome** | Avancé | Objectif + vérification (à vous la démarche) |

> Les solutions complètes ne sont **pas** dans ces fiches : chercher fait partie de l'apprentissage. En cas de blocage, ouvrez une *issue* (modèle « Question » fourni).

---

## Les 4 TP

| TP | Titre | Durée | Compétence | Fiche |
|---|---|---|---|---|
| **TP1** | Application console & POO | 4h | SLAM1 | [📘 Ouvrir TP1](PlaylistApp/TP1_GUIDE.md) |
| **TP2** | Persistance avec Entity Framework Core | 6h | SLAM3, SLAM2 | [📗 Ouvrir TP2](PlaylistAppEF/TP2_GUIDE.md) |
| **TP3** | API REST & architecture SOA | 4h | SLAM4 | [📕 Ouvrir TP3](PlaylistAppAPI/TP3_GUIDE.md) |
| **TP4** | Architecture événementielle (EOA) | 4h | SLAM4 | [🎏 Ouvrir TP4](PlaylistAppAPI/TP4_GUIDE.md) |

### Progression logique

```
TP1              TP2                TP3                 TP4
Console      →   + base données  →  + API REST (SOA)  →  + événements (EOA)
"ça marche"      "ça se souvient"   "c'est exposé"       "ça évolue sans
                                                          tout casser"
```

Chaque TP **réutilise** le précédent : le TP3 (API) s'appuie sur la base de données du TP2. Vous construisez une vraie application, par couches.

---


## 🎓 Concepts & auto-évaluations

En complément des missions pratiques, le dossier **[cours/](cours/README.md)** contient une fiche par concept (POO, LINQ, ORM, migrations, REST, SOA, EOA…) avec une **explication didactique** et une **auto-évaluation**. Un **quiz interactif** est aussi intégré au [tableau de bord](https://ggaillard.github.io/playlist-csharp) (onglet « Quiz ») et compte dans votre progression.

## Avant de commencer

1. Créez votre dépôt depuis le template (bouton **Use this template**).
2. Ouvrez-le dans **GitHub Codespaces** (aucune installation locale).
3. Suivez les fiches **dans l'ordre** : TP1 → TP2 → TP3.
4. Cochez votre progression dans le `README.md` et committez régulièrement.

Détails complets : [GUIDE_ETUDIANT.md](GUIDE_ETUDIANT.md).

---

## Ce que vous saurez faire à la fin

- Lire et faire évoluer un code C# existant (la réalité du métier)
- Concevoir et interroger une base de données avec un ORM
- Construire et documenter une API REST
- Distinguer et appliquer les architectures SOA et EOA
- Valider votre travail par des tests automatiques
- Travailler en mode projet avec Git, Docker et l'intégration continue
