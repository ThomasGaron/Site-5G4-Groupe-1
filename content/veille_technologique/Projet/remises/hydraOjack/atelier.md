+++
title = "Atelier Hydra"
weight = 3
+++

# Objectif

Apprendre les bases de Hydra à l'aide de 5 exercices et questions.

**SI L'EXERCICE DEMANDE PLUSIEURS MANIPULATIONS, VEUILLEZ METTRE VOTRE ANCIEN CODE EN COMMENTAIRE, MAIS TOUJOURS LE CONSERVER.**

# Scénario

Vous êtes un DJ et artiste émergent. Vous désirez explorer le live-coding et ses merveilles pour potentiellement inclure cette technologie dans votre prochaine performance.

---

# Exercice 1 : Vos premiers pas

Votre objectif : tester les **sources** dans Hydra.

- Faites apparaître des ondulations.  
- Testez différentes formes géométriques.  
- Faites apparaître du bruit visuel.  

**Explorez :**
- Changez la fréquence d’oscillation.  
- Ajoutez des côtés à votre forme.  

### Question 1

C’est quoi une source dans Hydra ?

### Question 2

Quel paramètre modifie la fréquence de `osc()` ?

### Question 3

Quelles sont les différences visuelles entre `osc`, `shape` et `noise` ?

---

# Exercice 2 : Les transformations visuelles

Votre objectif : modifier une source avec `rotate`, `scale`, `color` ou encore `kaleid`.

1. Tapez :
```js
osc(10, 0.1)
    .rotate(0.2)
    .color(1, 0.5, 0.3)
    .out();
```
2. Ajoutez un effet de kaléidoscope:
```js
.kaleid(6)
```
3. Testez aussi quelque chose du genre:
```js
.scale(0.4)
.pixelate(40, 40)
```
**Explorez :**
- Changez les valeurs.  
- Testez différentes couleurs.  

### Question 1
Que fait rotate() sur l’image ?

### Question 2
Comment color() influence-t-il l’image ?

### Question 3
Quel effet est votre préféré ? Pourquoi ?

---

# Exercice 3: Combiner les sources
Votre objectif: apprendre à mixer des sources

1. 
```js
osc(10, 0.1)
    .blend(noise(3, 0.2), 0.3)
    .out();
```
2. remplacez `blend` par `add`
puis par `mult`
3. Ajoutez une forme
```js
osc(5)
    .add(shape(3, 0.3, 0.2)).out();
```
**Explorez :**
- Comparez visuellement `add` et `blend`.  
- Testez `diff`.  
- Essayez de mélanger deux formes.  

### Question 1
Quelle combinaison rend l’image la plus lumineuse ?

### Question 2
Quel opérateur produit l’effet le plus texturé ?

### Question 3
Quelle est votre combinaison préférée et pourquoi ?

---

# Exercice 4: Les feedback et buffers
Votre objectif: utiliser les buffers (`src(o0)`)

1. Tapez
```js
osc(10, 0.1)
    .out(o0);
```
2. Ajoutez des transformations visuelles
3. Patientez quelques secondes et admirez l'effet

**Explorez :**
- Remplacez les valeurs de vos transformations.  
- Ajoutez `colorama(0.1)` dans votre boucle.  

### Question 1
C’est quoi un buffer dans Hydra ?

### Question 2
Pourquoi le feedback crée-t-il un effet infini ?

### Question 3
Comment éviter un feedback trop violent ou explosif ?

---

# Exercice 5: Les interactions
Votre objectif: Controler un visuel avec votre souris ou le temps

1. Avec votre souris
```js
shape(4)
  .scale(() => 0.5 + mouse.y * 0.002)
  .rotate(() => mouse.x * 0.01)
  .color(0.2, 1, 0.6)
  .out();
```
2. Avec le temps
```js
shape(50)
  .scale(() => 0.5 + 0.2 * Math.sin(time))
  .out();
  ```
**Explorez :**
- Faites changer la couleur selon `mouse.x`.  
- Ajoutez un effet dynamique de `kaleid()`.  
- Combinez les deux effets.  

### Question 1
Que contrôle mouse.x dans votre visuel ?

### Question 2
Comment évolue la forme avec Math.sin(time) ?

### Question 3
Idée d’utilisation en performance live ?

---

# Exercice 6: Mini projet personnel au choix

Créez votre propre composition créative en utilisant:

- Une sourcem de votre choix
- Au moins **deux** transformations
- Au moins **une** combinaison
- Un élément de **feedback** ou d'**interaction**

# Corrigé (exemples possibles)

## Ex01
```js
osc(8, 0.1, 1).out();
shape(1).out();
noise(3).out();
```
## Ex02
````js
osc(10, 0.1)
    .rotate(0.2)
    .color(1, 0.5, 0.3)
    .kaleid(6)
    .out();
````
## Ex03
```js
osc(10, 0.1).add(noise(3, 0.2)).out();
osc(10, 0.1).mult(noise(3, 0.2)).out();
osc(5).add(shape(3, 0.3, 0.2)).out();
```
## Ex04

```js
osc(10, 0.1).out(o0);

src(o0)
    .rotate(0.03)
    .scale(1.01)
    .colorama(0.1)
    .out(o0);
```
## Ex05
```js
shape(4)
    .scale(() => 0.5 + mouse.y * 0.002)
    .rotate(() => mouse.x * 0.01)
    .color(0.2, 1, 0.6)
    .out();

shape(50)
    .scale(() => 0.5 + 0.2 * Math.sin(time))
    .out();
```
