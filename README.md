

# EA113 – Générateur de son

Projet réalisé dans le cadre du module **EA113 – Électronique analogique** en première année du département **Électronique** à l’**ENSEIRB-MATMECA**.

**Auteurs :**  
Mattéo BINET  
Alexandre BROMET  

**Professeur :**  
Jean-Michel VINASSA  


**Date :** Mai 2025  

---

## 1. Introduction

Ce projet s’inscrit dans la continuité du module EA113 et a pour objectif la **conception d’un générateur de son analogique**.  
Le but est de produire un **signal audio modulé en fréquence** à l’aide d’un **oscillateur commandé en tension (VCO)**, répondant à un cahier des charges défini.  
L’étude comprend la **conception théorique**, la **simulation sous Proteus**, et la **validation expérimentale** du montage.

Le système repose sur l’utilisation de plusieurs blocs fonctionnels :
- Un **générateur de modulation** à base de NE555.
- Un **circuit d’adaptation de signal**.
- Un **oscillateur commandé en tension (VCO)**, composé d’un comparateur à hystérésis, d’un multiplieur et d’un intégrateur.
- Un **étage d’adaptation de sortie**.

---

## 2. Cahier des charges

Le générateur doit produire un signal **triangulaire** dont la fréquence varie entre deux valeurs extrêmes, selon une modulation lente de type wobulation.

**Spécifications principales :**

- **Forme du signal :** triangulaire, périodique et symétrique  
- **Fréquences extrêmes :** F_min = 100 Hz, F_max = 1500 Hz (valeurs ajustables selon l’effet sonore)  
- **Type de modulation :** pseudo-linéaire ou binaire  
- **Fréquence de modulation :** de l’ordre du hertz  
- **Amplitude de sortie :** environ 0 à 100 mV  
- **Alimentation :** ±15 V  
- **Compatibilité :** entrée d’un amplificateur audio  
- **Stabilité et robustesse :** le montage doit être fiable et reproductible sur PCB  

---

## 3. Générateur de modulation NE555

Le **NE555** est utilisé pour produire une tension de modulation **triangulaire** variant entre 5 V et 10 V, avec une fréquence d’environ **4 kHz**.  
Les valeurs des composants utilisées sont :
- RA = 600 kΩ  
- RB = 200 kΩ  
- C2 = 360 nF  

Ce signal sert ensuite à piloter le VCO après une étape d’adaptation pour ajuster les niveaux de tension.

---

## 4. Adaptation du signal du NE555

L’étage d’adaptation abaisse et inverse la tension issue du NE555 pour obtenir une tension modulante (Vmod) comprise entre **−3.5 V** et **−8.5 V**.  
L’adaptation est réalisée à l’aide d’un **amplificateur opérationnel inverseur**, selon la relation :

Vmod = a × Vmod1 + b

avec :  
- a = −1  
- b = 2 V  

Un **amplificateur suiveur** est également placé en amont pour adapter l’impédance.

---

## 5. Voltage Controlled Oscillator (VCO)

Le **VCO** est le cœur du générateur.  
Il transforme la tension Vmod en une fréquence de sortie f proportionnelle à cette tension.  
Le montage comprend :
1. Un **comparateur à hystérésis** fixant les seuils de commutation.  
2. Un **multiplieur** modulant la pente de la rampe selon Vmod.  
3. Un **intégrateur** générant le signal triangulaire.

### 5.1 Comparateur à hystérésis

Le comparateur définit deux seuils ±VH et commute la sortie entre ±Vsat :

f = Vmod / (4 × R10 × C7 × VH)

Les résistances choisies sont : R8 = R9 = 100 kΩ ⇒ VH = Vsat.

### 5.2 Multiplieur

Le multiplieur réalise une multiplication entre la sortie du comparateur et la tension de modulation Vmod.  
Il utilise un amplificateur opérationnel et un transistor BS170 jouant le rôle d’interrupteur.  
Le gain varie entre −1 et +1 selon l’état du transistor.

### 5.3 Intégrateur

L’intégrateur convertit le signal du multiplieur en une tension triangulaire.  
La fréquence d’oscillation obtenue est :

f = (1 / (4 × R10 × C7)) × (R8 / (R8 + R9)) × Vsat × |Vmod|

Valeurs retenues :  
- R10 = 1.85 kΩ  
- C7 = 100 nF  

La fréquence varie alors de **350 Hz à 850 Hz**, conformément au cahier des charges.

---

## 6. Adaptation du signal de sortie

Le signal triangulaire issu du VCO présente une amplitude de plusieurs volts.  
Pour être compatible avec l’entrée de l’amplificateur audio, une **adaptation de niveau** est nécessaire.  
Un **diviseur de tension** formé par R11 = 47 kΩ et un potentiomètre RV1 = 2 kΩ permet de régler l’amplitude de sortie entre **0 et 150 mV**.

---

## 7. Simulation et validation

Les simulations effectuées sous **Proteus** ont permis de valider :
- Le bon fonctionnement du NE555 et du VCO.  
- La cohérence entre les valeurs de fréquence simulées et calculées.  
- La stabilité du signal triangulaire et la justesse de la modulation.  

**Résultats obtenus :**  
| Paramètre | Valeur simulée | Observation |
|------------|----------------|--------------|
| F_min | 350 Hz | Conforme |
| F_max | 850 Hz | Conforme |
| Forme du signal | Triangulaire | Conforme |
| Amplitude de sortie | ≈100 mV | Ajustable |

---

## 8. Réalisation et tests

La **réalisation du PCB** a été effectuée après validation de la simulation.  
Le montage a ensuite été testé sur table, avec alimentation symétrique ±15 V.

Les essais expérimentaux ont confirmé :
- La stabilité du signal généré.  
- La variation de fréquence conforme à la modulation.  
- L’amplitude de sortie adaptée à l’amplificateur audio.  

---

## 9. Conclusion

Ce projet a permis de concevoir et de valider un **générateur de son analogique modulé en fréquence**, reposant sur une architecture complète à base de NE555, TL074 et TL071.  
Il a permis de consolider les compétences acquises en **électronique analogique**, notamment la conception de circuits oscillants, la modulation et la mise en œuvre sur PCB.

Les résultats expérimentaux sont conformes aux attentes, et le système peut être intégré à un amplificateur audio pour une restitution sonore claire et stable.

---

## 10. Contenu du dépôt GitHub

Le dépôt [EA113_-_Generateur_de_son]([https://github.com/matteob29/EA113_-_Generateur_de_son]) contient les éléments suivants :


- [Datasheet/](./Datasheet)      → Datasheet des coomposants utilisés
- [PCB/](./PCB)                  → Layouts et typons
- [Proteus/](./Proteus)          → Schémas et fichiers de simulation (.pdsprj)  
- [Rapport/](./Rapport)          → générateur de son.pdf  
- [Simulations/](./Simulations)  → Résultats théoriques sous Proteus
- [README.md](./README.md)

  
---

© 2025 — Mattéo BINET, Alexandre BROMET  
Département Électronique — EA113  
Enseirb-Matmeca — Générateur de son
