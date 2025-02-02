<script type="text/javascript" async
  src="https://polyfill.io/v3/polyfill.min.js?features=es6">
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<img style="float: right;" src="../../images/image_inf8790.png" alt="image_inf8790" width="250"/>

## INF8790 - Fondements de l'IA
# 05 - Raisonnement sous incertitude

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A : Coin Flips (Discret)
1.	On génère deux suites de lancers de pièce indépendants. Ces pièces étant pipées, la première à 80 % de chances d'être pile, alors que la seconde n'a que 30 % d'être pile.
1.	On compare la probabilité jointe $$P(\text{coin1=Pile et coin2=Pile})$$ avec le produit $$P(\text{coin1=Pile}) \times P(\text{coin2=Pile})$$.
1.	On illustre ainsi le concept de probabilité conditionnelle, d’indépendance et de vérification empirique.

```python
import numpy as np

# 1. Simuler un lancer de pièce équilibré
#    H = 1 (Pile), T = 0 (Face)
N = 10000  # nombre de simulations
coin1 = np.random.choice([0, 1], size=N, p=[0.5, 0.5])

# 2. Calculer les probabilités empiriques
#    P(coin1 = 1) = ?
p_coin1 = np.mean(coin1 == 1)
```

## B : Théorème de Bayes (Exemple médical)
1.	On définit un petit scénario de dépistage d’une maladie.
1.	On applique la formule de Bayes pour calculer la probabilité d’être réellement malade sachant que le test est positif.
1.	Cet exemple montre comment mettre à jour une croyance (la probabilité d’être malade) après avoir observé une évidence (test positif).

```python
# On définit :
#  - p_disease = prévalence de la maladie dans la population
#  - p_positive_if_disease = sensibilité (probabilité qu'un test soit positif si la personne est malade)
#  - p_positive_if_healthy = taux de faux positifs (probabilité qu'un test soit positif si la personne est saine)

p_disease = 0.01              # 1% de la population est malade
p_positive_if_disease = 0.95  # 95% de chances d'avoir un test positif si malade
p_positive_if_healthy = 0.05  # 5% de chances d'avoir un test positif si sain

.../...
```

## C : théorème de Bayes _vs_ méthode de Monte-Carlo

Vous devez écrier un code Python illustrant le théorème de Bayes (reprenant l'exemple médical précédent) via une [simulation de type Monte-Carlo](https://fr.wikipedia.org/wiki/Méthode_de_Monte-Carlo). 

L’idée est de générer artificiellement une population, d’y appliquer le taux de maladie et de tester chaque individu avec des probabilités de vrais positifs / faux positifs, afin de mesurer empiriquement $$P(maladie \mid positif)$$.

1.	**Paramètres**
-	_p_disease_ = 0.01 : la maladie touche 1 % de la population.
-	_p_positive_if_disease_ = 0.95 : 95 % de vrais positifs si la personne est malade.
-	_p_positive_if_healthy_ = 0.05 : 5 % de faux positifs si la personne est saine.
2.	**Génération de la population**
-	On crée un tableau booléen _population_maladie_ de taille N,
    - True pour « malade »,
    -	False pour « sain ».
-	Le taux de malades est environ 1 % (on compare np.random.rand() à _p_disease_).

```python
# Population simulée
N = 1_000_000  # taille de la population simulée
np.random.seed(42)  # pour la reproductibilité (optionnel)

# 1. Générer qui est malade / qui est sain
#    True = malade, False = sain
population_maladie = np.random.rand(N) < p_disease
```

3.	**Simuler le résultat du test**
- Pour chaque individu, on tire un nombre aléatoire.
- Si la personne est malade, la proba de test positif est _p_positive_if_disease_.
- Sinon, c’est _p_positive_if_healthy_.

```python
# Simuler le résultat du test pour chaque individu
#    - si l'individu est malade, proba de test positif = p_positive_if_disease
#    - si l'individu est sain, proba de test positif = p_positive_if_healthy
test_result = np.zeros(N, dtype=bool)
for i in range(N):
  .../...
````

4.	**Calculer les probabilités empiriques**
- _p_test_positif_empirique_ : fraction d’individus testés positifs parmi les N.
- _p_maladie_given_test_positif_ : proportion de malades parmi ceux testés positifs.
5.	**Comparaison avec la théorie**
- Formule de Bayes (analytique) :

$$P(maladie | test+) = [P(positif | maladie) × P(maladie)] / P(positif)$$
- On compare le résultat simulé à la formule théorique pour voir si ça concorde (en général, la loi des grands nombres fait que la simulation se rapproche du calcul analytique).

## D : Distribution Continue (Loi Normale)
On montre que les variables aléatoires peuvent être à la fois discrètes (parties A et B) et continues (Partie C).

1.	En échantillonnant une loi normale, on visualise la distribution avec un histogramme.
1.	On constate que la moyenne empirique et l’écart-type empirique se rapprochent des valeurs théoriques (0 et 1), illustrant la notion de grands nombres et la représentation de l’incertitude par une densité.

```python
import matplotlib.pyplot as plt

# 1. Générer des données selon une distribution normale
#    Moyenne = 0, Ecart-type = 1, 10 000 échantillons
data = np.random.normal(loc=0.0, scale=1.0, size=10000)

.../...
```

<details>
  <summary>Solution(s)</summary>
  <a href="https://colab.research.google.com/drive/1l6bG9qHchT7VsZ_9uocNJzlwzxjAc20p?usp=sharing">inf8790_probabilites.ipynb</a>
</details>


## Pour aller (bien) plus loin :
- [5. Bayesian Statistics.ipynb](https://colab.research.google.com/github/minireference/scipy2015_tutorial/blob/master/notebooks/5.%20Bayesian%20Statistics.ipynb)
- [Text Classification using Naive Bayes Algorithm for ML in Google Colab](https://medium.com/@sarakarim/text-classification-using-naive-bayes-algorithm-for-ml-in-google-colab-eea17a68c2d7)


--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025