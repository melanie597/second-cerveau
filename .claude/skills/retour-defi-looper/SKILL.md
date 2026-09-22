---
name: retour-defi-looper
description: >
  Génère un retour écrit sur une création d'élève pour le Défi Looper (exercice mensuel
  de l'accompagnement Zèbre où les élèves s'enregistrent en train de superposer des
  boucles vocales au looper avec une contrainte donnée, puis partagent leur vidéo pour
  avoir un retour). Mélanie dicte ses observations brutes sur ce qu'elle a entendu/vu
  (musicalité, construction, contrainte, pistes de progression...) et cette skill les
  reformule dans sa trame en 5 blocs, avec son ton et son vocabulaire habituels. Se
  déclenche dès que l'utilisatrice dicte un retour de défi looper ou formule une
  intention équivalente — "retour défi looper [prénom]", "fais le retour looper de X",
  "j'ai une création à commenter pour le défi", ou simplement le fait de dicter ses
  observations sur une vidéo de défi juste après avoir mentionné "défi looper". Cette
  skill ne regarde jamais la vidéo/l'audio elle-même (pas d'outil pour ça) — elle
  travaille uniquement à partir de ce que Mélanie décrit.
---

# retour-defi-looper — génère un retour de style Mélanie pour une création du Défi Looper

## Principe

Mélanie dicte ses observations à l'oral sur une création d'élève (Défi Looper). Cette
skill ne fait qu'une chose : reformuler et structurer ces observations dans sa trame
habituelle, avec son style — elle n'invente jamais de contenu musical que Mélanie n'a
pas décrit.

## Avant de générer

Lis en entier le manuel de style
[`../../sops/2026-07-31_guide-retours-defis-looper.md`](../../sops/2026-07-31_guide-retours-defis-looper.md)
avant chaque génération — ne te fie pas à un souvenir approximatif du ton, il contient
la philosophie, la structure exacte, le vocabulaire favori et les formulations à
éviter.

Lis aussi
[`references/exemple-daniele-virelangue.md`](references/exemple-daniele-virelangue.md) :
premier cas réel validé par Mélanie, qui précise des choix de forme non couverts par le
manuel (bullet points dans "Pour aller encore plus loin", emoji ✅ dans "Retour sur la
contrainte", comment relier une progression au retour du défi précédent). Sert de
gabarit de calibrage, pas juste de documentation annexe.

Vérifie que tu as, dans ce que Mélanie a dicté :
- le **prénom** de l'élève
- des **observations substantielles** sur la création (pas juste "c'était bien") : au
  moins de quoi nourrir le bloc "ce qui ressort particulièrement"
- une indication sur la **contrainte du défi** et si/comment elle a été utilisée

Si un de ces trois éléments manque, demande-le avant de générer plutôt que de deviner
ou de laisser un bloc vide/générique.

## Génération du retour

Structure la réponse selon les 5 blocs du manuel de style :

1. **Bravo Prénom 🌟** — phrase chaleureuse
2. **🎶 Ce qui ressort particulièrement** — développe plusieurs observations dictées par
   Mélanie, en expliquant toujours *pourquoi* ça fonctionne musicalement (jamais une
   liste de compliments)
3. **🌬️ Retour sur la contrainte** — la contrainte est-elle présente ? comment est-elle
   utilisée ? comment pourrait-elle être encore plus mise en valeur ?
4. **🌻 Pour aller encore plus loin** — pistes formulées en "tu pourrais...", jamais "il
   faut..." ; concrètes, pas génériques
5. **👏 En résumé** — 2-3 phrases positives, tournées vers la suite

Respecte scrupuleusement les règles du manuel : voir d'abord la beauté, jamais de ton
correctif, questions ouvertes plutôt que réponses toutes faites, vocabulaire favori
(musicalité, univers, texture, ancrage, narration, intention...), formulations
typiques.

**Ne reformule que ce que Mélanie a réellement observé** — si elle a dicté peu de
détails sur un aspect (ex: la construction), ne comble pas le vide en inventant une
progression ou une structure qu'elle n'a pas mentionnée.

## Après génération

Présente le retour dans le chat pour validation — cette skill ne publie ni n'envoie
jamais rien elle-même (Mélanie poste ensuite elle-même, sur Facebook, Schoolmaker ou
WhatsApp selon où la vidéo a été partagée). Si Mélanie veut ajuster un passage, retravaille
uniquement ce passage plutôt que de régénérer tout le retour.
