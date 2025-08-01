# MiniGrid - Apprentissage par Renforcement avec PPO

Ce projet implémente un agent d'apprentissage par renforcement utilisant l'algorithme Proximal Policy Optimization (PPO) pour résoudre des environnements MiniGrid. L'agent est entraîné à naviguer dans différents types de grilles pour atteindre un objectif, en utilisant une architecture de réseau de neurones convolutifs.

![MiniGrid Environment](https://minigrid.farama.org/_images/empty-env.png)

## Aperçu du Projet

Ce projet vise à démontrer l'application de l'apprentissage par renforcement profond à des tâches de navigation dans des environnements discrets. Deux environnements sont implémentés :

1. **MiniGrid-Empty-8x8-v0** : Un environnement simple où l'agent doit naviguer dans une grille vide pour atteindre un objectif.
2. **MiniGrid-DoorKey-8x8-v0** : Un environnement plus complexe où l'agent doit trouver une clé, la ramasser, puis l'utiliser pour ouvrir une porte avant d'atteindre l'objectif.

L'agent utilise une architecture de réseau de neurones avec un modèle Actor-Critic pour apprendre une politique optimale à travers l'interaction avec l'environnement.

## Installation

### Prérequis

- Python 3.10
- pip (gestionnaire de paquets Python)

### Étapes d'installation

1. Clonez ce dépôt :
   ```bash
   git clone https://github.com/squadronxfr/mini_grid.git
   cd mini_grid
   ```

2. Créez un environnement virtuel (recommandé) :
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Sur Windows : .venv\Scripts\activate
   ```

3. Installez les dépendances :
   ```bash
   pip install -r requirements.txt
   ```

## Utilisation

### Exécution des notebooks

Le projet contient deux notebooks Jupyter principaux :

1. **mini_grid.ipynb** : Entraîne un agent dans l'environnement MiniGrid-Empty-8x8-v0.
2. **mini_grid_door_key.ipynb** : Entraîne un agent dans l'environnement MiniGrid-DoorKey-8x8-v0 plus complexe.

Pour exécuter un notebook :
```bash
jupyter notebook mini_grid.ipynb
```
ou
```bash
jupyter notebook mini_grid_door_key.ipynb
```

### Visualisation des résultats avec TensorBoard

Les métriques d'entraînement sont enregistrées pour être visualisées avec TensorBoard :
```bash
tensorboard --logdir logs/mini_grid
```

## Environnements

### MiniGrid-Empty-8x8-v0

Un environnement simple composé d'une grille vide de taille 8x8. L'agent (triangle rouge) doit naviguer jusqu'à l'objectif (carré vert) en utilisant les actions suivantes :
- Tourner à gauche
- Tourner à droite
- Avancer

### MiniGrid-DoorKey-8x8-v0

Un environnement plus complexe où l'agent doit :
1. Trouver une clé dans la grille
2. Ramasser la clé
3. Naviguer jusqu'à une porte verrouillée
4. Utiliser la clé pour déverrouiller la porte
5. Traverser la porte pour atteindre l'objectif

Cet environnement nécessite une planification à plus long terme et une meilleure exploration.

## Implémentation

### Architecture du modèle

L'agent utilise une architecture de réseau de neurones Actor-Critic avec :
- **Entrée** : Images RGB partielles de l'environnement (vue de l'agent)
- **Couches convolutives** : Extraction des caractéristiques spatiales
- **Couches denses** : Traitement des caractéristiques extraites
- **Deux têtes de sortie** :
  - **Acteur (Policy)** : Prédit la distribution de probabilité sur les actions
  - **Critique (Value)** : Estime la valeur de l'état actuel

### Algorithme PPO

L'implémentation utilise l'algorithme Proximal Policy Optimization (PPO) qui offre :
- Stabilité d'entraînement grâce au clipping des ratios de probabilité
- Estimation d'avantage généralisée (GAE) pour réduire la variance
- Optimisation par mini-batch
- Régularisation par entropie pour encourager l'exploration

### Stratégie d'exploration

Pour assurer une exploration efficace, l'agent utilise :
- Une stratégie epsilon-greedy avec décroissance progressive
- Une régularisation par entropie dans la fonction de perte
- Une normalisation des avantages pour stabiliser l'apprentissage

## Résultats

Les performances de l'agent sont évaluées par :
- Le taux de succès (pourcentage d'épisodes où l'agent atteint l'objectif)
- La récompense moyenne par épisode
- Le nombre moyen de pas nécessaires pour atteindre l'objectif

Les modèles entraînés sont sauvegardés dans les fichiers :
- `ppo_minigrid_model.weights.h5` pour l'environnement Empty
- `ppo_minigrid_doorkey_model.weights.h5` pour l'environnement DoorKey

## Dépannage

### Problème de récompenses nulles

Si l'agent reçoit constamment des récompenses nulles (0.000) et n'apprend pas :
1. Vérifiez que l'exploration epsilon-greedy est correctement implémentée
2. Assurez-vous que la normalisation des avantages gère correctement le cas où tous les retours sont identiques
3. Augmentez le coefficient d'entropie pour encourager plus d'exploration

## Améliorations futures

- Implémentation d'autres algorithmes (A2C, SAC, etc.) pour comparaison
- Support pour des environnements MiniGrid plus complexes
- Visualisation améliorée du comportement de l'agent
- Parallélisation de l'entraînement pour accélérer l'apprentissage
- Implémentation de techniques de curriculum learning

## Licence

Ce projet est sous licence MIT. Voir le fichier LICENSE pour plus de détails.
