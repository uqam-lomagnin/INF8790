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
# 07 - Réseaux de neurones et apprentissage profond

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A - Chien ou Chat ? Les réseaux neuronaux sont là pour vous répondre !

Le but de l'exercice est d'entraîner un réseau de neurones à reconnaître les chats des chiens...

Pour cela, nous allons nous baser sur les images fournies par Microsoft : [Kaggle Cats and Dogs Dataset](https://www.microsoft.com/en-us/download/details.aspx?id=54765).

L'exercice peut se faire en suivant le tutoriel suivant : [Image classification from scratch](https://keras.io/examples/vision/image_classification_from_scratch/).

<img style="float: right;" align="right" src="gpu_t4.png" alt="gpu" width="250"/>

:warning: Vous devez utiliser des GPUs (en lieu et place de CPUs).

:bulb: Cet exercice fonctionne en utilisant JAX en backend (voir [Getting started with Keras](https://keras.io/getting_started/))

:bulb: Implémentez l'option 1 (afin de bénéficier de l'accélération GPU) pour ce qui concerne la _data_augmentation_.

Comme vous pouvez le voir ci dessous, la mémoire du GPU est utilisée presque à son maximum :
<img src="ram_gpu.png" alt="ram_gpu" width="500"/>

:bulb: Pour éviter un temps d'apprentissage trop long (ce qui peut amener à perdre la session du _notebook_), réduire le nombre d'_epochs_ (choisir p. ex. 10).

![alt text](<25 epochs.png>)
---

<details>
  <summary>Solution complète</summary>
    La solution complète sera révélée samedi soir prochain.
<div style="display: none;">
  <a href="https://colab.research.google.com/drive/1qcoX7BjD_YAFMs84DNhRybohudy1Y_lz?usp=sharing">inf8790_keyras.ipynb</a>
</div>
</details>

## B - 

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025