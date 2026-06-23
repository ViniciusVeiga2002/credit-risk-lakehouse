# 001 - Produtos de Crédito

## Objetivo

Este documento define os produtos de crédito que serão simulados no projeto Credit Risk Lakehouse.

O objetivo é representar uma carteira multiproduto com diferentes perfis de risco, probabilidades de inadimplência e perdas esperadas.

---

## Produtos da Carteira

| Produto | Descrição | Nível de Risco | LGD Inicial |
|----------|------------|----------|----------|
| Empréstimo Pessoal | Crédito sem garantia real | Alto | 75% |
| Cartão de Crédito | Crédito rotativo sem garantia | Alto | 85% |
| Financiamento de Veículo | Crédito com garantia do veículo | Médio | 35% |
| Consignado | Parcelas descontadas diretamente da folha ou benefício | Baixo | 15% |

---

## Justificativa dos Perfis de Risco

### Empréstimo Pessoal

Produto sem garantia real.

Em caso de inadimplência, a recuperação tende a ser baixa.

LGD inicial assumida: 75%.

---

### Cartão de Crédito

Produto rotativo sem garantia.

Normalmente apresenta alta severidade de perda após o default.

LGD inicial assumida: 85%.

---

### Financiamento de Veículo

Possui garantia através do veículo financiado.

Parte da exposição pode ser recuperada por meio da retomada e venda do bem.

LGD inicial assumida: 35%.

---

### Consignado

As parcelas são descontadas diretamente da folha de pagamento ou benefício.

Apresenta menor risco de inadimplência e maior recuperação.

LGD inicial assumida: 15%.

---

## Premissas Iniciais

- Um cliente pode possuir múltiplos produtos.
- Cada produto possui comportamento de risco distinto.
- A LGD varia conforme o produto.
- O EAD será calculado a partir da exposição em aberto.
- A Perda Esperada será calculada através da fórmula:

Perda Esperada = PD × LGD × EAD

---

## Evoluções Futuras

Ao longo do projeto poderão ser adicionados:

- Taxas de juros específicas por produto;
- Garantias detalhadas para financiamentos;
- Limites de crédito para cartões;
- Comportamento de pré-pagamento;
- Renegociação de dívidas;
- Fluxos de recuperação de crédito.