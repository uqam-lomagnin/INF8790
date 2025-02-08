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
- Tester plusieurs valeurs de `k` (nombre de clusters)
- Utiliser la **méthode du coude** pour choisir la meilleure valeur de `k`
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

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025