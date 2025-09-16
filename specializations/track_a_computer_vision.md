# Spécialisation Track A : MLOps pour la Computer Vision

**Durée estimée : 4 heures**

## Objectifs du Track

À la fin de ce track de spécialisation, les apprenants seront capables de :

-   **Construire** des pipelines de données robustes et efficaces pour des projets de Computer Vision (CV).
-   **Mettre en œuvre** des stratégies de monitoring spécifiques aux modèles de CV.
-   **Optimiser** et **déployer** des modèles de CV sur des appareils Edge à ressources contraintes.

---

## A1 : Pipelines de Données pour la Computer Vision

La Computer Vision présente des défis uniques en matière de gestion des données en raison de la taille et de la complexité des données visuelles.

### A1.1. Gestion des Données Visuelles à Grande Échelle

Les datasets en CV peuvent rapidement atteindre plusieurs téraoctets. Une gestion efficace est donc primordiale.

-   **Stockage** : Utilisation de solutions de stockage objet (Amazon S3, Google Cloud Storage) qui sont scalables et rentables.
-   **Formats de Données Optimisés** : Pour accélérer la lecture des données lors de l'entraînement, il est courant de convertir les images brutes en formats optimisés comme **TFRecord** (pour TensorFlow) ou **WebDataset**. Ces formats permettent de sérialiser et de stocker de grands ensembles de données de manière efficace, en réduisant les goulots d'étranglement I/O.
-   **Versioning avec DVC** : Comme vu dans le tronc commun, **DVC** est particulièrement adapté pour versionner de grands datasets d'images sans surcharger le dépôt Git.

### A1.2. Augmentation de Données (Data Augmentation)

L'augmentation de données est une technique cruciale en CV pour augmenter artificiellement la taille et la diversité du jeu de données d'entraînement. Cela permet d'améliorer la robustesse et la capacité de généralisation du modèle.

-   **Techniques Courantes** : Rotations, retournements, zooms, changements de luminosité, de contraste, et de saturation.
-   **Implémentation** : L'augmentation peut être réalisée **hors ligne** (offline), en créant de nouvelles images avant l'entraînement, ou **en ligne** (online), en appliquant des transformations à la volée pendant l'entraînement. L'approche en ligne est généralement préférée car elle est plus flexible et ne nécessite pas de stocker les images augmentées.
-   **Outils** : Des bibliothèques comme **Albumentations** ou les fonctions intégrées de **TensorFlow/Keras** et **PyTorch** sont largement utilisées.

---

## A2 : Spécificités du Monitoring en Computer Vision

Le monitoring des modèles de CV en production va au-delà des métriques de classification ou de régression classiques.

### A2.1. Monitoring de la Qualité d'Image

La performance d'un modèle de CV est très sensible à la qualité des images qu'il reçoit en entrée. Il est donc essentiel de monitorer les aspects suivants :

-   **Luminosité et Contraste** : Détecter si les images en production sont systématiquement plus sombres ou plus claires que les données d'entraînement.
-   **Flou (Blur)** : Mesurer le niveau de flou des images. Une augmentation du flou peut indiquer un problème avec le capteur (ex: caméra sale).
-   **Résolution** : S'assurer que la résolution des images en entrée correspond à celle attendue par le modèle.

### A2.2. Dérive des Données Spécifique à la CV

En plus des métriques statistiques générales, la dérive en CV peut être très visuelle.

-   **Changements d'Illumination** : Un modèle entraîné sur des images de jour peut mal fonctionner la nuit.
-   **Changements d'Angle de Caméra** : Si une caméra de surveillance est déplacée, les objets apparaîtront sous un angle différent, ce qui peut perturber le modèle.
-   **Apparition de Nouveaux Objets** : Un modèle de détection d'objets peut être confronté à des objets qu'il n'a jamais vus.

Des techniques comme la comparaison d'histogrammes de couleurs ou l'analyse de la distribution des features extraites par le modèle peuvent aider à détecter ces dérives.

---

## A3 : Déploiement sur l'Edge (Edge Computing)

De nombreuses applications de CV (voitures autonomes, drones, caméras intelligentes) nécessitent que les prédictions soient faites directement sur l'appareil (l'"Edge"), en raison de contraintes de latence, de connectivité ou de confidentialité.

### A3.1. Optimisation de Modèles pour l'Edge

Les appareils Edge ont des ressources (CPU, mémoire, énergie) limitées. Il est donc nécessaire d'optimiser les modèles avant de les déployer.

| Technique | Description | Avantages | Inconvénients |
| :--- | :--- | :--- | :--- |
| **Quantization** | Réduction de la précision des poids du modèle (ex: de 32-bit float à 8-bit integer). | Réduction significative de la taille du modèle et de la latence d'inférence. | Peut entraîner une légère perte de précision. |
| **Pruning (Élagage)** | Suppression des connexions (poids) les moins importantes dans le réseau de neurones. | Réduit la taille du modèle et le nombre de calculs. | Peut être complexe à mettre en œuvre efficacement. |
| **Distillation** | Entraînement d'un petit modèle (l'"étudiant") à imiter le comportement d'un grand modèle plus performant (le "professeur"). | Permet de créer des modèles compacts et rapides avec une bonne performance. | Nécessite un modèle "professeur" bien entraîné. |

### A3.2. Frameworks pour le Déploiement Edge

Une fois le modèle optimisé, il doit être converti dans un format compatible avec les frameworks d'inférence pour l'Edge.

-   **TensorFlow Lite (TFLite)** : Le framework de Google pour le déploiement sur les appareils mobiles (Android, iOS) et les microcontrôleurs. Il est hautement optimisé pour l'inférence sur CPU et GPU mobiles.
-   **ONNX Runtime** : Un moteur d'inférence multi-plateforme qui supporte le format **ONNX (Open Neural Network Exchange)**. ONNX est un format standard ouvert qui permet de représenter les modèles de ML, ce qui facilite l'interopérabilité entre différents frameworks (ex: entraîner en PyTorch, déployer avec ONNX Runtime).
-   **NVIDIA TensorRT** : Un SDK de NVIDIA pour l'optimisation et l'inférence haute performance sur les GPU NVIDIA, souvent utilisé pour les applications Edge qui nécessitent une très faible latence (ex: robotique, véhicules autonomes).

Le déploiement sur l'Edge introduit également des défis en matière de mise à jour des modèles. Des stratégies de mise à jour "over-the-air" (OTA) doivent être mises en place pour déployer de nouvelles versions des modèles sur une flotte d'appareils distribués.
