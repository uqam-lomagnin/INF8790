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
# 09 - Heuristiques, stratégies de parcours et planification

Algorithmes de recherche : BFS, DFS, A*, et applications comme la planification (séquentielle) et la résolution de puzzles. Processus de décision markoviens (MDP).

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A - Algorithmes de parcours : BFS et DFS.

Écrire les codes Python permettant de parcourir un arbre en largeur (BFS) et en profondeur (DFS).

En partant de l'arbre suivant :

```
        A
      / | \
     B  C  D
    / \     |
   E   F    G
```

Représentation de ce graphe sous forme de dictionnaire d'adjacence
```Python
graphe = {
    'A': ['B', 'C', 'D'],
    'B': ['E', 'F'],
    'C': [],
    'D': ['G'],
    'E': [],
    'F': [],
    'G': []
}
```

- `bfs(graphe, 'A')` doit retourner `['A', 'B', 'C', 'D', 'E', 'F', 'G']`.
- `dfs(graphe, 'A')` doit retourner `['A', 'B', 'E', 'F', 'C', 'D', 'G']`.


---
<details>
  <summary>Solution complète</summary>
  <a href="https://colab.research.google.com/drive/1Vag8CpZVeZaI_N324OmH9Jynz1-4NETy?usp=sharing">inf8790_bfs_dfs.ipynb</a>
</details>

## B - Parcours de labyrinthe

### Parcours en force avec recherche en largeur

Considérez une grille représentant un labyrinthe où chaque cellule peut être libre (0) ou contenir un obstacle (1). Par exemple :

```Python
maze = [
    [0,0,1,0],
    [0,1,0,0],
    [0,0,0,1],
    [0,0,0,0]
]
```

- Représenter cette grille et implémenter une recherche en largeur (BFS) permettant de trouver le chemin le plus court entre un point de départ (0,0) et un point d’arrivée (3,3).
- Résultat attendu : `[(0, 0), (1, 0), (2, 0), (2, 1), (2, 2), (3, 2), (3, 3)]`

### Algorithme de décision markovien (MDP)

Considérons à nouveau la grille (maze) suivante :
```Python
maze = [
    [0,0,1,0],
    [0,1,0,0],
    [0,0,0,1],
    [0,0,0,0]
]
```
Chaque cellule libre (0) représente un état où l’agent peut se déplacer. Chaque déplacement vers une cellule voisine libre engendre un coût immédiat de -1 (on cherche donc le chemin le plus court, donc à minimiser le coût total accumulé).

En appliquant l’algorithme d’itération des valeurs (Value Iteration) propre aux MDP :
1.	Calculer la politique optimale indiquant le meilleur mouvement à effectuer à partir de chaque cellule pour atteindre le but (3,3).
2.	Visualiser clairement les valeurs estimées à chaque itération, jusqu’à convergence.

Résultat attendu (politique finale) :
```
 v  |  <  |  ■  |  v 
 v  |  ■  |  v  |  < 
 >  |  >  |  v  |  ■ 
 >  |  >  |  >  |  G 
```
Explication des symboles de la politique finale :
-	\> : Aller à droite
-	< : Aller à gauche
-	^ : Aller en haut
-	v : Aller en bas
-	G : Objectif final
-	■ : Obstacle

<details>
  <summary>Solution complète</summary>
    La solution complète sera révélée samedi soir prochain.
<div style="display: none;">
  <a href="https://colab.research.google.com/drive/1vinu7VcqrKGSLUusIUMcL6eLyIh8Odds?usp=sharing">inf8790_labyrinthe.ipynb</a>
</div>
</details>

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025