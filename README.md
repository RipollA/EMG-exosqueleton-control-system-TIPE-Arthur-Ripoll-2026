# TIPE 2026 — L’exosquelette : de l’homme à la machine, de la machine à l’homme

**Étude de la chaîne fonctionnelle d’un système d’assistance au mouvement**

Projet réalisé individuellement en **CPGE PT** dans le cadre de la **session TIPE 2026**.

## Présentation

Ce TIPE porte sur la conception et l’étude de la chaîne fonctionnelle d’un **exosquelette d’assistance au mouvement**, avec un intérêt particulier pour l’interaction entre le corps humain et le système artificiel.

L’objectif général est de partir d’un signal physiologique très faible — le **signal électromyographique de surface (EMG)** — afin de :

- mesurer l’intention de mouvement de l’utilisateur ;
- préserver l’information utile malgré le bruit et les perturbations ;
- extraire une grandeur exploitable ;
- commander un actionneur ;
- fournir une assistance mécanique adaptée au mouvement.

Le système est ainsi étudié comme une **boucle corps–système fermée** : l’utilisateur agit sur la machine par son activité musculaire, puis la machine agit en retour sur l’utilisateur en fournissant une assistance mécanique.

---

## Problématique

> **Dans un environnement biomécanique où les signaux EMG sont aussi bruités qu’imprévisibles, comment assurer la stabilité d’une boucle de commande corps-système tout en garantissant un temps de réponse minimal grâce au lissage par moindres carrés polynomiaux ?**

Cette problématique place au centre du projet le compromis entre :

- qualité du filtrage ;
- conservation de la dynamique du signal ;
- rejet des perturbations ;
- stabilité de la commande ;
- temps de réponse du système.

---

## Architecture étudiée

Le projet est structuré autour de deux grandes parties :

### Chaîne d’information

Elle assure :

1. l’acquisition du signal EMG ;
2. le conditionnement analogique ;
3. le filtrage ;
4. le redressement ;
5. l’adaptation du niveau du signal ;
6. l’extraction d’une grandeur représentative de l’intention motrice.

### Chaîne d’énergie

Elle assure :

1. la conversion de la consigne en commande moteur ;
2. l’électronique de puissance ;
3. l’actionnement ;
4. l’asservissement du moteur ;
5. la production de l’assistance mécanique.

L’ensemble constitue une chaîne complète allant du **signal musculaire mesuré** jusqu’à la **réponse mécanique de l’actionneur**.

---

## 1. Étude du signal EMG

Le signal EMG de surface présente plusieurs difficultés :

- amplitude très faible, de l’ordre du millivolt ;
- présence d’artefacts de mouvement ;
- bruit thermique ;
- interférences électromagnétiques ;
- perturbations liées à l’interface peau-électrode ;
- bruit secteur à 50 Hz.

Ces contraintes imposent un traitement du signal suffisamment robuste pour améliorer le rapport signal/bruit sans introduire un retard incompatible avec une commande d’exosquelette.

Une attention particulière est portée au **CMRR (Common Mode Rejection Ratio)** afin de rejeter les perturbations communes aux électrodes avant amplification.

---

## 2. Chaîne de conditionnement analogique

La chaîne d’information est conçue pour transformer un signal EMG faible et bruité en une grandeur exploitable.

Elle comprend notamment :

- une interface de mesure ;
- un étage tampon ;
- un filtre actif ;
- un redresseur de précision ;
- un filtre passe-bas ;
- un amplificateur à gain réglable.

Chaque bloc répond à une contrainte physique particulière du signal.

L’objectif n’est pas de créer de l’information, mais de **préserver l’information utile contenue dans le signal musculaire tout en éliminant autant que possible les perturbations**.

---

## 3. Modélisation et fonctions de transfert

Les différents blocs de la chaîne d’information ont été modélisés à l’aide de leurs **fonctions de transfert**.

L’étude comprend notamment :

- la modélisation du filtre ;
- l’étude du gain ;
- l’étude de la phase ;
- la correction d’une résonance ;
- l’analyse de la chaîne complète à l’aide de diagrammes de Bode.

Cette démarche permet de relier directement :

**composants électroniques → modèle mathématique → comportement fréquentiel → performances du système.**

---

## 4. Filtrage et préservation de l’information

Une partie importante du travail porte sur le compromis entre filtrage et temps de réponse.

Un filtrage trop faible laisse subsister les perturbations, tandis qu’un filtrage trop agressif risque de :

- lisser excessivement le signal ;
- déformer les pics ;
- introduire un retard ;
- réduire la réactivité de l’assistance.

Le travail bibliographique et expérimental m’a conduit à étudier des approches de **lissage polynomial local**, en particulier le filtre de **Savitzky–Golay**.

Cette méthode repose sur un ajustement polynomial local par la méthode des moindres carrés et permet de lisser un signal tout en préservant certaines caractéristiques locales importantes.

Deux pistes d’amélioration ont notamment été comparées :

- ajustement polynomial direct ;
- filtrage de Savitzky–Golay.

---

## 5. Interface homme-machine

Une interface expérimentale a été réalisée afin d’observer directement la relation entre :

- l’état musculaire ;
- le signal EMG mesuré ;
- le signal conditionné.

Les essais permettent notamment d’identifier :

- le repos musculaire ;
- la contraction ;
- les saturations ;
- le retour au repos.

Cette étape permet de vérifier expérimentalement que la chaîne d’information traduit correctement l’évolution de l’activité musculaire.

---

## 6. Étude et identification du moteur

La chaîne d’énergie repose sur un **moteur à courant continu**.

Afin de construire un modèle exploitable pour l’asservissement, plusieurs paramètres du moteur ont été étudiés ou déterminés :

- résistance d’induit \(R\) ;
- inductance \(L\) ;
- constante de couple \(K_c\) ;
- constante de force contre-électromotrice \(K_e\) ;
- coefficient de frottement visqueux \(f\) ;
- moment d’inertie équivalent \(J_{eq}\).

Des procédures expérimentales ont notamment été utilisées pour déterminer :

- l’inductance du moteur ;
- le moment d’inertie équivalent.

Cette étape relie ainsi directement les mesures expérimentales au modèle dynamique du moteur.

---

## 7. Asservissement du moteur

Le moteur est intégré dans une boucle d’asservissement en vitesse.

Un **correcteur PI** est utilisé afin de réduire l’erreur statique tout en conservant une réponse dynamique adaptée.

Les valeurs retenues dans l’étude sont :

- \(K_p = 0,80\)
- \(K_i = 0,70\ \text{s}^{-1}\)
- \(T_i \approx 1,14\ \text{s}\)

Le réglage du correcteur est effectué à partir de la comparaison entre :

- la réponse non corrigée ;
- la réponse corrigée ;
- la consigne.

L’objectif est d’annuler l’erreur statique tout en conservant un dépassement modéré.

---

## 8. Validation du système complet

La dernière partie du projet consiste à réunir :

**chaîne d’information × chaîne d’énergie → système asservi exploitable**

La validation s’appuie notamment sur :

- l’étude fréquentielle du système complet ;
- les diagrammes de Bode ;
- la réponse temporelle à un signal EMG ;
- des simulations réalisées sous **MATLAB** ;
- la vérification de l’exigence de CMRR.

La réponse temporelle permet d’observer comment l’enveloppe extraite du signal musculaire se transforme progressivement en consigne puis en réponse mécanique du système.

---

## 9. Vérification du CMRR

Le rejet du mode commun est un point important de la chaîne d’acquisition EMG.

L’objectif retenu est :

**CMRR ≥ 90 dB**

L’étude montre que le système satisfait cette exigence avec une valeur mesurée supérieure à ce seuil.

Cette vérification permet de valider la capacité de l’étage d’entrée à rejeter efficacement les perturbations communes aux électrodes.

---

## Résultats et apports du projet

Ce TIPE m’a permis de travailler sur une chaîne complète associant :

- biomécanique ;
- électromyographie ;
- électronique analogique ;
- traitement du signal ;
- modélisation mathématique ;
- fonctions de transfert ;
- filtrage actif ;
- diagrammes de Bode ;
- identification expérimentale ;
- électronique de puissance ;
- moteur à courant continu ;
- asservissement ;
- correcteur PI ;
- simulation MATLAB ;
- validation expérimentale.

Par rapport à mon TIPE de 2025, ce projet met davantage l’accent sur :

- la formalisation du besoin ;
- la définition d’exigences mesurables ;
- la modélisation mathématique ;
- le lien entre théorie et expérimentation ;
- l’identification de paramètres physiques ;
- la validation quantitative du système.

---

## MCOT — Mise en cohérence des objectifs du TIPE

La MCOT présente le contexte scientifique du projet, sa problématique, ses objectifs et la bibliographie commentée associée.

Les trois domaines principaux retenus sont :

- **Sciences industrielles — traitement du signal** ;
- **Sciences industrielles — génie électrique** ;
- **Mathématiques — mathématiques appliquées**.

L’étude bibliographique porte notamment sur :

- l’électromyographie de surface ;
- les sources de bruit du signal EMG ;
- les filtres analogiques et numériques ;
- le compromis entre filtrage et latence ;
- le filtre de Savitzky–Golay ;
- la commande d’exosquelettes à partir de signaux EMG ;
- la biomécanique et l’assistance physique.

➡️ **[Consulter la MCOT et la bibliographie commentée](MCOT_Arthur_Ripoll_2026.pdf)**

---

## Références scientifiques principales

La bibliographie s’appuie notamment sur les travaux de :

- C. J. De Luca sur l’électromyographie de surface ;
- De Luca, Gilmore, Kuznetsov et Roy sur le filtrage du signal EMG ;
- R. W. Schafer sur le filtre de Savitzky–Golay ;
- Benjamin Treussart, Franck Geffard, Nicolas Vignais et Frédéric Marin sur la commande d’un exosquelette de membre supérieur par EMG ;
- P. Horowitz et W. Hill pour l’électronique analogique ;
- des travaux consacrés à l’interface électrode-peau et aux dispositifs d’assistance physique.

La bibliographie complète et commentée est disponible dans la MCOT.

---

## Technologies et outils utilisés

### Électronique et traitement du signal

- électromyographie de surface (EMG)
- électronique analogique
- amplificateurs opérationnels
- filtrage actif
- redressement de précision
- amplification
- étude du CMRR
- traitement du signal

### Modélisation et automatique

- fonctions de transfert
- transformée de Laplace
- diagrammes de Bode
- modélisation d’un moteur à courant continu
- identification expérimentale
- asservissement
- correcteur PI

### Programmation et simulation

- **Python**
- **MATLAB**
- acquisition et traitement de données
- simulations temporelles et fréquentielles
- ajustement polynomial
- filtre de Savitzky–Golay

---

## Documentation

### Présentation complète du TIPE

➡️ **[Consulter le diaporama complet](TIPE_Arthur_Ripoll_2026.pdf)**

Le document présente notamment :

- le cycle d’assistance homme-machine ;
- la chaîne fonctionnelle de l’exosquelette ;
- les caractéristiques du signal EMG ;
- le diagramme des exigences ;
- l’architecture électronique retenue ;
- les fonctions de transfert ;
- la chaîne de conditionnement ;
- les essais expérimentaux ;
- l’étude fréquentielle ;
- l’identification du moteur ;
- l’asservissement PI ;
- la validation du système complet ;
- les pistes d’amélioration.

### MCOT et bibliographie

➡️ **[Consulter la MCOT et la bibliographie commentée](MCOT_Arthur_Ripoll_2026.pdf)**

---

## Auteur

**Arthur Ripoll**  
CPGE PT — Session TIPE 2026

---

## À propos de cette présentation

**Note :** Ce TIPE, réalisé en 2026, témoigne d’une nette progression dans ma manière de concevoir, modéliser, expérimenter et présenter un projet par rapport à mon travail de 2025. Il ne reflète toutefois pas encore pleinement le niveau de rigueur, de structuration et d’exigence que j’applique aujourd’hui à mes présentations et à ma documentation technique.

---

> **Remarque :** ce projet a été réalisé dans un cadre académique et constitue une étude expérimentale d’un système d’assistance au mouvement. Il ne s’agit pas d’un dispositif médical certifié.
