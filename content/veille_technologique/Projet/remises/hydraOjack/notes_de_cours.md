+++
title = "Notes de cours"
weight = 2
+++

# 1. Introduction

Hydra Ojack est une plateforme pour développer des visuels impressionants en temps réel dans votre navigateur. C'est gratuit, open source et pour tous.

- Hydra est écrit en **JavaScript** et compile en **[WebGL](#webgl)**.
- Sa syntaxe est inspirée de la **synthèse vidéo modulaire et analogique**.
- Tout se passe dans le **navigateur** !

Hydra s'inscrit dans le domaine du _live-coding visuel_, une pratique où l’artiste génère des images en modifiant du code en temps réel. Cette approche permet une expérimentation rapide et une exploration créative directe. Hydra se distingue aussi par son accessibilité, car tout fonctionne directement dans le navigateur, sans installation ou configuration complexe.

Les notes de cours suivantes ont pour objectif de présenter les principes fondamentaux de Hydra, ses sources visuelles, ses transformations, son pipeline graphique, ainsi que quelques exemples pratiques pour comprendre comment créer des compositions génératives.
_Plusieurs exercices sont disponnibles dans la section [atelier](/veille_technologique/Projet/remises/hydraOjack/atelier/)_

> "Hydra is a system for creating live visuals, inspired by analog video synths and modular synthesis."
> **Olivia Jack (Ojack)**

---

# 2. À propos de la créatrice : Olivia Jack

Originaire de San Francisco, **Olivia Jack**, créatrice de Hydra, est une programmeuse et artiste qui travaille avec des applications open-source, de la cartographie, du live-coding et des interfaces expérimentales depuis près de 15 ans. Elle est fascinée par le live-coding, qui lui permet d’entrer dans un dialogue continu et stimulant avec la machine. Comme Hydra le démontre, elle apprécie particulièrement l'algorithmie et le chaos. Elle réside et travaille actuellement en Colombie.

Elle aime changer les valeurs ainsi que les ordres des choses simplement pour voir leur résultat, elle n'emprunte pas un chemin linéaire et préfère tester plusieurs différentes choses avec son code.  
Elle n'a pas uniquement créé Hydra,

En bref, Olivia est une personne qui adore expérimenter !

---

# 3. Contexte et origine de Hydra

Son inspiration vient directement des **synthétiseurs vidéo analogiques** des années 90 (et légèrement avant). Un synthétiseur permet de transformer les signaux électriques en formes lumineuses !

Voilà donc ce que Olivia a fait : elle a recréé cette approche modulaire mais dans un contexte récent, tout au chaud dans notre navigateur web.  
La vision de ce projet est que chaque fonction agit comme un module connectable !

## La philosophie d'Hydra

Hydra utilise une philosophie de type **"web-native"**, ce qui signifie :

- Il n'y a aucune installation requise,
- il s'exécute en JavaScript,
- le rendu est optimisé/accéléré par [WebGL](#webgl),
- le partage est simple et fait via des [snippets](#snippets)

---

# 4. Pourquoi Hydra ?

Je trouvais tout simplement les résultats agréables et impressionnants !

Hydra est largement utilisé chez les **live-coders**, les membres de communautés de **musique électronique** et d'**art numérique** :

- Il permet de créer des visuels rapidement pour :
  - des performances live (des concerts ou sets de DJ) ;
  - de l'art interactif ;
  - des expérimentations d'art génératif.
- Il offre une **boucle de feedback courte** : on modifie le code -> l'image change.
- C'est idéal pour **enseigner** :
  - les bases de la synthèse visuelle,
  - la programmation GPU sans entrer dans la complexité des shaders.

Hydra se situe donc à la frontière entre :

- un langage artistique,
- et un framework technique pour exploiter la puissance de la carte graphique

---

# 5. Son architecture générale

Hydra repose sur quelques concepts centraux :

## Sources

Une **source** est un flux vidéo de base à partir duquel Hydra construit une image. Voici les principales :

| Fonction     | Description                                                    |
| ------------ | -------------------------------------------------------------- |
| `osc()`      | Génère des oscillations : lignes ondulées, motifs périodiques. |
| `noise()`    | Produit une texture organique aléatoire, comme des nuages.     |
| `shape()`    | Crée une forme géométrique (cercle, triangle, polygone).       |
| `gradient()` | Génère un dégradé de couleurs animé.                           |
| `src(o0)`    | Réutilise le contenu d'un buffer pour créer du feedback.       |
| `camera()`   | Utilise la webcam comme source vidéo.                          |

Chaque source génère une image de base qui sera ensuite transformée.

## Buffers

C'est quoi un **buffer** ?

C'est une zone mémoire qui stocke une image ... Hydra en a 4 --> `o0`, `o1`, `o2`, `o3`  
C'est plus facile avec un exemple je vous l'avoue :

```js
osc(10, 0.1, 1).out(o0);
```

-> o0 contient un oscillateur maintenant

```js
src(o0).rotate(0.03).out(o0);
```

-> Hydra prend l'image qu'on fait avant, lui applique une rotation (`rotate()`) et le renvoie dans notre buffer `o0`.
Résultat :

- La rotation s'ajoute petit à petit
- Ça fait un tourbillon **infini**
- Ça donne un tunnel complètement psychédélique qui se répète en boucle à l'infini et se déforme continuellement !

## Chaînage fonctionnel

Hydra utilise un **pipeline fonctionnel** : chaque appel renvoie un objet sur lequel on peut enchaîner des transformations.

Exemple :

```js
osc(10, 0.1, 0.8).rotate(0.2).color(1, 0.5, 0.3).out();
```

Dans cet exemple :

- 'osc()' génère la source de base.
- 'rotate()' applique une rotation sur la source.
- 'color()' modifie la couleur du signal visuel.
- 'out()' envoie le résultat final vers la sortie d'affichage.

Ce modèle de chaînage permet de lire le code visuel comme une suite logique d'opérations, un peu comme des modules branchés les uns à la suite des autres dans un synthétiseur vidéo.  
Chaque ligne représente une transformation indépendante qui s'applique sur le résultat de la précédente, ce qui rend Hydra particulièrement intuitif pour le live-coding.

---

# 6. Le pipeline graphique : de JavaScript à WebGL

Hydra prend le code que tu écris en JS et le transforme en images animées.  
Hydra est simplement un traducteur !

1- Tu écris du code simple, par exemple : osc().rotate().out()

2- Hydra comprend ce que tu veux dessiner, comme : fais ça, fais ci

3- Hydra traduit tout ça pour la carte graphique, qui elle ne comprend que des maths compliquées.

4- La carte graphique (via WebGL) dessine l'image à l'écran.

5- Ce processus recommence 60x par seconde, ce qui donne une animation fluide.

Résultat :  
Tu écris du JavaScript hyper simple, et Hydra s'occupe de toute la partie compliquée pour afficher des visuels en temps réel.

---

# 7. Les sources visuelles

Une **source** c'est l'image qui commence tout dans Hydra. C'est cette source qui est ensuite transformée, combinée ou encore utilisée en feedback.

| Source                            | Description simple et utile                                                                                     |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `osc(freq, sync, offset)`         | Génère des oscillations périodiques : lignes, vagues, motifs répétitifs. Très utilisé pour les fonds abstraits. |
| `noise(scale, speed)`             | Produit un bruit organique aléatoire, semblable à des nuages ou des textures fluides.                           |
| `shape(sides, radius, smoothing)` | Crée une forme géométrique : triangle, carré, polygone, cercle (100+ côtés).                                    |
| `gradient(speed)`                 | Génère un dégradé de couleurs animé. Peut servir de fond ou de modulation.                                      |
| `solid(r, g, b, a)`               | Produit une couleur unie. Utile comme fond ou comme masque.                                                     |
| `voronoi(scale, speed)`           | Crée un motif dynamique en cellules, très populaire pour les effets « génératifs ».                             |
| `src(o0)`                         | Réutilise l'image stockée dans un buffer (feedback). Permet des effets de boucle et de déformation infinie.     |
| `camera()`                        | Utilise la webcam comme source vidéo. Parfait pour les performances interactives.                               |

<sub>Descriptions de la documentation officielle Hydra : https://hydra.ojack.xyz/docs/docs/reference/</sub>

Ces sources constituent la base de Hydra. Les sections suivantes (8 et 9) présentent les transformations et opérations utilisées pour construire des visuels plus complexes.

---

# 8. Transformations visuelles

Les **transformations** modifient une **source** pour lui donner un effet visuel particulier : rotation, zoom, effet kaléidoscope...

| Transformation   | Description                                                                        |
| ---------------- | ---------------------------------------------------------------------------------- |
| `rotate(angle)`  | Fait tourner l'image. Plus l'angle augmente, plus la rotation est rapide.          |
| `scale(valeur)`  | Agrandit ou rétrécit l'image. Utile pour zoomer ou créer des effets de pulsation.  |
| `pixelate(x, y)` | Rend l'image pixélisée pour un effet rétro ou stylisé.                             |
| `kaleid(n)`      | Crée un effet de kaléidoscope avec _n_ répétitions. Très utilisé en art génératif. |
| `invert()`       | Inverse les couleurs (négatif).                                                    |
| `posterize(n)`   | Réduit le nombre de couleurs, donnant un effet cartoon.                            |

Exemple :

```js
osc(20).kaleid(6).scale(0.5).invert().out();
```

---

# 9. Opérations entre sources

Hydra permet de mélanger plusieurs images ensemble. Ces opérations créent des visuels complexes en combinant les couches.

| Operation            | Description                                                                          |
| -------------------- | ------------------------------------------------------------------------------------ |
| `add(src)`           | Additionne deux images. Rend le résultat plus lumineux et plus dense.                |
| `mult(src)`          | Multiplie les couleurs pixel par pixel. Donne un effet d'ombre ou de texture.        |
| `blend(src, amount)` | Mélange deux sources selon un pourcentage (0 = rien, 1 = totalement la seconde).     |
| `diff(src)`          | Affiche les différences entre deux images, créant un effet de contours ou de glitch. |
| `mask(src)`          | Utilise une image comme masque pour révéler ou cacher certaines zones.               |

Exemple :

```js
osc(10).add(noise(3)).blend(shape(3), 0.3).out();
```

---

# 10. Feedback et buffers

Comme abordé dans la section 5, le feedback est super important. Ce dernier permet de réutiliser une image pour la déformer davantage. C'est grâce à cela qu'il est possible de créer des spirales, des tunnels ou des effets infinis caractéristiques de cette technologie.

---

# 11. Interaction en temps réel

Hydra permet de créer des visuels qui réagissent. Au temps, à votre souris ou même à l'audio. C'est ce genre d'interactions qui rendent vos créations encore plus vivantes !

| Interaction           | Description                                                                                                                                  |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `time`                | Variable qui augmente en continu. Permet d'animer automatiquement les effets (rotation, couleur, fréquence).                                 |
| `mouse.x` / `mouse.y` | Coordonnées de la souris. Elles permettent de contrôler la rotation, la taille ou la couleur du visuel en bougeant simplement la souris.     |
| Audio                 | Avec des extensions, Hydra peut réagir au son. Les basses, les aigus et d'autres facteurs sonores peuvent influencer les formes et couleurs. |

Exemple d'une rotation contrôlée par votre curseur :

```js
osc(10, 0.1, 1)
  .rotate(() => mouse.x * 0.005)
  .out();
```

Voici un autre exemple grâce à l'interaction de temps :

```js
shape(67)
  .scale(() => 0.5 + 0.2 * Math.sin(time))
  .out();
```

*testez les ;)*

---

# 12. Exemples de compositions Hydra (code expliqué)

## Exemple simple

```js
osc(8, 0.1, 1).out();
```

- `osc(8, 0.1, 1)` crée un oscillateur -> une série de lignes ondulées
- `8` -> c'est la fréquence, le nombre de lignes visibles
- `0.1` -> c'est la vitesse du mouvement de vague
- `1` -> c'est l'intensité
- `.out()` -> c'est l'affichage du résultat

## Exemple avancé

```js
osc(20, 0.05).kaleid(6).color(1, 0.3, 0.8).out();
```

- `osc(20, 0.05)` -> Fréquence élevée = motifs serrés et l'oscillateur bouge doucement (vitesse de 0.05)
- `kaleid(6)` -> Effet kaléidoscope de 6 miroirs
- `color(1, 0.3, 0.8)` -> RGB, rose

## Exemple utilisant feedback

```js
osc(10, 0.1, 1).out(o0);

src(o0).rotate(0.03).scale(1.01).out(o0);
```

- `osc(10, 0.1, 1)` -> l'oscillateur est envoyé dans le buffer o0
- `src(o0)` -> renvoie l'image précédente
- `rotate(0.03)` -> fait tourner légèrement l'image
- `scale(1.01)` -> agrandit de 1 % à chaque frame  
  _En bref, on prend l'image précédente, on la modifie et on écrit par-dessus_

## Exemple intéractif

```js
shape(4)
  .scale(() => 0.5 + mouse.y * 0.002)
  .rotate(() => mouse.x * 0.01)
  .color(0.2, 1, 0.6)
  .out();
```

- `shape(4)` -> 4 côtés donc un carré
- `scale(() => 0.5 + mouse.y * 0.002)` -> la souris descend vers le bas = la forme grandit
- `rotate(() => mouse.x * 0.01)` -> en bougeant la souris sur l'axe de x, la forme tourne

---

# 13. Cas d'usage de Hydra dans l'industrie

Hydra donne des résultats tellement impressionnants d'un point de vue artistique, voici quelques vidéos intéressantes le démontrant !  
Ce n'est pas seulement Hydra bien entendu, mais les événements de live coding se déroulent presque tous de la même manière. En voici quelques exemples. Ils vous aideront à mieux comprendre cette _hype_ du live coding et sa communauté !

<div style="text-align:center;">
  <iframe width="560" height="315" 
    src="https://www.youtube.com/embed/RkInFdN_rGI"
    title="Live Coding Performance"
    frameborder="0"
    allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

<div style="text-align:center;">
  <iframe width="560" height="315" 
    src="https://www.youtube.com/embed/oR5r3mk58BI"
    title="Hydra Live Coding Performance"
    frameborder="0"
    allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

### Ou encore voici des vidéos de musique créées avec Hydra :

<div style="text-align:center;">
  <iframe width="560" height="315" 
    src="https://www.youtube.com/embed/Uzf6uhTrc_8"
    title="Hydra music video 1"
    frameborder="0"
    allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

### Et pour finir, mon préféré :

<div style="text-align:center;">
  <iframe width="560" height="315" 
    src="https://www.youtube.com/embed/sTPQ0-dWhlg"
    title="Hydra music video 2"
    frameborder="0"
    allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

---

Je vous invite à cliquer [ICI](https://hydra.ojack.xyz/docs/docs/community/) si vous désirez explorer davantage les projets de la communauté

---

# 14. Forces et limites de Hydra

Hydra est un outil puissant pour créer des visuels en temps réel, mais comme toute technologie, il présente à la fois des avantages et des contraintes. Cette section vise à comprendre ce qui rend Hydra efficace, mais aussi dans quelles situations il atteint ses limites.

---

## 14.1 Forces techniques

### **1. Retour visuel instantané**

Hydra affiche le rendu visuel **dès qu'une ligne de code est modifiée**, sans compilation ni rechargement.  
Cela permet une démarche d'expérimentation rapide et intuitive.

### **2. Exécution GPU via WebGL**

Même si l'utilisateur écrit du JavaScript très simple, Hydra transforme les opérations en **shaders exécutés sur la carte graphique**.  
Avantages :

- animations très fluides,
- très bonne performance,
- rendu en temps réel, même avec des effets complexes.

### **3. Architecture modulaire et fonctionnelle**

Le chaînage d'opérations (`osc().color().rotate().out()`) rend Hydra :

- lisible,
- logique,
- flexible,
- proche de l'approche des synthétiseurs vidéo analogiques.

Chaque transformation s'applique sur la précédente, ce qui facilite la création artistique en direct.

### **4. Accessibilité et simplicité**

Hydra fonctionne **directement dans le navigateur**, sans installation ni configuration.  
Cela en fait un outil idéal pour :

- un enseignement,
- les ateliers débutants ;
- les performances improvisées ;
- les environnements où l'installation de logiciels est limitée.

---

## 14.2 Forces artistiques

### **1. Approche expérimentale**

Hydra encourage fortement l'exploration.  
Modifier un paramètre peut produire un effet inattendu, ce qui pousse à la créativité et à la découverte.

### **2. Style visuel unique**

Hydra permet de produire facilement des visuels :

- psychédéliques ;
- organiques ;
- géométriques ;
- abstraits.

Les modulations, feedbacks et oscillations créent une esthétique visuelle distincte.

### **3. Parfait pour le live-coding**

Grâce à la mise à jour instantanée, l'artiste peut "jouer" avec l'image comme avec un instrument.  
Hydra est très utilisé dans le mouvement **algorave** et dans les performances multimédias.

---

## 14.3 Limites techniques (navigateur)

### **1. Dépendance à WebGL**

Hydra repose entièrement sur [WebGL](#webgl), ce qui implique :

- limitations au niveau des textures ;
- restrictions selon la carte graphique ;
- compatibilité variable selon les navigateurs.

### **2. Performances variables selon l'appareil**

- Sur un PC récent → rendu fluide.
- Sur un vieux portable → baisse importante de FPS.
- Sur téléphone → parfois incompatible ou instable.

### **3. Pas idéal pour des pipelines professionnels**

Hydra n'est pas un outil de production comme :

- **TouchDesigner**
- **Unreal Engine**
- **After Effects**

Dès que le projet devient complexe ou doit être exporté, Hydra atteint rapidement ses limites.

---

## 14.4 Complexité des shaders : abstraite mais non contrôlée

Hydra masque toute la complexité de **GLSL**, ce qui est à la fois :

- un énorme avantage pour les débutants,
- une limite pour les utilisateurs avancés.

### **Avantage : simplicité**

L'utilisateur n'a pas besoin de comprendre :

- matrices,
- buffers mémoire,
- compilation GLSL,
- optimisation GPU.

### **Inconvénient : manque de contrôle avancé**

Hydra ne permet pas :

- d'écrire ses propres shaders personnalisés ;
- d'optimiser manuellement la pipeline graphique ;
- d'accéder à toutes les capacités de WebGL.

L'outil reste donc volontairement abstrait, ce qui le rend simple mais moins flexible que d'autres environnements professionnels.

---

## **Résumé rapide**

**Forces :**

- Très rapide à expérimenter
- Fonctionne sur GPU
- Accessible à tous
- Excellent pour le live-coding
- Esthétique visuelle forte

**Limites :**

- Dépend du navigateur
- Performances variables
- Pas idéal pour des projets complexes
- Pas de contrôle shader avancé

---

# 15. Conclusion

En conclusion, Hydra vous permet de générer des visuels impressionnants et originaux dans votre navigateur. Grâce à son architecture modulaire et sa syntaxe très simple, cette technologie est extrêmement accessible. En quelques lignes simples il est possible de créer des animations complexes et interactives !

Hydra est particulièrement pertinent dans le monde de l'art génératif et la montée de cette tendance dans les performances audiovisuelles ainsi que la création numérique pour tous. Sans installation, Hydra est accessible autant pour les enseignants, les ateliers stimulants ou pour des artistes qui souhaitent expérimenter sans se casser la tête.

Au final, Hydra n'est pas seulement un outil de programmation, c'est une manière d'explorer votre créativité à travers le code. C'est aussi comprendre les différentes combinaisons et les transformations afin de produire des compositions impressionnantes. Grâce à Hydra, de Olivia Jack, le code devient un instrument et chaque ligne ouvre la porte à une expérience visuelle complètement unique.

---

# 16. Références

- [Site officiel d'Hydra](https://hydra.ojack.xyz)
- [Presentation d'Olivia Jack](https://www.youtube.com/watch?v=lxLqcl-8GZE)
- [GitHub du projet](https://github.com/hydra-synth/hydra)
- [Documentation officielle](https://hydra.ojack.xyz/docs/docs/learning/getting-started/)
- [Introduction au live Coding Musique et visuels](https://www.youtube.com/watch?v=-QY2x6aZzqc)

---

# 17. Quelques définitions pratiques

## webgl

WebGL est une API graphique pour le navigateur qui permet de dessiner en 2D/3D en utilisant la carte graphique (GPU). Hydra utilise WebGL pour exécuter ses shaders et afficher les visuels en temps réel.

## snippets

Un "snippet" est un petit bout de code réutilisable. Dans Hydra, les snippets sont des exemples de code pré-écrits que l'on peut charger pour comprendre comment certaines fonctions fonctionnent.

## live-coding

Le live-coding est une pratique où l'on écrit et modifie du code en temps réel pendant une performance, souvent accompagnée de musique et de visuels générés en direct.
