# Conventions Schoolmaker — programme "Zèbre - Replay" / section "Super Facilitateur"

Ce fichier calibre la rédaction du titre et de la description de la leçon
Schoolmaker correspondant à l'atelier du jour (étape 5 du `SKILL.md`). Les deux
conventions ci-dessous ont été établies en observant des leçons existantes du
programme, pas inventées — reste dessus, ne dérive pas vers un style plus générique.

## Titre de la leçon

Format : `AAAA-MM-JJ Thème 1 + Thème 2 + Thème 3 + ...`

Chaque segment = un sujet concret abordé pendant l'atelier, formulé court (nom du
concept/jeu/technique + au besoin un seul bénéfice ou angle, pas une liste de
bénéfices). Exemple réel observé sur une leçon existante :

> 2026-08-25 Le langage imaginaire pour libérer l'expressivité + Progression
> grimace-corps-son-langage + Dialoguer et monologuer + Le jeu de la radio pour
> changer d'intention + L'intention, clé du solo + Zoom sur un programme de circle
> songs à la saison

Contrainte dure : environ **255 caractères espaces compris** au total (titre complet,
date incluse). Compte les caractères exactement avant de proposer une version à
l'utilisatrice — ne te fie pas à une estimation visuelle. Si le premier jet dépasse,
raccourcis les segments (moins de bénéfices listés par segment, formulations plus
courtes) plutôt que de couper un segment entier, sauf si l'atelier a vraiment couvert
peu de sujets distincts.

## Description de la leçon

Une liste de bullets `✨ ` (l'emoji sparkle, pas une étoile ⭐), chacun une
**prise à emporter autonome** : une phrase complète et exploitable seule, sans avoir
besoin du bullet précédent pour la comprendre.

**Regroupement par bloc thématique** : quand l'atelier a couvert plusieurs sujets
distincts, groupe les bullets apparentés sous un **titre de bloc en gras** (le nom du
sujet, ex: `**Faire évoluer un circle song**`), avec une ligne vide avant chaque
nouveau titre de bloc. C'est la technique validée par l'utilisatrice — les
transcriptions brutes d'autres leçons (voir exemples A et B ci-dessous) n'en ont pas
parce que ce sont des captures telles quelles, pas parce que la convention l'interdit.
Ne pas regrouper avec un titre de bloc si l'atelier n'a couvert qu'un seul sujet
cohérent — un titre de bloc unique au-dessus de toute la liste n'apporte rien.

**Éviter de répéter la même amorce de phrase** sur des bullets adjacents (ex: ne pas
enchaîner "Faire évoluer un circle song en..." trois fois de suite) — varie le verbe
ou la construction d'ouverture, même quand les bullets traitent du même sujet.

Deux exemples réels complets (programmes différents, pour calibrer la densité et le
ton — noter que les points techniques/musicaux peuvent inclure de la notation en
texte brut quand c'est pertinent, comme les degrés de gamme dans le 2e exemple) :

**Exemple A — atelier sur les prérequis et la justesse :**

```
✨ Clarifier en amont le cadre de l'atelier (tous niveaux ou prérequis) pour prévenir les problèmes de justesse

✨ Lister des prérequis en compétences observables plutôt qu'en "niveau" flou, pour aider les participants à s'auto-évaluer : "je suis capable de…"

✨ Recruter par audios envoyés en amont ou par audition collective en situation plutôt que par entretien individuel classique

✨ Le jeu Unisson Express pour évaluer rapidement la capacité d'un groupe à se caler ensemble sur une même note

✨ Si quelqu'un est vraiment à côté : faire un glissando (sirène) avec elle/lui jusqu'à la bonne note.

✨ Pendant un circle song, leviers rapides : baisser le volume du pupitre concerné, faire le signe de la main à l'oreille pour dire « écoute », couper et rechanter la mélodie a cappella pour voir si elle/il la recapte, soutien "en sandwich" entre deux personnes solides pour aider quelqu'un à retrouver la justesse, en dernier recours distribuer une autre voix plus simple ou une percussion

✨ En cycle long, demander en fin de séance si la personne a conscience de sa justesse et l'autorisation de la reprendre devant le groupe

✨ Construire un même morceau avec plusieurs niveaux de voix et de rythme (du lead facile à l'accompagnement plus complexe) pour que chacun soit challengé à sa mesure

✨ Pratique : le jeu du ping-pong en binômes ou petits groupes pour s'approprier les degrés

✨ Varier la tierce et la seconde pour explorer différentes couleurs de gammes (majeure, mineure, andalouse, indienne)
1 2 3 4 5 4 3 2 1
1 2 b3 4 5 b3 2 1
1 b2 b3 4 5 b3 b2 1
1 b2 3 4 5 3 b2 1

✨ Le jeu ping-pong polygammes : plusieurs sous-groupes qui explorent des gammes différentes en simultané, avec possibilité de changer de groupe

✨ Varier régulièrement les binômes et trinômes pour chanter de partenaire et dynamiser la pratique
```

**Exemple B — atelier sur les virelangues :**

```
✨ Les virelangues, une matière polyvalente et un véritable terrain de jeu pour nous facilitateurs : parfait pour lâcher prise, rire ensemble, réveiller la voix, travailler la diction, oser improviser sans pression de "bien faire"

✨ Découverte de la liste de 115 virelangues réutilisables en icebreaker, échauffement, jeu vocal, co-improvisation ou circle song

✨ Pratique du jeu en 3 étapes (lecture, rythme, mélodie) pour transformer un virelangue en matière musicale

✨ Temps en sous-groupes pour créer ses propres variantes de consignes à partir des virelangues

✨ Les virelangues pratiqués sous forme de Ich kiki pour travailler le temps contre temps et/ou Ich kiki mélodique pour la conscience mélodique

✨ Ich kiki version battle en deux lignes pour driver les départs en grand groupe

✨ Virelangues comme matière musicale pour jouer ensuite sur le volume du chœur

✨ Séquence minimale où chacun ajoute une boucle complémentaire
```

## Piège technique : lazy-load du champ Description

Le champ Description de la page d'édition de leçon (admin Schoolmaker) ne se monte/
n'hydrate qu'une fois scrollé dans le viewport (lazy-load). Concrètement :
- Juste après avoir chargé ou rechargé la page, le champ peut apparaître vide même
  quand du contenu existe déjà en base — ce n'est pas une preuve d'absence de
  contenu, juste que le composant n'est pas encore monté.
- Toujours scroller explicitement jusqu'au champ (`scroll_to` sur son ref) avant d'y
  cliquer, d'y taper, ou de conclure qu'il est vide.

## Méthode fiable pour éditer et vérifier ce champ

Le `ref` renvoyé par `find`/`read_page` pour ce textbox ne correspond pas toujours à
la zone `contenteditable` réelle (l'éditeur riche "Lexxy") — cliquer dessus peut ne
rien focaliser, et taper ensuite ne fait alors rien du tout, silencieusement.

1. Après `scroll_to`, clique sur des **coordonnées à l'intérieur du texte visible**
   (pas sur le ref du wrapper).
2. Vérifie que le focus a bien pris avant de taper quoi que ce soit :
   `document.activeElement` doit être un `DIV.lexxy-editor__content` avec
   `isContentEditable: true` (usage debug de `javascript_tool` uniquement — jamais
   pour écrire le contenu à ta place).
3. Pour appliquer un titre de bloc en gras : `cmd+b`, taper le texte du titre,
   `cmd+b` à nouveau pour désactiver, puis `Return`.
4. Une fois tapé, ne te fie **jamais** au seul toast "La leçon a été mise à jour" —
   c'est un signal optimiste côté client, pas une preuve de sauvegarde serveur.
   Le test fiable : recharger la page en entier (navigation complète, pas juste
   revenir en arrière), `scroll_to` de nouveau jusqu'au champ, puis relire son
   contenu via `innerText` (et `querySelectorAll('b,strong')` si tu veux vérifier
   les titres en gras) — seulement là tu as la confirmation que ça a pris.
