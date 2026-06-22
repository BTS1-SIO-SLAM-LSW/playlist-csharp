# 🎏 Concept — Événements et publish/subscribe (EOA)

> **TP concerné :** TP4 · **Temps de lecture :** 8 min

---

## 1. Le problème

Quand on crée une chanson, on veut peut-être journaliser, mettre à jour des stats, notifier… Tout mettre dans le Controller le rend énorme et fragile : c'est le **couplage fort**.

## 2. L'idée : annoncer au lieu d'ordonner

L'**EOA** (*Event-Oriented Architecture*) renverse la logique : au lieu d'ordonner chaque action, on **publie un événement** (« chanson ajoutée »), et **ceux que ça intéresse réagissent**.

> 📣 **Analogie de la gare :** le haut-parleur annonce un train. Il ne sait pas qui écoute. Les voyageurs concernés réagissent ; les autres ignorent.

## 3. Les 3 rôles (publish/subscribe)

| Rôle | Qui | Fait |
|---|---|---|
| Émetteur | Controller | publie l'événement |
| Bus | `EventBus` | distribue aux abonnés |
| Abonnés | `AuditHandler`, `StatistiquesHandler` | réagissent |

L'émetteur **ne connaît pas** ses abonnés : **découplage total**. Pour ajouter un comportement, on crée un handler et on l'abonne — **sans toucher** au Controller.

## 4. SOA et EOA se complètent

| | SOA | EOA |
|---|---|---|
| Logique | « fais et réponds » | « ceci est arrivé, réagissez » |
| Couplage | direct | découplé |
| Moment | synchrone | asynchrone |

---

## 5. Auto-évaluation

**Q1.** Quel problème l'EOA résout-elle ?
<details><summary>▸ Voir la réponse</summary>

Le **couplage fort** : éviter que le code émetteur (le Controller) connaisse et appelle directement tous les effets de bord (audit, stats, notifications). On découple via des événements.
</details>

**Q2.** Citez les trois rôles du patron publish/subscribe.
<details><summary>▸ Voir la réponse</summary>

L'**émetteur** (publie), le **bus** (distribue), les **abonnés/handlers** (réagissent).
</details>

**Q3.** Pour ajouter une notification par e-mail, doit-on modifier le Controller ?
<details><summary>▸ Voir la réponse</summary>

Non. On crée un **nouveau handler** et on l'**abonne** à l'événement. Le Controller reste inchangé : c'est tout l'intérêt du découplage.
</details>

**Q4.** SOA et EOA sont-elles concurrentes ?
<details><summary>▸ Voir la réponse</summary>

Non, **complémentaires**. On enregistre la chanson en SOA (synchrone), puis on publie un événement en EOA pour les effets de bord (asynchrone, découplé).
</details>

---

✅ Cochez ce concept dans le [tableau de bord](https://ggaillard.github.io/playlist-csharp).
⬅️ [Retour aux concepts](README.md)
