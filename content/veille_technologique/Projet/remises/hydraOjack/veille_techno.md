+++
title = "Veille technologique"
weight = 1
+++

# Contexte et objectif
L'objectif est de s'assurer que le contenu de cours sur Hydra reste actuel et pertinent, en s'appuyant sur des outils modernes (LLM, documentation officielle, articles récents et réseaux sociaux).

# Mon premier prompt a une LLM et sa réponse 
**Modèle : Perplexity**
**Quand ? : Décembre 2025**

## Mon prompt
Explique-moi Hydra (Ojack Hydra) comme si j'étais un étudiant en 5e session en informatique qui connaît JS mais pas le live-coding visuel. Donne les concepts importants, l'architecture, le pipeline graphique et les cas d'usage.

## Réponse du LLM
Hydra est un outil open-source de live-coding visuel par Olivia Jack, fonctionnant dans le navigateur via JavaScript et WebGL pour des visuels temps réel. Il utilise une syntaxe en chaînes de fonctions inspirée des synthétiseurs modulaires, sans interface graphique.​

Concepts clés
Sources comme osc() ou noise() génèrent des textures de base ; transformations (rotate(), color()) et modulations (modulate()) les modifient. Outputs multiples (o0-o3) servent de buffers pour mixer, avec render() pour affichage. Chaînage modulaire : osc().rotate().out(o0).​

Architecture
Modulaire et stateless : code JS évalué en boucle, compilé en shaders GPU. Intègre webcam/vidéo via initCam(), support peer-to-peer sans serveur.​

Pipeline
Source → texture ; 2) Transformations → géométrie/couleur ; 3) Blending/modulation → combinaison ; 4) Output → buffer puis écran, à 60 FPS avec feedback.​

Usages
Visuels live/VJing, effets interactifs (glitch, fractals), collaboration en ligne, intégration JS (P5.js), apprentissage synthèse vidéo.​

---

# Mon analyse de ce prompt et de la réponse 
La réponse est globalement complète pour un premier contact : elle relie bien Hydra au contexte du live-coding, explique les briques de base (sources + opérations) et donne une vue simplifiée du pipeline graphique (sources → transformations → buffers → sortie).
Comparé à un moteur de recherche, le LLM offre une synthèse pédagogique directement adaptée à un profil d'étudiant en info, là où un moteur renverrait surtout vers la doc ou des tutos isolés qu'il faudrait lire et organiser soi-même.

La réponse permet a l'etudiant d'organiser les notions pour un cours. Par contre, la réponse ne remplace pas la lecture de la documentation officielle pour les détails fins (paramètres exacts, comportements particuliers), ni l'exploration pratique dans l'interface Hydra.

---

# Article recent sur Hydra (Juin 2025)
- "Getting started with hydra" <https://hydra.ojack.xyz/docs/docs/learning/getting-started/>
- Cet article explique comment bien débuter avec Hydra
- Il est pertinent car c'est une porte d'entrée pour cette technologie, sa récente mise à jour est également la preuve que ces articles sont périodiquement entretenus.

*Il est extrêmement difficile de trouver des articles récents sur cette technologie car Hydra est un outil niché.*

--- 

# Mes sources de référence
- **Site et documentation officielle de Hydra** :  
  <https://hydra.ojack.xyz>  
  Fournit les guides d'apprentissage, la liste des fonctions et des exemples interactifs.
- **Dépôt GitHub officiel hydra-synth** :  
  <https://github.com/hydra-synth/hydra>  
  Sert à vérifier l'activité du projet (commits récents, bugs). Confirme que l'outil est encore activement maintenu.
- **Article / interview de fond sur Hydra (Février 2019)** :  
  Article de Create Digital Music présentant Hydra et Olivia Jack, décrivant la philosophie du projet, son lien avec les synthés vidéo analogiques et la culture du live-coding :  
  <https://cdm.link/hydra-olivia-jack/>
- **Documentaire sur Algorave** :
  <https://www.youtube.com/watch?v=Crz6R4p_owI>

---

# Cours sur le sujet
- **Tutoriel relativement récent** : <https://www.creativeapplications.net/tutorial/hydra-tutorial-series-for-everyone/>
- **Workshop Live Coding Audioreactive Visuals with Hydra (InterAccess, 2024)** : <https://www.interaccess.org/workshop/live-coding-audioreactive-visuals-with-hydra>
- **Workshop Live Coding in Hydra (Creative Coding Utrecht, Iterations 2024)** :  <https://creativecodingutrecht.nl/en/archive/iterations-2024-workshop-livecoding-in-hydra>

--- 

# Veille sur les réseaux et médias
- Compte Instagram Hydra Video Synth : <https://www.instagram.com/hydravideosynth/>
- Youtube propose beaucoup de tutoriels, du live-coding etc.

---

# Stratégie de mise à jour de mes notes de cours
La stratégie de mise à jour repose sur une veille régulière :

- **Fréquence** :  
    - Vérification des nouveautés majeures (changelog GitHub, nouvelles docs).
    - Survol des événements sociaux sur les réseaux sociaux.
- **Mise a jour**:
    - Sortie d'une nouvelle version importante de Hydra ou changement d'API documenté.
    - Sortie de nouveaux tutoriels qualitatifs sur la technologie Hydra.
    - Évolution des pratiques de la communauté (nouvelles tendances).
- **Notes de cours et exemples**:
    - Intégration de 1–2 nouveaux exemples "inspirants" par année des étudiants.
