# 🌐 Concept — HTTP, REST et codes de statut

> **TP concerné :** TP3 · **Temps de lecture :** 8 min

---

## 1. HTTP : le dialogue web

Sur le web, tout est **requête** puis **réponse** :
```
CLIENT  ── requête  (méthode + URL + données) ─▶  SERVEUR
CLIENT  ◀─ réponse  (code de statut + données) ──  SERVEUR
```

## 2. REST : des ressources et des verbes

REST organise l'API autour de **ressources** (ici « chanson ») identifiées par une **URL**, manipulées par les **verbes HTTP** :

| Action | Verbe | URL | Succès |
|---|---|---|---|
| Lister | `GET` | `/api/chansons` | 200 |
| Voir n°5 | `GET` | `/api/chansons/5` | 200 |
| Créer | `POST` | `/api/chansons` | 201 |
| Modifier | `PUT` | `/api/chansons/5` | 204 |
| Supprimer | `DELETE` | `/api/chansons/5` | 204 |

> 🧠 L'URL dit **quoi**, le verbe dit **quelle action**.

## 3. Les codes de statut

| Famille | Sens | Exemples |
|---|---|---|
| 2xx | Succès | 200, 201, 204 |
| 4xx | Erreur du client | 400, 404, 409 |
| 5xx | Erreur du serveur | 500 |

> 🧠 2xx « c'est bon », 4xx « tu t'es trompé », 5xx « je me suis trompé ».

---

## 4. Auto-évaluation

**Q1.** Quel verbe pour créer une ressource, et quel code en cas de succès ?
<details><summary>▸ Voir la réponse</summary>

`POST`, et le code **201 Created**.
</details>

**Q2.** Que signifie un code 404 ?
<details><summary>▸ Voir la réponse</summary>

**Not Found** : la ressource demandée n'existe pas. C'est une erreur **4xx** (côté client : il a demandé quelque chose d'inexistant).
</details>

**Q3.** Dans REST, qu'est-ce qui distingue l'URL du verbe ?
<details><summary>▸ Voir la réponse</summary>

L'**URL** identifie la **ressource** (quoi). Le **verbe** indique l'**action** à effectuer dessus (lire, créer, modifier, supprimer).
</details>

---

✅ Cochez ce concept dans le [tableau de bord](https://ggaillard.github.io/playlist-csharp).
⬅️ [Retour aux concepts](README.md)
