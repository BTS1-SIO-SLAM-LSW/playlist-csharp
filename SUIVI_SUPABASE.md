# 📡 Suivi centralisé de la progression

Le tableau de bord (`docs/index.html`) enregistre la progression de chaque étudiant
dans **Supabase**, en plus du navigateur. Vous voyez donc l'avancement de la classe
sans que personne ait à exporter quoi que ce soit.

- **Adresse étudiante** : https://ggaillard.github.io/playlist-csharp/
- **Projet Supabase** : `pjuymnnblbydpjlpnoeh` (région Frankfurt)
- **Code classe** : `BTS2-SLAM-2026`

L'étudiant saisit son **numéro** une fois. Aucun nom, aucun prénom, aucune adresse
n'est stocké. S'il n'est pas connecté, ou si le réseau tombe, le tableau de bord
continue de fonctionner en local exactement comme avant.

---

## 1. Créer la classe — à exécuter une seule fois

Supabase → **SQL Editor** → **New query** → coller → **Run**.

```sql
-- La classe
insert into public.classes (code, nom, annee) values
  ('BTS2-SLAM-2026', 'BTS SIO 2 - SLAM - PlaylistApp C#', '2026-2027');

-- 25 etudiants, numerotes 01 a 25, sans aucune donnee nominative
insert into public.eleves (classe_id, numero)
select c.id, lpad(g::text, 2, '0')
  from public.classes c, generate_series(1, 25) g
 where c.code = 'BTS2-SLAM-2026';

-- Les 5 TP. Ouverts en permanence : les etudiants avancent a leur rythme.
insert into public.seances (classe_id, numero, titre, notee, ouverte)
select c.id, v.numero, v.titre, false, true
  from public.classes c,
       (values
         (0, 'TP0 - Mise en place de l environnement'),
         (1, 'TP1 - Console et POO'),
         (2, 'TP2 - Entity Framework Core'),
         (3, 'TP3 - API REST et SOA'),
         (4, 'TP4 - Architecture evenementielle')
       ) as v(numero, titre)
 where c.code = 'BTS2-SLAM-2026';
```

Ajuster `generate_series(1, 25)` au nombre réel d'étudiants.
Pour en ajouter un en cours d'année :

```sql
insert into public.eleves (classe_id, numero)
select id, '26' from public.classes where code = 'BTS2-SLAM-2026';
```

---

## 2. Suivre la classe

### Vue d'ensemble

```sql
select e.numero,
       count(*) filter (where r.reponse = 'true')                as missions,
       count(*) filter (where r.reponse in ('ok','ko'))          as quiz_repondus,
       count(*) filter (where r.reponse = 'ok')                  as quiz_reussis,
       to_char(max(r.updated_at), 'DD/MM HH24:MI')               as derniere_activite
  from public.eleves e
  join public.classes c on c.id = e.classe_id
  left join public.reponses r on r.eleve_id = e.id
 where c.code = 'BTS2-SLAM-2026'
 group by e.numero
 order by e.numero;
```

### Avancement TP par TP

```sql
select s.titre,
       count(distinct r.eleve_id) filter (where r.reponse = 'true') as etudiants_actifs,
       count(*) filter (where r.reponse = 'true')                   as missions_validees
  from public.seances s
  join public.classes c on c.id = s.classe_id
  left join public.reponses r on r.seance_id = s.id
 where c.code = 'BTS2-SLAM-2026'
 group by s.numero, s.titre
 order by s.numero;
```

### Qui bloque sur quoi

```sql
select r.question as mission, count(*) as nb_valide
  from public.reponses r
  join public.eleves e  on e.id = r.eleve_id
  join public.classes c on c.id = e.classe_id
 where c.code = 'BTS2-SLAM-2026' and r.reponse = 'true'
 group by r.question
 order by nb_valide asc;
```

Les missions en haut de liste sont celles que le moins d'étudiants ont validées.

---

## 3. Fin d'année — purge RGPD

```sql
select public.purger_annee('2026-2027');
```

Supprime classes, étudiants et réponses de l'année. Irréversible.

---

## Correspondance des identifiants

| Clé enregistrée | Signification | TP |
|---|---|---|
| `tp2-m1` | mission 1 du TP2 | 2 |
| `tp0-3` | mission 3 du TP0 | 0 |
| `q-3-2` | question 3 du quiz TP3 | 3 |
| `q-3-2-pick` | option choisie par l'étudiant | 3 |

Valeurs : `true` pour une mission cochée, `ok` / `ko` pour une réponse de quiz.

---

## Si ça coince

| Symptôme | Cause probable |
|---|---|
| « Numéro refusé » | Le numéro n'existe pas dans `eleves` pour cette classe |
| La barre reste grise | `assets/config.js` non chargé — vérifier les balises `<script>` en fin de `docs/index.html` |
| « Enregistrement en attente » | Séance fermée (`ouverte = false`) ou réseau coupé |
| Rien ne remonte côté enseignant | L'étudiant n'a pas saisi son numéro : il travaille en local |
