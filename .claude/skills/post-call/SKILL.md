---
name: post-call
description: >
  Exécute la routine automatique Bodysong déclenchée juste après un call de vente.
  Le cas par défaut est un call R1-R2 fusionné (diagnostic + pitch dans le même
  rendez-vous, format actuel de Mélanie) ; les calls séparés (R1 seul, puis R2, puis
  éventuellement R3) restent une option quand le prospect n'est pas encore qualifié ou
  que le temps a manqué. Dans les deux cas : retrouve l'enregistrement dans Fathom,
  crée la transcription dans Google Drive, met à jour la fiche prospect dans Notion
  (résumé structuré, analyse closer, étape pipe, température, pronostic), et génère la
  suite adaptée (trame du call suivant, ou séquence de nurturing + plan de suivi) selon
  où en est le prospect. Déclenche cette skill dès que l'utilisatrice tape une commande
  de la forme `post-call [TYPE] [PRENOM] [NOM]` (ex: "post-call R1-R2 Justine Hock",
  "post-call R1 Isabelle Mordant-Desanti"), ou formule la même intention autrement —
  "fais le post-call pour Justine", "traite le call que je viens de faire avec
  Isabelle", "j'ai fini mon rendez-vous avec Céline Lauron, tu peux t'en occuper". Ne
  pas attendre la syntaxe exacte : toute demande de traiter/documenter un call de vente
  qui vient de se terminer doit déclencher cette skill.
---

# post-call — routine automatique après un call de vente

Cette skill reconstruit une routine métier de Bodysong (accompagnement de pédagogues
de la voix) : après chaque call de vente, elle documente le call et met à jour le
pipeline commercial sans que l'utilisatrice ait à le faire manuellement.

Elle s'exécute **de bout en bout sans demander de confirmation à chaque étape** — les
seules confirmations sont celles que les règles de permission standard de Claude Code
imposent déjà pour les actions à risque (ex: appuyer sur "envoyer"). Ne pas ajouter de
garde-fou supplémentaire au-delà de ça.

Pour la structure exacte des toggles (sections, ton, niveau de détail) et un exemple
réel complet, lis [`references/exemple-r1r2-defiez.md`](references/exemple-r1r2-defiez.md)
avant de générer le contenu — ce document sert de gabarit de calibrage, pas juste de
documentation annexe.

## Paramètres de la commande

`post-call [TYPE] [PRENOM] [NOM]`

- `TYPE` : `R1-R2` (call fusionné diagnostic+pitch — **c'est le cas par défaut**, à
  utiliser aussi si l'utilisatrice ne précise pas de TYPE et que le contexte ne dit pas
  le contraire), ou `R1` / `R2` / `R3` / `EE` pour un call isolé dans un process en
  plusieurs étapes.
- `PRENOM NOM` : nom du prospect, tel qu'il apparaît dans Fathom et dans Notion (les
  deux graphies peuvent légèrement différer — voir étape 3).

Repère le TYPE dans le nommage réel des fichiers Drive existants pour rester cohérente
avec la convention en place : `AAAA-MM-JJ TYPE PRENOM NOM`, avec un tiret dans
`R1-R2` (pas `R1+R2`, pas `R1 R2`).

Quand un call R1 seul vient d'avoir lieu (pas encore pitché — prospect pas encore
qualifié, ou temps manquant), le TYPE est `R1` et l'étape 8 générera une trame pour le
R2 à venir plutôt qu'une séquence de nurturing.

`R3` est différent des autres : ce n'est pas un call de diagnostic/pitch, mais un call
de **déblocage** — reprendre contact avec un prospect déjà pitché (que le pitch ait eu
lieu en `R1-R2` fusionné ou en `R1` puis `R2` séparés) pour l'aider à avancer sur ce qui
le retient : trouver un financement, lever une objection précise, l'aider à se
décider, ou simplement refaire le point sur sa situation après un temps de nurturing.
Il peut donc arriver après n'importe lequel des deux parcours. Les étapes 5-6
(résumé, analyse) s'adaptent en conséquence — voir la note à l'étape 5.

## Qui fait quoi autour de cette routine

Utile pour comprendre où s'arrête le rôle de cette skill dans le process complet :

- **Avant le call** : Mathieu crée le canal WhatsApp à 3 (lui, Mélanie, le prospect),
  envoie le podcast/témoignages/infos de durée, puis le lien Zoom la veille. Mélanie
  n'intervient jamais sur ce canal avant le call.
- **Le call** : Mélanie mène le diagnostic et le closing.
- **Juste après le call (= cette skill)** : Mélanie envoie elle-même les messages de
  nurturing générés à l'étape 8, dans le canal à 3 déjà existant (cette skill ne crée
  jamais de canal, elle rédige seulement le contenu à envoyer).
- **Les jours/semaines suivants** : c'est Mathieu qui gère les relances de suivi
  (celles décrites dans le toggle `Plan de suivi`), au rythme annoncé par le prospect
  lui-même (ex: "je te dis sous une semaine" → relance à J+7, puis autant de fois que
  nécessaire).

## Hors scope

Trois toggles existent généralement déjà sur la fiche **avant** le call et ne sont ni
créés ni modifiés par cette routine : `Infos [TYPE]` (métadonnées de rendez-vous),
`Réponses formulaire` (réponses au formulaire de qualification), et `Trame [TYPE]`
(script de préparation pour le call — généré par un autre process, en amont). Si l'un
de ces trois n'existe pas encore sur la fiche, ce n'est pas à cette routine de le
créer — continue sans lui.

## Ressources fixes (Bodysong)

Ces IDs sont stables — ne pas les redemander à l'utilisatrice, les utiliser
directement :

| Ressource | Type | ID |
|---|---|---|
| Suivi des MP | Base Notion (fiches prospects) | `15132fc3-e413-803a-9335-c88130eb196e` |
| Ressources gratuites acquisition | Base Notion (vidéos/ressources nurturing) | `1d932fc3-e413-806d-8cdb-c427c813e0cd` |
| TRANSCRIPTIONS R1-R2-EE | Dossier Google Drive | `1KSczPPYkOA1BXIZvmHhKXpPffcKMRIMl` |
| Accompagnement Zèbre.pdf (présentation du programme envoyée en message 2 du nurturing — ne jamais dire "la proposition" au prospect, voir Étape 8 et 8bis) | Fichier Google Drive | `1UZcTT6SwOVJRawKvel8YXioHGlrtStY0` — lien : `https://drive.google.com/file/d/1UZcTT6SwOVJRawKvel8YXioHGlrtStY0/view?usp=sharing` |
| Trame R12 (script de référence du call fusionné diagnostic+pitch, créée le 21/09/2026 — fusionne Trame R1, Nouvelle Trame R2 et la formation DEA) | Page Notion | `3e232fc3-e413-80bc-a823-edc5188dd5f2` |

Si l'un de ces IDs ne résout plus rien (page/dossier supprimé ou renommé), ne pas
deviner un remplaçant : dis-le à l'utilisatrice et demande le bon lien avant de
continuer.

## Outils à charger

Ces MCP sont probablement différés en début de session. Charge-les en un seul appel
`ToolSearch` avant de commencer :

```
select:mcp__<fathom>__search_meetings,mcp__<fathom>__get_meeting_transcript,mcp__<fathom>__get_meeting_summary,mcp__<fathom>__find_person,mcp__<drive>__create_file,mcp__<drive>__search_files,mcp__<notion>__notion-search,mcp__<notion>__notion-fetch,mcp__<notion>__notion-update-page,mcp__<notion>__notion-create-pages,mcp__<notion>__notion-query-data-sources
```

(Les préfixes exacts changent d'une session à l'autre — repère-les dans la liste des
outils différés disponibles avant l'appel : ils correspondent aux intégrations Fathom,
Google Drive et Notion connectées au compte de l'utilisatrice.)

Avant de générer du contenu Notion avec des toggles, lis la description complète de
l'outil de création/mise à jour de pages Notion (`notion-create-pages` ou
`notion-update-page`) — elle documente la syntaxe markdown Notion exacte pour les blocs
toggle. Ne devine pas cette syntaxe.

## Déroulé

### Étape 1 — Retrouver le call dans Fathom

Cherche le call avec `search_meetings` / `list_meetings` en filtrant par nom du
prospect et date du jour. S'il y a plusieurs résultats le même jour pour ce prospect
(rare mais possible), prends le plus récent et signale l'ambiguïté dans la restitution
finale plutôt que de deviner en silence.

Si aucun call n'est trouvé pour ce prospect aujourd'hui : arrête-toi et dis-le à
l'utilisatrice — ne poursuis pas les étapes suivantes avec des données inventées.

Récupère la transcription complète (`get_meeting_transcript`) et la synthèse
(`get_meeting_summary`).

### Étape 2 — Créer la transcription dans Google Drive

Crée un Google Doc dans le dossier `TRANSCRIPTIONS R1-R2-EE` (ID ci-dessus).

Nom du fichier : `AAAA-MM-JJ TYPE PRENOM NOM` (date du call, pas la date du jour si
elles diffèrent) — reprends exactement le format observé dans le dossier (`R1-R2` avec
tiret pour un call fusionné).

Contenu : la transcription complète telle que retournée par Fathom, avec locuteurs et
timestamps — ne reformate pas, ne résume pas, c'est une archive brute. En revanche,
retire le lien Fathom hyperlien qui accompagne chaque timestamp
(`[00:00](https://fathom.video/calls/...?timestamp=0)`) : sur un call d'une heure, ça
représente des centaines de répétitions de la même URL et ça alourdit le document pour
rien. Garde uniquement le timestamp entre crochets, format `[MM:SS] Locuteur: texte`
(sans le `()` ni l'URL).

### Étape 3 — Retrouver la fiche prospect dans Notion

Cherche dans la base "Suivi des MP" (ID ci-dessus) par prénom + nom. Si le nom trouvé
dans Notion diffère légèrement de celui donné en argument (faute de frappe, nom de
scène, nom marital...), utilise ton jugement pour matcher — mais signale le rapprochement
fait dans la restitution finale plutôt que de le faire silencieusement.

Si aucune fiche ne correspond : arrête-toi et demande — ne crée pas de nouvelle fiche
prospect toi-même, ce n'est pas le rôle de cette routine.

Avant de modifier la fiche, note son état actuel (étape pipe, température, etc.) : tu
en auras besoin pour la restitution finale ("nouvelle étape pipe" implique de savoir
d'où on part).

Deux propriétés texte servent de **journal cumulatif** sur la fiche : `Sujets en cours
/ commentaire` et `Mots exacts du prospect (nurturing / follow up)`. Ne les écrase
jamais — ajoute une nouvelle entrée datée à la suite du contenu existant (avec `<br><br>`
avant la nouvelle entrée), sur le même modèle que les entrées déjà présentes.

### Étape 4 — Déterminer l'archétype du prospect

Avant de rédiger le résumé et l'analyse, détermine et enregistre la propriété
`Archetype` (select : Rationnel, Émotionnel, Prudent, Impulsif, Visionnaire) — cet
ordre est volontaire : connaître l'archétype en amont permet d'orienter le ton du
résumé, de l'analyse, et surtout des messages de nurturing à l'étape 8, plutôt que de
le renseigner après coup comme une simple case à cocher.

Déduis-le du **style de communication** du prospect pendant le call (pas de son
contenu ni de son secteur) :
- **Rationnel** : construit sa décision étape par étape, pose des questions précises sur le fonctionnement, compare, calcule
- **Émotionnel** : parle avec chaleur et passion, digresse sur du personnel, se décide au ressenti/à la connexion plus qu'au calcul
- **Prudent** : a besoin de temps, pèse le risque financier avec soin, veut réfléchir avant de s'engager, prudent avec l'argent
- **Impulsif** : décide vite, négocie et ajuste les détails rapidement en direct, veut avancer sans trop attendre
- **Visionnaire** : parle en termes de projet à long terme, d'impact, se projette loin au-delà du besoin immédiat

Un même prospect peut montrer plusieurs traits — choisis celui qui domine le plus
nettement sur l'ensemble du call, et si vraiment deux se valent, signale l'hésitation
dans la restitution finale plutôt que de trancher arbitrairement en silence.

Mets à jour cette propriété sur la fiche dès maintenant (pas besoin d'attendre l'étape
7 avec le reste des propriétés).

### Étape 5 — Toggle "Résumé [TYPE] (date du call)"

Génère un résumé structuré du call à partir de la transcription complète (pas
seulement de la synthèse Fathom — c'est le point de valeur ajoutée de cette étape).
Suis la structure et le ton du modèle "Résumé R1-R2" dans
[`references/exemple-r1r2-defiez.md`](references/exemple-r1r2-defiez.md) : sections à
emoji (Résonance Zèbre, Profil, Situation actuelle, Difficultés, Objectifs, Problèmes
centraux, Engagement & besoin, Importance, Urgence, Objections, Probabilité de
signature) — ancrées dans ce qui a été dit précisément pendant CE call, citations
verbatim quand elles existent. Si une section manque de matière dans la transcription,
le dire plutôt que la remplir au hasard.

**Cas `R3`** : le modèle de référence est calibré sur un call de diagnostic/pitch
(`R1-R2`), pas sur un call de déblocage. Adapte les sections au contenu réel d'un R3 —
ce qui a été fait pour lever le frein identifié précédemment (financement débloqué ou
pas, objection retraitée ou pas), l'état d'esprit du prospect à ce stade, et surtout
l'issue : décision prise (positive ou négative) ou encore en suspens. Les sections du
modèle qui ne s'appliquent pas à un déblocage (ex: "Résonance Zèbre", si le pitch de
fond a déjà eu lieu) peuvent être omises plutôt que remplies artificiellement.

### Étape 6 — Toggle "Analyse closer [TYPE] (date du call)"

Avant de rédiger cette analyse, lis dans cet ordre — c'est aussi l'ordre de priorité
si les sources se contredisent, une consigne directe de l'utilisatrice dans la
conversation primant toujours sur les deux :

1. [`references/methodologie-closer-dea.md`](references/methodologie-closer-dea.md) —
   condensé de la formation DEA (Rachid Aghbal, ajouté le 21/09/2026) : les 5
   archétypes de prospects avec leurs signaux de reconnaissance en call, le process
   ISKO/auto-closing en 4 temps (utile pour juger si la transition qualif → pitch
   était prématurée), les process en 5 étapes pour les objections finance et "en
   parler à quelqu'un" (repérer quelle étape précise a été sautée), les 7 excuses du
   cerveau, les repères de posture pendant la prise de paiement.
2. [`references/playbook-closer-chatgpt.md`](references/playbook-closer-chatgpt.md) et
   [`references/knowledge-base-closer-chatgpt.md`](references/knowledge-base-closer-chatgpt.md) —
   deux documents que l'utilisatrice affinait avec ChatGPT avant Claude sur ce dossier
   (ajoutés le 21/09/2026), jugés plus pertinents que la première version de cette
   étape sur la profondeur du diagnostic. Servent à affiner, jamais à écraser DEA ou
   une consigne directe — voir la mémoire `reference_sources_priorite_methodologie_vente`
   pour la hiérarchie complète des sources.

Structure de l'analyse (condensée depuis le format ChatGPT pour rester utilisable dans
un toggle de routine — ne pas produire une rubrique quand elle n'apporte rien sur ce
call précis, mieux vaut une analyse courte et dense qu'une checklist remplie
mécaniquement) :

- **Verdict commercial** : en 1-2 phrases, ce qui a réellement déterminé l'issue du
  call — pas un simple "bon call" / "mauvais call".
- **Forces** : 2-4 points précis, ancrés dans un moment réel du call, pas des
  généralités ("bonne écoute").
- **Ce qui a coûté des points** : classé par impact (majeur en premier), pas une
  liste plate. Distingue toujours ce qui a été réellement dit (fait) de ce que ça
  peut signifier (hypothèse) — ne jamais présenter une hypothèse comme un fait.
- **Qualification observée** : uniquement les dimensions qui posent vraiment question
  sur ce call précis (besoin réel, désir de résoudre maintenant, urgence, capacité à
  décider seule ou non, capacité financière, fit réel avec Zèbre) — pas la liste
  complète si tout est clair.
- **Objection principale** (s'il y en a une) : objection déclarée vs objection réelle
  probable (toujours signalée comme hypothèse, pas comme un fait), qualité du
  traitement au regard des process DEA.
- **Posture** : uniquement si un point saute aux yeux (neediness, sur-contrôle, peur
  du silence, pitch prématuré, closing prématuré) — rubrique absente si rien à
  signaler, pas de case vide remplie pour la forme.
- **Ce que j'aurais pu dire** : 1 à 3 formulations concrètes pour un moment précis du
  call, jamais une phrase générique de manuel. Ne jamais prétendre qu'une formulation
  alternative aurait garanti la vente — proposer, pas promettre.
- **Pattern à surveiller** : uniquement si un rapprochement avec un autre prospect te
  saute déjà aux yeux à partir de ce que tu sais en contexte — **ne lance pas de
  recherche systématique dans toute la base pour ça**, disproportionné à chaque
  exécution.
- **Priorités pour la suite** : jusqu'à 3 maximum, classées par impact — jamais une
  liste de dix corrections sans hiérarchie.

Inclus la température perçue et le pronostic de signature (%) dans cette analyse — il
n'existe pas de propriété Notion dédiée pour le pronostic, il vit uniquement ici et
dans la restitution finale.

### Étape 7 — Mettre à jour les propriétés Notion restantes

Propriétés confirmées dans le schéma réel de "Suivi des MP" (vérifié le 2026-08-31 —
si une des mises à jour échoue parce qu'un nom a changé, re-fetch le schéma via
`notion-fetch` sur l'ID de la base plutôt que de deviner) :

| Propriété | Type | Valeurs pertinentes |
|---|---|---|
| `Étape pipe` | select | ... R1 calé, Nurturer suite R1, R2 calé, En attente suite R2, Signature, Client... |
| `Température` | select | Froid, Tiède, Chaud |
| `Frein principal` | multi_select | (liste ouverte — choisis parmi les options existantes, n'invente pas de nouvelle valeur sans le signaler) |
| `Bénéfice principal désiré` | multi_select | idem |
| `Date du R1` / `Date du R2` / `Date du R3` / `Date du R4` | date | à renseigner selon le TYPE du call traité (les deux si `R1-R2` fusionné) |
| `Date prochaine action` | date | reprends l'échéance donnée par le prospect lui-même, pas une date arbitraire |
| `Échéance prospect / date de réactivation` | text | idem, en texte libre si la date n'est pas exacte ("fin septembre") |
| `Dernier FUP envoyé (post R2)` | text | description courte du message de nurturing envoyé à l'étape 7, si applicable |
| `Détail prochaine action (nurturing post R1 - follow up post R2)` | text | ce qui est prévu concrètement au prochain point de contact |
| `CODE` | select | Nurturer, FUP = follow up, V = vente, NV = non vendu, NQ = non qualifié, NRP = ne répond pas |
| `Ress 1` … `Ress 15 (date+sujet)` | text | slots pour tracer les ressources envoyées à l'étape 7 — utilise le premier slot vide, n'écrase pas un slot rempli |
| `Handicap et RQTH` | select | oui avec RQTH / oui sans RQTH / non |
| `Handicap nécessitant un aménagement spécifique` | text | description libre si mentionné |
| `Pitché` | checkbox | voir Étape 7bis ci-dessous — critère précis, jamais coché par défaut |

`Archetype` a déjà été rempli à l'étape 4 — pas besoin d'y revenir ici.

Pour les deux propriétés handicap, relis la transcription à la recherche de toute
mention d'un handicap ou d'une RQTH (le sujet n'est pas toujours amené spontanément,
donc une lecture attentive est nécessaire, pas juste un mot-clé évident). Si la
personne en parle, remplis les deux propriétés avec ce qui a été dit. **Si rien n'est
mentionné dans le call, mets `Handicap et RQTH` à `non`** plutôt que de laisser vide ou
deviner — c'est la valeur par défaut choisie par l'utilisatrice tant qu'elle n'a pas
encore pris l'habitude de poser la question systématiquement en call.

Attention : les `select`/`multi_select` Notion n'acceptent que des valeurs déjà
présentes dans les options existantes (sauf si l'outil de mise à jour permet d'en créer
une nouvelle à la volée). Si le call fait apparaître un frein ou un bénéfice qui ne
correspond à aucune option existante, signale-le dans la restitution finale plutôt que
de forcer une valeur approximative.

Mets à jour au minimum : `Étape pipe` (fais-la avancer si le call s'est bien passé et
qu'un prochain call est calé, ou reflète la réalité — ne fais pas avancer artificiellement
le pipe si le call ne le justifie pas), `Température`, la ou les dates du call traité,
`Frein principal` et `Bénéfice principal désiré` identifiés pendant le call.

**Cette discipline ne s'arrête pas à l'exécution initiale de la routine juste après le
call.** Chaque fois que la fiche est rouverte suite à un nouvel échange avec le prospect
pendant la phase de nurturing (réponse à un message, relance, nouvelle information reçue
par WhatsApp/mail, accord ou refus communiqué...), remets à jour les propriétés
concernées avec la même rigueur qu'à l'étape 7 — au minimum `Étape pipe` si la situation
a changé, `Détail prochaine action (nurturing post R1 - follow up post R2)`, `Dernier FUP
envoyé (post R2)`, `Date prochaine action`, et les journaux `Sujets en cours /
commentaire` / `Mots exacts du prospect (nurturing / follow up)`. Ne laisse jamais la
fiche refléter un état dépassé au prétexte que la routine post-call initiale est déjà
passée.

### Étape 7bis — Cocher "Pitché" si la proposition complète a été déroulée

Ajoutée le 14/09/2026 (demande de Mathieu). Sert à mesurer le vrai taux de closing sur
les pitchs réels — le dashboard du vendredi est branché dessus.

Sur chaque call (`R1`, `R1-R2` fusionné, `R2` ou `R3`), coche la case `Pitché` sur la
fiche si, et seulement si, la proposition complète a été déroulée pendant CE call :

- l'offre est nommée (Zèbre)
- sa structure est présentée (parcours, modalités, durée, contenu)
- le prix est annoncé
- une demande de décision ou d'engagement suit

**Test en une question : est-ce que la personne est repartie en position de dire oui ou
non ?** Si oui, coche. Si elle est repartie avec seulement un ordre de grandeur ("entre
3000 et 5000€"), un prix lâché sans structure, une présentation partielle, ou si le
call a été écourté avant la présentation : ne coche pas.

La case se coche une fois pour toutes, au premier call où le pitch a eu lieu. Elle ne
se décoche jamais ensuite, même si le prospect dit non par la suite — elle mesure le
fait d'avoir pitché, pas le résultat. Coche-la dans la foulée du call, avant de passer
au call suivant s'il y en a un autre à traiter.

### Étape 8 — Branches conditionnelles

D'abord regarde si le call a abouti à une **décision définitive** — c'est le cas le
plus fréquent en sortie de `R3` : signature, ou refus clair et assumé du prospect.

**Décision prise** (signature ou refus) :

- Mets à jour `Étape pipe` (`Signature` / `Client` si oui, ou l'étape de sortie
  correspondante si non) et `CODE` (`V = vente`, ou `NV = non vendu` / `NQ = non
  qualifié` selon le cas — utilise le libellé exact de l'option Notion, pas juste la
  lettre).
- Si c'est un oui : pas de trame ni de séquence de nurturing "en attente" à générer,
  mais enchaîne directement sur la séquence d'onboarding (voir Étape 8bis
  ci-dessous) — la boucle commerciale ne s'arrête pas net, elle change juste de nature.
- Si c'est un refus : note simplement l'issue et la raison réelle donnée par le
  prospect (utile pour améliorer les prochains calls, pas juste pour archiver). Rien
  d'autre à générer dans ce cas.

Sinon, détermine si un **prochain call est déjà calé** suite à celui-ci (regarde le
calendrier, les notes prises pendant le call, ou ce que le prospect a explicitement dit)
— c'est ce qui distingue les deux branches suivantes, pas le TYPE en lui-même :

**Un prochain call est calé** (ex: R1 seul vient d'avoir lieu et un R2 est booké ; ou,
plus rarement, un R1-R2 fusionné où un point de closing supplémentaire a été calé) :

- Génère une trame personnalisée pour ce prochain call, qui reprend les points clés de
  celui-ci (objections à retraiter, motivations à réactiver, informations déjà
  données à ne pas répéter) → toggle `Trame [TYPE suivant] adaptée` (ex: `Trame R2
  adaptée`).
- **Si le TYPE de CE call est `R1` (pas encore pitché) : envoie quand même la séquence
  de messages post-R1, même si le R2 est booké quelques jours plus tard.** Corrigée le
  21/09/2026 (cas Natalia Tziganov, R2 calé 3 jours après le R1) — l'ancienne version de
  cette étape disait de sauter le nurturing DM "sauf si l'écart de temps est important",
  ce qui a fait sauter la séquence à tort. Erreur : ce n'est PAS la séquence de
  "nurturing en attente d'une décision" (celle-là ne s'applique qu'à la branche
  suivante, quand aucun prochain call n'est calé) — c'est la clôture normale d'un R1,
  qui a sa propre raison d'être quel que soit l'écart de temps avant le R2 : remercier,
  reformuler ce qui a résonné pendant le call, et envoyer 1-2 témoignages miroirs +
  le lien vers la page "tous les témoignages" pour nourrir la réflexion avant le
  prochain call. Utilise le gabarit **"Message 1 — gabarit fixe (Connexion post-R1, R2
  pas encore fait)"** du fichier de référence
  ([`references/exemple-r1r2-defiez.md`](references/exemple-r1r2-defiez.md)), PAS le
  gabarit post-R1-R2 fusionné (qui présente déjà l'offre, inadapté ici puisque rien n'a
  été pitché) → toggle `Nurturing post R1 — ressources + DM`, mêmes règles de sélection
  de témoignages (lire la transcription complète avant de choisir, jamais déduire du
  seul titre) et de traçabilité (slots `Ress N`) que dans la branche "aucun prochain
  call n'est calé" ci-dessous.
- Si le TYPE de CE call est `R1-R2` fusionné et qu'un point de closing supplémentaire a
  été calé (le pitch a déjà eu lieu) : pas de séquence de témoignages — ce n'est plus un
  message de conversion (même logique qu'à l'étape 8bis) — un message de confirmation
  logistique suffit, inutile de nurturer entre les deux sauf si l'écart de temps est
  important (utilise ton jugement).

**Aucun prochain call n'est calé** (le prospect a besoin de temps, de réfléchir, de
trouver un financement, etc. — c'est le cas le plus fréquent après un R1-R2 fusionné) :

- Consulte la base "Ressources gratuites acquisition" (ID ci-dessus) et sélectionne
  2-5 ressources pertinentes pour ce prospect spécifiquement (pas les premières de la
  liste — matche sur ce qui a été dit pendant le call, en particulier sur l'objection
  principale et sur un profil de témoignage miroir). Rédige la séquence de messages
  WhatsApp complète, prête à envoyer, en suivant le ton et la structure du modèle dans
  le fichier de référence → toggle `Nurturing post [TYPE] — ressources + DM`. Adapte le
  ton à l'`Archetype` déterminé à l'étape 4 : plus factuel/structuré pour un Rationnel,
  plus chaleureux/connexion pour un Émotionnel, rassurant et sans pression de temps
  pour un Prudent, direct et orienté action pour un Impulsif, relié à la vision long
  terme pour un Visionnaire.
  - Ces messages sont **toujours** adressés au groupe WhatsApp à 3 avec Mathieu, et
    **signés Mélanie** — c'est elle la closer officielle. Ne signe d'un autre nom que
    si la propriété `Closer` de la fiche indique explicitement quelqu'un d'autre
    (remplacement ponctuel, ex: absence pour maladie).
  - Note chaque ressource envoyée dans le premier slot `Ress N (date+sujet)` libre.
- Génère un plan de suivi à moyen terme → toggle `Plan de suivi` : objectif du
  nurturing, séquence, signal de réouverture, ce qu'il faut faire si le signal est
  positif, action de relance si aucun signal d'ici l'échéance, objections à anticiper à
  la reprise de contact. Structure complète dans le fichier de référence.

### Étape 8bis — Onboarding après signature (si "oui")

Confirmée par l'utilisatrice le 02/09/2026 (cas Catherine Bonard). Se déclenche dès
qu'un "oui" ferme arrive, que ce soit pendant le call lui-même (branche "Décision
prise" ci-dessus) ou plus tard pendant une séquence de nurturing en cours — dans les
deux cas, bascule immédiatement sur cette procédure au lieu du nurturing "en attente".

**Séquence de messages, dans cet ordre** (groupe WhatsApp à 3, signés Mélanie) :

1. **Bienvenue** — pas de gabarit fixe unique constaté à ce stade (contrairement au
   message de connexion post-call), calibrer sur le ton chaleureux habituel. Exemple
   réel (Catherine, 02/09/2026) : "Merveilleux, bienvenue dans l'aventure 🚀 Avec
   Mathieu, on se réjouit de t'accompagner ces prochains mois."
2. **Rappel de la présentation du programme** — gabarit fixe, le même que le message 2
   du nurturing pré-signature (voir `references/exemple-r1r2-defiez.md`) : "Comme
   promis, voici la présentation du programme, avec le détail du parcours et des
   modalités, pour que tu puisses la relire tranquillement :" + lien fixe
   `Accompagnement Zèbre.pdf` (voir Ressources fixes), même si déjà montrée pendant le
   call. **Ne jamais dire "la proposition"** — confirmé par l'utilisatrice le
   17/09/2026 : "proposition" sonne trop conditionnel/hésitant, comme si c'était en
   option, alors qu'elle veut que le prospect s'en saisisse. Dire "la présentation du
   programme" à la place, partout où ce document est mentionné dans un message au
   prospect (avant ou après signature).
3. **Collecte des infos pratico-pratiques** — un seul message qui pose les questions
   nécessaires pour préparer le document contractuel (voir ci-dessous). **Ne redemande
   jamais une info déjà connue** (email/téléphone du prospect : déjà dans les
   propriétés `E-mail` / `Téléphone` de la fiche Notion) — ne poser que ce qui manque
   réellement. Pas de témoignage ni de ressource "preuve sociale" à ce stade de la
   séquence (voir note plus bas) — ce ne sont plus des messages de conversion, juste de
   la logistique.

**Déterminer d'abord OUI ou NON** : est-ce qu'une structure (association, société,
auto-entreprise...) avec un numéro de Siret finance l'accompagnement, ou est-ce que le
prospect finance sur ses fonds propres en tant que particulier ? Cette info est
généralement déjà connue depuis le call (ex: mention d'une association qui a de la
trésorerie dédiée à la formation).

**OUI (structure avec Siret) → Convention de formation professionnelle**

Infos à collecter :
- Nom de la structure
- Siret de la structure (peut se chercher sur Pappers si le prospect ne l'a pas sous
  la main, mais demande-le lui d'abord — plus fiable et plus rapide)
- Adresse postale du siège
- E-mail de contact du client (le prospect lui-même, pas la structure — probablement
  déjà connu, voir plus haut)
- Téléphone portable du client (idem, probablement déjà connu)
- Représentée par (nom de la personne signataire, ex: président·e)
- En sa qualité de (ex: Présidente, Trésorier...)
- E-mail de la structure

**NON (particulier, pas de Siret) → Contrat de formation professionnelle**

Infos à collecter :
- Nom et prénom du client
- Adresse postale du client
- E-mail de contact du client (probablement déjà connu)
- Téléphone portable du client (probablement déjà connu)

⚠️ Dans ce cas uniquement, un délai légal de rétractation de 14 jours à partir de la
signature du contrat s'applique — à prendre en compte pour la date de démarrage
réelle, et potentiellement à rappeler au client.

**Modalités de paiement** (les deux cas confondus, à demander/proposer dans cet ordre
de préférence) :
- En 1 fois si possible
- Sinon en 3 fois
- Au pire en 4 fois (montant = prix total / 4, ex: 3900€ → 975€/mois)
- Cas spécifique "particulier, pas de structure" : acompte de 30% avant le démarrage,
  puis le solde en 6 fois jusqu'à la fin de la formation (ex: sur 3900€ → acompte
  1170€, puis 6x455€, un versement après chaque mois de la formation)

**Ne pas envoyer de témoignage ni la page "témoignages bruts" dans cette séquence** —
c'était de la preuve sociale pour convaincre un prospect hésitant, ça n'a plus sa place
une fois le "oui" obtenu (confirmé par l'utilisatrice le 02/09/2026, cas Catherine
Bonard) : ça casse le rythme d'un message de logistique et ça sonne comme si on
continuait à vendre à quelqu'un qui a déjà signé. Garder ces ressources pour un vrai
moment d'accueil dans la communauté plus tard (présentation du groupe Facebook,
premiers ateliers) plutôt que comme argument recyclé.

**Toujours proposer une date de démarrage précise plutôt que de laisser la question
ouverte** — orienter le client vers une date concrète (ex: le prochain lundi) en
formulant une phrase du type "on partirait sur [date] pour démarrer, ça te va ?"
plutôt que "tu voudrais démarrer à partir de quand ?". Tenir compte du délai de
rétractation de 14 jours si applicable (cas NON).

**Pour le paiement, cadrer les deux options en une phrase courte et fluide**, pas une
énumération de "si... alors" : en une fois → par virement ; en plusieurs fois (ex: 4x)
→ paiement échelonné. Formulation type confirmée par l'utilisatrice le 02/09/2026 :
"Pour le paiement, deux options : en une fois par virement, ou étalé en plusieurs
fois, par exemple en 4x. Dis-moi ce qui arrange le mieux [structure]."

Trace la progression de cette collecte dans le journal `Sujets en cours / commentaire`
au fur et à mesure que les infos arrivent (même logique d'append que d'habitude), et
mets à jour `Détail prochaine action` avec ce qui manque encore.

### Étape 9 — Restitution finale

Dans le chat, en français, résume :

- Lien vers le Google Doc créé (transcription)
- Lien vers la fiche Notion mise à jour
- Nouvelle étape pipe (et l'étape précédente, pour le contexte)
- Température du prospect
- Pronostic de signature (%)
- Quelle branche de l'étape 8 a été prise (trame du prochain call, ou nurturing + plan
  de suivi) et pourquoi
- Toute ambiguïté ou hésitation rencontrée en route (call non trouvé du premier coup,
  nom approximatif, propriété Notion introuvable, frein/bénéfice sans option
  correspondante...) — mieux vaut signaler un doute que le masquer.

Ne rends pas de compte étape par étape avant la fin — exécute tout, puis restitue en
une fois.
