<img style="float: right;" src="../../images/image_inf8790.png" alt="image_inf8790" width="250"/>

## INF8790 - Fondements de l'IA
# 04 - Représentation des connaissances

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A - Graphes génériques

### Exercice : Relations de descendance avec NetworkX

- Utiliser [NetworkX](https://networkx.org) pour modéliser les relations suivantes :

```script
Jean est parent de Marie.
Marie est parent de Suzanne.
Suzanne est parent de Thomas.
```

- Trouver les descendants de Jean
- Déterminer les ancêtres ultimes (ie les nœuds qui n'ont pas eux-même d'arcs entrants)

Vous pouvez vous inspirer du notebook suivant : [NetworkX.ipynb](https://colab.research.google.com/github/jdwittenauer/ipython-notebooks/blob/master/notebooks/libraries/NetworkX.ipynb).

<details>
  <summary>Solution</summary>
  <a href="https://colab.research.google.com/drive/1ljCIJi2IbDJjWm71vrSMKjFuKZ793X6C?usp=sharing">inf8790_networkx.ipynb</a>
</details>

## B Graphes de connaissances

### Exercice RDF & SPARQL

En vous inspirant du notebook [Sparql.ipynb](https://colab.research.google.com/github/joerg84/Graph_Powered_ML_Workshop/blob/master/Sparql.ipynb), modélisez les relations suivantes :

```script
Jean est parent de Marie.
Marie est parent de Suzanne.
Suzanne est parent de Thomas.
```

- Trouver les enfants de Jean
- Trouver les descendants de Jean
- Déterminer les ancêtres ultimes (ie les nœuds qui n'ont pas eux-même d'arcs entrants)

<details>
  <summary>Solution</summary>
  <a href="https://colab.research.google.com/drive/1XCL3cgRhqS2DDAwsfQkVJgrZE58SkPdV?usp=sharing">inf8790_sparql.ipynb</a>
</details>

### Exercice avec Cypher

En vous inspirant du notebook [Cypher Intro.ipynb](https://colab.research.google.com/drive/1zgTCEOFdskYRQ45COYRww7sA6fTXE66S?usp=sharing), modélisez les relations suivantes :

```script
Jean est parent de Marie.
Marie est parent de Suzanne.
Suzanne est parent de Thomas.
```

- Lister l'ensemble des personnes
- Lister toutes les relation Parent / Enfant
- Trouver les enfants de Jean
- Trouver les descendants de Jean
- Déterminer les ancêtres ultimes (ie les nœuds qui n'ont pas eux-même d'arcs entrants)

<details>
  <summary>Solution</summary>
  <a href="https://colab.research.google.com/drive/1qNeiZmQd9k9MtiBBePj1vZNkP6pwxUTZ?usp=sharing">inf8790_cypher.ipynb</a>
</details>

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025