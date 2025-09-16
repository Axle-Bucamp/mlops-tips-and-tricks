# Module 2 : Le Cycle de Vie du Machine Learning

**Durée estimée : 3 heures**

## Objectifs du Module

À la fin de ce module, les apprenants seront capables de :

- **Décrire** en détail chaque étape du cycle de vie d'un projet de Machine Learning.
- **Mettre en œuvre** le versioning des données et des modèles à l'aide d'outils standards.
- **Utiliser** un outil de suivi d'expériences pour comparer et reproduire des résultats.
- **Comprendre** le rôle et l'importance d'un registre de modèles dans un environnement de production.

---

## 2.1. Vue d'Ensemble du Cycle de Vie

Le cycle de vie du Machine Learning est un processus itératif qui transforme des données brutes et des objectifs métier en un service de prédiction fiable et maintenable. Chaque étape produit des artefacts qui doivent être versionnés et gérés rigoureusement pour assurer la reproductibilité et la gouvernance. Les 7 étapes clés, telles qu'identifiées par des leaders du secteur comme Google Cloud [1], sont les suivantes :

1.  **Extraction des données**
2.  **Analyse et validation des données**
3.  **Préparation des données (Feature Engineering)**
4.  **Entraînement du modèle**
5.  **Évaluation du modèle**
6.  **Validation du modèle**
7.  **Service et monitoring du modèle**

Ce module se concentre sur les aspects opérationnels de la gestion des artefacts produits à chaque étape.

---

## 2.2. Gestion des Données : Le Fondement de Tout Projet ML

En MLOps, les données sont traitées comme un citoyen de première classe, au même titre que le code. Leur gestion est cruciale pour la stabilité et la performance des modèles.

### 2.2.1. Versioning des Données avec DVC

Contrairement au code, les grands ensembles de données ne peuvent pas être stockés directement dans Git. Des outils comme **DVC (Data Version Control)** ont été créés pour résoudre ce problème. DVC fonctionne en tandem avec Git :

- **Git** stocke de petits fichiers de métadonnées (fichiers `.dvc`) qui pointent vers les données réelles.
- Un **stockage distant** (comme Amazon S3, Google Cloud Storage, ou même un simple serveur SSH) héberge les fichiers de données volumineux.

Ce mécanisme permet de versionner des téraoctets de données tout en gardant le dépôt Git léger et rapide. Chaque commit Git peut ainsi correspondre à une version précise du code **et** des données, garantissant une reproductibilité parfaite.

### 2.2.2. Validation des Données avec Great Expectations

La qualité des données entrantes est un facteur de risque majeur. Des données incorrectes peuvent entraîner des échecs silencieux du modèle en production. **Great Expectations** est un outil open-source qui permet de définir des "attentes" sur les données, qui sont essentiellement des tests unitaires pour les données.

Exemples d'attentes :
- `expect_column_values_to_not_be_null('user_id')`
- `expect_column_values_to_be_between('age', 18, 99)`
- `expect_column_mean_to_be_between('price', 10.0, 50.0)`

Ces validations peuvent être intégrées dans un pipeline CI/CD pour rejeter automatiquement les données de mauvaise qualité avant qu'elles n'atteignent le pipeline d'entraînement.

---

## 2.3. Expérimentation : Suivi et Reproductibilité

La phase d'entraînement d'un modèle est hautement expérimentale. Les data scientists testent de multiples algorithmes, hyperparamètres et jeux de données. Sans un suivi rigoureux, ce processus devient chaotique et non reproductible.

### 2.3.1. Suivi des Expériences avec MLflow

**MLflow** est un outil open-source devenu un standard de l'industrie pour la gestion du cycle de vie du ML. Sa composante **MLflow Tracking** permet d'enregistrer systématiquement les éléments de chaque exécution (run) :

- **Paramètres** : Les hyperparamètres utilisés (ex: `learning_rate=0.01`).
- **Métriques** : Les résultats de l'évaluation (ex: `accuracy=0.92`).
- **Artefacts** : Tout fichier de sortie, y compris le modèle entraîné lui-même, des graphiques de performance, ou des fichiers de résultats.
- **Code Source** : La version du code (commit Git) utilisée pour l'exécution.

MLflow fournit une interface utilisateur pour visualiser, comparer et filtrer les exécutions, ce qui facilite grandement la sélection du meilleur modèle et la compréhension des facteurs qui influencent la performance.

---

## 2.4. Gestion des Modèles : Le Registre de Modèles

Une fois qu'un modèle a été jugé performant lors de la phase d'expérimentation, il doit être promu pour le déploiement. C'est le rôle du **Registre de Modèles (Model Registry)**.

Le registre de modèles est une base de données centralisée pour les modèles entraînés. Il agit comme un pont entre la data science et les opérations. Des outils comme **MLflow Model Registry**, ou les registres intégrés dans **AWS SageMaker** et **Vertex AI**, fournissent ces fonctionnalités.

Un registre de modèles permet de :

- **Gérer le cycle de vie du modèle** : Assigner des statuts aux versions de modèles (ex: `Staging`, `Production`, `Archived`).
- **Stocker les métadonnées** : Associer chaque version de modèle à son exécution d'entraînement, ses métriques de performance, et la version des données utilisée.
- **Assurer la gouvernance** : Mettre en place des processus d'approbation pour la transition d'un modèle de `Staging` à `Production`.
- **Faciliter le déploiement** : Fournir un identifiant unique et stable pour chaque version de modèle, que les pipelines de déploiement peuvent utiliser pour récupérer l'artefact correct.

Le tableau suivant illustre un exemple de cycle de vie dans un registre :

| Nom du Modèle | Version | Statut | Accuracy | Approuvé par |
| :--- | :--- | :--- | :--- | :--- |
| `fraud-detector` | 1 | Archived | 0.92 | user_A |
| `fraud-detector` | 2 | Staging | 0.94 | - |
| `fraud-detector` | 3 | Production | 0.95 | user_B |

L'utilisation d'un registre de modèles est une pratique fondamentale du MLOps qui garantit que seuls les modèles validés et testés sont déployés en production, tout en maintenant une traçabilité complète de leur origine.

## Références

[1] Google Cloud. (2024). *MLOps: Continuous delivery and automation pipelines in machine learning*. [https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)

---

## 2.5. Exemple Pratique : DVC et MLflow en Action

Pour rendre ces concepts plus concrets, voici un exemple simple qui combine DVC pour le versioning des données et MLflow pour le suivi des expériences.

### Scénario

Imaginons que nous avons un fichier de données `data.csv` et que nous voulons entraîner un modèle simple de régression logistique. Nous voulons versionner nos données et suivre les performances de notre modèle.

### Étape 1 : Initialisation et Versioning des Données avec DVC

Tout d'abord, nous initialisons notre projet et mettons nos données sous contrôle de version avec DVC.

```bash
# 1. Initialiser Git et DVC
git init
dvc init

# 2. Configurer le stockage distant (ici, un répertoire local pour l'exemple)
mkdir -p /tmp/dvc-storage
dvc remote add -d local_storage /tmp/dvc-storage

# 3. Ajouter les données à DVC
echo "feature1,feature2,target" > data.csv
echo "1.0,2.0,0" >> data.csv
echo "2.5,3.5,1" >> data.csv
dvc add data.csv

# 4. Valider les changements avec Git
git add data.csv.dvc .gitignore
git commit -m "Add initial dataset v1"

# 5. Pousser les données vers le stockage distant
dvc push
```

À ce stade, `data.csv` est remplacé par un petit fichier `data.csv.dvc` dans votre dépôt Git, tandis que les données réelles sont dans `/tmp/dvc-storage`.

### Étape 2 : Entraînement et Suivi avec MLflow

Maintenant, nous écrivons un script Python `train.py` pour entraîner notre modèle et suivre l'expérience avec MLflow.

```python
import mlflow
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Démarrer une nouvelle exécution MLflow
with mlflow.start_run():
    # Log des paramètres
    mlflow.log_param("solver", "lbfgs")
    mlflow.log_param("C", 1.0)

    # Charger les données (DVC garantit que c'est la bonne version)
    df = pd.read_csv('data.csv')
    X = df[['feature1', 'feature2']]
    y = df['target']

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.5, random_state=42)

    # Entraîner le modèle
    model = LogisticRegression(solver='lbfgs', C=1.0)
    model.fit(X_train, y_train)

    # Évaluer le modèle et log la métrique
    y_pred = model.predict(X_test)
    accuracy = accuracy_score(y_test, y_pred)
    mlflow.log_metric("accuracy", accuracy)

    print(f"Accuracy: {accuracy}")

    # Log du modèle en tant qu'artefact
    mlflow.sklearn.log_model(model, "model")

    print("Modèle et métriques enregistrés dans MLflow.")

```

Pour exécuter ce script et visualiser les résultats :

```bash
# Exécuter le script d'entraînement
python train.py

# Lancer l'interface utilisateur de MLflow
# Les résultats seront visibles à http://127.0.0.1:5000
mlflow ui
```

Cet exemple simple montre comment DVC et MLflow travaillent ensemble pour assurer la reproductibilité à la fois des données et des expériences, formant ainsi la base d'un pipeline MLOps robuste.




![Cycle de vie MLOps](mlops_lifecycle.png)

