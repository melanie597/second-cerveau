---
name: pre-call
description: >
  Exécute la routine automatique Bodysong déclenchée avant les calls de vente à venir
  (mini-diagnostics = R1-R2 fusionné, format par défaut de Mélanie). Pour chaque fiche
  prospect Notion dont le call est calé ("Étape pipe" = "R1 calé") et qui a déjà son
  toggle "Profil R1" (généré en amont par la routine de préqualification WhatsApp de
  Mathieu) mais pas encore de trame de préparation, fusionne ce profil avec la page de
  référence "Trame R12" pour produire une trame personnalisée directement sur la fiche.
  Se déclenche manuellement quand l'utilisatrice tape `pré-call` (ou une formulation
  équivalente : "fais le pré-call", "prépare mes trames de la semaine", "quelles fiches
  n'ont pas encore de trame ?"), et automatiquement chaque lundi matin pour couvrir les
  réservations de la semaine. Peut être relancée n'importe quel jour de la semaine sans
  risque de doublon : elle ne retraite jamais une fiche qui a déjà sa trame.
---

# pre-call — routine automatique avant un call de vente

Cette skill prépare Mélanie avant ses calls de vente R1-R2 (mini-diagnostics). Elle ne
gère jamais l'appel séparé "R2 seul" (prospect déjà passé par un R1) : ce cas est
couvert par la skill post-call elle-même (Étape 8, toggle `Trame R2 adaptée`), qui a
déjà accès au résumé du R1 pour personnaliser ce call suivant. **pre-call ne s'occupe
que du tout premier call d'un prospect.**

## Qui fait quoi autour de cette routine

- **Avant le call, en amont** : quand un prospect réserve un mini-diagnostic, la
  routine de préqualification WhatsApp de Mathieu crée la fiche (si elle n'existe pas
  déjà), y dépose le contenu du formulaire de réservation dans un toggle **`Profil R1`**
  (activité actuelle, projet collectif, blocage, objectif à 6 mois, niveau d'élan,
  déclencheur de la réservation), et passe `Étape pipe` à `R1 calé`. Cette skill ne
  refait jamais ce travail — elle en dépend.
- **Cette skill (pre-call)** : lit `Profil R1`, le fusionne avec la Trame R12 de
  référence, et dépose le résultat dans un nouveau toggle sur la même fiche.
- **Le call** : Mélanie mène le call en s'appuyant sur ce toggle.
- **Juste après le call** : la skill post-call prend le relais (résumé, analyse closer,
  mise à jour du pipe, suite adaptée).

## Ressources fixes (Bodysong)

| Ressource | Type | ID |
|---|---|---|
| Trame R12 (référence, call fusionné diagnostic+pitch) | Page Notion | `3e232fc3-e413-80bc-a823-edc5188dd5f2` |
| Suivi des MP | Base Notion (fiches prospects) | `15132fc3-e413-803a-9335-c88130eb196e` |
| Suivi des MP | Data source (pour les requêtes filtrées) | `collection://59164ef7-d030-457c-8e7b-4c7e3f20f48d` |

Si l'un de ces IDs ne résout plus rien, ne pas deviner un remplaçant : dis-le à
l'utilisatrice et demande le bon lien avant de continuer.

## Outils à charger

Ces MCP sont probablement différés en début de session. Charge-les en un seul appel
`ToolSearch` avant de commencer :

```
select:mcp__<notion>__notion-fetch,mcp__<notion>__notion-update-page,mcp__<notion>__notion-query-data-sources
```

(Le préfixe exact change d'une session à l'autre — repère-le dans la liste des outils
différés disponibles avant l'appel : il correspond à l'intégration Notion connectée au
compte de l'utilisatrice.)

## Style de la trame — règle non négociable

Lis d'abord [`references/format-trame.md`](references/format-trame.md) avant de rédiger
quoi que ce soit. Résumé : bullets courts uniquement, **jamais de guillemets, jamais de
point d'interrogation, jamais de commentaire/blabla explicatif**. Une trame, c'est un
pense-bête à scanner en dix secondes pendant l'appel, pas un script à lire ni un
document de formation. Cette règle a été posée explicitement par l'utilisatrice le
21/09/2026 après une première version rejetée — ne pas y revenir sans qu'elle le
redemande.

## Déroulé

### Étape 1 — Repérer les fiches candidates

Interroge la base "Suivi des MP" (`notion-query-data-sources` sur le data source
ci-dessus) filtrée sur `Étape pipe = "R1 calé"`. C'est le signal fiable qu'un premier
call est bien calé et pas encore passé (Mathieu fait avancer cette propriété dès la
réservation, et la skill post-call la fait avancer à son tour après le call — donc une
fiche qui traîne longtemps sur "R1 calé" est presque toujours une fiche à traiter ici).

Si aucune fiche n'a ce statut : dis-le simplement (mode manuel) ou ne dis rien du tout
(mode automatique, voir Étape 5) — ce n'est pas une erreur, certaines semaines sont plus
calmes.

### Étape 2 — Vérifier la présence du Profil R1 et l'absence de trame

Pour chaque fiche candidate, fetch la page complète et cherche deux toggles (par
correspondance approximative sur le titre, insensible à la casse et aux petites
variations d'accents/espaces — ne pas exiger une correspondance exacte au caractère
près) :

- Un toggle contenant **"Profil R1"** → c'est la matière première.
- Un toggle contenant **"Trame de préparation"** (ex: `🎯 Trame de préparation R1-R2`)
  → si déjà présent, cette fiche est déjà traitée, passe à la suivante sans y toucher.

**Si `Profil R1` est absent** alors que `Étape pipe = R1 calé` : ne fabrique rien à sa
place. Note cette fiche à part dans la restitution finale ("en attente du profil côté
Mathieu") plutôt que de deviner un profil à partir du nom seul.

### Étape 3 — Fusionner Profil R1 + Trame R12

Pour chaque fiche qui a un `Profil R1` mais pas encore de trame :

1. Relis le contenu de `Profil R1` sur cette fiche (activité actuelle, projet
   collectif, blocage exprimé, objectif à 6 mois, niveau d'élan, déclencheur de la
   réservation — la liste exacte des champs peut varier légèrement d'une fiche à
   l'autre, prends ce qui est là).
2. Relis la structure complète de la page Trame R12 (référence ci-dessus) — c'est le
   squelette (sections I à X) à personnaliser, pas à réinventer.
3. Rédige un nouveau toggle **`🎯 Trame de préparation R1-R2`** sur la fiche, construit
   ainsi :
   - Une ligne de rappel en tête : le script détaillé complet vit dans la page Trame
     R12 (insère une vraie mention de page Notion vers cette page, pas juste un lien
     texte — voir la syntaxe `<mention-page>` dans `notion://docs/enhanced-markdown-spec`
     si besoin de se rafraîchir la mémoire), ce toggle ne contient que la
     personnalisation pour ce call précis.
   - Une sous-partie par section de la Trame R12 (I à X) **mais uniquement les sections
     où il y a une vraie personnalisation à apporter** à partir du Profil R1 — pas la
     peine de répéter une section générique qui n'apporte rien de plus que la
     référence. Sur un profil pauvre en détails, la trame personnalisée peut être très
     courte (quelques sections seulement) : c'est normal, ne remplis pas pour remplir.
   - Dans chaque section retenue, reformule en bullets courts ce qu'il faut adapter :
     le sujet précis à creuser en découverte, l'angle de connexion si le prospect
     arrive sans nurturing vu, le témoignage miroir le plus pertinent pour le pitch,
     l'objection la plus probable à anticiper compte tenu du profil, l'archétype
     pressenti (à confirmer en live) pour calibrer le coût de l'inaction et le
     follow-up. Applique strictement le style de
     [`references/format-trame.md`](references/format-trame.md).
4. Insère ce toggle sur la fiche (`update_content`, `insert_content` en fin de page ou
   juste après le toggle `Profil R1` — garde ce qui existe déjà, n'écrase jamais
   `Profil R1` ni les autres toggles de la fiche).

### Étape 4 — Cas particulier : rien à personnaliser

Si le `Profil R1` est vide ou quasi vide (formulaire à peine rempli, prospect qui a
juste réservé sans répondre aux questions facultatives) : crée quand même le toggle,
mais réduit-le à une ligne honnête plutôt que d'inventer du contenu — ex: "Profil
formulaire quasi vide à ce stade, découverte à mener sans a priori." Ne jamais
fabriquer une problématique ou un blocage qui ne vient pas du Profil R1.

### Étape 5 — Deux modes de déclenchement

**Mode manuel** (`pré-call` tapé dans le chat) : traite toutes les fiches candidates
trouvées à l'Étape 1, peu importe la date du call à venir. Termine toujours par une
restitution dans le chat (voir Étape 6).

**Mode automatique** (tâche planifiée le lundi matin) : même déroulé, mais reste
silencieuse s'il n'y a rien à faire (aucune fiche candidate, ou toutes déjà traitées) —
l'utilisatrice n'est pas forcément devant l'écran. Si au moins une trame a été créée,
une restitution courte suffit (pas besoin de détailler chaque trame comme en mode
manuel, juste la liste des prénoms traités).

### Étape 6 — Restitution (mode manuel)

Dans le chat, en français, résume :

- Fiches où une trame a été créée (prénom + lien vers la fiche)
- Fiches déjà traitées, sautées sans y toucher
- Fiches en attente du `Profil R1` côté Mathieu (rien créé, à surveiller)
- Toute ambiguïté rencontrée (toggle au titre légèrement différent, Profil R1 très
  pauvre, etc.)

Ne rends pas de compte fiche par fiche en cours de route — traite tout, puis restitue
en une fois.

## Hors scope (pour l'instant)

- La création de la fiche elle-même et du toggle `Profil R1` : c'est le travail de la
  routine de Mathieu, cette skill ne le duplique jamais.
- Le cas R2 séparé (prospect déjà passé par un R1, deuxième call à préparer) : couvert
  par la skill post-call (Étape 8), pas par pre-call.
- La réservation ou la modification de créneaux Calendly : cette skill ne touche jamais
  au calendrier, elle ne fait que lire l'état des fiches Notion.
