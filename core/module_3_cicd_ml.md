# Module 3 : Pipelines CI/CD pour le ML

**Durée estimée : 4 heures**

## Objectifs du Module

À la fin de ce module, les apprenants seront capables de :

- **Concevoir** un pipeline de Continuous Integration (CI) qui valide le code, les données et les modèles.
- **Implémenter** un pipeline de Continuous Training (CT) qui automatise le réentraînement des modèles.
- **Comparer** les différentes stratégies de Continuous Delivery (CD) pour le déploiement de modèles.
- **Choisir** un outil d'orchestration de pipeline adapté à un cas d'usage donné.

---

## 3.1. Le Triptyque de l'Automatisation en MLOps

L'automatisation est le cœur du MLOps. Elle se décline en trois concepts interdépendants : CI, CT, et CD. Ce triptyque transforme le cycle de vie du ML d'un processus manuel et sujet aux erreurs en une "usine à modèles" automatisée et fiable.

![Pipeline CI/CD pour le ML](ci_cd_ml_pipeline.png)

- **Continuous Integration (CI)** : Se concentre sur la phase de build et de test, en s'assurant que tout nouvel artefact (code, données, modèle) est valide et ne casse pas le système existant.
- **Continuous Training (CT)** : Une nouveauté du MLOps, qui automatise la phase d'entraînement pour produire de nouvelles versions de modèles.
- **Continuous Delivery (CD)** : Gère la prise en charge d'un modèle validé et son déploiement sécurisé en production.

## 3.2. Continuous Integration (CI) : Plus que du Code

En MLOps, la CI va bien au-delà des tests unitaires du code. Un pipeline de CI pour le ML doit valider l'ensemble des composants qui définissent un système ML.

Un pipeline de CI typique pour le ML inclut les étapes suivantes :

1.  **Validation du Code** : Exécution des tests unitaires et d'intégration pour le code de préprocessing, d'entraînement, et d'inférence.
2.  **Validation des Données** : Vérification de la qualité et du schéma des nouvelles données à l'aide d'outils comme Great Expectations. Cette étape est cruciale pour éviter le "garbage in, garbage out".
3.  **Entraînement et Évaluation du Modèle (Test)** : Le modèle est entraîné sur un sous-ensemble de données pour s'assurer que le pipeline d'entraînement fonctionne. La performance du modèle est ensuite comparée à un seuil de base pour éviter les régressions majeures.

Ce pipeline est généralement déclenché à chaque `git push` sur une branche de développement, garantissant que le code est toujours dans un état livrable.

## 3.3. Continuous Training (CT) : L'Usine à Modèles

Le CT est le processus qui automatise le réentraînement des modèles pour les maintenir à jour. Un pipeline de CT est une version plus complète du pipeline d'entraînement utilisé en CI. Il est déclenché non pas par des changements de code, mais par des événements liés aux données ou à la performance.

### Déclencheurs de CT :

- **Nouvelles Données Disponibles** : Un batch de nouvelles données labellisées est disponible.
- **Dérive des Données Détectée** : Les outils de monitoring détectent que les statistiques des données en production ont changé de manière significative.
- **Dégradation de la Performance du Modèle** : La performance du modèle en production est passée en dessous d'un seuil critique.
- **Programmation Régulière** : Réentraînement périodique (ex: tous les jours, toutes les semaines).

Un pipeline de CT complet exécute toutes les étapes, de la préparation des données à la validation du modèle, et si le nouveau modèle est meilleur que celui en production, il le pousse vers le Registre de Modèles, prêt pour le déploiement.

## 3.4. Continuous Delivery (CD) : Déploiement Sécurisé

Le CD prend le relais une fois qu'un nouveau modèle a été validé et enregistré. Son rôle est de déployer ce modèle en production de manière sûre et contrôlée.

### Stratégies de Déploiement :

| Stratégie | Description | Avantages | Inconvénients |
| :--- | :--- | :--- | :--- |
| **Déploiement Direct (Recreate)** | L'ancienne version du modèle est arrêtée et la nouvelle est démarrée. | Simple à mettre en œuvre. | Provoque une interruption de service. |
| **Blue-Green** | La nouvelle version (Green) est déployée à côté de l'ancienne (Blue). Le trafic est basculé d'un coup vers la nouvelle version. | Pas d'interruption de service, rollback instantané. | Coûteux, car nécessite le double de ressources. |
| **Canary** | Un petit pourcentage du trafic (ex: 5%) est envoyé vers la nouvelle version. Si tout se passe bien, le trafic est progressivement augmenté. | Déploiement à faible risque, détection précoce des problèmes. | Plus complexe à mettre en œuvre et à monitorer. |
| **A/B Testing** | Plusieurs versions du modèle sont déployées en parallèle, et leur performance est comparée sur des métriques métier. | Permet de prendre des décisions de déploiement basées sur l'impact réel. | Le plus complexe à mettre en place. |

Le choix de la stratégie dépend du niveau de criticité de l'application et de la maturité de l'équipe MLOps.

## 3.5. Orchestration de Pipelines

Pour mettre en œuvre ces pipelines CI/CT/CD, on utilise des outils d'orchestration qui permettent de définir, d'exécuter et de monitorer des workflows complexes sous forme de graphes acycliques dirigés (DAGs).

### Principaux Orchestrateurs :

- **Kubeflow Pipelines** : Solution open-source native à Kubernetes. Idéale pour les entreprises qui ont déjà adopté Kubernetes. Chaque étape du pipeline est un conteneur Docker, ce qui offre une grande flexibilité.
- **Apache Airflow** : Outil d'orchestration de workflows très populaire, écrit en Python. Bien qu'il ne soit pas spécifiquement conçu pour le ML, il est souvent utilisé pour les pipelines de données et de CT.
- **Vertex AI Pipelines (GCP)** : Service managé sur Google Cloud, basé sur Kubeflow Pipelines. Il simplifie grandement la gestion de l'infrastructure sous-jacente.
- **AWS Step Functions & SageMaker Pipelines (AWS)** : Combinaison de services AWS pour créer des pipelines serverless ou gérés pour le ML.

Le choix de l'orchestrateur dépend de l'écosystème cloud, des compétences de l'équipe et des besoins en termes de scalabilité et de flexibilité.

## Références

[1] Google Cloud. (2024). *MLOps: Continuous delivery and automation pipelines in machine learning*. [https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)

