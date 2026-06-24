002 - Jobs, Stages, Tasks e Shuffle no Apache Spark

Objetivo

Compreender como o Spark executa uma aplicação internamente através dos conceitos de:

* Job
* Stage
* Task
* Partition
* Shuffle

Além de analisar a execução através da Spark UI.

⸻

Ambiente Utilizado

Cluster local executado via Docker:

Jupyter Notebook
        ↓
Spark Driver
        ↓
Spark Master
        ↓
Spark Worker
        ↓
Executores

Recursos disponíveis:

1 Worker
2 Cores
2 GB RAM

⸻

Spark Master UI

Acessada através de:

http://localhost:8080

Responsável por exibir:

* Workers registrados
* Aplicações em execução
* Recursos disponíveis
* Memória
* Cores

Não exibe detalhes de Jobs e Stages.

⸻

Spark Application UI

Acessada através de:

http://localhost:4040

Responsável por exibir:

* Jobs
* Stages
* Tasks
* Shuffles
* SQL
* Executors
* Métricas de execução

É a principal ferramenta para análise de performance.

⸻

Geração de Dataset

Código executado:

clientes = []
for i in range(100000):
    clientes.append(
        (
            i,
            f"Cliente_{i}",
            (i % 15000) + 1000
        )
    )
df_clientes = spark.createDataFrame(
    clientes,
    ["id_cliente", "nome", "renda"]
)

Total de registros:

100.000 linhas

⸻

Primeira Action

Código:

df_clientes.count()

Resultado:

100000

⸻

O que aconteceu internamente?

Fluxo de execução:

Action
↓
Job
↓
Stage
↓
Tasks
↓
Worker
↓
Resultado

⸻

Conceito: Job

Um Job representa um trabalho completo solicitado ao Spark.

Exemplo:

df_clientes.count()

gerou um Job.

Pode ser entendido como:

"O que o usuário pediu para o Spark fazer."

⸻

Conceito: Stage

Um Stage representa uma etapa do Job.

O Spark divide um Job em múltiplos Stages quando necessário.

Exemplo:

Job
├── Stage 1
└── Stage 2

Normalmente novos Stages surgem quando ocorre um Shuffle.

⸻

Conceito: Task

Task é a menor unidade de execução do Spark.

Cada Task é enviada para um Core disponível.

Relação:

Partição
↓
Task
↓
Core

⸻

Observação na Spark UI

Foi identificado:

Tasks: 2/2

Motivo:

2 Partições
2 Cores

Logo:

Partição 1 → Task 1
Partição 2 → Task 2

Executadas em paralelo.

⸻

Relação Entre os Componentes

DataFrame
↓
Partições
↓
Tasks
↓
Cores
↓
Execução Paralela

⸻

Conceito: Shuffle

Shuffle é a redistribuição de dados entre partições.

É uma das operações mais custosas do Spark.

⸻

Exemplo Conceitual

Antes:

Partição 1
5000
5000
8000
Partição 2
5000
3000
8000

Após o Shuffle:

Partição 1
5000
5000
5000
Partição 2
8000
8000
3000

Os dados são reorganizados para que registros relacionados fiquem juntos.

⸻

Por que o Shuffle é caro?

Porque envolve:

1. Serialização

Transformação dos objetos em bytes.

Objeto
↓
Bytes

⸻

2. Escrita temporária

Frequentemente os dados são gravados em disco.

Memória
↓
Disco

⸻

3. Rede

Em clusters reais:

Servidor A
↓
Rede
↓
Servidor B

A transferência entre máquinas é muito mais lenta que acesso local à memória.

⸻

4. Desserialização

Reconstrução dos objetos no destino.

Bytes
↓
Objeto

⸻

Operações Baratas

Normalmente não geram Shuffle:

select()
filter()
withColumn()
drop()

Essas operações costumam ser executadas localmente em cada partição.

⸻

Operações Caras

Normalmente geram Shuffle:

groupBy()
join()
distinct()
orderBy()
repartition()
dropDuplicates()

São as principais responsáveis por problemas de performance.

⸻

Experimento com Group By

Código executado:

df_clientes.groupBy("renda").count().show()

⸻

O que ocorreu?

A Spark UI mostrou:

Stage 3
Tasks: 2/2
Shuffle Write: 228.1 KiB

seguido por:

Stage 5
Tasks: 1/1
Shuffle Read: 228.1 KiB

⸻

Interpretação

Primeiro Stage:

Ler dados
↓
Redistribuir dados
↓
Shuffle Write

Segundo Stage:

Ler Shuffle
↓
Agrupar registros
↓
Produzir resultado

⸻

O Insight Mais Importante

O principal gargalo em aplicações Spark normalmente não é CPU.

Também não costuma ser memória.

Na maioria dos cenários o maior impacto de performance está em:

Shuffle

Porque ele exige:

* Comunicação entre partições
* Comunicação entre executores
* Uso de rede
* Uso de disco
* Criação de novos Stages

⸻

Regra Prática

Sempre que aparecer:

Shuffle Read
Shuffle Write

na Spark UI, é um indicativo de que o Spark precisou movimentar dados entre partições.

Esse é um dos primeiros pontos a serem investigados em análises de performance.

⸻

Fluxo Completo de Execução do Spark

DataFrame
↓
Transformation
↓
Action
↓
Job
↓
Stage
↓
Task
↓
Core
↓
Resultado

⸻

Principais Aprendizados

* Um Job representa um trabalho completo.
* Um Stage representa uma etapa do Job.
* Uma Task representa uma unidade executável.
* Cada partição gera Tasks.
* Tasks são executadas pelos Cores disponíveis.
* O Spark trabalha em paralelo através das partições.
* Shuffle é a redistribuição de dados entre partições.
* Shuffle normalmente cria novos Stages.
* Operações como groupBy e join costumam gerar Shuffle.
* O Shuffle é uma das principais causas de degradação de performance em Spark.
* A Spark UI é a ferramenta principal para análise e diagnóstico de performance.
* A compreensão de Jobs, Stages, Tasks e Shuffle é fundamental para otimização de pipelines de Big Data.