# Projeto Final de Spark - Campanha Nacional de Vacinação contra Covid-19

## Overview do Projeto e Objetivos

Este projeto final de Spark tem como objetivo criar visualizações de dados relacionados à Campanha Nacional de Vacinação contra Covid-19. O projeto foi dividido em dois níveis: básico e avançado, onde o foco principal será no nível básico. Os exercícios podem ser realizados em qualquer linguagem de programação, proporcionando liberdade criativa e diferentes abordagens para as soluções.

## Sumário

1. [Overview do Projeto e Objetivos](#overview-do-projeto-e-objetivos)
2. [Sobre o Projeto](#sobre-o-projeto)
3. [Sobre os Dados](#sobre-os-dados)
4. [Sobre os Arquivos](#sobre-os-arquivos)
5. [Execução do Projeto](#execução-do-projeto)
   - 5.1. [Enviar os dados para o hdfs](#51-enviar-os-dados-para-o-hdfs)
   - 5.2. [Otimizar todos os dados do hdfs para uma tabela Hive particionada por município](#52-otimizar-todos-os-dados-do-hdfs-para-uma-tabela-hive-particionada-por-município)
   - 5.3. [Criar as 3 visualizações pelo Spark com os dados enviados para o HDFS](#53-criar-as-3-visualizações-pelo-spark-com-os-dados-enviados-para-o-hdfs)
     - 5.3.1 [Criação da Visualização 1]()
     - 5.3.2 [Criação da Visualização 2]()
     - 5.3.3 [Criação da Visualização 3]()
   - 5.4. [Salvar a Visualização 1 em Tabela Hive]()
   - 5.5. [Salvar a Segunda Visualização como Formato Parquet com Compressão Snappy]()
   - 5.6. [Envio dos dados para um tópico Kafka (via terminal)]()
     - 5.6.1 [Criação do tópico Kafka (via terminal)]()
     - 5.6.2 [Envio dos dados para o tópico Kafka (via Confluent)]()
   - 5.7. [Criar a visualização com os dados enviados para o HDFS (via Spark):]()
   - 5.8. [Enviar os dados da visualização 3 para um tópico Elastic]()
6. [Conclusão](#conclusão)
7. [Contato](#contato)

## Sobre o Projeto

O projeto final de Spark é uma oportunidade para os participantes colocarem em prática os conhecimentos adquiridos no treinamento, trabalhando com dados reais da Campanha Nacional de Vacinação contra Covid-19. O desafio está dividido em nível básico e avançado. A solução será feita usando Spark , Hive , Kafka , Jupyter Notebook , Docker/Docker-compose , Elastic e Kibana.

## Sobre os Dados

Os dados utilizados no projeto estão disponíveis em um arquivo compactado, cujo link é fornecido abaixo. Esses dados são referentes ao Painel Geral da Covid-19 no Brasil e estão disponíveis no site oficial do governo brasileiro sobre a pandemia.

Link dos Dados: [Dados do Painel Geral da Covid-19 - 06 de julho de 2021](https://mobileapps.saude.gov.br/esusvepi/files/unAFkcaNDeXajurGB7LChj8SgQYS2ptm/04bd3419b22b9cc5c6efac2c6528100d_HIST_PAINEL_COVIDBR_06jul2021.rar)

Os dados da Campanha Nacional de Vacinação contra Covid-19 estão disponíveis em formato CSV com as seguintes colunas:

### Documentação Técnica dos dados

Os dados da Campanha Nacional de Vacinação contra Covid-19 estão disponíveis em formato CSV com as seguintes colunas:

| Nome da Coluna         | Tipo do Dado     | Amostra de Dados                            | Descrição do Campo                                                                                                               |
|-----------------------|------------------|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| regiao                | String           | "Brasil", "Norte", "Sul"                   | Região geográfica à qual os dados se referem.                                                                                    |
| estado                | String           | "RO", "SP", "MG"                           | Sigla do estado brasileiro ao qual os dados pertencem.                                                                           |
| municipio             | String           | "São Paulo", "Porto Velho", "Belo Horizonte" | Nome do município brasileiro ao qual os dados pertencem. (Valor vazio para registros referentes ao Brasil inteiro, sem distinção de municípios) |
| coduf                 | String           | "11", "12", "76"                          | Código único do estado brasileiro ao qual os dados pertencem.                                                                    |
| codmun                | String           | "150420", "150430", "150440"              | Código único do município brasileiro ao qual os dados pertencem.                                                                 |
| codRegiaoSaude        | String           | "210147125", "1777225", "3450205"         | Código da região de saúde à qual os dados pertencem.                                                                             |
| nomeRegiaoSaude       | String           | "Centro-Oeste", "Norte", "Sul"            | Nome da região de saúde à qual os dados pertencem.                                                                               |
| data                  | Date             | "2020-02-25", "2020-03-20", "2020-06-15"  | Data da ocorrência dos dados.                                                                                                    |
| semanaEpi             | Integer          | 9, 12, 25                                 | Número da semana epidemiológica em que os dados foram registrados.                                                               |
| populacaoTCU2019      | Integer          | 210147125, 1777225, 2512078               | População estimada para o local (estado ou município) em 2019, conforme dados do TCU.                                           |
| casosAcumulado        | Integer          | 3, 1, 5032                                | Número acumulado de casos de Covid-19 até a data informada.                                                                      |
| casosNovos            | Integer          | 4, 1, 163                                 | Número de novos casos de Covid-19 registrados na data informada.                                                                 |
| obitosAcumulado       | Integer          | 0, 3, 127                                 | Número acumulado de óbitos por Covid-19 até a data informada.                                                                    |
| obitosNovos           | Integer          | 1, 1, 7                                   | Número de novos óbitos por Covid-19 registrados na data informada.                                                               |
| Recuperadosnovos      | Integer          | 0, 2, 23                                  | Número de novos recuperados de Covid-19 registrados na data informada.                                                            |
| emAcompanhamentoNovos | Integer          | 0, 5, 140                                 | Número de novos casos de Covid-19 em acompanhamento na data informada.                                                            |
| interior/metropolitana | Bool            | 0, 1                                      | Indicação se o local (estado ou município) é classificado como "interior" (0) ou "metropolitana" (1). |


## Sobre os Arquivos

### Docker-compose

O projeto utiliza o Docker para orquestrar diferentes serviços relacionados a uma análise de dados sobre a campanha nacional de vacinação contra Covid-19. Os serviços estão descritos no arquivo `docker-compose-oficial.yml` e incluem:

- `namenode`: Serviço para o Hadoop Distributed File System (HDFS) que gerencia os metadados do sistema de arquivos.
- `datanode`: Serviço para o HDFS que armazena os dados do sistema de arquivos.
- `hive-server`: Serviço para o Apache Hive, uma ferramenta de data warehousing que permite consultar e analisar grandes conjuntos de dados armazenados no HDFS.
- `hive-metastore`: Serviço para o Hive Metastore, que armazena os metadados do Hive, como esquemas e partições de tabelas.
- `hive-metastore-postgresql`: Serviço para o banco de dados PostgreSQL usado pelo Hive Metastore para armazenar seus metadados.
- `hbase-master`: Serviço para o Apache HBase, um banco de dados NoSQL distribuído que roda sobre o HDFS.
- `jupyter-spark`: Serviço para o Jupyter Notebook com suporte para Spark, permitindo a análise interativa de dados usando a linguagem Python e Pyspark.
- `elasticsearch`: Serviço para o Elasticsearch, um mecanismo de busca e análise de dados em tempo real.
- `kibana`: Serviço para o Kibana, uma interface de usuário para visualizar e explorar dados armazenados no Elasticsearch.
- `logstash`: Serviço para o Logstash, que coleta, processa e envia logs e outros dados para o Elasticsearch.
- `zookeeper`: Serviço para o Apache ZooKeeper, um sistema de coordenação distribuído usado por outros serviços para gerenciar a alta disponibilidade.
- `broker`: Serviço para o Apache Kafka, uma plataforma de streaming distribuída que lida com fluxos de eventos em tempo real.
- `schema-registry`: Serviço para o Confluent Schema Registry, que gerencia esquemas de dados para o Kafka.
- `connect`: Serviço para o Kafka Connect, que permite a integração de dados entre Kafka e outros sistemas.
- `control-center`: Serviço para o Confluent Control Center, que fornece uma interface de gerenciamento e monitoramento para o ambiente Kafka.
- `ksqldb-server`: Serviço para o ksqlDB, que permite executar consultas SQL em tempo real no Kafka.
- `ksqldb-cli`: Serviço para o ksqlDB CLI, uma ferramenta de linha de comando para interagir com o ksqlDB.
- `ksql-datagen`: Serviço para o ksqlDB Data Generator, uma ferramenta para gerar dados de teste para o ksqlDB.
- `rest-proxy`: Serviço para o Confluent Kafka REST Proxy, que permite acessar o Kafka por meio de APIs REST.

Esses serviços permitem a implantação de um ambiente completo para análise de dados em tempo real e consultas SQL usando diversas ferramentas populares, como Spark, Hive e ksqlDB. O projeto utiliza imagens de contêiner pré-configuradas para cada serviço, facilitando a implantação e gerenciamento do ambiente de análise de dados.

### Jupyter Notebook

O código é um Jupyter Notebook desenvolvido para realizar transformações de dados da Covid-19 utilizando o Apache Spark. Ele carrega arquivos CSV contendo informações sobre a pandemia, combina os dados em um DataFrame único e cria visualizações com indicadores-chave (KPIs) relacionados aos casos confirmados, óbitos e recuperações.

#### Pacotes Utilizados

O projeto utiliza os seguintes pacotes:

- `pyspark.sql`: Biblioteca do Apache Spark para manipulação de dados em formato tabular.
- `pyspark.sql.functions`: Módulo do Spark para operações com funções e agregações.
- `json`: Biblioteca Python para trabalhar com dados JSON.

#### Spark Session

O Spark Session é criado utilizando a biblioteca `pyspark.sql.SparkSession`. É uma interface principal do Apache Spark para programação com DataFrame e SQL. O Spark Session é utilizado para a configuração e inicialização do ambiente de execução do Spark e é a porta de entrada para trabalhar com o Spark DataFrame.

#### Passos do Código

1. Criação do ambiente Spark com suporte ao Hive através da Spark Session.
2. Criação de diretórios no HDFS para armazenar os arquivos de dados.
3. Envio dos arquivos CSV para o HDFS.
4. Carregamento dos arquivos CSV em DataFrames Spark.
5. Combinação dos DataFrames em um único DataFrame.
6. Criação de uma tabela Hive chamada "covid_por_local" e carregamento dos dados do DataFrame nela.
7. Realização de três visualizações de dados (KPIs) sobre a Covid-19, com informações sobre casos recuperados, casos confirmados e óbitos confirmados.
8. Salvamento da primeira visualização como uma tabela Hive chamada "covid19_recuperados_acompanhamento".
9. Salvamento da segunda visualização em formato Parquet com compressão Snappy.
10. Conversão dos dados da terceira visualização em um DataFrame para serem enviados ao Kafka.
11. Criação de um tópico Kafka chamado "obitosCv19".

#### Observações

- O código utiliza recursos do Hive para persistir os dados e possibilitar análises futuras.
- O Apache Kafka é utilizado para o streaming de dados em tempo real.

#### Importante

É necessário ter as dependências e pacotes do ambiente Spark configurados corretamente para a execução deste código.



## Execução do Projeto

A execução do projeto envolve várias etapas, que são detalhadas abaixo:

### 5.1. Enviar os dados para o hdfs
Para realizar a análise dos dados da Covid-19, o primeiro passo do projeto é enviar os arquivos de dados para o HDFS (Hadoop Distributed File System). Essa etapa é fundamental para que o Spark possa acessar e processar os dados de forma distribuída.

O código utiliza comandos do Hadoop (por meio do utilitário hdfs dfs) para criar diretórios no HDFS destinados a receber os arquivos de dados. Em seguida, os arquivos CSV contendo informações sobre a pandemia são enviados para esses diretórios recém-criados.

Os comandos para criar os diretórios e enviar os arquivos são executados no ambiente de execução do Jupyter Notebook e interagem diretamente com o HDFS, garantindo que os dados necessários para a análise estejam disponíveis no local apropriado.

Após a conclusão desta etapa, os dados estão prontos para serem carregados nos DataFrames Spark e, assim, dar continuidade à análise dos indicadores da Covid-19. A transferência dos arquivos para o HDFS é um processo crucial para possibilitar a escalabilidade e o processamento paralelo do Spark em grandes volumes de dados.

```python
# criando diretório pai HDFS para o projeto 
!hdfs dfs -mkdir -p /projeto-final

# criando diretório para receber os arquivos de dados
!hdfs dfs -mkdir /projeto-final/dados

# Listando diretórios pai no HDFS
!hdfs dfs -ls /

# Enviando arquivos para hdfs na pasta criada
!hdfs dfs -put HIST_PAINEL_COVIDBR_2020_Parte1_06jul2021.csv /projeto-final/dados
!hdfs dfs -put HIST_PAINEL_COVIDBR_2020_Parte2_06jul2021.csv /projeto-final/dados
!hdfs dfs -put HIST_PAINEL_COVIDBR_2021_Parte1_06jul2021.csv /projeto-final/dados
!hdfs dfs -put HIST_PAINEL_COVIDBR_2021_Parte2_06jul2021.csv /projeto-final/dados

# checando arquivos enviados na pasta alvo
!hdfs dfs -ls /projeto-final/dados
```

Esses comandos utilizam a interface da linha de comando do Hadoop (hdfs dfs) para criar diretórios e enviar os arquivos HIST_PAINEL_COVIDBR_*.csv para a pasta /projeto-final/dados no HDFS. O ponto de exclamação (!) no início de cada linha indica que esses comandos são executados diretamente no shell do ambiente do Jupyter Notebook.

Abaixo, a imagem mostrando o resultado do envio dos dados para HDFS : 
![Dados no HDFS](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2001.png)

### 5.2. Otimizar todos os dados do hdfs para uma tabela Hive particionada por município
Para otimizar os dados do HDFS e criar uma tabela Hive particionada por município, precisamos realizar as seguintes etapas no código:

    1. Combinar os DataFrames individuais em um único DataFrame usando a função union.
    2. Salvar o DataFrame resultante no formato Hive particionado por município.

A seguir, o código que realiza essas etapas:
```python
# Carregar cada arquivo CSV em um DataFrame
df1 = spark.read.csv("/projeto-final/dados/HIST_PAINEL_COVIDBR_2020_Parte1_06jul2021.csv", header=True, sep=';')
df2 = spark.read.csv("/projeto-final/dados/HIST_PAINEL_COVIDBR_2020_Parte2_06jul2021.csv", header=True, sep=';')
df3 = spark.read.csv("/projeto-final/dados/HIST_PAINEL_COVIDBR_2021_Parte1_06jul2021.csv", header=True, sep=';')
df4 = spark.read.csv("/projeto-final/dados/HIST_PAINEL_COVIDBR_2021_Parte2_06jul2021.csv", header=True, sep=';')

# Combinando os DataFrames em um único DataFrame spark
df_total = df1.union(df2).union(df3).union(df4)

# Popular a tabela Hive criada com os dados do DataFrame spark (particionado por município)
df_total.write.partitionBy("municipio").saveAsTable("covid_por_local", mode="overwrite")
```
O código acima carrega cada arquivo CSV em um DataFrame individual e, em seguida, combina todos esses DataFrames em um único DataFrame chamado df_total usando a função union. Em seguida, ele escreve o DataFrame df_total como uma tabela Hive chamada covid_por_local, particionada por município. A opção mode="overwrite" garante que a tabela será recriada caso já exista.

Abaixo, a imagem mostrando o resultado : 
![img2](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2002.png)

### 5.3. Criar as 3 visualizações pelo Spark com os dados enviados para o HDFS
Neste item 5.3, são apresentadas as etapas para criar as três visualizações utilizando o Spark com os dados previamente enviados para o HDFS (Hadoop Distributed File System). O projeto utiliza diversas tecnologias e serviços, como o Apache Spark para processamento e análise de dados, HDFS para armazenamento distribuído, Hive para a criação de tabelas particionadas, além do uso do Kafka para a criação de um tópico para os dados resultantes da terceira visualização.

As etapas do item 5.3 é dividida em :

#### 5.3.1 Criação da Visualização 1
Nesta etapa, o código calcula os KPIs (Indicadores-Chave de Desempenho) de "Casos Recuperados" e "Em Acompanhamento" a partir do DataFrame df_total. Em seguida, exibe os valores calculados na saída padrão.
 ```python
# Calcular os KPIs
kpi_recuperados = df_total.agg(sum("recuperadosNovos")).collect()[0][0]
kpi_em_acompanhamento = df_total.agg(sum("emAcompanhamentoNovos")).collect()[0][0]

# Exibir os KPIs
print("Visualização 1 - KPIs de Casos Recuperados e Em Acompanhamento:")
print("Casos Recuperados:", kpi_recuperados)
print("Em Acompanhamento:", kpi_em_acompanhamento)

 ```

A expressão `.collect()[0][0]` é usada para extrair o resultado de uma função de agregação para obter um valor único, que é o KPI desejado, a partir do DataFrame. Portanto, os valores retornados por .collect() são usados para exibir os KPIs nas visualizações criadas no código.

Nesta etapa, o código calcula os KPIs (Indicadores-Chave de Desempenho) de "Casos Recuperados" e "Em Acompanhamento" a partir do DataFrame df_total. Em seguida, exibe os valores calculados na saída padrão.

Abaixo, a imagem mostrando o resultado : 
![img3](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2003.png)

#### 5.3.2 Criação da Visualização 2
Nesta parte, o código calcula os KPIs de "CASOS CONFIRMADOS", incluindo os valores "Acumulados", "Casos Novos" e "Incidência". Novamente, os cálculos são feitos com base nos dados contidos no DataFrame df_total, e os resultados são exibidos na saída padrão.
```python
# Calcular os KPIs
kpi_acumulados = df_total.agg(sum("casosAcumulado")).collect()[0][0]
kpi_casos_novos = df_total.agg(avg("casosNovos")).collect()[0][0]
kpi_incidencia = kpi_casos_novos / kpi_acumulados

# Exibir os KPIs
print("Visualização 2 - CASOS CONFIRMADOS:")
print("Acumulados:", kpi_acumulados)
print("Casos Novos:", kpi_casos_novos)
print("Incidência:", kpi_incidencia)
```

Abaixo, a imagem mostrando o resultado : 
![img4](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2004.png)

#### 5.3.3 Criação da Visualização 3
Aqui, o código calcula os KPIs relacionados aos "ÓBITOS CONFIRMADOS", incluindo os valores de "Óbitos Acumulados", "Óbitos Novos", "Letalidade" e "Mortalidade". Os cálculos são realizados usando os dados do DataFrame df_total, e os resultados são exibidos na saída padrão.
```python
# Calcular os KPIs
kpi_obitos_acumulados = df_total.agg(sum("obitosAcumulado")).collect()[0][0]
kpi_obitos_novos = df_total.agg(avg("obitosNovos")).collect()[0][0]
kpi_letalidade = (kpi_obitos_acumulados / kpi_acumulados) * 100
kpi_mortalidade = kpi_obitos_acumulados / kpi_acumulados

# Exibir os KPIs
print("Visualização 3 - ÓBITOS CONFIRMADOS:")
print("Óbitos Acumulados:", kpi_obitos_acumulados)
print("Óbitos Novos:", kpi_obitos_novos)
print("Letalidade (%):", kpi_letalidade)
print("Mortalidade:", kpi_mortalidade)

```

Abaixo, a imagem mostrando o resultado : 
![img5](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2005.png)

### 5.4 Salvar a Visualização 1 em Tabela Hive
Após a criação da Visualização 1, os resultados dos KPIs de "casos recuperados" e "em acompanhamento" são salvos em uma tabela Hive chamada "covid19_recuperados_acompanhamento". A tabela Hive é uma tabela particionada que armazena os dados no HDFS em um formato estruturado e otimizado para consultas.
```python
# Salvar a primeira visualização como tabela Hive
df_total.select(sum("recuperadosNovos").alias("totalRecuperadosNovos"), sum("emAcompanhamentoNovos").alias("totalEmAcompanhamentoNovos")).write.saveAsTable("covid19_recuperados_acompanhamento", mode="overwrite")

```
Abaixo, a imagem mostrando o resultado : 
<IMAGEM 0006>

### 5.5 Salvar a Segunda Visualização como Formato Parquet com Compressão Snappy
Nesta etapa, a Visualização 2 é salva em formato Parquet com compressão Snappy. O DataFrame kpi_visualizacao2 é criado com os resultados dos KPIs calculados anteriormente. O formato Parquet é um formato de armazenamento colunar altamente eficiente para grandes conjuntos de dados, e a compressão Snappy é utilizada para reduzir o espaço de armazenamento ocupado pelos dados.
```python
# Criar DataFrame com os KPIs da segunda visualização
kpi_visualizacao2 = spark.createDataFrame([(kpi_acumulados, kpi_casos_novos, kpi_incidencia)], ["casos_acumulados", "casos_novos", "incidencia"])

# Salvar como formato Parquet com compressão Snappy
kpi_visualizacao2.write.parquet("user/kpi_visualizacao2.parquet", compression="snappy", mode="overwrite")

df_viz2 = spark.read.parquet("user/kpi_visualizacao2.parquet")
df_viz2.show()
```

Abaixo, a imagem mostrando o resultado : 
<IMAGEM 0007>


### 5.6 Envio dos dados para um tópico Kafka (via terminal)

Essa etapa se divide em duas partes::

    1. Criação do tópico Kafka (via terminal)
    2. Envio dos dados para o tópico criado via interface gráfica (Confluent.)

#### 5.6.1 Criação do tópico Kafka (via terminal)
O código executa um comando no terminal para criar um tópico Kafka chamado "obitoscvd19". O tópico é configurado com uma partição e um fator de replicação. O Kafka é um sistema de mensagens distribuído que permite a troca de dados entre diversas aplicações em tempo real, sendo muito utilizado para streaming de eventos.

```
kafka-topics --bootstrap-server localhost:9092 --topic obitoscvd19 --create --partitions 1 --replication-factor 1
```

Abaixo, a imagem mostrando o resultado : 
<IMAGEM 0008>

#### 5.6.2 Envio dos dados para o tópico Kafka (via Confluent)
Durante o desenvolvimento do projeto, a proposta inicial era utilizar o Jupyter Notebook integrado com o Kafka para o envio dos dados. No entanto, após diversas tentativas, não foi possível realizar o envio dos dados pelo Jupyter Notebook para o Kafka. Diante desse impasse, uma alternativa foi adotada para garantir o prosseguimento do projeto.

A solução encontrada consistiu em utilizar o KSQLDB para criar os dados e enviá-los para o tópico Kafka previamente criado. Para isso, foi criada uma mensagem contendo os KPIs (Indicadores-Chave de Desempenho) esperados para a Visualização 3. Os KPIs incluídos na mensagem foram:

"Visualização": ÓBITOS CONFIRMADOS
"Óbitos Acumulados": 274784085.0
"Óbitos Novos": 0.6021753615221359
"Letalidade (%)": 2.74826075895997
"Mortalidade": 0.0274826075895997


Esses dados foram estruturados conforme o schema definido e enviados para o tópico "obitosCv19" utilizando comandos do KSQLDB.

1. Criar a STREAM no KSQL: No editor KSQL, foi criada a STREAM `stream_obitos_confirmados` para receber os dados da Visualização 3 - ÓBITOS CONFIRMADOS para armazenar os dados.

```sql
CREATE STREAM stream_obitos_confirmados (
    "Visualização" VARCHAR,
    "Óbitos Acumulados" DOUBLE,
    "Óbitos Novos" DOUBLE,
    "Letalidade (%)" DOUBLE,
    "Mortalidade" DOUBLE
) WITH (
    KAFKA_TOPIC='obitoscvd19',
    VALUE_FORMAT='JSON'
);
```

2. Inserir os dados na STREAM criada: No editor KSQL, foram inseridos os dados da Visualização 3 - ÓBITOS CONFIRMADOS na STREAM usando o comando INSERT INTO. 
```sql
INSERT INTO stream_obitos_confirmados (
    "Visualização",
    "Óbitos Acumulados",
    "Óbitos Novos",
    "Letalidade (%)",
    "Mortalidade"
) VALUES (
    'ÓBITOS CONFIRMADOS',
    274784085.0,
    0.6021753615221359,
    2.74826075895997,
    0.0274826075895997
);
```
Com isso, o tópico kafka `obitoscvd19` está conectado ao stream `stream_obitos_confirmados`.

Embora tenha enfrentado dificuldades com a integração inicial do Jupyter Notebook com o Kafka, a abordagem adotada com o KSQLDB permitiu avançar no projeto e garantir a disponibilidade dos dados na plataforma Kafka. Futuramente, será possível reavaliar a integração com o Jupyter Notebook para tornar o processo ainda mais eficiente e otimizado.

<IMAGEM 0009>
<IMAGEM 0010>

Para checar os dados no tópico Kafka, foi utilizado o seguinte comando (via terminal) `kafka-console-consumer --topic obitoscvd19 --bootstrap-server localhost:9092 --from-beginning` e obtido o resultado : 

<IMAGEM 0011>

### 5.7 Criar a visualização com os dados enviados para o HDFS (via Spark):

O código abaixo utiliza o Apache Spark para criar uma visualização dos dados armazenados no HDFS, relacionados aos casos e óbitos da COVID-19, apresentando informações relevantes para diferentes regiões e municípios.

```python
# Agrupamento e Agregação de Dados
viz_spark = covid_por_local.groupBy('regiao', 'municipio') \
    .agg(
        sum('casos_novos').alias('casos'),
        sum('obitos_novos').alias('obitos'),
        last('data').alias('atualizacao'),
        last('populacao').alias('agg_pop'),
    ) \
    .withColumn('incidencia', round((col('casos') * 100_000) / col('agg_pop'), 1)) \
    .withColumn('mortalidade', round((col('obitos') * 100_000) / col('agg_pop'), 1)) \
    .select('regiao', 'municipio', 'casos', 'obitos', 'incidencia', 'mortalidade', 'atualizacao') \
    .orderBy(col('municipio').asc_nulls_first(), 'regiao')

# Exibição dos Resultados
viz_spark.show()
```
Explicação do código:
    
    1. Agrupamento e Agregação de Dados: O código utiliza o método groupBy para agrupar os dados por 'regiao' e 'municipio', e a função de agregação sum para calcular a soma dos 'casos_novos' e 'obitos_novos' para cada grupo. Também é utilizado o método last para obter a última data de atualização e a última população registrada para cada grupo.

    2. Cálculo da Incidência e Mortalidade: Utilizando a função withColumn, o código calcula a incidência e a mortalidade por 100.000 habitantes para cada grupo. A incidência é calculada pela fórmula (casos confirmados * 100.000) / população, e a mortalidade é calculada pela fórmula (óbitos * 100.000) / população.

    3. Seleção e Ordenação dos Campos: O código seleciona os campos 'regiao', 'municipio', 'casos', 'obitos', 'incidencia', 'mortalidade' e 'atualizacao' do DataFrame resultante e os organiza em ordem crescente de 'municipio'.

    4. Exibição dos Resultados: Por fim, o código utiliza o método show() para exibir os resultados da visualização no formato de tabela, mostrando as informações dos casos, óbitos, incidência, mortalidade e a data de atualização para cada região e município.

Essa visualização pode fornecer insights valiosos para análise da evolução da COVID-19 em diferentes regiões e municípios, permitindo a identificação de padrões e tendências ao longo do tempo. Além disso, o cálculo da incidência e mortalidade por 100.000 habitantes ajuda a comparar a situação entre diferentes localidades, levando em consideração o tamanho da população de cada região.

Abaixo, o resultado : 
<IMAGEM 0012>

### 5.8. Enviar os dados da visualização 3 para um tópico Elastic



## Conclusão

O projeto proporciona uma experiência prática na utilização de ferramentas como Spark, Hive, Kafka e Elastic para análise e visualização de dados relacionados à campanha de vacinação contra a Covid-19. Ao final do projeto, os participantes terão desenvolvido soluções criativas e poderão compartilhar suas realizações no GitHub, criando um repositório organizado e documentado.

## Contato

Para qualquer dúvida ou interesse em contribuir com o projeto, entre em contato através do seguinte email: [cithsantos@gmail.com](mailto:cithsantos@gmail.com)
