# Module 1 : Introduction au MLOps

**Durée estimée : 2 heures**

## Objectifs du Module

À la fin de ce module, les apprenants seront capables de :

- **Distinguer** les concepts clés d'Intelligence Artificielle (IA), de Machine Learning (ML), de Deep Learning (DL) et de modélisation statistique.
- **Comprendre** la nécessité du MLOps pour industrialiser les projets de Machine Learning.
- **Identifier** les différences fondamentales entre les approches DevOps traditionnelles et le MLOps.
- **Décrire** les principales étapes du cycle de vie MLOps.

---

## 1.1. Définitions et Taxonomie

Pour construire des systèmes ML robustes, il est essentiel de maîtriser le vocabulaire et les concepts fondamentaux qui sous-tendent ce domaine. Bien que souvent utilisés de manière interchangeable, les termes IA, Machine Learning et Deep Learning représentent des concepts distincts avec des relations hiérarchiques.

### 1.1.1. L'Intelligence Artificielle (IA)

L'**Intelligence Artificielle** est un vaste champ de l'informatique qui englobe toutes les théories et techniques visant à créer des machines capables de simuler l'intelligence humaine. Cela inclut des approches très diverses, allant des systèmes à base de règles (comme les premiers moteurs de recherche ou les systèmes experts) aux approches statistiques plus modernes.

> L'IA est un terme générique pour un large éventail d'approches algorithmiques, incluant les systèmes basés sur des règles, les méthodes statistiques, et les systèmes apprenants.

### 1.1.2. Le Machine Learning (ML)

Le **Machine Learning** est un sous-domaine de l'IA qui se concentre sur la conception d'algorithmes capables d'**apprendre à partir des données**. Plutôt que d'être explicitement programmés pour une tâche, les systèmes de ML utilisent des données pour ajuster leurs paramètres internes afin d'améliorer leur performance sur cette tâche. L'optimisation, souvent par des méthodes comme la descente de gradient, est au cœur du processus d'apprentissage.

### 1.1.3. Le Deep Learning (DL)

Le **Deep Learning** (ou apprentissage profond) est à son tour un sous-domaine du Machine Learning. Il se caractérise par l'utilisation de **réseaux de neurones profonds**, c'est-à-dire des architectures composées de nombreuses couches de neurones (d'où le terme "profond"). Ces architectures permettent d'apprendre des représentations de données de plus en plus complexes et abstraites, ce qui a conduit à des avancées spectaculaires dans des domaines comme la reconnaissance d'images et le traitement du langage naturel.

Le tableau suivant résume les relations entre ces concepts :

| Concept | Définition | Exemples |
| :--- | :--- | :--- |
| **Intelligence Artificielle** | Champ général de la simulation de l'intelligence humaine. | Systèmes experts, chatbots, moteurs de recommandation. |
| **Machine Learning** | Algorithmes qui apprennent à partir des données via l'optimisation. | Régression linéaire, arbres de décision, SVM. |
| **Deep Learning** | Sous-domaine du ML utilisant des réseaux de neurones profonds. | CNN (Convolutional Neural Networks), RNN (Recurrent Neural Networks), Transformers. |

---

## 1.2. La Nécessité du MLOps : Du Notebook à la Production

Le développement de modèles de Machine Learning commence souvent dans un environnement de recherche et d'expérimentation, typiquement un notebook (Jupyter, Colab). Si cet environnement est excellent pour l'exploration, il est notoirement insuffisant pour construire des applications fiables et scalables.

Le passage du **"proof-of-concept"** (preuve de concept) à un **système en production** est un défi majeur qui met en lumière de nombreuses problématiques non présentes dans la phase d'expérimentation :

- **Reproductibilité** : Comment garantir que les résultats obtenus dans le notebook peuvent être reproduits de manière fiable ?
- **Scalabilité** : Comment le système se comportera-t-il avec un volume de données beaucoup plus important et un grand nombre d'utilisateurs ?
- **Automatisation** : Comment automatiser le réentraînement et le redéploiement du modèle lorsque de nouvelles données sont disponibles ?
- **Monitoring** : Comment surveiller la performance du modèle en production et détecter sa dégradation (dérive) ?
- **Collaboration** : Comment plusieurs data scientists et ingénieurs peuvent-ils collaborer efficacement sur le même projet ?

Le **MLOps (Machine Learning Operations)** est la discipline qui vise à répondre à ces questions en appliquant les principes de DevOps au cycle de vie des systèmes de Machine Learning.

---

## 1.3. DevOps vs. MLOps : Les Différences Fondamentales

Bien que le MLOps s'inspire fortement du DevOps, il présente des défis uniques qui le distinguent des pratiques de développement logiciel traditionnelles. Comme le souligne un article de Google Cloud, un système ML est bien plus que du simple code [1].

Le tableau ci-dessous met en évidence les principales différences :

| Aspect | DevOps Traditionnel | MLOps |
| :--- | :--- | :--- |
| **Composants** | Code source | Code + Données + Modèle |
| **Tests** | Tests unitaires, tests d'intégration. | Tests de code + Validation des données + Évaluation de la qualité du modèle. |
| **Déploiement** | Déploiement d'une application ou d'un service. | Déploiement d'un pipeline d'entraînement qui déploie à son tour un service de prédiction. |
| **Performance** | La performance de l'application est généralement stable. | La performance du modèle peut se dégrader avec le temps (dérive des données). |
| **Monitoring** | Monitoring de l'infrastructure et de la performance de l'application. | Monitoring de l'infrastructure + Monitoring de la performance du modèle et de la dérive des données. |
| **Nature** | Déterministe (le même code produit le même résultat). | Stochastique (la nature statistique des modèles introduit de la variabilité). |

Une propriété unique du MLOps est le concept de **Continuous Training (CT)**, qui n'a pas d'équivalent direct en DevOps. Le CT concerne l'automatisation du réentraînement des modèles pour s'adapter aux nouvelles données et maintenir leur performance.

---

## 1.4. Le Cycle de Vie MLOps

Le cycle de vie MLOps est un processus itératif qui vise à automatiser et à fiabiliser la livraison de systèmes ML. Il peut être décomposé en plusieurs étapes clés, qui seront détaillées dans les modules suivants :

1.  **Cadrage du Projet (Framing)** : Définition du problème métier, des métriques de succès et des données nécessaires.
2.  **Ingénierie des Données (Data Engineering)** : Ingestion, validation, nettoyage et transformation des données.
3.  **Ingénierie des Modèles (Model Engineering)** : Entraînement, évaluation et sélection des modèles.
4.  **Déploiement des Modèles (Model Deployment)** : Service du modèle en tant qu'API et intégration dans les applications.
5.  **Monitoring des Modèles (Model Monitoring)** : Surveillance de la performance technique et métier du modèle en production.

Ce cycle est continu : les informations issues du monitoring peuvent déclencher une nouvelle itération, par exemple en lançant un pipeline de réentraînement (CT) avec de nouvelles données.

## Références

[1] Google Cloud. (2024). *MLOps: Continuous delivery and automation pipelines in machine learning*. [https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)

