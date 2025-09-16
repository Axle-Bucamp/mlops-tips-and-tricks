# Plateforme Cloud Track : MLOps avec AWS SageMaker

**Durée estimée : 4 heures**

## Objectifs du Track

À la fin de ce track, les apprenants seront capables de :

-   **Orchestrer** un pipeline de Machine Learning de bout en bout avec Amazon SageMaker Pipelines.
-   **Gérer** le cycle de vie de leurs modèles à l'aide du SageMaker Model Registry.
-   **Centraliser** et **partager** des features avec le SageMaker Feature Store.
-   **Analyser** et **monitorer** les biais et l'explicabilité des modèles avec SageMaker Clarify.

---

## 1. Introduction à l'Écosystème MLOps d'AWS

Amazon Web Services (AWS) propose une suite d'outils complète et intégrée pour le MLOps, centrée autour d'**Amazon SageMaker**. SageMaker n'est pas un seul service, mais une plateforme modulaire qui couvre l'ensemble du cycle de vie du Machine Learning, de la préparation des données au monitoring en production.

Ce track se concentre sur les composants clés de SageMaker qui permettent d'implémenter les principes MLOps vus dans le tronc commun.

---

## 2. SageMaker Pipelines : Orchestration de Workflows ML

**Amazon SageMaker Pipelines** est le service d'orchestration de workflows ML d'AWS. Il permet de créer, d'automatiser et de gérer des pipelines de CI/CD/CT de manière native dans l'écosystème AWS.

### 2.1. Concepts Clés

-   **Pipeline** : La définition du workflow, spécifiée sous forme de graphe (DAG).
-   **Étapes (Steps)** : Chaque nœud du graphe est une étape. SageMaker propose des types d'étapes prédéfinis pour les tâches courantes (entraînement, transformation, etc.).
-   **Exécution (Execution)** : Une instance d'un pipeline, exécutée avec des paramètres spécifiques.
-   **Traçabilité (Lineage)** : SageMaker Pipelines capture automatiquement la lignée des données et des modèles. Pour chaque modèle, on peut remonter à l'exécution du pipeline qui l'a créé, aux données utilisées, et au code source.

### 2.2. Intégration avec les Services AWS

La force de SageMaker Pipelines réside dans son intégration native avec les autres services AWS :

-   **Amazon S3** pour le stockage des données et des artefacts.
-   **Amazon ECR** pour le stockage des images Docker personnalisées.
-   **AWS Step Functions** pour orchestrer des workflows plus complexes qui incluent des étapes non-ML.
-   **Amazon EventBridge** pour déclencher des pipelines en réponse à des événements (ex: un nouveau fichier de données déposé dans S3).

---

## 3. SageMaker Model Registry : Gestion du Cycle de Vie des Modèles

Le **SageMaker Model Registry** est le service de registre de modèles de SageMaker. Il est profondément intégré avec SageMaker Pipelines.

### 3.1. Groupes de Modèles et Versions

-   **Model Package Group** : Regroupe toutes les versions d'un même modèle (ex: `churn-prediction-model`).
-   **Model Version** : Une version spécifique du modèle, avec ses propres métriques de performance et son statut.

### 3.2. Statuts et Approbations

Chaque version de modèle a un statut (`PendingManualApproval`, `Approved`, `Rejected`). Le processus d'approbation peut être géré manuellement via la console AWS ou automatisé. Par exemple, un pipeline peut enregistrer un modèle avec le statut `PendingManualApproval`, et un manager peut ensuite l'approuver pour le déploiement. Cette approbation peut à son tour déclencher un pipeline de CD.

---

## 4. SageMaker Feature Store : Centralisation des Features

Le **SageMaker Feature Store** est un référentiel centralisé pour le stockage, le partage et la gestion des features pour les modèles de ML.

### 4.1. Résoudre les Problèmes de Feature Engineering

Le Feature Store s'attaque à deux problèmes courants :

-   **Duplication du Travail** : Plusieurs équipes peuvent réimplémenter les mêmes features, ce qui est inefficace.
-   **Incohérence Entraînement/Inférence (Training-Serving Skew)** : Des différences subtiles entre le code de feature engineering utilisé pour l'entraînement et celui utilisé pour l'inférence en temps réel peuvent dégrader considérablement les performances. Le Feature Store garantit que la même logique de transformation est utilisée dans les deux cas.

### 4.2. Stockage en Ligne et Hors Ligne

Le Feature Store maintient deux types de stockage :

-   **Offline Store** : Optimisé pour le stockage à faible coût de grandes quantités de données historiques (généralement sur S3). Utilisé pour l'entraînement de modèles.
-   **Online Store** : Optimisé pour une lecture à faible latence de features individuelles (généralement sur une base de données NoSQL rapide). Utilisé pour l'inférence en temps réel.

---

## 5. SageMaker Clarify : IA Responsable

**Amazon SageMaker Clarify** est un outil intégré qui aide les data scientists et les ingénieurs MLOps à construire des modèles de manière plus responsable en fournissant des capacités d'explicabilité et de détection de biais.

### 5.1. Détection de Biais

Clarify peut analyser les biais potentiels à différentes étapes du cycle de vie :

-   **Biais Pré-entraînement** : Mesurer les déséquilibres dans les données d'entraînement avant même de commencer l'entraînement.
-   **Biais Post-entraînement** : Évaluer si le modèle entraîné produit des prédictions biaisées.

Clarify génère des rapports détaillés qui quantifient les biais pour différents groupes démographiques.

### 5.2. Explicabilité des Modèles

Clarify intègre une implémentation de l'algorithme **SHAP (SHapley Additive exPlanations)** pour expliquer les prédictions des modèles. Il peut calculer les valeurs SHAP pour l'ensemble du jeu de données de test, ce qui permet d'avoir une vue globale de l'importance des features, ainsi que pour des prédictions individuelles, ce qui est essentiel pour le débogage et la conformité réglementaire.

L'utilisation de ces outils SageMaker permet de construire des pipelines MLOps robustes, scalables et responsables, entièrement gérés dans l'écosystème AWS.
