# Spécialisation Track C : MLOps pour l'IA Générative

**Durée estimée : 5 heures**

## Objectifs du Track

À la fin de ce track de spécialisation, les apprenants seront capables de :

-   **Orchestrer** des pipelines complexes pour des modèles d'IA Générative (GenAI) multi-étapes.
-   **Utiliser** ComfyUI pour prototyper et industrialiser des workflows de génération d'images.
-   **Concevoir** des systèmes agentiques basés sur les LLMs et comprendre leurs défis opérationnels.
-   **Mettre en œuvre** des stratégies d'évaluation et de gestion d'artefacts adaptées à la GenAI.

---

## C1 : Orchestration de Pipelines de Génération

Les applications d'IA Générative modernes reposent rarement sur un seul modèle. Elles combinent plusieurs modèles dans des pipelines complexes pour atteindre des résultats sophistiqués.

### C1.1. Le Paradigme des Graphes de Nœuds avec ComfyUI

**ComfyUI** est une interface utilisateur graphique basée sur des nœuds pour les modèles de diffusion d'images. Il a gagné une immense popularité car il expose le fonctionnement interne des pipelines de diffusion (comme Stable Diffusion) de manière visuelle et modulaire.

-   **Philosophie** : Chaque étape du processus de diffusion (chargement du modèle, encodage du prompt, échantillonnage, décodage VAE) est un **nœud** dans un graphe. Les utilisateurs peuvent connecter ces nœuds pour construire des workflows personnalisés.
-   **Avantages pour le MLOps** :
    -   **Prototypage Rapide** : Permet d'expérimenter rapidement avec des pipelines complexes (ex: ControlNet, LoRA, upscaling) sans écrire de code.
    -   **Reproductibilité** : Le graphe entier peut être sauvegardé sous forme de fichier JSON, capturant l'intégralité du workflow, y compris les prompts et les seeds. Ce fichier devient un artefact versionnable.
    -   **Industrialisation** : Un workflow ComfyUI peut être exécuté de manière programmatique via une API, ce qui permet de l'intégrer dans des pipelines MLOps plus larges pour la génération en batch ou le déploiement en tant que service.

### C1.2. Exemple de Workflow ComfyUI

Un workflow typique pour générer une image avec ControlNet dans ComfyUI pourrait ressembler à ceci :

1.  **Load Checkpoint** : Charger le modèle Stable Diffusion de base.
2.  **Load ControlNet Model** : Charger le modèle ControlNet (ex: `control_v11p_sd15_openpose`).
3.  **Load Image** : Charger l'image de contrôle (ex: une image d'une personne qui pose).
4.  **CLIP Text Encode** : Encoder le prompt textuel positif et négatif.
5.  **Apply ControlNet** : Appliquer le ControlNet à la diffusion, en utilisant l'image de contrôle.
6.  **KSampler** : Exécuter l'échantillonneur de diffusion pour générer l'image latente.
7.  **VAE Decode** : Décoder l'image latente en une image visible.
8.  **Save Image** : Sauvegarder l'image finale.

Ce graphe explicite rend le processus de génération entièrement transparent et débogable.

---

## C2 : Systèmes Agentiques avec les LLMs

Une autre tendance majeure de la GenAI est le développement de **systèmes agentiques** ou **agents autonomes** basés sur les LLMs. Ces agents peuvent raisonner, planifier et utiliser des outils pour accomplir des tâches complexes.

### C2.1. Architecture d'un Agent LLM

Un agent LLM typique est construit autour d'une boucle **ReAct (Reason + Act)** :

1.  **Raisonnement (Reason)** : Le LLM reçoit un objectif et décide de la prochaine action à entreprendre. Il peut décomposer l'objectif en sous-tâches.
2.  **Action (Act)** : Le LLM choisit un **outil** à utiliser et génère les arguments pour cet outil. Les outils peuvent être des fonctions Python, des APIs externes, ou même d'autres LLMs.
3.  **Observation** : L'outil est exécuté, et le résultat (l'observation) est renvoyé au LLM.
4.  **Itération** : Le LLM prend connaissance de l'observation et décide de la prochaine action, jusqu'à ce que l'objectif soit atteint.

Des frameworks comme **LangChain** et **LlamaIndex** fournissent des abstractions pour construire de tels agents.

### C2.2. Défis Opérationnels des Agents (AgentOps)

Le déploiement d'agents autonomes en production introduit des défis MLOps uniques :

-   **Monitoring de la Trajectoire** : Il ne suffit pas de monitorer les entrées et les sorties. Il faut tracer et visualiser la **trajectoire complète** de l'agent (la séquence de pensées, d'actions et d'observations) pour comprendre et déboguer son comportement.
-   **Évaluation Basée sur la Tâche** : L'évaluation d'un agent ne peut pas se baser sur des métriques simples. Il faut définir des scénarios de test complexes et mesurer le **taux de succès** de l'agent dans l'accomplissement de ses tâches.
-   **Gestion des Outils** : Chaque outil est une dépendance qui doit être versionnée et maintenue. Un changement dans une API externe peut casser le comportement de l'agent.
-   **Sécurité et Contrôle des Coûts** : Un agent autonome peut potentiellement entrer dans des boucles infinies ou utiliser des outils de manière inattendue, ce qui peut entraîner des coûts élevés ou des actions non désirées. Des mécanismes de garde-fou (guardrails) et de limitation sont essentiels.

---

## C3 : Évaluation et Gestion des Artefacts

(Contenu existant sur l'évaluation et la gestion des artefacts, qui reste pertinent)

### C3.1. Métriques de Qualité Objective

...

### C3.2. Évaluation Humaine (Human-in-the-Loop)

...

### C3.3. Suivi de la Provenance (Provenance Tracking)

...

### C3.4. Watermarking (Filigrane Numérique)

...
