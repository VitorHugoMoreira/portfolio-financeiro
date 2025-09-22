# portfolio-financeiro

# Pipeline de Concessões de Crédito no Azure

## Descrição do Projeto
Este projeto implementa um **pipeline completo de dados** utilizando **Azure Data Factory** e **Azure Databricks** para ingestão, processamento e análise de dados de concessões de crédito.  

O objetivo foi simular um cenário real de análise bancária, utilizando dados públicos do **Banco Central do Brasil**, garantindo confiabilidade, governança e escalabilidade na arquitetura.  

A solução envolveu:
- **Ingestão** de dados de ambientes **on-premises** e nuvem com o **Azure Data Factory**.  
- **Processamento distribuído** com **PySpark** no **Databricks**, armazenando dados em **Delta Lake**.  
- **Análises SQL** e exploração de insights diretamente no Databricks.  
- **Versionamento de notebooks** e integração com **DevOps**, assegurando boas práticas de governança.  

---

## Tecnologias e Ferramentas
- **Azure Data Factory** → Orquestração e ingestão de dados  
- **Azure Databricks** → Processamento e análise distribuída  
- **Delta Lake** → Armazenamento confiável e escalável  
- **PySpark / Spark SQL** → Transformação de dados  
- **Azure Data Lake Storage Gen2** → Data Lake em nuvem  
- **Integration Runtime** → Conexão com dados on-premises  
- **Azure DevOps + GitHub** → Versionamento e automação  

---

## Dataset Utilizado
Utilizei o conjunto **“Concessões de crédito - Total”** do Banco Central do Brasil, que contém dados sobre operações de crédito contratadas no Sistema Financeiro Nacional.  

📎 Link: [Concessões de Crédito - Banco Central](https://dadosabertos.bcb.gov.br/dataset/20631-concessoes-de-credito---total)

Formato: **CSV**

---

## Pipeline do Projeto

### 1 - Ingestão com Azure Data Factory
- **Linked Services**: conexão com SQL Server on-premises e Azure Blob Storage  
- **Datasets**: representando tabelas e arquivos de saída  
- **Pipelines**: movimentação dos dados para o Data Lake (camadas *raw/bronze*)  

### 2 - Processamento com Azure Databricks
```python
# Leitura dos dados raw
df = spark.read.csv("/mnt/datalake/raw/concessoes_credito.csv", header=True, inferSchema=True)

# Limpeza e transformação
from pyspark.sql.functions import col, to_date, date_format
df_clean = df.withColumn("Data", to_date(col("Data"), "yyyy-MM-dd"))
df_clean = df_clean.withColumn("AnoMes", date_format(col("Data"), "yyyy-MM"))

# Armazenamento em Delta Lake
df_clean.write.format("delta").mode("overwrite").save("/mnt/datalake/bronze/concessoes_credito_delta")
````

### 3 - Análise com SQL no Databricks

```sql
-- Registrar tabela Delta
CREATE TABLE concessoes_credito
USING DELTA
LOCATION '/mnt/datalake/bronze/concessoes_credito_delta';

-- Volume de crédito concedido por mês
SELECT AnoMes, SUM(Valor) AS TotalConcedido
FROM concessoes_credito
GROUP BY AnoMes
ORDER BY AnoMes;
```

### 4 - Visualização

* Gráficos no Databricks para evolução do crédito concedido ao longo do tempo
* Insights extraídos de consultas SQL e análises interativas

---

## Insights Obtidos

* **Sazonalidade**: padrões mensais nas concessões de crédito
* **Impacto Econômico**: influência de eventos macroeconômicos nas concessões
* **Eficiência Operacional**: ganho de escalabilidade e governança com Delta Lake

---

## Possibilidades Futuras

* **Integração com Power BI** → dashboards interativos
* **Automação de Jobs** no Databricks → execução periódica
* **Machine Learning** → uso do MLflow para análise de risco de crédito

---

## Estrutura do Projeto

```
azure-pipeline-concessoes-credito/
│── README.md
│
├── notebooks/
│   ├── 01_ingestao_raw.py
│   ├── 02_transformacao_bronze.py
│   └── 03_analise_credito.sql
│
├── pipelines_adf/
│   ├── linked_services.json
│   ├── datasets.json
│   └── pipeline_movimentacao.json
│
└── data/
    ├── raw/       # CSV original
    └── bronze/    # Dados tratados em Delta Lake
```

---

## Links

* [Portal de Dados Abertos do Banco Central](https://dadosabertos.bcb.gov.br/)
* [Documentação Azure Data Factory](https://learn.microsoft.com/azure/data-factory/)
* [Documentação Azure Databricks](https://learn.microsoft.com/azure/databricks/)

---

**Resumo:** Este projeto mostra a construção de um pipeline moderno de dados no Azure, cobrindo **ingestão, processamento, análise, versionamento e governança**.
```
