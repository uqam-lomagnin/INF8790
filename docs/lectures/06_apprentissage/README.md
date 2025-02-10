<script type="text/javascript" async
  src="https://polyfill.io/v3/polyfill.min.js?features=es6">
</script>
<script type="text/javascript" async>
  window.MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],  // Enables single $ for inline math
      displayMath: [['$$', '$$'], ['\\[', '\\]']]
    },
    svg: {
      fontCache: 'global'
    }
  };
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/3.2.2/es5/tex-mml-chtml.js">
</script>

<img style="float: right;" src="../../images/image_inf8790.png" alt="image_inf8790" width="250"/>

## INF8790 - Fondements de l'IA
# 06 - Apprentissage machine

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A - Classification automatique (AutoML) avec FLAML

Dans cet exercice, vous allez :

1. Charger le jeu de données Iris et le visualiser.
2. Séparer les données en ensembles d’entraînement et de test.
3. Utiliser la librairie **[FLAML](https://microsoft.github.io/FLAML/)** pour automatiser la sélection du modèle et l’optimisation des hyperparamètres (AutoML).
4. Évaluer les performances du modèle sur le jeu de test.

---

### 1. Installation et importation des librairies

#### 1.1 Installation

Si vous utilisez Google Colab, vous devrez installer la bibliothèque **FLAML**.  
Exécutez la commande ci-dessous dans une cellule Colab :

```python
!pip install flaml dask[dataframe]
```

*(Ignorer cette étape si vous avez déjà installé FLAML sur votre environnement.)*

#### 1.2 Imports

Créez une cellule pour importer les librairies suivantes :

- `pandas` et `numpy` pour la manipulation des données.  
- `sklearn.datasets` pour charger le jeu de données Iris.  
- `sklearn.model_selection` pour séparer les données en sets d’entraînement et de test.  
- `sklearn.metrics` pour évaluer la performance du modèle.  
- `matplotlib.pyplot` et `seaborn` pour la visualisation.  
- `flaml.AutoML` pour l’entraînement AutoML.

```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, confusion_matrix
import seaborn as sns
import matplotlib.pyplot as plt

# Pour désactiver les avertissements
import warnings
warnings.filterwarnings("ignore")

from flaml import AutoML
```

---

### 2. Chargement et préparation des données

1. Chargez le jeu de données Iris grâce à la fonction `load_iris()`.
2. Affichez quelques exemples (par exemple, les 5 premières lignes) pour comprendre sa structure.

**Exemple** :

```python
# Charger le dataset
iris = load_iris()
X = iris.data
y = iris.target

# Transformer en DataFrame pour un affichage plus lisible
df_iris = pd.DataFrame(X, columns=iris.feature_names)
df_iris["target"] = y

# Afficher les 5 premières lignes
df_iris.head()
```

---

### 3. Séparation entraînement / test

1. Séparez les données en deux ensembles : entraînement (train) et test.  
2. Utilisez un ratio de 80 % pour l’entraînement et 20 % pour le test (paramètre `test_size=0.2`).  
3. Fixez le `random_state` pour la reproductibilité.

**Exemple** :

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, 
    y, 
    test_size=0.2, 
    random_state=42
)
```

---

### 4. Configuration et entraînement du modèle AutoML (FLAML)

1. Instanciez un objet `AutoML()`.  
2. Créez un dictionnaire `settings` pour configurer le temps maximal d’optimisation, la métrique à optimiser (par exemple `accuracy`), la tâche (classification), le niveau de verbosité, etc.  
3. Lancez l’entraînement avec la méthode `fit(...)` en lui passant `X_train, y_train` et les paramètres définis.

**Exemple** :

```python
automl = AutoML()

settings = {
    "time_budget": 60,        # temps max, en secondes
    "metric": "accuracy",     # métrique à optimiser
    "task": "classification",
    "log_file_name": None,    # pour éviter la création d'un fichier de log
    "log_type": "none",       # pour éviter l'affichage dans la console
    "verbose": 0,             # niveau de verbosité minimal
    "seed": 42
}

# Entraînement
automl.fit(X_train, y_train, **settings)
```

---

### 5. Évaluation du meilleur modèle

1. Récupérez le meilleur modèle, sa configuration et sa performance sur la validation interne.  
2. Générez des prédictions sur l’ensemble de test (`X_test`).  
3. Calculez l’accuracy ou d’autres métriques.  
4. Affichez la matrice de confusion pour mieux visualiser les erreurs de classification.

**Exemple** :

```python
print("Meilleur estimateur :", automl.best_estimator)
print("Meilleure configuration :", automl.best_config)
print("Meilleur score validé :", automl.best_loss)

# Prédictions
y_pred = automl.predict(X_test)

# Calcul de la performance
accuracy = accuracy_score(y_test, y_pred)
print("Accuracy sur le jeu de test :", accuracy)

# Matrice de confusion
cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(6,4))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=iris.target_names,
            yticklabels=iris.target_names)
plt.title("Matrice de confusion")
plt.xlabel("Prédictions")
plt.ylabel("Vérités terrain")
plt.show()
```

---

### 6. Analyse et questions

1. Quel algorithme FLAML a-t-il retenu en tant que meilleur estimateur ?  
2. Quel est le score obtenu sur le jeu de test ?  
3. Pouvez-vous proposer d’autres pistes pour améliorer les performances (ex : changer la métrique, augmenter le temps d’entraînement, etc.) ?

---

### 7. Pour aller plus loin (idées d'exploration)

- Tester d’autres jeux de données (Wine, Breast Cancer, etc.).  
- Comparer la performance de FLAML à d’autres outils d’AutoML ou à un modèle choisi et réglé « à la main ».  
- Visualiser l’impact de la variation de `time_budget`.   
- S'inspirer de [05.02-Introducing-Scikit-Learn.ipynb](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/05.02-Introducing-Scikit-Learn.ipynb)


<details>
  <summary>Solution complète</summary>
  <a href="https://colab.research.google.com/drive/1_sjzMIYaKkesqoFYxsFAwEp6wMNRmcus?usp=sharing">inf8790_AutoML.ipynb</a>
</details>

## B - Clustering et PCA sur le jeu de données Iris

### Objectif
L'objectif de cet exercice est de vous familiariser avec l'apprentissage non supervisé en appliquant l'algorithme **[K-Means](https://fr.wikipedia.org/wiki/K-moyennes)** sur le jeu de données **Iris**. Vous apprendrez également à visualiser les résultats en réduisant la dimension des données avec **PCA**.

---

### Instructions

#### 1. Charger et explorer les données
- Importer `load_iris` depuis `sklearn.datasets`
- Convertir les données en DataFrame Pandas
- Vérifier les valeurs manquantes et visualiser un aperçu des données

#### 2. Prétraitement des données
- Normaliser les données avec `StandardScaler`

#### 3. Appliquer l'algorithme K-Means
- Tester plusieurs valeurs de `k` (nombre de clusters) ;
- Utiliser la **méthode du coude** pour choisir la meilleure valeur de `k`.
 
```python
# 6. Méthode du coude pour estimer un nombre optimal de clusters
inertia_values = []
k_values = range(2, 10)
for k in k_values:
    kmeans_test = KMeans(n_clusters=k, random_state=42, n_init=10)
    kmeans_test.fit(X_scaled)
    inertia_values.append(kmeans_test.inertia_)

plt.figure(figsize=(6, 4))
plt.plot(k_values, inertia_values, marker='o')
plt.title("Méthode du coude (Elbow Method)")
plt.xlabel("Nombre de clusters k")
plt.ylabel("Inertie (Within-Cluster Sum of Squares)")
plt.show()
```

- Entraîner un modèle K-Means avec la valeur optimale de `k`
- Assigner un cluster à chaque observation

#### 4. Évaluer la qualité du clustering
- Calculer le **score de silhouette**

#### 5. Visualisation des résultats
- Appliquer une **réduction de dimension avec PCA** (2 composantes principales)
- Tracer un scatter plot des clusters trouvés
- Comparer avec les classes réelles (à titre d'analyse)

#### 6. Comparaison avec les classes réelles
- Construire une **matrice de contingence** entre les clusters et les vraies classes
- Interpréter les résultats et analyser les similitudes/différences

---

### Consignes
- Documenter chaque étape dans votre notebook
- Utiliser des visualisations adaptées (`matplotlib` et `seaborn`)
- Comparer vos résultats avec les vraies classes et expliquer les observations

---

### Solution

<details>
  <summary>Solution complète</summary>
  <a href="https://colab.research.google.com/drive/1Fh0Wh4VnReRjX8x9fKm1yMA28DiAYh-u?usp=sharing">inf8790_non_supervisé.ipynb</a>
</details>

---

### Bonus (optionnel)
- Essayer d'autres algorithmes de clustering (`DBSCAN`, `AgglomerativeClustering`)
- Tester la réduction de dimension avec `t-SNE` au lieu de PCA

## C - Apprentissage par renforcement

Le but de l'exercice est d'implémenter l'algorithme ci-dessous en Python, ceci afin d'entraîner un agent à s'échapper d'un [Frozen Lake](https://gymnasium.farama.org/environments/toy_text/frozen_lake/).

Code d'initialisation :
```python
!pip install gymnasium==1.0.0

# Import des packages
import gymnasium as gym
import numpy as np
import random

# Création de l'environnement
is_slippery=False
env = gym.make('FrozenLake-v1', is_slippery=is_slippery)  
# - is_slippery=True: le sol est glissant (difficile de contrôler les déplacements)
# - mode par défaut: 4x4
```
:bulb: Le mode non-glissant est préférable dans un premier temps pour faciliter l'apprentissage.

---

### Algorithme à traduire en Python

1. **Détermination du nombre d’états et d’actions**  
   - $n\_states = \text{env.observation\_space.n}$  
   - $n\_actions = \text{env.action\_space.n}$  

2. **Paramètres de Q-learning**  
   - $\alpha = 0.8$  (taux d’apprentissage)  
   - $\gamma = 0.95$ (facteur de discount)  
   - $\epsilon = 1.0$ (taux d’exploration initial)  
   - $\epsilon_{\min} = 0.01$ (valeur minimale pour $\epsilon$)  
   - $\epsilon_{\text{decay}} = 0.995$ (facteur de décroissance de $\epsilon$)  
   - $\text{num\_episodes} = 10000$  

3. **Initialisation de la table $Q$**  
   - $Q \leftarrow \mathbf{0}$, de dimension $(n\_states,\, n\_actions)$.  

4. **Boucle d’apprentissage**  
   - Déclarer une liste $\text{all\_rewards}$ vide.  
   - **Pour** chaque épisode $\,\text{episode} = 1 \dots \text{num\_episodes}$ :  
     1. $s, \text{info} \leftarrow \text{env.reset()}$  
     2. $\text{done} \leftarrow \text{False}$  
     3. $\text{total\_reward} \leftarrow 0$  

     4. **Tant que** $\text{done} = \text{False}$ :  
        1. **Choix de l’action** $a$ :  
           - Tirer un nombre aléatoire $u$ dans $[0,1]$.  
           - *Si* $u < \epsilon$, alors $a \leftarrow \text{action aléatoire}$.  
           - *Sinon*, $a \leftarrow \displaystyle \arg\max_{a'}\, Q[s,\, a']$.  
        2. **Effectuer l’action** $a$ :  
           $$
           s',\, r,\, \text{done},\, \text{truncated},\, \text{info} 
           \;\leftarrow\; \text{env.step}(a).
           $$
        3. **Mettre à jour** la valeur $Q$ :  
           $$
           Q[s,\, a] 
           \;\leftarrow\; Q[s,\, a] 
           \;+\; \alpha \Bigl(r 
              \;+\; \gamma\,\max_{a''}\,Q[s',\, a''] 
              \;-\; Q[s,\, a]\Bigr).
           $$
        4. $s \leftarrow s'$  
        5. $\text{total\_reward} \leftarrow \text{total\_reward} + r$  

     5. **Mettre à jour** $\epsilon$ :  
        - *Si* $\epsilon > \epsilon_{\min}$, alors  
          $$
          \epsilon \;\leftarrow\; \epsilon \;\times\; \epsilon_{\text{decay}}.
          $$

     6. **Ajouter** $\text{total\_reward}$ à la liste $\text{all\_rewards}$.  

---

### Interprétation

- À chaque **épisode**, on réinitialise l’environnement pour obtenir un état initial $s$.
- On exécute une **boucle** jusqu’à ce que $\text{done} = \text{True}$.
- On **sélectionne** l’action $a$ en suivant une stratégie $\epsilon$-gloutonne :
  - Avec probabilité $\epsilon$, on **explore** en choisissant une action aléatoire.
  - Sinon, on **exploite** la meilleure action courante selon $Q$.
- On **observe** la récompense $r$ et le nouvel état $s'$.
- On **met à jour** la valeur $Q(s,a)$ via la formule :
  $$
    Q(s,a) \leftarrow Q(s, a) + \alpha 
    \Bigl(r + \gamma \,\max_{a''}\,Q(s',a'') - Q(s,a)\Bigr).
  $$
- On **réduit** $\epsilon$ progressivement afin de diminuer l’exploration.
- On **enregistre** la récompense totale de l’épisode dans $\text{all\_rewards}$ pour analyser la progression. 

---------------

Code pour tester la politique apprise :

```python
def test_agent(env, Q, episodes=100):
    successes = 0
    steps = 0
    for _ in range(episodes):
        obs, info = env.reset()
        episode_over = False
        while not episode_over:
            action = np.argmax(Q[obs, :])  # meilleure action
            obs, reward, terminated, truncated, info = env.step(action)
            if terminated and reward == 1.0:
                successes += 1
            steps += 1

            episode_over = terminated or truncated
    return successes / episodes, steps / episodes

success_rate, steps = test_agent(env, Q, episodes=1000)
print(f"Taux de réussite de l'agent sur 1000 épisodes de test : {success_rate*100:.2f}%")
print(f"Durée moyenne d'un épisode : {int(steps)} pas")
```

<details>
  <summary>Solution complète</summary>
  <a href="https://colab.research.google.com/drive/1LFrWmbmxjVYXVteHNRcsHmF4fUNBiguK?usp=sharing">inf8790_renforcement.ipynb</a>
</details>

Pour aller plus loin : [Training an Agent](https://gymnasium.farama.org/introduction/train_agent/)

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025