# 🔗 Concept — Les relations entre entités (1-N, N-N)

> **TP concerné :** TP2 · **Temps de lecture :** 7 min

---

## 1. L'idée

Les données du monde réel sont **liées** : un artiste a plusieurs chansons, une playlist contient plusieurs chansons. On modélise ces liens par des **relations**.

## 2. Relation un-à-plusieurs (1-N)

Un artiste → plusieurs chansons, mais une chanson → un seul artiste.
```
Artiste (1) ───< (N) Chanson
```
En base : la table `Chansons` porte une **clé étrangère** `ArtisteId`.

## 3. Relation plusieurs-à-plusieurs (N-N)

Une playlist contient plusieurs chansons **et** une chanson peut être dans plusieurs playlists.
```
Playlist (N) ───< PlaylistChanson >─── (N) Chanson
```
On ne peut pas relier directement : on crée une **table de liaison** `PlaylistChanson` qui contient les deux clés étrangères (`PlaylistId`, `ChansonId`) et souvent un ordre (`Position`).

> 🧠 La table de liaison est **la** solution standard pour modéliser un N-N. C'est un grand classique du référentiel BTS (MLD).

---

## 4. Auto-évaluation

**Q1.** Donnez un exemple de relation 1-N dans le projet.
<details><summary>▸ Voir la réponse</summary>

Un **artiste** possède plusieurs **chansons**, mais chaque chanson n'a qu'un artiste. (Ou : une playlist a plusieurs entrées d'ordre.)
</details>

**Q2.** Comment modélise-t-on une relation N-N ?
<details><summary>▸ Voir la réponse</summary>

Avec une **table de liaison** (ici `PlaylistChanson`) qui porte les deux clés étrangères (`PlaylistId` et `ChansonId`). Elle relie les deux tables.
</details>

**Q3.** Que contient en général la table de liaison en plus des deux clés ?
<details><summary>▸ Voir la réponse</summary>

Souvent des données propres à l'association, comme la **position** de la chanson dans la playlist, ou une date d'ajout.
</details>

---

✅ Cochez ce concept dans le [tableau de bord](https://ggaillard.github.io/playlist-csharp).
⬅️ [Retour aux concepts](README.md)
