# Plateforme Cloud Track : MLOps avec Google Cloud Vertex AI

**Durée estimée : 4 heures**

## Objectifs du Track

À la fin de ce track, les apprenants seront capables de :

-   **Concevoir** et **exécuter** des pipelines ML serverless avec Vertex AI Pipelines.
-   **Gérer** et **versionner** des modèles de manière centralisée avec le Vertex AI Model Registry.
-   **Utiliser** Vertex AI Feature Store pour une gestion cohérente des features entre l'entraînement et l'inférence.
-   **Expliquer** les prédictions de leurs modèles à l'aide de Vertex AI Explainable AI.

---

## 1. Introduction à Vertex AI : La Plateforme Unifiée de Google Cloud

**Vertex AI** est la plateforme de Machine Learning unifiée de Google Cloud Platform (GCP). Elle vise à simplifier et à accélérer l'ensemble du cycle de vie du ML en fournissant une suite d'outils intégrés, allant de la préparation des données au déploiement et au monitoring de modèles. L'un des principaux avantages de Vertex AI est son approche "serverless" pour de nombreux composants, ce qui permet aux équipes de se concentrer sur le développement de modèles plutôt que sur la gestion de l'infrastructure.

---

## 2. Vertex AI Pipelines : Orchestration Serverless Basée sur Kubeflow

**Vertex AI Pipelines** est un service entièrement managé qui permet d'orchestrer des workflows de ML. Il est basé sur le projet open-source **Kubeflow Pipelines (KFP)**, ce qui signifie que les pipelines définis avec le SDK KFP peuvent être exécutés sur Vertex AI sans modification majeure.

### 2.1. Composants et SDK

-   **SDK Kubeflow Pipelines (KFP)** : Les pipelines sont définis en Python à l'aide du SDK KFP. Chaque fonction décorée avec `@component` devient une étape du pipeline, qui est exécutée dans son propre conteneur Docker.
-   **Compilation** : Le pipeline défini en Python est compilé en un fichier YAML (une définition de DAG) qui peut ensuite être soumis à l'API Vertex AI pour exécution.
-   **Exécution Serverless** : Contrairement à une installation open-source de Kubeflow qui nécessite de gérer un cluster Kubernetes, Vertex AI Pipelines provisionne et gère automatiquement les ressources de calcul nécessaires pour chaque étape du pipeline.

### 2.2. Intégration avec l'Écosystème Google Cloud

Vertex AI Pipelines s'intègre de manière transparente avec les autres services GCP :

-   **Google Cloud Storage (GCS)** pour le stockage des artefacts.
-   **BigQuery** pour l'ingestion de données à grande échelle.
-   **Vertex AI Training** pour les jobs d'entraînement distribués.
-   **Cloud Functions** et **Cloud Scheduler** pour déclencher des pipelines en réponse à des événements ou à des intervalles réguliers.

---

## 3. Vertex AI Model Registry : Le Hub Central pour les Modèles

Le **Vertex AI Model Registry** est un service centralisé pour stocker, gérer et versionner les modèles de Machine Learning. Il sert de source de vérité unique pour tous les modèles entraînés au sein d'une organisation.

### 3.1. Fonctionnalités Clés

-   **Versioning** : Chaque modèle enregistré peut avoir plusieurs versions. Le registre stocke l'artefact du modèle ainsi que les métadonnées associées.
-   **Évaluation** : Il est possible d'attacher des métriques d'évaluation à chaque version de modèle, ce qui permet de comparer facilement leurs performances.
-   **Déploiement** : Le registre est directement intégré avec **Vertex AI Endpoints**. On peut déployer une version de modèle spécifique sur un endpoint en quelques clics ou via l'API, en spécifiant la répartition du trafic pour des stratégies de déploiement blue-green ou canary.

---

## 4. Vertex AI Feature Store : Cohérence entre Entraînement et Inférence

Le **Vertex AI Feature Store** (maintenant partie de BigQuery) offre une solution centralisée pour la gestion des features, garantissant la cohérence entre l'entraînement et l'inférence.

### 4.1. Architecture et Concepts

-   **EntityType** : Un groupe de features sémantiquement liées (ex: les features d'un `client`).
-   **Feature** : Une propriété mesurable (ex: `age`, `montant_dernier_achat`).
-   **Serving** : Le Feature Store propose deux modes de service :
    -   **Batch Serving** : Pour extraire de grandes quantités de données pour l'entraînement de modèles, généralement à partir de BigQuery.
    -   **Online Serving** : Pour récupérer des features à faible latence pour l'inférence en temps réel, via une API optimisée.

En utilisant le Feature Store, on s'assure que les calculs de features sont définis une seule fois et réutilisés partout, éliminant ainsi le risque de "training-serving skew".

---

## 5. Vertex AI Explainable AI : Comprendre les Prédictions

**Vertex AI Explainable AI** est un service intégré qui aide à comprendre les prédictions des modèles en fournissant des attributions de features.

### 5.1. Méthodes d'Attribution

Explainable AI supporte plusieurs méthodes d'attribution populaires :

-   **SHAP (SHapley Additive exPlanations)** : Une méthode basée sur la théorie des jeux qui fournit des attributions précises, mais qui peut être coûteuse en calcul.
-   **Integrated Gradients** : Une autre méthode d'attribution qui est souvent plus rapide que SHAP.
-   **XRAI (eXplainable AI for Images)** : Une méthode spécifiquement conçue pour les modèles de classification d'images, qui met en évidence les régions de l'image qui ont le plus contribué à la prédiction.

### 5.2. Intégration

L'explicabilité peut être configurée directement lors du déploiement d'un modèle sur un Vertex AI Endpoint. Une fois activée, l'API de prédiction peut renvoyer non seulement la prédiction, mais aussi les attributions de features qui expliquent cette prédiction, sans avoir à appeler un service séparé.

En combinant ces services, Vertex AI offre une plateforme puissante et hautement automatisée pour implémenter des pratiques MLOps modernes sur Google Cloud.
