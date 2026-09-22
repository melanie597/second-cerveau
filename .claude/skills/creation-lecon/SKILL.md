---
name: creation-lecon
description: >
  Aide Mélanie à rédiger le corps ("Texte leçon") d'une nouvelle leçon
  Bodysong à partir de ce qu'elle dicte, en respectant la trame déjà en
  place sur la plateforme de ressources (documentée dans la page Notion
  "2.2. Plateforme de ressources : copie" — cette skill la lit comme
  référence mais ne la modifie jamais). Détecte si la leçon dictée est un
  outil/jeu (Warm up, Jeux vocaux, Jeux rythmiques, Co-improvisation,
  Circle songs...) ou une leçon de programme (Alignement, Business, Super
  Facilitateur) et applique la trame correspondante : structure "Kézako"
  (Quoi / Qui / Combien / Où / Comment / Pourquoi / Durée / Quand, puis
  "Les tips du Super Facilitateur") pour la première, structure narrative
  en "tu" avec sous-titres thématiques pour la seconde. Termine toujours
  par un squelette de checklist "Étapes". Se déclenche dès que Mélanie
  dicte le contenu d'une nouvelle leçon ou dit vouloir en rédiger une —
  "nouvelle leçon sur X", "j'ai une leçon à écrire", "rédige-moi la leçon
  sur les sculptures sonores", "je te dicte le contenu pour une leçon",
  "j'ai un nouvel outil à documenter"... même sans dire explicitement
  "Notion" ou "Schoolmaker". Ne concerne PAS la description courte en
  bullets "✨" publiée sur Schoolmaker après l'atelier hebdomadaire (c'est
  le rôle de la skill post-atelier, sur un format différent) — cette
  skill-ci rédige le corps complet et long d'une leçon.
---

# creation-lecon — rédaction du corps d'une nouvelle leçon

Mélanie te dicte le contenu d'une leçon (souvent à l'oral, donc de façon
brute, dans le désordre, avec des hésitations). Ton rôle : transformer ça en
un texte de leçon structuré, dans le ton Bodysong, qui suit la même trame
que les 145 leçons déjà écrites sur la plateforme de ressources — pas une
structure que tu inventes.

**Tu ne touches jamais à la page Notion "2.2. Plateforme de ressources :
copie"** — elle sert uniquement de référence de style, jamais de cible
d'écriture. Le résultat de cette skill est un texte que tu proposes dans le
chat ; c'est Mélanie qui décide ensuite où il va (Notion, Schoolmaker,
ailleurs) et qui le colle elle-même.

## Étape 1 — Identifier le type de leçon

Deux trames coexistent sur la plateforme, et elles ne se mélangent pas.
Avant d'écrire quoi que ce soit, détermine laquelle s'applique — demande à
Mélanie si le module/programme visé n'est pas clair dans ce qu'elle a dicté.

**Trame A — outil / jeu** (modules Warm up, Jeux vocaux, Jeux rythmiques,
Co-improvisation, Circle songs, et plus généralement toute leçon qui
enseigne un exercice ou un dispositif d'animation reproductible) : lis
[`references/trame-outil.md`](references/trame-outil.md).

**Trame B — programme** (modules Alignement, Business, Super Facilitateur,
et plus généralement toute leçon qui accompagne une réflexion, un
positionnement ou une étape du parcours plutôt qu'un exercice ponctuel) :
lis [`references/trame-programme.md`](references/trame-programme.md).

Si le contenu dicté ne colle clairement à aucune des deux (ça arrive), dis-le
à Mélanie plutôt que de forcer une trame qui ne correspond pas — propose la
plus proche et explique pourquoi, laisse-la trancher.

## Étape 2 — Écouter avant d'écrire

Lis en entier ce que Mélanie a dicté avant de commencer à rédiger. Repère ce
qui manque pour remplir la trame (ex: elle a décrit le déroulé d'un jeu mais
pas dit combien de participants ni la durée) — pose la question plutôt que
d'inventer un chiffre ou une durée plausible. Une leçon avec un champ
manquant signalé vaut mieux qu'une leçon complète mais inventée.

Ce que tu ne dois jamais inventer : chiffres (durée, nombre de participants),
liens vers des documents, âges/publics précis. Ce que tu peux reformuler
librement : la mise en mots des étapes, les tips, les transitions — tant que
ça reste fidèle à ce que Mélanie a dit.

## Étape 3 — Rédiger dans le ton Bodysong

Les deux fichiers de référence donnent des exemples complets et les
conventions de ton (tutoiement systématique, emojis en tête de paragraphe
pour scander plutôt qu'en décoration de fin de phrase, formule d'ouverture
"Astuce 💡 Tu peux lire la vidéo en accéléré (x1,25)" pour une leçon vidéo,
etc.) — calibre-toi dessus, ne pars pas d'un ton générique de formation en
ligne.

## Étape 4 — Proposer, pas imposer

Présente le texte rédigé dans le chat, pas ailleurs. Attends-toi à des
allers-retours — c'est un texte qui va être lu par des élèves, Mélanie va
vouloir ajuster des formulations. Ne traite jamais le premier jet comme
définitif et ne l'applique nulle part de ton propre chef : ni la page
Notion, ni Schoolmaker.

## Étape 5 — Squelette "Étapes"

Termine systématiquement par un bloc "Étapes" au format checklist, sur le
modèle observé dans les deux trames :

```
**Étapes**
- [ ] Valider la leçon
- [ ] Accéder au document "[nom du document]" : [lien à compléter]
```

Le premier item ("Valider la leçon") est quasi systématique dans l'existant,
garde-le. Pour les items suivants, mets un placeholder `[lien à compléter]`
si Mélanie n'a pas donné le lien du document associé — ne cherche pas à le
deviner ou à le générer.
