001 - Visão Geral do Domínio

Objetivo

Este documento descreve as principais entidades de negócio que compõem o ecossistema de crédito do projeto Credit Risk Lakehouse.

O objetivo é criar um modelo de dados coerente para suportar:

* Engenharia de Dados
* Feature Engineering
* Modelagem de PD
* Cálculo de LGD
* Cálculo de EAD
* Cálculo de PDD
* Monitoramento da carteira

⸻

Entidades Principais

Cliente

Representa uma pessoa que possui relacionamento com a instituição financeira.

Exemplos de atributos:

* cliente_id
* sexo
* idade
* estado_civil
* escolaridade
* renda_mensal
* tempo_emprego
* uf
* data_cadastro

⸻

Bureau de Crédito

Representa informações externas do cliente.

Exemplos:

* score_bureau
* quantidade_restricoes
* quantidade_consultas
* valor_total_dividas

⸻

Proposta de Crédito

Representa uma solicitação de crédito realizada por um cliente.

Exemplos:

* proposta_id
* cliente_id
* produto
* valor_solicitado
* prazo
* data_proposta

⸻

Contrato

Representa uma proposta aprovada.

Exemplos:

* contrato_id
* cliente_id
* produto
* valor_contratado
* taxa_juros
* prazo
* data_contratacao

⸻

Parcela

Representa uma obrigação financeira futura.

Exemplos:

* parcela_id
* contrato_id
* numero_parcela
* valor
* vencimento

⸻

Pagamento

Representa a liquidação total ou parcial de uma parcela.

Exemplos:

* pagamento_id
* parcela_id
* data_pagamento
* valor_pago

⸻

Evento de Default

Representa a ocorrência de inadimplência.

Exemplos:

* default_id
* cliente_id
* contrato_id
* data_default
* dias_atraso

⸻

Relacionamentos

Cliente
↓
Proposta
↓
Contrato
↓
Parcela
↓
Pagamento

Cliente
↓
Bureau

Contrato
↓
Evento de Default

⸻

Produtos da Carteira

* Empréstimo Pessoal
* Cartão de Crédito
* Financiamento de Veículo
* Consignado

⸻

Objetivo Analítico

A partir dessas entidades será possível:

* Calcular PD
* Calcular LGD
* Calcular EAD
* Calcular PDD
* Treinar modelos
* Monitorar modelos
* Monitorar a carteira