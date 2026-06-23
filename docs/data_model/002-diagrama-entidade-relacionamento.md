# 002 - Diagrama Entidade-Relacionamento

## Objetivo

Este documento apresenta a primeira versão do modelo entidade-relacionamento do projeto Credit Risk Lakehouse.

O objetivo é mapear as principais entidades do domínio de crédito antes da implementação dos pipelines em PySpark.

---

## Diagrama ERD

```mermaid
erDiagram
    CLIENTE ||--|| BUREAU_CREDITO : possui
    CLIENTE ||--o{ PROPOSTA_CREDITO : realiza
    PROPOSTA_CREDITO ||--o| CONTRATO : pode_gerar
    CLIENTE ||--o{ CONTRATO : possui
    CONTRATO ||--o{ PARCELA : gera
    PARCELA ||--o{ PAGAMENTO : recebe
    CONTRATO ||--o| EVENTO_DEFAULT : pode_ter

    CLIENTE {
        bigint cliente_id PK
        string sexo
        int idade
        string estado_civil
        string escolaridade
        decimal renda_mensal
        int tempo_emprego_meses
        string uf
        date data_cadastro
    }

    BUREAU_CREDITO {
        bigint cliente_id PK, FK
        int score_bureau
        int qtd_restricoes
        int qtd_consultas_90d
        int qtd_consultas_365d
        decimal valor_total_dividas
        date data_referencia
    }

    PROPOSTA_CREDITO {
        bigint proposta_id PK
        bigint cliente_id FK
        string produto
        decimal valor_solicitado
        int prazo_meses
        date data_proposta
        string status_proposta
    }

    CONTRATO {
        bigint contrato_id PK
        bigint proposta_id FK
        bigint cliente_id FK
        string produto
        decimal valor_contratado
        decimal taxa_juros
        int prazo_meses
        date data_contratacao
        string status_contrato
    }

    PARCELA {
        bigint parcela_id PK
        bigint contrato_id FK
        int numero_parcela
        decimal valor_parcela
        date data_vencimento
        string status_parcela
    }

    PAGAMENTO {
        bigint pagamento_id PK
        bigint parcela_id FK
        date data_pagamento
        decimal valor_pago
        string forma_pagamento
    }

    EVENTO_DEFAULT {
        bigint default_id PK
        bigint contrato_id FK
        bigint cliente_id FK
        date data_default
        int dias_atraso
        decimal saldo_em_aberto
    }