001 - Fundamentos Iniciais do Apache Spark

Objetivo

Validar o ambiente Spark local e compreender os conceitos fundamentais de processamento distribuído utilizando:

* Docker
* Jupyter Notebook
* Spark Master
* Spark Worker
* PySpark

⸻

Arquitetura do Ambiente

Jupyter Notebook
        ↓
Spark Driver
        ↓
Spark Master
        ↓
Spark Worker
        ↓
Execução das Tasks

Ambiente executado localmente em Docker.

Componentes:

* Jupyter Notebook
* Spark Master
* Spark Worker
* PostgreSQL

⸻

Criação da SparkSession

Código executado:

from pyspark.sql import SparkSession
spark = (
    SparkSession.builder
    .appName("TesteSpark")
    .master("spark://spark-master:7077")
    .getOrCreate()
)

Objetivo:

Criar uma sessão Spark e conectar o notebook ao cluster.

Resultado:

Conexão estabelecida com sucesso.

Informações observadas:

Version: 3.5.1
Master: spark://spark-master:7077
AppName: TesteSpark

⸻

Criação do Primeiro DataFrame

Código:

dados = [
    (1, "João", 5000),
    (2, "Maria", 8000),
    (3, "Pedro", 3000)
]
df = spark.createDataFrame(
    dados,
    ["id_cliente", "nome", "renda"]
)

DataFrame criado:

id_cliente	nome	renda
1	João	5000
2	Maria	8000
3	Pedro	3000

⸻

Exibição dos Dados

Código:

df.show()

Resultado:

+----------+------+-----+
|id_cliente| nome |renda|
+----------+------+-----+
|1         |João  |5000 |
|2         |Maria |8000 |
|3         |Pedro |3000 |
+----------+------+-----+

⸻

Conceito: DataFrame

DataFrame é uma coleção distribuída de dados organizada em linhas e colunas.

Semelhante a:

* Tabela SQL
* DataFrame Pandas

Porém distribuído e processado pelo Spark.

⸻

Conceito: Schema

Código:

df.printSchema()

Resultado:

root
 |-- id_cliente: long
 |-- nome: string
 |-- renda: long

O Schema representa:

* Nome das colunas
* Tipos de dados
* Estrutura do DataFrame

Pode ser considerado o contrato dos dados.

⸻

Conceito: Transformation

Exemplo:

df_filtrado = df.filter(df.renda > 4000)

Características:

* Não executa imediatamente
* Apenas cria um plano lógico
* Retorna um novo DataFrame

Exemplos:

* filter()
* select()
* join()
* groupBy()
* withColumn()
* orderBy()

⸻

Conceito: Action

Exemplos:

df.show()
df.count()

Características:

* Disparam a execução do plano
* Geram processamento real

Exemplos de Actions:

* show()
* count()
* collect()
* first()
* take()
* write.parquet()

⸻

Conceito: Lazy Evaluation

O Spark não executa imediatamente as Transformations.

Exemplo:

df_filtrado = df.filter(df.renda > 4000)

Nesse momento:

Nenhum processamento ocorreu.

O Spark apenas registra o plano.

A execução acontece somente quando uma Action é chamada.

Exemplo:

df_filtrado.show()

⸻

Explain Plan

Código:

df_filtrado.explain(True)

Resultado:

== Parsed Logical Plan ==
'Filter (renda#2L > 4000)
+- LogicalRDD [id_cliente#0L, nome#1, renda#2L], false
== Analyzed Logical Plan ==
id_cliente: bigint, nome: string, renda: bigint
Filter (renda#2L > cast(4000 as bigint))
+- LogicalRDD [id_cliente#0L, nome#1, renda#2L], false
== Optimized Logical Plan ==
Filter (isnotnull(renda#2L) AND (renda#2L > 4000))
+- LogicalRDD [id_cliente#0L, nome#1, renda#2L], false
== Physical Plan ==
*(1) Filter (isnotnull(renda#2L) AND (renda#2L > 4000))
+- *(1) Scan ExistingRDD[id_cliente#0L,nome#1,renda#2L]

⸻

Parsed Logical Plan

Representa exatamente o que foi escrito pelo desenvolvedor.

Filter (renda > 4000)

⸻

Analyzed Logical Plan

O Spark valida:

* Schema
* Tipos de dados
* Colunas

O Spark converteu:

4000

para:

cast(4000 as bigint)

⸻

Optimized Logical Plan

Atuação do Catalyst Optimizer.

O Spark reescreveu:

renda > 4000

para:

isnotnull(renda)
AND
renda > 4000

⸻

Physical Plan

Plano real de execução.

Filter
↓
Scan ExistingRDD

Esse plano é enviado para os executores.

⸻

Catalyst Optimizer

O Catalyst é o otimizador interno do Spark.

Responsável por:

* Reescrever consultas
* Melhorar performance
* Eliminar operações desnecessárias

⸻

Partições

Código:

df.rdd.getNumPartitions()

Resultado:

2

O DataFrame foi dividido em duas partições.

⸻

Alteração do Número de Partições

Código:

df.repartition(4)

Resultado:

4

⸻

Shuffle

Código:

df.repartition(4).explain(True)

Resultado relevante:

Exchange RoundRobinPartitioning(4)

A palavra:

Exchange

indica:

Shuffle

Shuffle significa movimentação de dados entre partições.

É uma operação cara porque consome:

* CPU
* Memória
* Rede
* Tempo

⸻

Visualização Física das Partições

Código:

df.rdd.glom().collect()

Resultado:

[
 [Row(id_cliente=1, nome='João', renda=5000)],
 [
  Row(id_cliente=2, nome='Maria', renda=8000),
  Row(id_cliente=3, nome='Pedro', renda=3000)
 ]
]

Interpretação:

Partição 1:

João

Partição 2:

Maria
Pedro

⸻

Tasks

Cada partição gera uma Task.

Fluxo:

Partição
↓
Task
↓
Core

⸻

Paralelismo

Código:

spark.sparkContext.defaultParallelism

Resultado:

2

Configuração atual do cluster:

1 Worker
2 cores

O Spark identificou capacidade para executar duas Tasks simultaneamente.

⸻

Relação Entre os Conceitos

DataFrame
↓
Partition
↓
Task
↓
Core
↓
Execução Paralela

⸻

Principais Aprendizados

* Spark trabalha com processamento distribuído.
* DataFrames são divididos em partições.
* Cada partição gera Tasks.
* Tasks são executadas pelos Workers.
* Transformations não executam imediatamente.
* Actions disparam a execução.
* O Catalyst Optimizer melhora os planos automaticamente.
* Shuffles são operações caras.
* O número de partições influencia diretamente a performance.
* Mais partições não significa necessariamente mais velocidade.
* O Spark é uma engine de paralelismo.
* O paralelismo é obtido através da divisão dos dados em partições.
* O número ideal de partições depende do volume de dados e da capacidade do cluster.
