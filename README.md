# Petit Éditeur GLSL

Un éditeur de shaders léger pour Windows, pensé pour écrire, régler et tester en temps réel des effets visuels générés par le GPU — le genre d'effets qu'on trouve sur [Shadertoy](https://www.shadertoy.com/). Compatible avec le code Shadertoy tel quel : pas de syntaxe spéciale à apprendre.

**[⬇ Télécharger la dernière version](../../releases/latest)**

![Aperçu de l'éditeur](docs/screenshot1.png)

![Aperçu de l'éditeur](docs/screenshot2.png)

## Ce que ça permet de faire

- **Aperçu en direct** : le rendu se met à jour à chaque frappe, sans bouton "compiler" à cliquer.
- **Curseurs de réglage automatiques** : chaque valeur numérique de votre code (couleurs, tailles, vitesses…) devient un curseur ajustable à la souris dans un panneau à côté, regroupé par catégorie, avec un bouton "réinitialiser" et un bouton "randomiser" pour explorer rapidement des variantes.
- **Plusieurs passes de rendu** (Image + Buffers A à D) pour construire des effets en plusieurs étapes qui s'alimentent les uns les autres, comme sur Shadertoy.
- **Entrées interactives** : la souris (position, clic) et le clavier peuvent piloter votre effet en direct.
- **Textures** : chargez vos propres images, utilisez des textures générées automatiquement (damier, bruit), ou des images à 360° (cubemap) pour des reflets/environnements.
- **Glisser-déposer** une image directement sur un emplacement de texture.
- **Export d'image** : sauvegardez l'image actuellement affichée en PNG.
- **Projets** : enregistrez votre travail dans un fichier de projet et rouvrez-le plus tard, avec une liste des fichiers récents.
- **Indicateur de performance** : un petit graphique affiche le temps de calcul de chaque image.
- **Préférences** : taille de police de l'éditeur, mini-carte, vitesse de mise à jour de l'aperçu.
- **Réduction de code ("Golf")** : un outil en un clic pour compacter votre shader au minimum d'octets, utile pour les concours de code le plus court possible.

## Installation

1. Cliquez sur **[⬇ Télécharger la dernière version](../../releases/latest)** ci-dessus (ou allez dans l'onglet **Releases** du dépôt).
2. Téléchargez le programme d'installation (`PetitEditeurGLSL-Setup-x.x.x.exe`), dans la section **Assets** de la release.
3. Lancez-le et suivez les instructions à l'écran.
4. Une fois installé, lancez **Petit Éditeur GLSL** depuis le menu Démarrer.

> Windows 64 bits requis, avec une carte graphique compatible.

## Prise en main

1. Ouvrez l'application : un shader d'exemple s'affiche déjà à l'écran.
2. Modifiez le code dans l'éditeur à gauche — l'aperçu à droite se met à jour automatiquement.
3. Repérez le panneau des curseurs : chaque nombre détecté dans votre code y apparaît sous forme de réglage que vous pouvez glisser sans toucher au texte.
4. Utilisez les onglets **Image / Buffer A / B / C / D / Common** au-dessus de l'éditeur pour construire un effet en plusieurs passes.
5. Assignez des images, des textures procédurales, une souris ou un clavier à un emplacement `iChannel` via le panneau à droite de l'aperçu.
6. Une fois satisfait du résultat, utilisez le menu **Fichier** pour enregistrer votre projet, exporter une image PNG, ou exporter le code une fois "golfé" (compacté).

### Raccourcis utiles

| Action | Raccourci |
|---|---|
| Annuler | Ctrl+Z |
| Rétablir | Ctrl+Y |

## Besoin d'aide ?

Une question, un problème, une idée d'amélioration ? Ouvrez une [issue](../../issues) sur ce dépôt.

## Licence

Petit Éditeur GLSL est **gratuit**, mais son code source n'est pas fourni.
Voir [LICENSE.md](LICENSE.md) pour le détail de ce qui est autorisé ou non.
