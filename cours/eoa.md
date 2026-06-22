# 🎏 Concept — Événements et publish/subscribe (EOA)

> **TP concerné :** TP4 · **Temps de lecture :** 10 min

---

## 1. Le problème

Quand on crée une chanson, on veut peut-être journaliser, mettre à jour des stats, notifier… Tout mettre dans le Controller le rend énorme et fragile : c'est le **couplage fort**.

## 2. L'idée : annoncer au lieu d'ordonner

L'**EOA** (*Event-Oriented Architecture*) renverse la logique : au lieu d'ordonner chaque action, on **publie un événement** (« chanson ajoutée »), et **ceux que ça intéresse réagissent**.

> 📣 **Analogie de la gare :** le haut-parleur annonce un train. Il ne sait pas qui écoute. Les voyageurs concernés réagissent ; les autres ignorent.

## 3. Les 3 rôles (publish/subscribe)

```mermaid
sequenceDiagram
    participant Ctrl as ChansonsController (émetteur)
    participant Bus as EventBus
    participant A as AuditHandler (abonné)
    participant S as StatistiquesHandler (abonné)
    Ctrl->>Bus: PublishAsync(ChansonAjouteeEvent)
    par Le bus distribue à tous les abonnés
        Bus->>A: HandleChansonAjoutee(e)
        Bus->>S: HandleChansonAjoutee(e)
    end
    Note over Ctrl,S: l'émetteur ne connaît PAS ses abonnés → découplage total
```

| Rôle | Qui | Fait |
|---|---|---|
| Émetteur | Controller | publie l'événement |
| Bus | `EventBus` | distribue aux abonnés |
| Abonnés | `AuditHandler`, `StatistiquesHandler` | réagissent |

Pour ajouter un comportement (ex. un e-mail), on crée un handler et on l'abonne — **sans toucher** au Controller.

## 4. SOA et EOA se complètent

```mermaid
flowchart LR
    C["POST /api/chansons"] --> Ctrl["Controller"]
    Ctrl -->|"1. SOA : enregistre (synchrone)"| DB[("SQLite")]
    Ctrl -->|"2. EOA : publie l'événement"| Bus(("EventBus"))
    Bus -.->|"asynchrone, découplé"| H1["Audit"]
    Bus -.-> H2["Stats"]
    Bus -.-> H3["…futur handler"]
```

| | SOA | EOA |
|---|---|---|
| Logique | « fais et réponds » | « ceci est arrivé, réagissez » |
| Couplage | direct | découplé |
| Moment | synchrone | asynchrone |

> ⚙️ **Passage à l'échelle :** ici le bus est *en mémoire*. En production, on remplace `InMemoryEventBus` par un vrai courtier de messages (**Kafka**, **RabbitMQ**) sans changer la logique des émetteurs ni des abonnés — c'est le même contrat publish/subscribe.

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
