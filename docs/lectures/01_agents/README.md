Voici un exemple concret avec du code fonctionnel pour illustrer l’utilisation des plateformes LangGraph, AutoGen, et CrewAI avec Ollama. Ce contenu est formaté en Markdown pour une intégration dans votre cours sur GitHub.

# Agents Intelligents et Systèmes Multi-Agents avec LangGraph, AutoGen, CrewAI, et Ollama

Ce document présente des exemples concrets d’utilisation des principales plateformes d’agents intelligents, avec du code fonctionnel.

J'utilise Conda pour organiser mes environnements de librairies Python.

Pour créer un environnement Conda basé sur Python 3.12.8 nommé `inf8790`, utilisez la commande suivante :

```bash
conda create --name inf8790 python=3.12.8
conda activate inf8790
pip install 'crewai[tools]'
```

---

🛠️ Exemple 1 : CrewAI pour la collaboration humain-agent

[Build your first CrewAI Agent](https://docs.crewai.com/quickstart)

```bash
$ crewai create crew latest-ai-development


Creating folder latest_ai_development...
Cache expired or not found. Fetching provider data from the web...
Downloading  [####################################]  291265/14498
Select a provider to set up:
1. [openai]
2. anthropic
3. gemini
4. groq
5. ollama
6. watson
7. bedrock
8. azure
9. cerebras
10. other
q. Quit
Select a model to use for Openai:
1. gpt-4
2. gpt-4o
3. gpt-4o-mini
4. [o1-mini]
5. o1-preview
q. Quit
Enter the number of your choice or 'q' to quit: 4
Enter your OPENAI API key (press Enter to skip): ....
API keys and model saved to .env file
Selected model: o1-mini
  - Created latest_ai_development/.gitignore
  - Created latest_ai_development/pyproject.toml
  - Created latest_ai_development/README.md
  - Created latest_ai_development/knowledge/user_preference.txt
  - Created latest_ai_development/src/latest_ai_development/__init__.py
  - Created latest_ai_development/src/latest_ai_development/main.py
  - Created latest_ai_development/src/latest_ai_development/crew.py
  - Created latest_ai_development/src/latest_ai_development/tools/custom_tool.py
  - Created latest_ai_development/src/latest_ai_development/tools/__init__.py
  - Created latest_ai_development/src/latest_ai_development/config/agents.yaml
  - Created latest_ai_development/src/latest_ai_development/config/tasks.yaml
Crew latest-ai-development created successfully!

$ crewai install
$ crewai update
$ crewai run
```

Et voici le rapport généré avec comme paramètre `"topic": "AI LLMs"` : [CrewAI Report](latest_ai_development/report.md)

![open_ai_usage](images/open_ai_usage.png)


## 🛠️ **Exemple 2 : Utilisation de LangGraph pour orchestrer plusieurs agents**

