# 🔎 Concept — Les requêtes LINQ

> **TP concerné :** TP1 (et au-delà) · **Temps de lecture :** 7 min

---

## 1. L'idée

**LINQ** (*Language Integrated Query*) permet d'interroger une collection avec une syntaxe lisible : filtrer, trier, transformer, regrouper — sans écrire de boucles `for`.

## 2. Les opérations de base

```csharp
// Filtrer : garder les chansons de rock
chansons.Where(c => c.Genre == "Rock");

// Trier par artiste
chansons.OrderBy(c => c.Artiste);

// Transformer : ne garder que les titres
chansons.Select(c => c.Titre);

// Compter / premier / existence
chansons.Count(c => c.Annee > 2000);
chansons.FirstOrDefault(c => c.Id == 5);
chansons.Any(c => c.Note == 5);
```

> 🧠 `c => c.Genre == "Rock"` est une **expression lambda** : « pour chaque chanson `c`, garder celles dont le genre est Rock ». Le `=>` se lit « va vers » ou « tel que ».

## 3. On enchaîne les opérations

```csharp
chansons
    .Where(c => c.Note >= 4)        // filtrer
    .OrderByDescending(c => c.Note) // trier
    .Take(3);                       // garder les 3 premières
```

---

## 4. Auto-évaluation

**Q1.** Que fait `chansons.Where(c => c.Annee >= 2000)` ?
<details><summary>▸ Voir la réponse</summary>

Renvoie toutes les chansons **dont l'année est supérieure ou égale à 2000** (un filtre). Les autres sont écartées.
</details>

**Q2.** Quelle est la différence entre `Where` et `Select` ?
<details><summary>▸ Voir la réponse</summary>

`Where` **filtre** (garde certains éléments) ; `Select` **transforme** (projette chaque élément vers autre chose, par exemple juste le titre). On les combine souvent.
</details>

**Q3.** Comment obtenir les 3 chansons les mieux notées ?
<details><summary>▸ Voir la réponse</summary>

```csharp
chansons.OrderByDescending(c => c.Note).Take(3);
```
On trie par note décroissante, puis on prend les 3 premières.
</details>

---

✅ Cochez ce concept dans le [tableau de bord](https://ggaillard.github.io/playlist-csharp).
⬅️ [Retour aux concepts](README.md)
