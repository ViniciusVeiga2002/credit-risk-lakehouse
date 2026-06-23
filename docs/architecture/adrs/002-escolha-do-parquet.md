# ADR-002 - Escolha do Parquet

## Status

Aceito

## Contexto

O projeto Credit Risk Lakehouse irá processar grandes volumes de dados utilizando Spark.

Precisamos de um formato de armazenamento eficiente para:

- Leitura analítica;
- Processamento distribuído;
- Compressão;
- Particionamento;
- Escalabilidade.

## Decisão

Foi escolhido o formato Parquet como principal formato de armazenamento das camadas Bronze, Silver, Gold e Feature Store.

## Justificativa

### Formato Colunar

O Parquet armazena dados por coluna e não por linha.

Isso reduz significativamente a quantidade de dados lidos durante consultas analíticas.

### Compressão

O formato oferece excelente taxa de compressão.

Isso reduz consumo de disco e tráfego de dados.

### Integração com Spark

Parquet é um dos formatos nativos mais utilizados pelo Spark.

### Particionamento

O formato funciona muito bem com estratégias de particionamento.

### Mercado

É amplamente utilizado em:

- Databricks
- AWS Glue
- EMR
- Azure Synapse
- Google Dataproc

## Consequências Positivas

- Melhor performance.
- Menor uso de armazenamento.
- Melhor integração com Spark.
- Escalabilidade.

## Consequências Negativas

- Não é facilmente legível por humanos.
- Exige ferramentas específicas para inspeção.

## Alternativas Consideradas

### CSV

Rejeitado devido à baixa eficiência para workloads analíticos.

### JSON

Rejeitado devido ao maior volume de armazenamento e menor performance.

## Resultado

Parquet será o formato padrão do projeto.