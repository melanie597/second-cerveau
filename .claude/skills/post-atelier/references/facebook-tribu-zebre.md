# Post Facebook hebdomadaire — groupe "La Tribu Zèbre"

Groupe : https://www.facebook.com/groups/latribuzebre?locale=fr_FR

Ce fichier calibre la préparation du post Facebook que l'utilisatrice publie elle-même
chaque mardi après l'atelier, une fois le titre et la description Schoolmaker validés
(étape 5 du `SKILL.md`). **Rappel de l'étape 7** : on va jusqu'à saisir le texte
complet dans le composeur de post du groupe Facebook, brouillon prêt — mais on ne
clique jamais sur publier/envoyer, et on ne s'occupe jamais de la photo. Il ne doit
rester à l'utilisatrice qu'à ajouter sa photo et cliquer sur le bouton d'envoi.

## Gabarit

```
Chers Zèbres,
L'atelier Super Facilitateur vocal du jour est disponible en replay.
Voici les sujets clés abordés ensemble :

[titre de bloc 1 en gras Unicode]
✨ bullet
✨ bullet
✨ bullet

[titre de bloc 2 en gras Unicode]
✨ bullet
✨ bullet
```

L'ouverture ("Chers Zèbres, ... Voici les sujets clés abordés ensemble :") est fixe,
ne la reformule pas. Le contenu qui suit reprend **exactement** les bullets validés
sur Schoolmaker à l'étape 5 — même texte, même regroupement par thème, complet (pas
de version condensée sauf si l'utilisatrice le demande explicitement ce jour-là).

## Exemple réel complet (atelier à sujet unique, donc un seul bloc, sans titre)

```
Chers Zèbres,
L'atelier Super Facilitateur vocal du jour est disponible en replay.
Voici les sujets clés abordés ensemble :
✨ Pratique du langage imaginaire (grommelot) pour libérer l'expressivité et préparer l'improvisation vocale
✨ Une progression en douceur : grimace, corps, son, puis langage imaginaire
✨ Dialoguer et monologuer en langage imaginaire pour développer l'écoute et le fil narratif
✨ Le jeu de la radio pour changer d'intention en cours d'improvisation
✨ L'intention comme plus grand apprentissage du langage imaginaire, et clé d'un solo vocal réussi
✨ Zoom sur un programme de circle songs à la saison
```

Quand l'atelier couvre un seul sujet cohérent (comme ici), il n'y a qu'un bloc de
bullets, sans titre de bloc — le titre de bloc n'a d'intérêt que pour distinguer
plusieurs sujets distincts entre eux.

## Titres de bloc en gras : pourquoi de l'Unicode, pas du markdown

Le composeur de post Facebook standard ne rend aucune mise en forme markdown —
`**Titre**` s'affiche tel quel, astérisques compris, jamais en gras. Pour un vrai
rendu gras visible sur Facebook (et sur la plupart des plateformes texte brut), il
faut substituer chaque lettre par son équivalent Unicode "Mathematical Sans-Bold" —
ce sont des caractères différents, pas un style, donc ils s'affichent en gras partout,
y compris dans un post ou un commentaire sans aucune option de formatage.

Table de conversion (lettres/chiffres non listés — accents, ponctuation — restent
inchangés) :

| Standard | Bold Unicode |
|---|---|
| A–Z | 𝗔–𝗭 (U+1D5D4 et suivants) |
| a–z | 𝗮–𝘇 (U+1D5EE et suivants) |
| 0–9 | 𝟬–𝟵 (U+1D7EC et suivants) |

Pour générer la conversion de façon fiable (ne pas taper les caractères Unicode à la
main, risque d'erreur) :

```python
def to_bold_sans(s):
    bold_map = {}
    for i, c in enumerate("ABCDEFGHIJKLMNOPQRSTUVWXYZ"):
        bold_map[c] = chr(0x1D5D4 + i)
    for i, c in enumerate("abcdefghijklmnopqrstuvwxyz"):
        bold_map[c] = chr(0x1D5EE + i)
    for i, c in enumerate("0123456789"):
        bold_map[c] = chr(0x1D7EC + i)
    return "".join(bold_map.get(c, c) for c in s)
```

Exemple (titres de la leçon du 2026-09-01) :

| Original | Converti |
|---|---|
| Quiz miaou miaou | 𝗤𝘂𝗶𝘇 𝗺𝗶𝗮𝗼𝘂 𝗺𝗶𝗮𝗼𝘂 |
| Construire un atelier découverte de 45 minutes | 𝗖𝗼𝗻𝘀𝘁𝗿𝘂𝗶𝗿𝗲 𝘂𝗻 𝗮𝘁𝗲𝗹𝗶𝗲𝗿 𝗱é𝗰𝗼𝘂𝘃𝗲𝗿𝘁𝗲 𝗱𝗲 𝟰𝟱 𝗺𝗶𝗻𝘂𝘁𝗲𝘀 |
| Faire évoluer un circle song | 𝗙𝗮𝗶𝗿𝗲 é𝘃𝗼𝗹𝘂𝗲𝗿 𝘂𝗻 𝗰𝗶𝗿𝗰𝗹𝗲 𝘀𝗼𝗻𝗴 |

(Les caractères accentués comme "é" ne font pas partie du jeu Mathematical
Sans-Bold — ils restent affichés en style normal au milieu du reste du mot en gras,
c'est normal et déjà validé par l'utilisatrice.)
