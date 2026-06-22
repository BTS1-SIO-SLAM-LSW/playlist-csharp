# 🔄 Concept — Les migrations

> **TP concerné :** TP2 · **Temps de lecture :** 7 min

---

## 1. L'idée

Une **migration** est un script **versionné** qui décrit comment faire évoluer la structure de la base (ajouter une table, une colonne…). C'est « Git pour le schéma de la base ».

## 2. Le cycle en 3 temps

```mermaid
flowchart LR
    M["1️⃣ Modifier<br/>la classe C#<br/>(ex : + propriété Label)"] --> G["2️⃣ Générer<br/>dotnet ef migrations add"]
    G --> S["📄 Script de migration<br/>(versionné dans Git)"]
    S --> U["3️⃣ Appliquer<br/>dotnet ef database update"]
    U --> DB[("🗄️ Base à jour")]
```

```bash
# 1. On modifie une classe C# (ex. ajouter une propriété Label)
# 2. On génère la migration
dotnet ef migrations add AjoutLabel
# 3. On l'applique à la base
dotnet ef database update
```

> 🧠 **Règle d'or :** on ne modifie **jamais** la base à la main. On modifie le **modèle C#**, on **génère** une migration, on l'**applique**. Traçabilité totale, et toute l'équipe a le même schéma.

## 3. Pourquoi versionner le schéma ?

Chaque migration est un fichier daté, rejouable, partagé via Git. Un collègue qui récupère le projet lance `database update` et obtient **exactement** la même structure — comme on récupère le code.

## 4. Le piège classique

Mettre `DateTime.UtcNow` comme valeur par défaut dans le *seed* crée des **migrations fantômes** : à chaque génération, la date change, donc EF croit qu'il faut tout réécrire. On utilise une **date fixe**.

---

## 5. Auto-évaluation

**Q1.** Qu'est-ce qu'une migration ?
<details><summary>▸ Voir la réponse</summary>

Un script versionné décrivant un **changement de structure** de la base (table, colonne, relation…). Il permet de faire évoluer la base de façon traçable et reproductible.
</details>

**Q2.** Dans quel ordre fait-on les choses pour ajouter une colonne ?
<details><summary>▸ Voir la réponse</summary>

1) modifier la **classe C#**, 2) `dotnet ef migrations add ...`, 3) `dotnet ef database update`. Jamais de modification directe de la base.
</details>

**Q3.** Pourquoi éviter `DateTime.UtcNow` dans les données initiales (seed) ?
<details><summary>▸ Voir la réponse</summary>

Parce que la valeur change à chaque génération de migration → EF Core génère des **migrations « fantômes »** qui veulent réécrire les dates. On met une **date fixe**.
</details>

---

✅ Cochez ce concept dans le [tableau de bord](https://ggaillard.github.io/playlist-csharp).
⬅️ [Retour aux concepts](README.md)
