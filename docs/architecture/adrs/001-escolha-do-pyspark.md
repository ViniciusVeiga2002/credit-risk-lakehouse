ADR-001 - Escolha do PySpark

Status

Aceito

Contexto

O projeto Credit Risk Lakehouse tem como objetivo principal estudar Big Data, Engenharia de Dados, Risco de Crédito e Monitoramento de Modelos.

Precisamos de uma tecnologia capaz de:

* Processar grandes volumes de dados;
* Executar transformações distribuídas;
* Trabalhar com arquitetura Lakehouse;
* Ser amplamente utilizada pelo mercado;
* Permitir integração com Python.

Decisão

Foi escolhido o PySpark como principal framework de processamento de dados.

Justificativa

Escalabilidade

O Spark foi projetado para processamento distribuído e suporta desde execução local até clusters com centenas de máquinas.

Adoção de Mercado

PySpark é amplamente utilizado em:

* Bancos;
* Fintechs;
* Empresas de tecnologia;
* Plataformas de dados modernas.

Integração com Python

A equipe do projeto possui familiaridade com Python.

Isso reduz a curva de aprendizado inicial.

Ecossistema

PySpark integra facilmente com:

* Parquet
* PostgreSQL
* Jupyter Notebook
* Docker
* Scikit-Learn
* MLflow

Objetivo Educacional

O Spark permite estudar conceitos importantes de Big Data:

* Lazy Evaluation
* DAG
* Shuffle
* Partitioning
* Broadcast Join
* Catalyst Optimizer
* Data Skew

Consequências

Positivas

* Aprendizado alinhado ao mercado.
* Arquitetura escalável.
* Facilidade para evoluir para Databricks futuramente.

Negativas

* Curva de aprendizado mais alta do que Pandas.
* Maior complexidade de ambiente.
* Necessidade de compreender conceitos distribuídos.

Alternativas Consideradas

Pandas

Rejeitado por não representar cenários de Big Data.

Polars

Rejeitado por menor adoção em ambientes corporativos de dados.

Spark SQL puro

Rejeitado porque o objetivo do projeto inclui desenvolvimento em Python.

Resultado

PySpark será o motor principal de processamento de dados do projeto.