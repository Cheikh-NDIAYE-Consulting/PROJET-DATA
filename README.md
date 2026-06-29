# PROJET-DATA
# PySpark AI Data Engineer Lakehouse RAG

Projet portfolio orienté **AI Data Engineer / Data Engineer GenAI / Data Platform IA**.

Ce projet part d'un socle PySpark Lakehouse classique, puis ajoute les briques qui permettent de se positionner sur les nouveaux métiers IA : **RAG, feature store, vector store, API d'assistant métier, guardrails, MLflow et orchestration DataOps**.

## Objectif métier

Une enseigne retail omnicanale veut fiabiliser ses données et les rendre exploitables par :

- les équipes BI pour les KPI ;
- les Data Scientists pour des features ML ;
- les équipes IA pour un assistant RAG métier ;
- les équipes Data Platform pour industrialiser les traitements.

L'assistant IA doit répondre à des questions comme :

- Quel canal génère le plus de chiffre d'affaires ?
- Quels produits performent le mieux ?
- Quels magasins ont les meilleurs KPI ?
- Quels segments clients méritent une action commerciale ?

## Pourquoi ce projet est adapté aux métiers IA

Les offres AI Data Engineer / GenAI Data Engineer demandent rarement uniquement de l'entraînement de modèles. Elles cherchent souvent une double compétence :

- Data Engineering : PySpark, Databricks, pipelines, orchestration, qualité, cloud ;
- IA appliquée : RAG, LLM, embeddings, feature engineering, MLOps, API, monitoring.

Ce projet montre exactement ce pont.

## Architecture

```text
Sources CSV / JSON
        ↓
Bronze PySpark : ingestion brute historisée
        ↓
Silver PySpark : nettoyage, typage, déduplication, qualité
        ↓
Gold PySpark : KPI métier pour BI / reporting
        ↓
+-----------------------------+-------------------------------+
| AI Feature Store            | RAG Business Corpus           |
| variables client / produit  | documents issus des Gold      |
+-----------------------------+-------------------------------+
        ↓                                      ↓
MLflow / modèle prévision CA         Embeddings / Vector Store
        ↓                                      ↓
MLOps léger                           FastAPI RAG Assistant
```

## Arborescence principale

```text
pyspark_ai_data_engineer_lakehouse_rag_cv/
├── airflow/dags/retail_lakehouse_dag.py
├── app/api.py
├── config/config.yml
├── databricks_notebooks/
│   ├── retail_lakehouse_demo.py
│   └── ai_data_engineer_databricks_demo.py
├── docs/
│   ├── architecture.md
│   ├── ai_architecture.md
│   ├── cv_snippets.md
│   └── ai_data_engineer_cv_snippets.md
├── scripts/generate_sample_data.py
├── src/jobs/
│   ├── bronze_ingest.py
│   ├── silver_transform.py
│   ├── dq_checks.py
│   ├── gold_kpis.py
│   └── streaming_orders.py
├── src/ai/
│   ├── ai_feature_store.py
│   ├── rag_corpus_builder.py
│   ├── vector_store_builder.py
│   ├── retriever.py
│   ├── prompt_builder.py
│   ├── guardrails.py
│   ├── rag_assistant.py
│   ├── ml_revenue_forecast.py
│   └── ai_monitoring.py
├── tests/
├── Dockerfile
├── docker-compose.yml
├── Makefile
├── requirements.txt
└── .github/workflows/ci.yml
```


## Données sources incluses

Le ZIP contient déjà des données sources synthétiques dans `data/raw/` :

- `customers.csv` ;
- `stores.csv` ;
- `products.csv` ;
- `orders.csv` ;
- `order_items.csv`.

Elles permettent de lancer le projet immédiatement. Le script `scripts/generate_sample_data.py` permet aussi de régénérer une volumétrie différente.

## Installation locale

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# ou .venv\Scripts\activate sur Windows PowerShell
pip install -r requirements.txt
```

## Lancer le pipeline Lakehouse classique

```bash
make pipeline
```

Cela exécute :

1. génération des données ;
2. ingestion Bronze ;
3. transformation Silver ;
4. contrôles qualité ;
5. agrégats Gold.

## Lancer la partie AI Data Engineer

```bash
make ai-pipeline
```

Cela ajoute :

1. feature store IA ;
2. génération du corpus RAG depuis les tables Gold ;
3. création du vector store ;
4. test d'une question RAG en mode offline.

## Tester l'assistant RAG

```bash
python -m src.ai.rag_assistant --question "Quel canal génère le plus de chiffre d'affaires ?"
```

## Lancer l'API FastAPI

```bash
make api
```

Puis :

```bash
curl -X POST http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{"question":"Quels produits performent le mieux ?"}'
```

## Lancer l'exemple MLOps MLflow

```bash
make ml
```

Cet exemple entraîne un modèle simple de prévision du chiffre d'affaires quotidien et trace l'expérience avec MLflow.

## Ce que le projet démontre sur un CV

- PySpark Data Engineering sur architecture Bronze/Silver/Gold ;
- préparation de données fiables pour BI, ML et IA générative ;
- construction de corpus RAG depuis des tables contrôlées ;
- vectorisation de documents et recherche sémantique ;
- API d'assistant métier ;
- feature engineering pour ML/scoring ;
- guardrails basiques : PII masking, prompt injection detection ;
- orchestration Airflow ;
- tests automatisés et CI/CD ;
- adaptation Databricks / Delta / MLflow / Vector Search.

## Positionnement recommandé

Ce projet doit être présenté comme :

> Projet AI Data Engineering montrant comment industrialiser une chaîne PySpark Lakehouse pour alimenter des cas d'usage BI, ML et GenAI/RAG.

Ne pas le présenter comme :

> Projet d'entraînement de LLM.

La valeur est dans la préparation, la fiabilisation et l'exposition des données pour l'IA.
