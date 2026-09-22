# Routine "post atelier" — Super Facilitateur vocal (mardi 12h-13h)

Process construit et testé en conditions réelles le 2026-09-01, avec l'atelier du
jour comme premier cas d'usage complet. Implémenté comme skill Claude Code :
`.claude/skills/post-atelier/` (SKILL.md + références `planification.md`,
`schoolmaker.md`, `facebook-tribu-zebre.md`).

## Déclencheurs

- Manuel : taper `post atelier` (ou formulation équivalente).
- Automatique : 5 vérifications planifiées le mardi (12h50, 13h05, 13h15, 13h25,
  13h45) — mais elles ne couvrent que la transcription Drive. Tout ce qui touche
  Schoolmaker/Facebook demande la validation humaine, donc reste manuel.

## Étapes du process

1. Retrouver l'atelier dans Fathom (titre "Super Facilitateur vocal, avec Mélanie").
2. Archiver la transcription complète dans Drive, dossier "00 ATELIERS COLLECTIFS ET
   FLASH" — nom `AAAA-MM-JJ Super Facilitateur Vocal Transcription`.
3. Proposer un **titre de leçon Schoolmaker** (`AAAA-MM-JJ Thème 1 + Thème 2 + ...`,
   ≤255 caractères) et une **description en bullets "✨"** groupés par thème sous des
   titres de bloc en gras — toujours validés en aller-retour avec moi avant
   application (compter plusieurs itérations de reformulation, normal).
4. Appliquer titre + description sur la leçon, **publier la leçon**, et **purger les
   leçons publiées de plus de 6 mois** (suppression → Corbeille Schoolmaker, pas
   définitive). Règle fixée une fois pour toutes, pas de revalidation à chaque fois
   sauf cas limite (date proche de 6 mois pile).
5. Préparer le **post Facebook "La Tribu Zèbre"** avec le même contenu (bullets +
   titres de bloc, convertis en gras Unicode car Facebook ne rend pas le markdown) —
   directement saisi dans le composeur du groupe, brouillon prêt, sans jamais cliquer
   sur envoyer. Photo toujours ajoutée par moi-même, jamais par l'assistant.

## Décisions notables

- Aucun connecteur MCP dédié pour Schoolmaker ni Facebook — tout passe par le
  navigateur Chrome connecté (extension Claude in Chrome), pas d'API directe.
- Le champ Description de Schoolmaker a un piège de lazy-load (ne se monte qu'au
  scroll) — la seule vérification fiable d'une sauvegarde est : recharger la page en
  entier, scroller jusqu'au champ, relire le contenu. Le toast de confirmation seul
  ne suffit pas.
- La convention de titre de bloc en gras dans la description Schoolmaker est une
  technique que j'ai validée (les transcriptions brutes d'autres leçons n'en ont pas,
  mais c'est parce que ce sont des captures telles quelles, pas une interdiction).

## Prochaine itération à construire

Mise à jour Notion + extraction d'insights pour le second cerveau — pas encore fait,
volontairement hors scope de cette version.
