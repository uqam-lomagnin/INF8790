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
# 10 - IA générative

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A - Diffusion, simple exemple 1D : Bruiter puis débruiter un signal

Exemple de code (très simplifié) d’un mini-processus de diffusion (Ce code illustre un principe conceptuel, pas un modèle complet.)

```Python
import numpy as np
import matplotlib.pyplot as plt

def forward_diffusion(x0, timesteps=5, noise_scale=0.1):
    """Ajoute du bruit à x0 itérativement."""
    x = x0
    noisy_samples = [x0]
    for t in range(timesteps):
        noise = np.random.normal(0, noise_scale, size=x.shape)
        x = x + noise  # On simule un "bruitage" simple
        noisy_samples.append(x)
    return noisy_samples

def simple_denoise(x_noisy, noise_scale=0.1):
    """Débruitage simple (heuristique)."""
    # On suppose qu'on estimate le bruit comme (x_noisy - median)
    median_val = np.median(x_noisy)
    return x_noisy - (x_noisy - median_val)*0.5

# Signal initial
x0 = np.linspace(-1, 1, 50) + 0.2*np.sin(10*np.linspace(-1,1,50))

# Bruitage progressif
noisy_list = forward_diffusion(x0, timesteps=5, noise_scale=0.05)

# Débruitage (processus inverse grossier)
x_recovered = simple_denoise(noisy_list[-1])

plt.plot(x0, label="Signal original")
plt.plot(noisy_list[-1], label="Signal bruité (étape finale)")
plt.plot(x_recovered, label="Signal débruité (heuristique)")
plt.legend()
plt.show()
```

### A1 - Reproduire et exécuter ce code dans un notebook

:warning: Veuillez noter que :
-	Ici, on n’a pas de réseau ni de véritable apprentissage : c’est une démonstration du concept “bruit → débruitage”.
-	Dans un vrai modèle de diffusion, on apprend la fonction de débruitage de façon paramétrique (réseau de neurones).
-	En pratique, on utilise des formules précises (DDPM, score matching).

### A2 - Compléter ce code avec un module d'apprentissage 

Pour cela :
-	Création du jeu de données :
Pour entraîner le réseau, on génère 1000 exemples en appliquant la fonction forward_diffusion à notre signal de base. Chaque exemple bruité (entrée) est associé au signal original (cible).
-	Réseau de neurones :
La classe DenoiseNet (à implémenter) définit une MLP simple avec une couche cachée de 64 neurones et une fonction d’activation ReLU. Le réseau prend en entrée un vecteur de taille 50 et retourne un vecteur de la même taille.
-	Boucle d’apprentissage :
On utilise la MSE (erreur quadratique moyenne) comme fonction de perte et l’optimiseur Adam. La boucle d’apprentissage s’exécute sur 300 époques, et le suivi de la perte est affiché tous les 50 epochs.
-	Évaluation et comparaison :
Après entraînement, un nouveau signal bruité est généré. On compare ensuite le débruitage heuristique et celui obtenu par le réseau en les affichant aux côtés du signal original.

---
<details>
  <summary>Solution complète</summary>
  <a href="https://colab.research.google.com/drive/1cwMLG6HOgWoVCGnVWAUqeBVx84kJCnT3?usp=sharing">inf8790_diffusion.ipynb</a>
</details>

## B - Stable Diffusion XL

Pour aller plus loin, vous pouvez étudier et reproduire : [‍Introducing Stable Diffusion XL (SDXL): the future of AI-driven art](https://www.ikomia.ai/blog/stable-diffusion-xl-sdxl-model)

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025