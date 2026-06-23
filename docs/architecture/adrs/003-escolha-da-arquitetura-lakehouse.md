# ADR-003 - Escolha da Arquitetura Lakehouse

## Status

Aceito

## Contexto

O projeto precisa representar um fluxo moderno de engenharia de dados e suportar:

- Dados brutos;
- Dados tratados;
- Dados analíticos;
- Feature Engineering;
- Machine Learning;
- Monitoramento.

## Decisão

Foi escolhida a arquitetura Lakehouse.

## Justificativa

A arquitetura Lakehouse combina características de:

### Data Lake

- Flexibilidade;
- Baixo custo;
- Armazenamento de dados brutos.

### Data Warehouse

- Governança;
- Dados organizados;
- Consistência analítica.

## Estrutura Definida

Raw
↓
Bronze
↓
Silver
↓
Gold
↓
Feature Store
↓
Modelos
↓
Monitoramento

## Benefícios

### Rastreabilidade

É possível voltar até os dados originais.

### Reprocessamento

As camadas podem ser reconstruídas quando necessário.

### Separação de Responsabilidades

Cada camada possui um propósito claro.

### Escalabilidade

O modelo funciona para pequenos e grandes volumes.

## Consequências Positivas

- Arquitetura moderna.
- Próxima do mercado.
- Boa integração com Spark.
- Boa organização dos dados.

## Consequências Negativas

- Maior número de camadas.
- Maior complexidade inicial.

## Alternativas Consideradas

### Data Warehouse Tradicional

Rejeitado por limitar o estudo de Big Data.

### Estrutura Única de Dados

Rejeitada por dificultar governança e reprocessamento.

## Resultado

A arquitetura oficial do projeto será Lakehouse.