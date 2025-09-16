# Module 4 : Monitoring, Observabilité et IA Responsable

**Durée estimée : 3 heures**

## Objectifs du Module

À la fin de ce module, les apprenants seront capables de :

- **Mettre en place** un système de monitoring pour suivre la performance d'un modèle en production.
- **Distinguer** les concepts de dérive des données (data drift) et de dérive des concepts (concept drift).
- **Appliquer** les principes de l'observabilité (logs, métriques, traces) aux systèmes ML.
- **Intégrer** des outils d'explicabilité et d'équité pour construire une IA plus responsable.

---

## 4.1. Monitoring : Garder un Œil sur la Production

Le déploiement d'un modèle n'est pas la fin du voyage, mais le début de sa vie en production. Le monitoring est essentiel pour s'assurer que le modèle continue de fournir de la valeur et de se comporter comme prévu. Un monitoring efficace en MLOps couvre plusieurs aspects.

### 4.1.1. Monitoring de la Performance du Modèle

Il s'agit de suivre les métriques de performance du modèle (celles utilisées lors de l'évaluation hors ligne) en temps réel. Cela nécessite une boucle de rétroaction (feedback loop) pour collecter la vérité terrain (ground truth) pour les prédictions du modèle.

- **Exemples de métriques** : Accuracy, Précision, Rappel, F1-score pour la classification ; RMSE, MAE pour la régression.
- **Défis** : La vérité terrain n'est souvent pas disponible immédiatement (ex: la prédiction de churn d'un client ne sera confirmée qu'à la fin du mois).

### 4.1.2. Monitoring de la Performance Opérationnelle

Il s'agit de suivre la performance de l'infrastructure qui sert le modèle.

- **Métriques clés** : Latence des prédictions (temps de réponse), débit (nombre de prédictions par seconde), taux d'erreur, utilisation du CPU/GPU.
- **Importance** : Un modèle, même très précis, est inutile s'il est trop lent ou fréquemment indisponible.

---

## 4.2. Gérer la Dérive (Drift)

La performance des modèles de ML se dégrade inévitablement avec le temps. Ce phénomène, appelé **dérive (drift)**, est une des raisons principales pour lesquelles le MLOps est nécessaire.

### 4.2.1. Dérive des Données (Data Drift)

La **dérive des données** se produit lorsque les propriétés statistiques des données d'entrée en production changent par rapport aux données d'entraînement. Le modèle est alors confronté à des données qu'il n'a jamais vues, ce qui peut dégrader ses performances.

- **Exemple** : Un modèle de scoring de crédit entraîné avant une crise économique verra ses performances chuter lorsque les profils financiers des demandeurs changent radicalement.
- **Détection** : En comparant les distributions statistiques (moyenne, variance, etc.) des features entre les données d'entraînement et les données de production.

### 4.2.2. Dérive des Concepts (Concept Drift)

La **dérive des concepts** est plus subtile. Elle se produit lorsque la relation entre les données d'entrée et la variable cible change. Les données d'entrée peuvent rester les mêmes, mais la signification de ces données a changé.

- **Exemple** : Dans un système de détection de spam, les spammeurs changent constamment leurs tactiques. Les caractéristiques d'un email de spam (le "concept" de spam) évoluent, même si les mots utilisés sont les mêmes.
- **Détection** : Généralement détectée par une chute de la performance du modèle, alors que la distribution des données d'entrée reste stable.

Le tableau suivant résume la différence :

| Type de Dérive | Ce qui Change | Exemple |
| :--- | :--- | :--- |
| **Data Drift** | La distribution des données d'entrée P(X). | Un changement démographique des utilisateurs. |
| **Concept Drift**| La relation entre les entrées et la sortie P(Y|X). | Un changement dans les préférences des consommateurs. |

La détection de la dérive est un signal clé pour déclencher un pipeline de **Continuous Training (CT)**.

---

## 4.3. Observabilité : Comprendre le "Pourquoi"

Alors que le monitoring vous dit **si** quelque chose ne va pas, l'**observabilité** vous aide à comprendre **pourquoi**. L'observabilité dans les systèmes ML repose sur trois piliers :

1.  **Logs** : Enregistrements détaillés des événements. Pour un service de prédiction, cela inclut les données d'entrée, la prédiction de sortie, et des informations contextuelles.
2.  **Métriques** : Mesures numériques agrégées dans le temps (ex: latence moyenne, taux de dérive).
3.  **Traces** : Visualisation du parcours d'une requête à travers les différents services d'un système distribué. Une trace peut montrer le temps passé dans le service d'authentification, le service de feature store, et le service de prédiction.

Une bonne observabilité est cruciale pour le débogage des systèmes ML complexes.

---

## 4.4. IA Responsable : Équité, Explicabilité et Sécurité

Construire des systèmes ML performants ne suffit pas. Il est impératif de s'assurer qu'ils sont également équitables, transparents et sécurisés.

### 4.4.1. Équité (Fairness)

Un modèle est inéquitable s'il produit des résultats systématiquement moins bons pour certains groupes démographiques (protégés par la loi ou l'éthique) que pour d'autres.

- **Outils** : Des bibliothèques comme **Fairlearn** (Microsoft) ou **AI Fairness 360** (IBM) permettent de mesurer les biais dans les données et les modèles, et proposent des algorithmes pour les atténuer.

### 4.4.2. Explicabilité (Explainability)

L'explicabilité (ou interprétabilité) concerne la capacité à comprendre pourquoi un modèle a pris une décision particulière. C'est une exigence légale dans certains domaines (comme le crédit, avec le droit à une explication) et un outil de débogage puissant.

- **SHAP (SHapley Additive exPlanations)** : Une méthode populaire basée sur la théorie des jeux qui explique la prédiction d'une instance en calculant la contribution de chaque feature à cette prédiction.
- **LIME (Local Interpretable Model-agnostic Explanations)** : Une autre technique qui explique les prédictions d'un modèle en l'approximant localement par un modèle plus simple et interprétable.

Des outils comme **SageMaker Clarify** (AWS) ou **Vertex AI Explainable AI** (GCP) intègrent ces techniques dans leurs plateformes.

### 4.4.3. Sécurité

Les modèles de ML sont vulnérables à des attaques spécifiques :

- **Attaques Adverses (Adversarial Attacks)** : De légères perturbations des données d'entrée, imperceptibles pour un humain, peuvent tromper complètement le modèle (ex: changer quelques pixels d'une image).
- **Empoisonnement de Données (Data Poisoning)** : Introduction de données malveillantes dans le jeu de données d'entraînement pour créer une "backdoor" dans le modèle.

La mise en place de défenses contre ces attaques, comme la validation robuste des entrées et l'entraînement contradictoire (adversarial training), est une partie intégrante de la sécurisation des pipelines MLOps.

