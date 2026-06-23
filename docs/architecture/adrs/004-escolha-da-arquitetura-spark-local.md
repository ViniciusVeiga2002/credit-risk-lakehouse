# ADR-004 - Escolha da Arquitetura Spark Local

## Status

Aceito

## Contexto

O projeto Credit Risk Lakehouse será executado em ambiente local utilizando um MacBook com Docker Desktop.

Embora o ambiente físico seja uma única máquina, o objetivo do projeto não é apenas utilizar a API do PySpark, mas compreender os conceitos fundamentais de processamento distribuído.

Precisamos decidir entre:

### Opção 1

Executar PySpark diretamente dentro de um único processo.

Exemplo:

Jupyter
↓
PySpark

### Opção 2

Simular uma arquitetura Spark distribuída.

Exemplo:

Jupyter
↓
Spark Master
↓
Spark Worker

## Decisão

Foi escolhida a arquitetura Spark composta por:

- Spark Master
- Spark Worker
- Jupyter Notebook

Todos executados localmente através do Docker Compose.

## Justificativa

### Aprendizado de Arquitetura

A separação entre Master e Worker permite compreender melhor:

- Driver
- Executor
- Tasks
- Partitions
- Jobs
- Stages

### Aproximação com Ambientes Reais

Ambientes corporativos normalmente utilizam:

- Databricks
- EMR
- Dataproc
- Synapse

Todos baseados em execução distribuída.

Mesmo localmente, a separação ajuda a reproduzir conceitos semelhantes.

### Escalabilidade Conceitual

A arquitetura poderá evoluir futuramente para múltiplos workers sem alterações significativas na lógica da aplicação.

### Objetivo Educacional

O projeto busca ensinar:

- Como Spark distribui tarefas;
- Como ocorre o paralelismo;
- Como funcionam partições;
- Como analisar planos de execução.

## Consequências Positivas

- Melhor compreensão da arquitetura Spark.
- Ambiente mais próximo do mercado.
- Facilidade para estudar execução distribuída.
- Base para crescimento futuro.

## Consequências Negativas

- Maior complexidade inicial.
- Mais containers em execução.
- Configuração um pouco mais extensa.

## Alternativas Consideradas

### PySpark Standalone

Rejeitado por esconder conceitos importantes de arquitetura distribuída.

### Databricks Community Edition

Rejeitado neste momento porque o objetivo é compreender também a infraestrutura local.

## Resultado

O ambiente inicial será composto por:

Jupyter Notebook
↓
Spark Master
↓
Spark Worker

Executados localmente via Docker Compose.