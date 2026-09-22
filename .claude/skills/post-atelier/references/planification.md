# Planification des vérifications automatiques

L'atelier "Super Facilitateur vocal" a lieu tous les mardis de 12h à 13h, avec parfois
un léger dépassement. Pour capter l'enregistrement dès qu'il est disponible dans
Fathom sans harceler l'utilisatrice, une tâche planifiée déclenche cette skill
(en mode automatique) aux créneaux suivants, chaque mardi :

- 12h50
- 13h05
- 13h15
- 13h25
- 13h45 ← **dernier créneau de la journée**

C'est ce dernier créneau (13h45) qui doit rompre le silence si l'atelier n'a toujours
pas été retrouvé dans Fathom à ce moment-là (voir étape 3 du `SKILL.md`) — tous les
créneaux précédents restent silencieux en cas d'échec, pour laisser le temps à
l'atelier de se terminer et à Fathom de traiter l'enregistrement.

## Recréer ces tâches planifiées

Si les tâches doivent être recréées (changement d'horaire de l'atelier, tâche
supprimée par erreur...), chacune des cinq est une tâche planifiée indépendante,
hebdomadaire, le mardi, à l'heure indiquée ci-dessus, avec le même prompt de
déclenchement : demander l'exécution de la skill `post-atelier` en mode automatique
pour le mardi du jour.
