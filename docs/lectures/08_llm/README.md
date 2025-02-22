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
# 08 - Langage naturel, compréhension et LLMs

:bulb: Suggestion : les exercices suivants peuvent être réalisés sur [Google Colab](https://colab.google).

## A - Utilisation de base d'un LLM en modes textuel et multi-modal.

1. Mise en place d'un serveur de LLM

    - Pour cela, nous allons utiliser [Ollama](https://ollama.com) - lequel ne fonctionne qu'en ligne de commande -. Voici comment l'installer dans l'environnement Google Colab : [Running Ollama on Google Colab (LLaMA3 model demo)](https://colab.research.google.com/github/casualcomputer/llm_google_colab/blob/main/setup_ollama_google_colab.ipynb).

    :bulb: Ollama fourni un accès à de très nombreux [modèles](https://ollama.com/search).

    :warning: Seul les modèles les plus petits peuvent être servis dans des environnement aux ressources limitées.

    :rocket: [LM Studio](https://lmstudio.ai) est équivalent à Ollama, mais à travers une interface graphique.

2. Appel à cet LLM pour répondre à des questions

    La bibliothèque choisie pour faire appel aux LLMs servis par Ollama est [LangChain](https://python.langchain.com/docs/introduction/).

    - Répliquer la première partie ("usage") des instructions de [OllamaLLM](https://python.langchain.com/docs/integrations/llms/ollama/). :bulb: Vous pouvez y ouvrir le notebook [ollama.ipynb](https://colab.research.google.com/github/langchain-ai/langchain/blob/master/docs/docs/integrations/llms/ollama.ipynb).

    :warning: Pour pouvoir avoir Ollama disponible, vous devez (re)lancer le code suivant:
    ```python
    subprocess.Popen("ollama serve", shell=True)
    ```

    - Jouez avec l'invite (prompt), par xemple pour obtenir la réponse en Français.

3. Analyse d'image

    - Reproduire la section "Multi-modal" d'[OllamaLLM](https://python.langchain.com/docs/integrations/llms/ollama/).

    :bulb: Pour récupérer en local l'image à analyser :
    ```shell
    ## Download the image using curl
    !curl -o ollama_example_img.jpg https://uqam-lomagnin.github.io/INF8790/lectures/08_llm/ollama_example_img.jpg
    ```

---
<details>
  <summary>Solution complète</summary>
  <a href="https://colab.research.google.com/drive/1yJ4ElLZI3XmFV1383a7fZRmqe7FfVNNy?usp=sharing">inf8790_ollama_langchain.ipynb</a>
</details>

## B - _Chatbot_

En partant de l'exercice précédent, construire un _chatbot_ (appel en boucle à un LLM en mode texte, avec mémorisation des échanges précédents).

- Indiquez votre nom en début de conversation, pour ensuite demander comment vous vous appelez...

:bulb: Pour analyser la trace de vos appels au LLM, vous pouvez utiliser [LangSmith](https://smith.langchain.com/settings) (site Web demandant d'être inscrit) ou bien [Phoenix by Arize](https://phoenix.arize.com) (gratuit, en mode local).

<details>
  <summary>Solution complète</summary>
    La solution complète sera révélée samedi soir prochain.
<div style="display: none;">
  <a href="">inf8790_.ipynb</a>
</div>
</details>

--------------- 

<img style="float: right;" align="right" src="../../images/uqam.png" alt="uqàm" width="100"/>

### Copyright (c)Laurent Magnin / UQÀM 2025