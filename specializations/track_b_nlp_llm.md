# Spécialisation Track B : MLOps pour le NLP et les Large Language Models (LLMs)

**Durée estimée : 5 heures**

## Objectifs du Track

À la fin de ce track de spécialisation, les apprenants seront capables de :

-   **Identifier** les défis uniques posés par les LLMs en production (LLMOps).
-   **Mettre en place** des stratégies de gestion de prompts et de fine-tuning.
-   **Appliquer** des techniques d'optimisation pour l'inférence de LLMs à grande échelle.
-   **Sécuriser** les applications basées sur les LLMs contre les attaques spécifiques.

---

## B1 : LLMOps - Les Nouveaux Défis Opérationnels

L'émergence des Large Language Models (LLMs) a introduit une nouvelle branche du MLOps, souvent appelée **LLMOps**. Bien que les principes de base du MLOps s'appliquent, les LLMs présentent des défis d'une toute autre ampleur.

### B1.1. Les Défis Uniques des LLMs

| Défi | Description | Impact sur les Opérations |
| :--- | :--- | :--- |
| **Taille des Modèles** | Les modèles peuvent avoir de plusieurs milliards à plus d'un billion de paramètres, nécessitant des centaines de gigaoctets de stockage. | Le stockage, le chargement en mémoire et le transfert des modèles deviennent des opérations complexes et coûteuses. |
| **Coûts Computationnels** | L'entraînement et même l'inférence nécessitent des clusters de GPU haut de gamme (ex: NVIDIA A100 ou H100). | Le coût de l'infrastructure est un facteur majeur. L'optimisation des ressources est primordiale. |
| **Latence d'Inférence** | La génération de texte est un processus séquentiel qui peut être lent, ce qui est problématique pour les applications interactives (chatbots). | Nécessite des techniques d'optimisation spécifiques pour réduire le temps de réponse. |
| **Comportement Émergent** | Les LLMs peuvent avoir des comportements inattendus et non déterministes, rendant le débogage et l'évaluation difficiles. | Le monitoring doit aller au-delà des métriques traditionnelles et inclure l'évaluation de la qualité sémantique. |

---

## B2 : Gestion de Prompts et Fine-Tuning

Avec les LLMs, une grande partie de l'ingénierie ne se fait plus au niveau de l'architecture du modèle, mais au niveau de la manière dont on interagit avec lui.

### B2.1. Ingénierie et Versioning de Prompts (Prompt Engineering)

Le **prompt** est le texte d'entrée qui guide le LLM. La qualité de la sortie dépend énormément de la qualité du prompt. Le **Prompt Engineering** est l'art de concevoir des prompts efficaces.

En LLMOps, les prompts sont traités comme des artefacts de première classe :

-   **Templates de Prompts** : Utilisation de templates (ex: avec des f-strings en Python) pour structurer les prompts de manière reproductible.
-   **Versioning de Prompts** : Les prompts doivent être versionnés (ex: dans un dépôt Git) au même titre que le code, car un changement de prompt peut radicalement altérer le comportement de l'application.
-   **Évaluation de Prompts** : Mise en place de pipelines pour évaluer automatiquement la performance de différents prompts sur un jeu de données de test.

### B2.2. Stratégies de Fine-Tuning

Le **fine-tuning** consiste à spécialiser un LLM pré-entraîné sur un jeu de données spécifique à une tâche. Le fine-tuning complet d'un grand modèle étant très coûteux, des techniques de **Parameter-Efficient Fine-Tuning (PEFT)** ont été développées.

-   **LoRA (Low-Rank Adaptation)** : Une des techniques PEFT les plus populaires. Au lieu de mettre à jour tous les poids du modèle, LoRA ajoute de petites matrices "adaptatrices" dans les couches du Transformer et n'entraîne que ces dernières. Cela réduit considérablement le nombre de paramètres à entraîner (souvent moins de 1% du total), rendant le fine-tuning beaucoup plus accessible.

---

## B3 : Optimisation de l'Inférence de LLMs

Servir des LLMs en production de manière efficace (faible latence, haut débit) est un défi majeur. Plusieurs techniques sont utilisées pour optimiser l'inférence.

### B3.1. Batching et PagedAttention

-   **Batching Continu (Continuous Batching)** : Contrairement au batching traditionnel où l'on attend que toutes les requêtes du batch soient terminées, le batching continu permet d'ajouter de nouvelles requêtes au batch dès que de la place se libère sur le GPU. Cela améliore considérablement le débit.
-   **PagedAttention** : Une innovation clé (popularisée par **vLLM**) qui résout le problème de la fragmentation de la mémoire GPU causée par les caches Clé-Valeur (KV cache) de longueur variable. En allouant la mémoire par blocs (pages) de manière non contiguë, PagedAttention permet d'atteindre un débit beaucoup plus élevé.

### B3.2. Outils de Serving de LLMs

Des frameworks spécialisés ont émergé pour implémenter ces optimisations :

-   **vLLM** : Un moteur d'inférence open-source très performant qui implémente PagedAttention et le batching continu.
-   **TensorRT-LLM (NVIDIA)** : Une bibliothèque de NVIDIA pour optimiser l'exécution des LLMs sur les GPU NVIDIA. Elle compile les modèles pour une performance maximale.
-   **LiteLLM** : Un outil qui fournit une interface unifiée (similaire à l'API d'OpenAI) pour interagir avec plus de 100 services de LLMs (OpenAI, Anthropic, Vertex AI, modèles auto-hébergés avec vLLM, etc.). Il simplifie grandement la gestion d'un environnement multi-modèles.

---

## B4 : Sécurité des LLMs

Les LLMs introduisent de nouvelles surfaces d'attaque qui nécessitent des mesures de sécurité spécifiques.

### B4.1. Attaques Spécifiques aux LLMs

-   **Prompt Injection** : Un utilisateur malveillant conçoit un prompt pour contourner les instructions originales et faire en sorte que le LLM exécute une action non désirée (ex: ignorer les instructions précédentes et révéler des informations confidentielles).
-   **Fuite de Données (Data Leakage)** : Le LLM peut accidentellement révéler des informations sensibles sur lesquelles il a été entraîné.
-   **Génération de Contenu Nocif** : Le modèle peut être utilisé pour générer de la désinformation, des discours de haine, ou du code malveillant.

### B4.2. Gardiens (Guardrails)

Pour se protéger contre ces attaques, on met en place des **"guardrails"**, qui sont des mécanismes de sécurité qui filtrent les entrées et les sorties du LLM.

-   **LLM-Guard (Protect AI)** : Un outil open-source qui permet de mettre en place une série de scanners pour détecter et bloquer les menaces, notamment :
    -   Détection de prompt injection.
    -   Détection de sujets non pertinents ou dangereux.
    -   Anonymisation des informations personnelles identifiables (PII).
    -   Détection de code malveillant.
-   **NeMo Guardrails (NVIDIA)** : Un autre framework open-source pour ajouter des barrières de sécurité programmables aux applications basées sur les LLMs.

L'intégration de ces gardiens est une étape essentielle dans le pipeline de production d'une application LLM, agissant comme un pare-feu entre l'utilisateur et le modèle.
