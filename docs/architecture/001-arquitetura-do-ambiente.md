# 001 - Arquitetura do Ambiente

## Objetivo

Este documento descreve a arquitetura do ambiente local do projeto Credit Risk Lakehouse.

O objetivo é criar um ambiente reproduzível, isolado e próximo de um cenário real de Engenharia de Dados, utilizando Docker, Spark, Jupyter Notebook e PostgreSQL.

---

## Visão Geral

O projeto será executado localmente no Mac, utilizando Docker Desktop para orquestrar os serviços necessários.

Fluxo geral:

```text
Mac
↓
Docker Desktop
↓
Docker Compose
↓
Containers