---
name: post-atelier
description: >
  Exécute la routine automatique Bodysong déclenchée juste après l'atelier collectif
  hebdomadaire "Super Facilitateur vocal" (tous les mardis, 12h-13h, avec marge en cas
  de dépassement). Version 3 de la routine : retrouve l'enregistrement dans Fathom,
  récupère la transcription complète et la dépose dans le dossier Google Drive
  "00 ATELIERS COLLECTIFS ET FLASH" ; propose un titre de leçon et une description en
  bullet points "✨" (groupés par thème sous des titres de bloc en gras) sur Schoolmaker
  à partir du contenu réellement abordé — toujours validés par l'utilisatrice avant
  d'être appliqués — puis publie la leçon et purge les leçons publiées de plus de 6
  mois ; enfin prépare (sans jamais l'envoyer) le texte du post Facebook hebdomadaire
  pour le groupe "La Tribu Zèbre", que l'utilisatrice publie elle-même avec sa propre
  photo. L'upload de la vidéo reste entièrement manuel (Mélanie s'en charge). Se
  déclenche soit manuellement quand l'utilisatrice tape `post atelier` (ou une
  formulation équivalente : "fais le post atelier", "traite l'atelier de ce matin"),
  soit automatiquement via une tâche planifiée qui vérifie plusieurs fois le mardi midi
  si l'atelier est terminé. D'autres actions (mise à jour Notion, extraction
  d'insights pour le second cerveau) seront ajoutées dans une version ultérieure — ne
  pas les inventer ici.
---

# post-atelier — routine automatique après l'atelier collectif du mardi

Cette skill documente la routine Bodysong qui suit l'atelier de groupe hebdomadaire
"Super Facilitateur vocal" (mardi 12h-13h). **Version 3** : elle archive la
transcription dans Drive, prépare et applique le titre + la description de la leçon
correspondante sur Schoolmaker, publie cette leçon, purge les leçons de plus de 6 mois,
puis prépare le texte du post Facebook hebdomadaire (jamais publié automatiquement).
Pas de mise à jour Notion ni d'extraction d'insights pour l'instant, ce sera une
itération suivante.

Elle s'exécute **sans demander de confirmation à chaque étape**, sauf dans les cas
d'ambiguïté ou d'échec décrits plus bas — **à l'exception notable de tout ce qui
touche Schoolmaker (étape 5)**, qui est un contenu visible par les élèves du programme
Zèbre : le titre et la description doivent toujours être proposés à l'utilisatrice et
validés par elle avant d'être appliqués sur la plateforme.

## Deux façons d'être déclenchée

1. **Manuelle** : l'utilisatrice tape `post atelier` (éventuellement suivi d'une date,
   ex: `post atelier 25/08`, pour retraiter un atelier passé). Sans date précisée, la
   cible est l'atelier du jour si on est mardi, sinon le mardi le plus récent.
2. **Automatique** : une tâche planifiée appelle cette routine plusieurs fois le mardi
   entre 12h50 et 13h45 (voir `references/planification.md` pour le détail des
   horaires et comment les recréer si besoin) pour vérifier si l'atelier est terminé
   et traiter l'enregistrement dès qu'il est disponible.

**Différence de comportement importante entre les deux modes** — voir étape 3 : le
mode automatique reste silencieux tant qu'il n'y a rien à faire, le mode manuel
explique toujours ce qu'il a trouvé ou pas trouvé.

## Ressources fixes (Bodysong)

| Ressource | Type | ID / valeur |
|---|---|---|
| Atelier collectif | Réunion Fathom | Titre : `Super Facilitateur vocal, avec Mélanie` (enregistrée par Mélanie Rallo) |
| 00 ATELIERS COLLECTIFS ET FLASH | Dossier Google Drive | `16v1TxHsu_FIJtWGwDJZtcxNEei-9Xhpi` |
| Zèbre - Replay | Programme Schoolmaker | `https://bodysong.schoolmaker.co/school-admin/products/994fee78-28d0-4f83-bb81-62264489bbd6` |
| Super Facilitateur | Section Schoolmaker (dans Zèbre - Replay) | `section_id=7e416b9f-dbf2-4c42-a4b0-816ecddc6acd` |

Si l'ID du dossier Drive ou l'un des IDs Schoolmaker ne résout plus rien (dossier/
section supprimé ou renommé), ne pas deviner un remplaçant : dis-le à l'utilisatrice
et demande le bon lien avant de continuer.

## Outils à charger

Ces MCP sont probablement différés en début de session. Charge-les en un seul appel
`ToolSearch` avant de commencer :

```
select:mcp__<fathom>__list_meetings,mcp__<fathom>__search_meetings,mcp__<fathom>__get_meeting_transcript,mcp__<drive>__search_files,mcp__<drive>__create_file
```

(Les préfixes exacts changent d'une session à l'autre — repère-les dans la liste des
outils différés disponibles avant l'appel : ils correspondent aux intégrations Fathom
et Google Drive connectées au compte de l'utilisatrice.)

Schoolmaker n'a pas de connecteur MCP dédié (vérifié : pas dans les outils connectés,
pas dans le registre de connecteurs disponibles). L'étape 5 passe donc par le
navigateur Chrome réel de l'utilisatrice, via les outils `mcp__claude-in-chrome__*`
(charge au minimum `tabs_context_mcp`, `navigate`, `computer`, `find`,
`javascript_tool` — ce dernier uniquement pour *lire* l'état de la page, jamais pour
modifier son contenu à sa place, voir étape 5). Si la connexion échoue au premier
essai ("Claude in Chrome n'est pas connecté"), redemande à l'utilisatrice de vérifier
que l'extension est installée et le panneau connecté, puis réessaie — c'est souvent
transitoire.

## Déroulé

### Étape 1 — Déterminer la date cible

Si une date est passée en argument, utilise-la. Sinon : si on est mardi, cible
aujourd'hui ; sinon, cible le mardi le plus récent précédant la date du jour.

### Étape 2 — Vérifier si cet atelier a déjà été traité

Avant d'aller chercher quoi que ce soit dans Fathom, cherche dans le dossier Drive
(ID ci-dessus, `search_files` avec `parentId`) un document dont le titre contient la
date cible au format `AAAA-MM-JJ` et "Super Facilitateur Vocal Transcription" (les
titres existants varient légèrement — espace en trop, tiret ou pas — matche de façon
tolérante sur date + mots-clés plutôt que sur une chaîne exacte).

Si un tel document existe déjà : l'atelier a déjà été traité.
- **Mode automatique** : arrête-toi silencieusement, ne fais rien de plus, ne notifie
  pas l'utilisatrice (une précédente vérification planifiée a déjà fait le travail).
- **Mode manuel** : dis-le à l'utilisatrice et donne le lien du document existant,
  ne le recrée pas.

### Étape 3 — Retrouver l'atelier dans Fathom

Cherche avec `list_meetings` (filtré sur la date cible, ou sur `created_after` /
`created_before` couvrant cette journée) une réunion dont le titre contient
"Super Facilitateur" et "Mélanie". S'il y en a plusieurs le même jour (rare), prends
la plus longue / la plus proche de 12h-13h et signale l'ambiguïté dans la restitution
finale.

**Si aucune réunion n'est trouvée** (l'atelier n'a pas encore eu lieu, est encore en
cours, ou Fathom n'a pas fini de la traiter) :
- **Mode automatique** : arrête-toi silencieusement — sauf si c'est le **dernier**
  créneau de vérification prévu dans la journée (voir
  [`references/planification.md`](references/planification.md)), auquel cas signale à
  l'utilisatrice que l'atelier du jour n'a toujours pas été retrouvé dans Fathom à ce
  stade, pour qu'elle puisse vérifier de son côté.
- **Mode manuel** : dis-le clairement à l'utilisatrice, ne poursuis pas avec des
  données inventées.

Une fois la réunion trouvée, récupère la transcription complète avec
`get_meeting_transcript`.

### Étape 4 — Créer la transcription dans Google Drive

Crée un Google Doc dans le dossier `00 ATELIERS COLLECTIFS ET FLASH` (ID ci-dessus).

Nom du fichier : `AAAA-MM-JJ Super Facilitateur Vocal Transcription` (date de
l'atelier, format et libellé identiques à la convention déjà en place dans ce dossier
— pas d'espace superflu, pas de tiret avant "Transcription").

Contenu : la transcription complète telle que retournée par Fathom, avec locuteurs et
timestamps — ne reformate pas, ne résume pas, c'est une archive brute. Retire le lien
Fathom hyperlien qui accompagne chaque timestamp
(`[00:00](https://fathom.video/calls/...?timestamp=0)`) et garde uniquement le
timestamp entre crochets, format `[MM:SS] Locuteur: texte` (sans le `()` ni l'URL) —
même règle que pour les transcriptions de calls de vente, pour rester cohérente avec
l'existant.

### Étape 5 — Titre et description de la leçon sur Schoolmaker

Cette étape s'appuie sur la transcription récupérée à l'étape 3 — pas besoin de la
relire depuis Drive. Lis d'abord
[`references/schoolmaker.md`](references/schoolmaker.md) : il documente les deux
conventions exactes (format du titre, format de la description) avec des exemples
réels tirés d'autres leçons du programme — calibre-toi dessus avant de rédiger quoi
que ce soit, ne les invente pas de mémoire.

**Mode automatique : cette étape est sautée.** Elle demande une validation humaine du
contenu (titre + description visibles par les élèves), donc elle n'a de sens qu'en
mode manuel, quand l'utilisatrice est présente pour valider. En mode automatique,
arrête-toi après l'étape 4 (transcription Drive) — ne touche pas à Schoolmaker.

**Mode manuel :**

1. Connecte-toi au navigateur (`tabs_context_mcp`), navigue vers la leçon du jour dans
   la section "Super Facilitateur" du programme "Zèbre - Replay" (IDs ci-dessus). Si tu
   ne connais pas l'URL exacte de la leçon (id de leçon), demande-la à l'utilisatrice —
   ne devine pas un id de leçon.
2. **Si la leçon n'existe pas encore** pour cette date : crée-la (nom provisoire
   `AAAA-MM-JJ`, type Vidéo) — c'est à cette routine de le faire désormais. Si
   l'utilisatrice l'a déjà créée elle-même (ex: pour ne pas attendre la fin de l'upload
   vidéo), réutilise cette leçon existante, ne la duplique pas.
3. Ne touche jamais à la vidéo elle-même (upload, suppression) — c'est intégralement
   manuel, toujours du ressort de l'utilisatrice.
4. Rédige un **titre proposé** suivant la convention documentée dans le fichier de
   référence, à partir des sujets réellement abordés pendant l'atelier (pas générique).
   Vérifie sa longueur exacte en comptant les caractères (ex: via un script), pas à
   l'oeil — la limite observée est d'environ 255 caractères espaces compris. Montre-le
   à l'utilisatrice dans le chat et attends sa validation ou ses corrections avant de
   l'appliquer sur la page.
5. Une fois le titre validé, rédige une **description proposée** en bullet points
   "✨" suivant la même convention (un bullet = une prise à emporter autonome). Groupe
   les bullets apparentés sous un **titre de bloc en gras** (technique validée par
   l'utilisatrice) quand l'atelier a couvert plusieurs sujets distincts — voir le
   fichier de référence pour le niveau de détail attendu par bullet et par titre de
   bloc, et pour la règle « ne pas répéter la même amorce de phrase sur des bullets
   adjacents ». Montre-la dans le chat et attends validation avant de l'appliquer —
   attends-toi à plusieurs allers-retours de reformulation avant le feu vert final, ne
   traite pas le premier jet comme définitif.
6. Une fois les deux validés : applique le titre puis la description sur la page. Suis
   la « Méthode fiable pour éditer et vérifier ce champ » du fichier de référence
   (clic dans le texte visible plutôt que sur le ref du wrapper, vérification du focus,
   `cmd+b` pour les titres de bloc, et surtout : ne jamais te fier au seul toast de
   confirmation — reload complet + `scroll_to` + relecture du contenu pour confirmer
   la sauvegarde côté serveur).

### Étape 6 — Publier la leçon et purger les leçons de plus de 6 mois

Une fois le titre et la description appliqués et confirmés sauvegardés (étape 5) :

1. Passe la leçon du jour de `Archivée` à `Publiée` (bouton "Cliquer pour publier" sur
   la page de la leçon).
2. Repère la leçon publiée la plus ancienne de la section "Super Facilitateur" (navigue
   via `Leçon précédente` en partant de la leçon du jour, ou consulte la liste complète
   de la section). Si sa date dépasse 6 mois par rapport à la date cible (étape 1) :
   supprime-la (elle part dans la Corbeille Schoolmaker, ce n'est pas une suppression
   définitive). Si elle a moins de 6 mois pile ou que le calcul est limite, ne la
   supprime pas et signale la date exacte à l'utilisatrice dans la restitution — c'est
   elle qui tranche dans le doute, ne force jamais une suppression sur un cas limite.
3. Cette étape touche à du contenu public (publication) et à une suppression — informe
   l'utilisatrice de ce que tu as fait dans la restitution finale (étape 8), mais pas
   besoin de validation préalable comme pour le titre/description : la publication de
   *cette* leçon a déjà été validée en amont (étape 5), et la purge des 6 mois est une
   règle fixe que l'utilisatrice a posée une fois pour toutes.

**Mode automatique : cette étape est également sautée**, pour les mêmes raisons que
l'étape 5.

### Étape 7 — Préparer le post dans le composeur Facebook de "La Tribu Zèbre"

Lis d'abord [`references/facebook-tribu-zebre.md`](references/facebook-tribu-zebre.md)
pour le gabarit exact, un exemple réel, et la table de conversion en gras Unicode.

**Cette étape va jusqu'au brouillon prêt à envoyer, mais ne clique jamais sur
publier/envoyer.** L'utilisatrice a été explicite : elle veut n'avoir plus qu'à
cliquer sur le bouton d'envoi elle-même. La limite est donc précise : tout faire
jusqu'à avoir le texte complet et correctement saisi dans le composeur du post, puis
s'arrêter — ne pas soumettre le formulaire, ne pas cliquer sur "Publier" / "Envoyer" /
un raccourci clavier équivalent.

1. Reprends l'intégralité des bullets "✨" de la description Schoolmaker validée à
   l'étape 5 (le post complet, pas une version condensée, sauf demande explicite
   contraire) — même contenu, même regroupement par thème.
2. Convertis les titres de bloc en gras Unicode (police "Mathematical Sans-Bold", ex:
   `𝗧𝗶𝘁𝗿𝗲`) plutôt qu'en `**markdown**` — Facebook ne rend pas le markdown dans le
   composeur de post standard, seuls de vrais caractères Unicode différents
   s'affichent visuellement en gras. Le fichier de référence documente la table de
   conversion / un script pour la générer.
3. Utilise l'ouverture fixe du gabarit ("Chers Zèbres, L'atelier Super Facilitateur
   vocal du jour est disponible en replay. Voici les sujets clés abordés ensemble :")
   — ne la reformule pas.
4. Via le navigateur (`mcp__claude-in-chrome__*`, même mécanisme que pour Schoolmaker —
   pas de connecteur MCP dédié à Facebook), navigue vers
   `https://www.facebook.com/groups/latribuzebre`, ouvre le composeur de nouvelle
   publication du groupe, clique dedans, et saisis le texte complet (voir la
   « Méthode fiable » du fichier de référence Schoolmaker pour la vigilance générale
   sur le focus des champs de saisie riches — vérifie que le texte est bien passé
   avant de t'arrêter, ne présume pas que la frappe a pris).
5. Laisse le brouillon tel quel dans le composeur, ne le soumets pas. Ne t'occupe
   jamais de la photo — c'est systématiquement l'utilisatrice qui la choisit et
   l'ajoute elle-même avant d'envoyer.
6. Dis à l'utilisatrice dans le chat que le post est prêt dans le composeur, qu'il ne
   reste plus qu'à ajouter la photo et cliquer sur envoyer.

**Mode automatique : cette étape est également sautée** (même raison que 5 et 6 — elle
dépend du contenu validé à l'étape 5).

### Étape 8 — Restitution

**Mode manuel** : dans le chat, en français, donne le lien du Google Doc créé, le
titre et la description finalement appliqués sur Schoolmaker (avec le lien de la
leçon), le nouveau statut de publication, toute suppression de leçon effectuée (ou le
cas limite signalé sans suppression), le texte du post Facebook préparé, et toute
ambiguïté rencontrée en route (plusieurs réunions le même jour, titre de document
existant légèrement différent de la convention, leçon Schoolmaker introuvable, etc.).

**Mode automatique** : si un document Drive a été créé, une simple confirmation
suffit (pas besoin de développer) — l'utilisatrice n'est pas devant l'écran à ce
moment-là. Rappelle que les étapes Schoolmaker et Facebook restent à faire en mode
manuel. Si rien n'a été fait (atelier pas encore trouvé, pas le dernier créneau), pas
de restitution du tout.

## Hors scope (pour l'instant)

Ne pas ajouter de ta propre initiative : mise à jour d'une fiche Notion, extraction
d'insights pour le second cerveau. Si l'utilisatrice en parle, c'est une évolution à
construire ensemble dans une prochaine itération de cette skill — pas à improviser ici.
