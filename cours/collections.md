# 📚 Concept — Les collections : `List` et `Dictionary`

> **TP concerné :** TP1 · **Temps de lecture :** 6 min

---

## 1. L'idée

Une **collection** stocke plusieurs valeurs dans une seule variable. Les deux plus courantes en C# sont `List<T>` et `Dictionary<K,V>`.

## 2. `List<T>` — une liste ordonnée

```csharp
List<Chanson> chansons = new();
chansons.Add(uneChanson);     // ajoute à la fin
chansons.Count;               // nombre d'éléments
chansons[0];                  // accès par position (index)
```
> 🧠 Une `List` **conserve l'ordre** d'insertion et **accepte les doublons**. Idéale pour une playlist (l'ordre compte).

## 3. `Dictionary<K,V>` — un annuaire clé → valeur

```csharp
Dictionary<int, Chanson> parId = new();
parId[1] = chanson;           // la clé 1 pointe vers cette chanson
var c = parId[1];             // accès direct par la clé (très rapide)
parId.ContainsKey(5);         // la clé existe-t-elle ?
```
> 🧠 Un `Dictionary` associe une **clé unique** à une valeur. Idéal pour retrouver une chanson par son `Id` instantanément, sans parcourir toute la liste.

## 4. Lequel choisir ?

| Besoin | Collection |
|---|---|
| Garder un ordre, autoriser doublons | `List<T>` |
| Retrouver vite par un identifiant unique | `Dictionary<K,V>` |

---

## 5. Auto-évaluation

**Q1.** Quelle collection garde l'ordre d'insertion ?
<details><summary>▸ Voir la réponse</summary>

La `List<T>`. Le `Dictionary` n'offre **aucune garantie d'ordre**.
</details>

**Q2.** Pourquoi `Bibliotheque` utilise un `Dictionary<int, Chanson>` plutôt qu'une `List` ?
<details><summary>▸ Voir la réponse</summary>

Pour retrouver une chanson par son `Id` **directement** (`_chansons[id]`), sans parcourir toute la collection. C'est beaucoup plus rapide quand il y a beaucoup d'éléments.
</details>

**Q3.** Peut-on avoir deux fois la même clé dans un `Dictionary` ?
<details><summary>▸ Voir la réponse</summary>

Non. Les clés sont **uniques**. Tenter d'ajouter une clé existante lève une exception (ou écrase la valeur selon la méthode utilisée).
</details>

---

✅ Cochez ce concept dans le [tableau de bord](https://ggaillard.github.io/playlist-csharp).
⬅️ [Retour aux concepts](README.md)
