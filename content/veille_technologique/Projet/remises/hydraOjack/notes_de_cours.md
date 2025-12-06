+++
title = "Notes de cours"
weight = 2
+++

# 1. Introduction
Hydra Ojack est une plateforme pour developper des visuels impressionants en temps reel dans votre navigateur. C'est gratuit, open source et pour tous.

- Hydra est ecrit en **JavaScript** et compile en **WebGL**.
- Sa syntaxe est inspiree de la **synthese video modulaire et analogique**.
- Tout se passe dans le **navigateur** !

Hydra s'inscrit dans le domaine du "live-coding visuel", une pratique ou l'artiste genere des images en modifiant du code en temps reel. Cette approche permet une experimentation rapide et une exploration creative directe. Hydra se distingue aussi par son accessibilite car tout fonctionne directement dans le navigateur, sans installation ou configuration complexe.

Les notes de cours qui suivent ont pour objectif de presenter les principes fondamentaux de Hydra, ses sources visuelles, ses transformations, son pipeline graphique ainsi que quelques exemples pratiques pour comprendre comment creer des compositions generatives.
*Plusieurs exercices sont disponnibles dans la section [atelier](/veille_technologique/Projet/remises/hydraOjack/atelier/)*


> "Hydra is a system for creating live visuals, inspired by analog video synths and modular synthesis."
> **Olivia Jack (Ojack)**

---

# 2. A propos de la creatrice : Olivia Jack

- Parcours
- Philosophie artistique
- Autres projets et implications

Originaire de San Francisco, **Olivia Jack** (creatrice de Hydra), est une programmeuse et une artiste qui travaille courament avec des applications "open-source", de la cartegraphie, du "live coding" ou encore avec des interfaces experimentales depuis pres de 15 ans. Elle est fascinee par le "live coding" et ce dernier lui permet d'entrer dans un dialogue continue et stimulant avec la machine. Elle apprecie particulierement, comme le demontre Hydra, l'algorithmie ainsi que le chaos. Elle reside et travaille en ce moment en Colombie.

Elle aime changer les valeurs ainsi que les ordres des choses simplement pour voir leur resultat, elle n'emprunte pas un chemin lineaire et prefere tester plusieures differentes choses avec son code.
Elle n'a pas uniquement cree Hydra, 

En brerf, Olivia est une personne qui adore experimente !

---
# 3. Contexte et origine de Hydra
- Inspirations : synthetiseurs video analogiques
- Approche modulaire
- Vision “web-native”

---

# 4. Pourquoi Hydra ?

- Rapidite d’experimentation
- Retour visuel instantane
- Accessibilite (navigateur seulement)
- Communaute de live-coding

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

# 5. Son architecture generale

- Sources (osc, noise, shape, gradient, camera, src)
- Transformations
- Buffers (o0, o1, o2, o3)
- Pipeline fonctionnel (chaining)

Hydra repose sur quelques concepts centraux :

## Sources

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
Chaque ligne represente une transformation independante qui s'applique sur le resultat de la precedente, ce qui rend Hydra particulierement intuitif pour le live-coding.

---

# 6. Le pipeline graphique : de JavaScript a WebGL
- Comment Hydra transforme du JS en shaders GPU
- Fonctionnement de WebGL en arriere-plan
- Rendu temps reel

---

# 7. Les sources visuelles
- osc() : oscillateurs
- noise() : bruit
- shape() : geometrie
- gradient() : degrades
- camera() : video en direct
- src() : reutilisation (feedback)

---

# 8. Transformations visuelles
- rotate(), scale(), pixelate(), kaleid(), invert(), posterize()
- Explication des parametres
- Exemple d’utilisation concrete

---

# 9. Operations entre sources
- add(), mult(), blend(), diff(), mask()
- Combinaison de couches
- Melanges complexes pour creer des visuels originaux

---

# 10. Feedback et buffers (element central)
- Qu’est-ce qu’un buffer ?
- Comment fonctionne src(o0) ?
- Creation de boucles retroactives
- Exemples visuels impressionnants

---

# 11. Interaction en temps reel
- time → animation automatique
- mouse.x / mouse.y → interaction directe
- audio (optionnel) → reactivite sonore
- Exemples pratiques

---

# 12. Exemples de compositions Hydra (code explique)
- Exemple simple
- Exemple avance
- Exemple utilisant feedback
- Exemple interactif

---

# 13. Cas d’usage de Hydra dans l’industrie
- Performances live (concerts, DJ sets)
- Art generatif
- Installations interactives
- Ateliers d’enseignement visuel

---

# 14. Forces et limites de Hydra
Hydra est un outil puissant pour creer des visuels en temps reel, mais comme toute technologie, il presente à la fois des avantages et des contraintes. Cette section vise à comprendre ce qui rend Hydra efficace, mais aussi dans quelles situations il atteint ses limites.

---

## 14.1 Forces techniques

### **1. Retour visuel instantane**
Hydra affiche le rendu visuel **des qu’une ligne de code est modifiee**, sans compilation ni rechargement.  
Cela permet une demarche d’experimentation rapide et intuitive.

### **2. Execution GPU via WebGL**
Même si l’utilisateur ecrit du JavaScript tres simple, Hydra transforme les operations en **shaders executes sur la carte graphique**.  
Avantages :
- animations tres fluides,
- tres bonne performance,
- rendu en temps reel, même avec des effets complexes.

### **3. Architecture modulaire et fonctionnelle**
Le chainage d’operations (`osc().color().rotate().out()`) rend Hydra :
- lisible,
- logique,
- flexible,
- proche de l’approche des synthetiseurs video analogiques.

Chaque transformation s'applique sur la precedente, ce qui facilite la creation artistique en direct.

### **4. Accessibilite et simplicite**
Hydra fonctionne **directement dans le navigateur**, sans installation ni configuration.  
Cela en fait un outil ideal pour :
- un enseignement,
- les ateliers debutants ;
- les performances improvisees ;
- les environnements ou l’installation de logiciels est limitee.

---

## 14.2 Forces artistiques

### **1. Approche experimentale**
Hydra encourage fortement l'exploration.  
Modifier un parametre peut produire un effet inattendu, ce qui pousse à la creativite et à la decouverte.

### **2. Style visuel unique**
Hydra permet de produire facilement des visuels :
- psychedeliques ;
- organiques ;
- geometriques ;
- abstraits.

Les modulations, feedbacks et oscillations creent une esthetique visuelle distincte.

### **3. Parfait pour le live-coding**
Grâce à la mise à jour instantanee, l’artiste peut "jouer" avec l’image comme avec un instrument.  
Hydra est tres utilise dans le mouvement **algorave** et dans les performances multimedias.

---

## 14.3 Limites techniques (navigateur)

### **1. Dependance à WebGL**
Hydra repose entierement sur WebGL, ce qui implique :
- limitations au niveau des textures ;
- restrictions selon la carte graphique ;
- compatibilite variable selon les navigateurs.

### **2. Performances variables selon l’appareil**
- Sur un PC recent → rendu fluide.  
- Sur un vieux portable → baisse importante de FPS.  
- Sur telephone → parfois incompatible ou instable.

### **3. Pas ideal pour des pipelines professionnels**
Hydra n’est pas un outil de production comme :
- **TouchDesigner**
- **Unreal Engine**
- **After Effects**

Des que le projet devient complexe ou doit être exporte, Hydra atteint rapidement ses limites.

---

## 14.4 Complexite des shaders : abstraite mais non contrôlee

Hydra masque toute la complexite de **GLSL**, ce qui est à la fois :
- un enorme avantage pour les debutants,
- une limite pour les utilisateurs avances.

### **Avantage : simplicite**
L’utilisateur n’a pas besoin de comprendre :
- matrices,
- buffers memoire,
- compilation GLSL,
- optimisation GPU.

### **Inconvenient : manque de contrôle avance**
Hydra ne permet pas :
- d’ecrire ses propres shaders personnalises ;
- d’optimiser manuellement la pipeline graphique ;
- d’acceder à toutes les capacites de WebGL.

L’outil reste donc volontairement abstrait, ce qui le rend simple mais moins flexible que d’autres environnements professionnels.

---

## **Resume rapide**

**Forces :**
- Tres rapide à experimenter  
- Fonctionne sur GPU  
- Accessible à tous  
- Excellent pour le live-coding  
- Esthetique visuelle forte  

**Limites :**
- Depend du navigateur  
- Performances variables  
- Pas ideal pour des projets complexes  
- Pas de contrôle shader avance  

---

# 15. Conclusion
- Synthese
- Pourquoi Hydra est pertinent dans le paysage actuel
- Ce que l'etudiant devrait retenir

---

# 16. References

- [Site officiel d'Hydra](https://hydra.ojack.xyz)
- [Presentation d'Olivia Jack](https://www.youtube.com/watch?v=lxLqcl-8GZE)
- [GitHub du projet](https://github.com/hydra-synth/hydra)
- [Documentation officielle](https://hydra.ojack.xyz/docs/docs/learning/getting-started/) 
