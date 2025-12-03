+++
title = "Notes de cours"
weight = 2
+++

Hydra Ojack est une plateforme pour developper des visuels impressionants en temps reel dans votre navigateur. C'est gratuit, open source et pour tous.

- Hydra est ecrit en **JavaScript** et compile en **WebGL**.
- Sa syntaxe est inspiree de la **synthese video modulaire et analogique**.
- Tout se passe dans le **navigateur** !

> "Hydra is a system for creating live visuals, inspired by analog video synths and modular synthesis."
> **Olivia Jack (Ojack)**

---

## Pourquoi Hydra ?

Hydra est largement utilise chez les **live-coders**, les membres de communautes de **musique electronique** et d'**art numerique** :

- Il permet de creer des visuels rapidement pour :
    - des performances live (des concerts ou sets de DJ);
    - de l'art interactif ;
    - des experimentations d'art generatif.
- Il offre une **boucle de feedback courte** : on modifie le code -> l'image change.
- C'est ideal pour **enseigner** :
    - les bases de la synthese visuelle ;
    - la programmation GPU sans entrer dans la complexite des shaders.

Hydra se situe donc a la frontiere entre :
- un langage artistique,
- et un framework technique pour exploiter la puissance de la carte graphique

---

## Son architecture generale

Hydra repose sur quelques concepts centraux :

### Sources

Une **source** c'est un flux video de base :

- `osc()` : oscillateurs (lignes ondulees periodiques)
- `noise()` : bruit (textures organiques, nuages ...)
- `shape()` : formes geometriques (cercles, triangles, polygones)
- `gradient()` : degrades de couleur
- `src(o0)` : reutilisation d'un buffer de sortie (feedback)
- camera / video

Chaque source genere une image de base qui sera ensuite transformee.

---

### Chainage fonctionnel

Hydra utilise un **pipeline fonctionnel** : chaque appel renvoie un objet sur lequel on peut enchainer des transformations.

Exemple :

```js
osc(10, 0.1, 0.8)
    .rotate(0.2)
    .color(1, 0.5, 0.3)
    .out()
```

dans cet exemple :
- 'osc()' genere la source de base.
- 'rotate()' applique une rotation sur la source.
- 'color()' modifie la couleur du signal visuel.
- 'out()' envoie le resultat final vers la sortie d'affichage.

Ce modele de chainage permet de lire le code visuel comme une suite logique d'operations, un peu comme des modules branches les uns a la suite des autres dans un synthetiseur video. 
Chaque ligne represente une transformation independante qui s'sapplique sur le resultat de la precedente, ce qui rend Hydra particulierement intuitif pour le live-coding.

