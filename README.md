# portfolio-financeiro

Perfeito 🙌
Aqui está o **README.md pronto em Markdown**, já formatado para você usar no GitHub:

---

```markdown
# 📊 Pipeline de Dados no Azure – Concessões de Crédito

## 🚀 Visão Geral
Este repositório apresenta um **pipeline completo de dados bancários** implementado no **Microsoft Azure**, combinando **Azure Data Factory (ADF)**, **Azure Databricks** e **Azure DevOps**.  
O objetivo foi construir uma solução ponta a ponta para **ingestão, processamento, versionamento e análise** de dados públicos do **Banco Central do Brasil** sobre **concessões de crédito**.

A arquitetura foi organizada em três camadas principais:  
1. **Ingestão (ADF)** → Dados SQL on-premises e em nuvem ingeridos no **Azure Data Lake** (camada raw/bronze).  
2. **Processamento (Databricks + PySpark)** → Limpeza, transformação e armazenamento confiável em **Delta Lake**.  
3. **Governança e Versionamento (DevOps + GitHub)** → Notebooks versionados, pipelines integrados ao **DevOps** e práticas de **CI/CD**.  

---

## 🛠️ Tecnologias Utilizadas
- **Azure Data Factory (ADF)** – Orquestração e ingestão de dados.  
- **Azure Databricks** – Processamento distribuído com PySpark e SQL.  
- **Delta Lake** – Armazenamento confiável e escalável.  
- **Azure Data Lake Storage Gen2** – Data lake estruturado em camadas (raw/bronze).  
- **Integration Runtime** – Conexão segura com ambientes on-premises.  
- **Azure DevOps + GitHub** – Versionamento de notebooks e pipelines, CI/CD.  
- **PySpark / Spark SQL** – Limpeza, transformação e análise.  

---

## 📂 Estrutura do Repositório
```

azure-credit-pipeline/
│
├── README.md                  # Documentação principal
│
├── notebooks/                 # Notebooks PySpark e SQL
│   ├── 01\_ingestao\_raw\.py
│   ├── 02\_transformacao\_bronze.py
│   └── 03\_analise\_credito.sql
│
├── pipelines\_adf/             # Pipelines do Data Factory (simulados em JSON)
│   ├── linked\_services.json
│   ├── datasets.json
│   └── pipeline\_movimentacao.json
│
├── devops/                    # Integração e automação (CI/CD)
│   ├── ci\_pipeline.yaml
│   ├── cd\_pipeline.yaml
│   └── git\_integration\_steps.md
│
└── data/                      # Dados de exemplo
├── raw/                   # CSV do Banco Central (entrada)
└── bronze/                # Dados tratados em Delta/Parquet

````

---

## 🔄 Pipeline do Projeto

### 1. Ingestão – **Azure Data Factory**
- Configuração de **linked services** (SQL Server on-premises + Blob Storage).  
- Criação de **datasets** (SQL + arquivos CSV).  
- Desenvolvimento de **pipelines** para mover dados para o Data Lake.  

### 2. Processamento – **Azure Databricks**
Exemplo de transformação em PySpark:
```python
from pyspark.sql.functions import col, to_date, date_format

# Leitura
df = spark.read.csv("/mnt/datalake/raw/concessoes_credito.csv", header=True, inferSchema=True)

# Transformação
df_clean = df.withColumn("Data", to_date(col("Data"), "yyyy-MM-dd")) \
             .withColumn("AnoMes", date_format(col("Data"), "yyyy-MM"))

# Escrita no Delta
df_clean.write.format("delta").mode("overwrite").save("/mnt/datalake/bronze/concessoes_credito_delta")
````

### 3. Análise – **SQL no Databricks**

```sql
SELECT AnoMes, SUM(Valor) AS TotalConcedido
FROM concessoes_credito
GROUP BY AnoMes
ORDER BY AnoMes;
```

### 4. Versionamento – **Azure DevOps + GitHub**

* Repositório Git integrado ao ADF.
* Pipelines CI/CD para testes e deploy de notebooks.
* Versionamento automático de JSONs e backups de pipelines.

---

## 📊 Insights Obtidos

* **Sazonalidade**: padrões mensais nas concessões de crédito.
* **Impacto econômico**: variações de acordo com contexto macroeconômico.
* **Eficiência**: avaliação da performance no processamento distribuído.

---

## 🚀 Possibilidades Futuras

* Integração com **Power BI** para dashboards avançados.
* Automação de execuções com **Databricks Jobs**.
* Integração com **MLflow** para modelagem de risco de crédito.

---

## 🔗 Fontes de Dados

* [Banco Central do Brasil – Concessões de Crédito (SCR)](https://dadosabertos.bcb.gov.br/dataset/20631-concessoes-de-credito---total)

