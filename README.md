# Projeto Final de Spark - Campanha Nacional de Vacinação contra Covid-19

## Overview do Projeto e Objetivos
O projeto "Análise e Visualização de Dados da COVID-19" teve como objetivo coletar, processar, armazenar e visualizar dados relacionados à pandemia da COVID-19. Utilizando diversas tecnologias e ferramentas, buscamos fornecer insights valiosos sobre casos e óbitos confirmados em diferentes regiões e municípios. O projeto abrangeu as seguintes etapas:

   1. Coleta de Dados: Foi realizada a coleta de dados da COVID-19 através de datasets do DataSUS utilizando a linguagem PySpark.

   2. Processamento e Transformação de Dados: Os dados coletados foram processados e transformados usando o Apache Spark para prepará-los para a análise.

   3. Armazenamento dos Dados: Os dados foram armazenados no Hadoop Distributed File System (HDFS) e também em tabelas gerenciadas pelo Hive para facilitar consultas e análises posteriores.

   4. Visualização dos Dados: Foram criados KPIs para cada visualização, permitindo uma análise mais clara dos indicadores-chave de desempenho relacionados à COVID-19.

   5. Integração com o Kafka e Elasticsearch: Foi configurado o Apache Kafka para possibilitar o streaming de dados e a integração com o Elasticsearch para indexar e disponibilizar os dados em tempo real.

Cinthia Santos - Engenheira de Dados - [LINKEDIN](https://www.linkedin.com/in/cinthialpsantos/)

## Sumário

1. [Overview do Projeto e Objetivos](#overview-do-projeto-e-objetivos)
2. [Sobre o Projeto](#sobre-o-projeto)
3. [Sobre os Dados](#sobre-os-dados)
4. [Sobre os Arquivos](#sobre-os-arquivos)
5. [Execução do Projeto](#execução-do-projeto)
   - 5.1. [Enviar os dados para o hdfs](#51-enviar-os-dados-para-o-hdfs)
   - 5.2. [Otimizar todos os dados do hdfs para uma tabela Hive particionada por município](#52-otimizar-todos-os-dados-do-hdfs-para-uma-tabela-hive-particionada-por-município)
   - 5.3. [Criar as 3 visualizações pelo Spark com os dados enviados para o HDFS](#53-criar-as-3-visualizações-pelo-spark-com-os-dados-enviados-para-o-hdfs)
     - 5.3.1 [Criação da Visualização 1](#531-cria%C3%A7%C3%A3o-da-visualiza%C3%A7%C3%A3o-1)
     - 5.3.2 [Criação da Visualização 2](#532-cria%C3%A7%C3%A3o-da-visualiza%C3%A7%C3%A3o-2)
     - 5.3.3 [Criação da Visualização 3](#533-cria%C3%A7%C3%A3o-da-visualiza%C3%A7%C3%A3o-3)
   - 5.4. [Salvar a Visualização 1 em Tabela Hive](#54-salvar-a-visualiza%C3%A7%C3%A3o-1-em-tabela-hive)
   - 5.5. [Salvar a Segunda Visualização como Formato Parquet com Compressão Snappy](#55-salvar-a-segunda-visualiza%C3%A7%C3%A3o-como-formato-parquet-com-compress%C3%A3o-snappy)
   - 5.6. [Envio dos dados para um tópico Kafka (via terminal)](#56-envio-dos-dados-para-um-t%C3%B3pico-kafka-via-terminal)
     - 5.6.1 [Criação do tópico Kafka (via terminal)](#561-cria%C3%A7%C3%A3o-do-t%C3%B3pico-kafka-via-terminal)
     - 5.6.2 [Envio dos dados para o tópico Kafka (via Confluent)](#562-envio-dos-dados-para-o-t%C3%B3pico-kafka-via-confluent)
   - 5.7. [Criar a visualização com os dados enviados para o HDFS (via Spark):](#57-criar-a-visualiza%C3%A7%C3%A3o-com-os-dados-enviados-para-o-hdfs-via-spark)
   - 5.8. [Enviar os dados da visualização 3 para um tópico Elastic](#58-enviar-os-dados-da-visualiza%C3%A7%C3%A3o-3-para-um-t%C3%B3pico-elastic)
      - 5.8.1 [Configuração do Conector para o Elasticsearch](#581-configura%C3%A7%C3%A3o-do-conector-para-o-elasticsearch)
      - 5.8.2 [Criação do índice](#582-cria%C3%A7%C3%A3o-do-%C3%ADndice)) 
      - 5.8.3 [Indexação de Dados no Elasticsearch](#583-indexação-de-dados-no-elasticsearch)
      - 5.8.4 [Checagem no Kibana](#584-checagem-no-kibana) 

6. [Conclusão](#conclusão)
7. [Contato](#contato)

## Sobre o Projeto
[voltar para sumário](#sum%C3%A1rio)

O projeto final de Spark é a conclusão do treinamento para colocar em prática os conhecimentos adquiridos de Big Data Engineer, trabalhando com dados reais da Campanha Nacional de Vacinação contra Covid-19. A solução será feita usando Spark , Hive , Kafka , Jupyter Notebook , Docker/Docker-compose , Elastic e Kibana.

O projeto foi desenvolvido para fornecer uma análise abrangente e em tempo real dos casos e óbitos da COVID-19 em diferentes regiões e municípios. O uso do Apache Spark permitiu processar e transformar grandes volumes de dados de forma eficiente, enquanto o HDFS e o Hive foram escolhidos para o armazenamento dos dados, garantindo escalabilidade e desempenho.

No entanto, durante o desenvolvimento, algumas dificuldades foram encontradas:

- Integração do Jupyter Notebook com o Kafka: Inicialmente, houve desafios para integrar o Jupyter Notebook ao Kafka para o envio de dados. Após várias tentativas, foi escolhido utilizar o KSQLDB para criar os dados e enviá-los para o tópico Kafka, o que permitiu contornar o problema e avançar no projeto.

- Indexação de Dados no Elasticsearch: Ao criar o índice no Elasticsearch para visualização dos dados, foi observadp que os documentos estavam vazios. Para resolver esse problema, o índice foi excluído e recriado com os mapeamentos corretos, o que possibilitou a indexação adequada dos dados.


## Sobre os Dados
[voltar para sumário](#sum%C3%A1rio)

Os dados utilizados no projeto estão disponíveis em um arquivo compactado, cujo link é fornecido abaixo. Esses dados são referentes ao Painel Geral da Covid-19 no Brasil e estão disponíveis no site oficial do governo brasileiro sobre a pandemia.

Link dos Dados: [Dados do Painel Geral da Covid-19 - 06 de julho de 2021](https://mobileapps.saude.gov.br/esusvepi/files/unAFkcaNDeXajurGB7LChj8SgQYS2ptm/04bd3419b22b9cc5c6efac2c6528100d_HIST_PAINEL_COVIDBR_06jul2021.rar)

Os dados da Campanha Nacional de Vacinação contra Covid-19 estão disponíveis em formato CSV com as seguintes colunas:

### Documentação Técnica dos dados
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

Neste item 5.3, são apresentadas as etapas para criar as três visualizações utilizando o Spark com os dados previamente enviados para o HDFS (Hadoop Distributed File System). O projeto utiliza diversas tecnologias e serviços, como o Apache Spark para processamento e análise de dados, HDFS para armazenamento distribuído, Hive para a criação de tabelas particionadas, além do uso do Kafka para a criação de um tópico para os dados resultantes da terceira visualização.

As etapas do item 5.3 é dividida em :

#### 5.3.1 Criação da Visualização 1
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

Após a criação da Visualização 1, os resultados dos KPIs de "casos recuperados" e "em acompanhamento" são salvos em uma tabela Hive chamada "covid19_recuperados_acompanhamento". A tabela Hive é uma tabela particionada que armazena os dados no HDFS em um formato estruturado e otimizado para consultas.
```python
# Salvar a primeira visualização como tabela Hive
df_total.select(sum("recuperadosNovos").alias("totalRecuperadosNovos"), sum("emAcompanhamentoNovos").alias("totalEmAcompanhamentoNovos")).write.saveAsTable("covid19_recuperados_acompanhamento", mode="overwrite")

```
Abaixo, a imagem mostrando o resultado : 
![img6](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2006.png)

### 5.5 Salvar a Segunda Visualização como Formato Parquet com Compressão Snappy
[voltar para sumário](#sum%C3%A1rio)

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
![img7](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2007.png)

### 5.6 Envio dos dados para um tópico Kafka (via terminal)
[voltar para sumário](#sum%C3%A1rio)

Essa etapa se divide em duas partes::

    1. Criação do tópico Kafka (via terminal)
    2. Envio dos dados para o tópico criado via interface gráfica (Confluent.)

#### 5.6.1 Criação do tópico Kafka (via terminal)
O código executa um comando no terminal para criar um tópico Kafka chamado "obitoscvd19". O tópico é configurado com uma partição e um fator de replicação. O Kafka é um sistema de mensagens distribuído que permite a troca de dados entre diversas aplicações em tempo real, sendo muito utilizado para streaming de eventos.

```
kafka-topics --bootstrap-server localhost:9092 --topic obitoscvd19 --create --partitions 1 --replication-factor 1
```

Abaixo, a imagem mostrando o resultado : 
![img8](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2008.png)

#### 5.6.2 Envio dos dados para o tópico Kafka (via Confluent)
[voltar para sumário](#sum%C3%A1rio)

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

![img9](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2009.png)
![img10](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2010.png)

Para checar os dados no tópico Kafka, foi utilizado o seguinte comando (via terminal) `kafka-console-consumer --topic obitoscvd19 --bootstrap-server localhost:9092 --from-beginning` e obtido o resultado : 

![img11](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2011.png)

### 5.7 Criar a visualização com os dados enviados para o HDFS (via Spark):
[voltar para sumário](#sum%C3%A1rio)

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
![img12](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2012.png)

### 5.8. Enviar os dados da visualização 3 para um tópico Elastic
[voltar para sumário](#sum%C3%A1rio)

Essa etapa é dividida em 4 partes : 
   1. Configuração do conector
   2. Criação de índice
   3. Indexação dos dados no Elastic
   4. Checagem no Kibana
      
#### 5.8.1 Configuração do Conector para o Elasticsearch
Para conectar o Kafka ao Elasticsearch e enviar dados para o índice "obitoscvd19", o processo foi executado da seguinte maneira:
No `Control Center` foi criada uma conexão manual ao Elasticsearch, inserindo as seguintes configurações no arquivo de propriedades do conector:

```properties
name=elastisearch-sink
topics=obitoscvd19
connector.class=io.confluent.connect.elasticsearch.ElasticsearchSinkConnector
connection.url=http://elasticsearch:9200
type.name=_doc
value.converter=org.apache.kafka.connect.json.JsonConverter
value.converter.schemas.enable=false
schema.ignore=true
key.ignore=true
behavior.on.malformed.documents=ignore 
transforms=extractValue
transforms.extractValue.type=org.apache.kafka.connect.transforms.ExtractField$Value
transforms.extractValue.field=payload
```
Abaixo mostra a conexão feita com o elasticsearch e o tópico do kafka que foi criado anteriormente:
![img13](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2013.png)


#### 5.8.2 Criação do índice
[voltar para sumário](#sum%C3%A1rio)

**Descrição do Problema**
Após a configuração do conector Elasticsearch no Control Center, o tópico "obitoscvd19" do Kafka estava sendo corretamente consumido e um índice com o mesmo nome era criado automaticamente no Elasticsearch. Porém, ao verificar o Kibana para visualizar os dados, percebeu-se que os documentos do índice estavam vazios, sem informações relevantes.

**Solução Implementada**
Para corrigir o problema de dados ausentes no Kibana, foram realizadas as seguintes etapas:

   1. Exclusão do Índice Vazio: Foi utilizado o comando "curl" no terminal para excluir o índice "obitoscvd19" previamente criado no Elasticsearch. O comando utilizado foi:
```
curl -X DELETE "http://localhost:9200/obitoscvd19"
```
   2. Criação de um Novo Índice com Mapeamentos Corretos: Após excluir o índice vazio, um novo índice com o mapeamento de schema correto foi criado para garantir a correta indexação dos dados provenientes do tópico do Kafka. O comando "curl" utilizado para criar o novo índice foi:
```
curl -X PUT "http://localhost:9200/obitoscvd19" -H "Content-Type: application/json" -d '{
  "settings": {
    "index": {
      "number_of_shards": "1",
      "number_of_replicas": "1"
    }
  },
  "mappings": {
    "properties": {
      "Visualização": { "type": "text" },
      "Óbitos Acumulados": { "type": "float" },
      "Óbitos Novos": { "type": "float" },
      "Letalidade (%)": { "type": "float" },
      "Mortalidade": { "type": "float" }
    }
  }
}'

```

#### 5.8.3 Indexação de Dados no Elasticsearch
[voltar para sumário](#sum%C3%A1rio)

Durante o desenvolvimento do projeto, a proposta inicial era utilizar o Jupyter Notebook integrado com o Elastic para o envio dos dados. No entanto, após diversas tentativas com criação de conector e refazer o índice, não foi possível realizar o envio dos dados pelo Jupyter Notebook. Diante desse impasse, uma alternativa foi adotada para garantir o prosseguimento do projeto:

- A indexação dos dados no Elasticsearch foi realizada usando o comando "curl" para enviar uma solicitação HTTP POST com o documento e dados a ser indexado. O comando "cURL" utilizado foi o seguinte:
```
curl -X POST "http://localhost:9200/obitoscvd19/_doc" -H "Content-Type: application/json" -d '{
  "Visualização": "ÓBITOS CONFIRMADOS",
  "Óbitos Acumulados": 274784085.0,
  "Óbitos Novos": 0.6021753615221359,
  "Letalidade (%)": 2.74826075895997,
  "Mortalidade": 0.0274826075895997
}'
```
Checando os dados no elastic :
![img14](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2014.png)


#### 5.8.4 Checagem no Kibana
[voltar para sumário](#sum%C3%A1rio)

No Kibana, foi verificado que o tópico nele criado (```obitoscvd19```) tem um dcoumento indexado, conforme feito no passo 5.8.3 :
![img15](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2015.png)

E também é possível verificar os dados pelo Kibana > Discover:
![img16](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2016.png)


## Conclusão
[voltar para sumário](#sum%C3%A1rio)

O projeto proporciona uma experiência prática na utilização de ferramentas como Spark, Hive, Kafka e Elastic para análise e visualização de dados relacionados à campanha de vacinação contra a Covid-19. Ao final do projeto, os participantes terão desenvolvido soluções criativas e poderão compartilhar suas realizações no GitHub, criando um repositório organizado e documentado.

## Contato

Para qualquer dúvida ou interesse em contribuir com o projeto, entre em contato através do seguinte email: [cithsantos@gmail.com](mailto:cithsantos@gmail.com)
# Projeto Final de Spark - Campanha Nacional de Vacinação contra Covid-19

## Overview do Projeto e Objetivos

Este projeto final de Spark tem como objetivo criar visualizações de dados relacionados à Campanha Nacional de Vacinação contra Covid-19. O projeto foi dividido em dois níveis: básico e avançado, onde o foco principal será no nível básico. Os exercícios podem ser realizados em qualquer linguagem de programação, proporcionando liberdade criativa e diferentes abordagens para as soluções .

## Sumário

1. [Overview do Projeto e Objetivos](#overview-do-projeto-e-objetivos)
2. [Sobre o Projeto](#sobre-o-projeto)
3. [Sobre os Dados](#sobre-os-dados)
4. [Sobre os Arquivos](#sobre-os-arquivos)
5. [Execução do Projeto](#execução-do-projeto)
   - 5.1. [Enviar os dados para o hdfs](#51-enviar-os-dados-para-o-hdfs)
   - 5.2. [Otimizar todos os dados do hdfs para uma tabela Hive particionada por município](#52-otimizar-todos-os-dados-do-hdfs-para-uma-tabela-hive-particionada-por-município)
   - 5.3. [Criar as 3 visualizações pelo Spark com os dados enviados para o HDFS](#53-criar-as-3-visualizações-pelo-spark-com-os-dados-enviados-para-o-hdfs)
     - 5.3.1 [Criação da Visualização 1](#531-cria%C3%A7%C3%A3o-da-visualiza%C3%A7%C3%A3o-1)
     - 5.3.2 [Criação da Visualização 2](#532-cria%C3%A7%C3%A3o-da-visualiza%C3%A7%C3%A3o-2)
     - 5.3.3 [Criação da Visualização 3](#533-cria%C3%A7%C3%A3o-da-visualiza%C3%A7%C3%A3o-3)
   - 5.4. [Salvar a Visualização 1 em Tabela Hive](#54-salvar-a-visualiza%C3%A7%C3%A3o-1-em-tabela-hive)
   - 5.5. [Salvar a Segunda Visualização como Formato Parquet com Compressão Snappy](#55-salvar-a-segunda-visualiza%C3%A7%C3%A3o-como-formato-parquet-com-compress%C3%A3o-snappy)
   - 5.6. [Envio dos dados para um tópico Kafka (via terminal)](#56-envio-dos-dados-para-um-t%C3%B3pico-kafka-via-terminal)
     - 5.6.1 [Criação do tópico Kafka (via terminal)](#561-cria%C3%A7%C3%A3o-do-t%C3%B3pico-kafka-via-terminal)
     - 5.6.2 [Envio dos dados para o tópico Kafka (via Confluent)](#562-envio-dos-dados-para-o-t%C3%B3pico-kafka-via-confluent)
   - 5.7. [Criar a visualização com os dados enviados para o HDFS (via Spark):](#57-criar-a-visualiza%C3%A7%C3%A3o-com-os-dados-enviados-para-o-hdfs-via-spark)
   - 5.8. [Enviar os dados da visualização 3 para um tópico Elastic](#58-enviar-os-dados-da-visualiza%C3%A7%C3%A3o-3-para-um-t%C3%B3pico-elastic)
      - 5.8.1 [Configuração do Conector para o Elasticsearch](#581-configura%C3%A7%C3%A3o-do-conector-para-o-elasticsearch)
      - 5.8.2 [Criação do índice](#582-cria%C3%A7%C3%A3o-do-%C3%ADndice)) 
      - 5.8.3 [Indexação de Dados no Elasticsearch](#583-indexação-de-dados-no-elasticsearch)
      - 5.8.4 [Checagem no Kibana](#584-checagem-no-kibana) 

6. [Conclusão](#conclusão)
7. [Contato](#contato)

## Sobre o Projeto
[voltar para sumário](#sum%C3%A1rio)

O projeto final de Spark é uma oportunidade para os participantes colocarem em prática os conhecimentos adquiridos no treinamento, trabalhando com dados reais da Campanha Nacional de Vacinação contra Covid-19. O desafio está dividido em nível básico e avançado. A solução será feita usando Spark , Hive , Kafka , Jupyter Notebook , Docker/Docker-compose , Elastic e Kibana.

## Sobre os Dados
[voltar para sumário](#sum%C3%A1rio)

Os dados utilizados no projeto estão disponíveis em um arquivo compactado, cujo link é fornecido abaixo. Esses dados são referentes ao Painel Geral da Covid-19 no Brasil e estão disponíveis no site oficial do governo brasileiro sobre a pandemia.

Link dos Dados: [Dados do Painel Geral da Covid-19 - 06 de julho de 2021](https://mobileapps.saude.gov.br/esusvepi/files/unAFkcaNDeXajurGB7LChj8SgQYS2ptm/04bd3419b22b9cc5c6efac2c6528100d_HIST_PAINEL_COVIDBR_06jul2021.rar)

Os dados da Campanha Nacional de Vacinação contra Covid-19 estão disponíveis em formato CSV com as seguintes colunas:

### Documentação Técnica dos dados
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

Neste item 5.3, são apresentadas as etapas para criar as três visualizações utilizando o Spark com os dados previamente enviados para o HDFS (Hadoop Distributed File System). O projeto utiliza diversas tecnologias e serviços, como o Apache Spark para processamento e análise de dados, HDFS para armazenamento distribuído, Hive para a criação de tabelas particionadas, além do uso do Kafka para a criação de um tópico para os dados resultantes da terceira visualização.

As etapas do item 5.3 é dividida em :

#### 5.3.1 Criação da Visualização 1
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

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
[voltar para sumário](#sum%C3%A1rio)

Após a criação da Visualização 1, os resultados dos KPIs de "casos recuperados" e "em acompanhamento" são salvos em uma tabela Hive chamada "covid19_recuperados_acompanhamento". A tabela Hive é uma tabela particionada que armazena os dados no HDFS em um formato estruturado e otimizado para consultas.
```python
# Salvar a primeira visualização como tabela Hive
df_total.select(sum("recuperadosNovos").alias("totalRecuperadosNovos"), sum("emAcompanhamentoNovos").alias("totalEmAcompanhamentoNovos")).write.saveAsTable("covid19_recuperados_acompanhamento", mode="overwrite")

```
Abaixo, a imagem mostrando o resultado : 
![img6](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2006.png)

### 5.5 Salvar a Segunda Visualização como Formato Parquet com Compressão Snappy
[voltar para sumário](#sum%C3%A1rio)

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
![img7](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2007.png)

### 5.6 Envio dos dados para um tópico Kafka (via terminal)
[voltar para sumário](#sum%C3%A1rio)

Essa etapa se divide em duas partes::

    1. Criação do tópico Kafka (via terminal)
    2. Envio dos dados para o tópico criado via interface gráfica (Confluent.)

#### 5.6.1 Criação do tópico Kafka (via terminal)
O código executa um comando no terminal para criar um tópico Kafka chamado "obitoscvd19". O tópico é configurado com uma partição e um fator de replicação. O Kafka é um sistema de mensagens distribuído que permite a troca de dados entre diversas aplicações em tempo real, sendo muito utilizado para streaming de eventos.

```
kafka-topics --bootstrap-server localhost:9092 --topic obitoscvd19 --create --partitions 1 --replication-factor 1
```

Abaixo, a imagem mostrando o resultado : 
![img8](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2008.png)

#### 5.6.2 Envio dos dados para o tópico Kafka (via Confluent)
[voltar para sumário](#sum%C3%A1rio)

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

![img9](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2009.png)
![img10](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2010.png)

Para checar os dados no tópico Kafka, foi utilizado o seguinte comando (via terminal) `kafka-console-consumer --topic obitoscvd19 --bootstrap-server localhost:9092 --from-beginning` e obtido o resultado : 

![img11](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2011.png)

### 5.7 Criar a visualização com os dados enviados para o HDFS (via Spark):
[voltar para sumário](#sum%C3%A1rio)

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
![img12](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2012.png)

### 5.8. Enviar os dados da visualização 3 para um tópico Elastic
[voltar para sumário](#sum%C3%A1rio)

Essa etapa é dividida em 4 partes : 
   1. Configuração do conector
   2. Criação de índice
   3. Indexação dos dados no Elastic
   4. Checagem no Kibana
      
#### 5.8.1 Configuração do Conector para o Elasticsearch
Para conectar o Kafka ao Elasticsearch e enviar dados para o índice "obitoscvd19", o processo foi executado da seguinte maneira:
No `Control Center` foi criada uma conexão manual ao Elasticsearch, inserindo as seguintes configurações no arquivo de propriedades do conector:

```properties
name=elastisearch-sink
topics=obitoscvd19
connector.class=io.confluent.connect.elasticsearch.ElasticsearchSinkConnector
connection.url=http://elasticsearch:9200
type.name=_doc
value.converter=org.apache.kafka.connect.json.JsonConverter
value.converter.schemas.enable=false
schema.ignore=true
key.ignore=true
behavior.on.malformed.documents=ignore 
transforms=extractValue
transforms.extractValue.type=org.apache.kafka.connect.transforms.ExtractField$Value
transforms.extractValue.field=payload
```
Abaixo mostra a conexão feita com o elasticsearch e o tópico do kafka que foi criado anteriormente:
![img13](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2013.png)


#### 5.8.2 Criação do índice
[voltar para sumário](#sum%C3%A1rio)

**Descrição do Problema**
Após a configuração do conector Elasticsearch no Control Center, o tópico "obitoscvd19" do Kafka estava sendo corretamente consumido e um índice com o mesmo nome era criado automaticamente no Elasticsearch. Porém, ao verificar o Kibana para visualizar os dados, percebeu-se que os documentos do índice estavam vazios, sem informações relevantes.

**Solução Implementada**
Para corrigir o problema de dados ausentes no Kibana, foram realizadas as seguintes etapas:

   1. Exclusão do Índice Vazio: Foi utilizado o comando "curl" no terminal para excluir o índice "obitoscvd19" previamente criado no Elasticsearch. O comando utilizado foi:
```
curl -X DELETE "http://localhost:9200/obitoscvd19"
```
   2. Criação de um Novo Índice com Mapeamentos Corretos: Após excluir o índice vazio, um novo índice com o mapeamento de schema correto foi criado para garantir a correta indexação dos dados provenientes do tópico do Kafka. O comando "curl" utilizado para criar o novo índice foi:
```
curl -X PUT "http://localhost:9200/obitoscvd19" -H "Content-Type: application/json" -d '{
  "settings": {
    "index": {
      "number_of_shards": "1",
      "number_of_replicas": "1"
    }
  },
  "mappings": {
    "properties": {
      "Visualização": { "type": "text" },
      "Óbitos Acumulados": { "type": "float" },
      "Óbitos Novos": { "type": "float" },
      "Letalidade (%)": { "type": "float" },
      "Mortalidade": { "type": "float" }
    }
  }
}'

```

#### 5.8.3 Indexação de Dados no Elasticsearch
[voltar para sumário](#sum%C3%A1rio)

Durante o desenvolvimento do projeto, a proposta inicial era utilizar o Jupyter Notebook integrado com o Elastic para o envio dos dados. No entanto, após diversas tentativas com criação de conector e refazer o índice, não foi possível realizar o envio dos dados pelo Jupyter Notebook. Diante desse impasse, uma alternativa foi adotada para garantir o prosseguimento do projeto:

- A indexação dos dados no Elasticsearch foi realizada usando o comando "curl" para enviar uma solicitação HTTP POST com o documento e dados a ser indexado. O comando "cURL" utilizado foi o seguinte:
```
curl -X POST "http://localhost:9200/obitoscvd19/_doc" -H "Content-Type: application/json" -d '{
  "Visualização": "ÓBITOS CONFIRMADOS",
  "Óbitos Acumulados": 274784085.0,
  "Óbitos Novos": 0.6021753615221359,
  "Letalidade (%)": 2.74826075895997,
  "Mortalidade": 0.0274826075895997
}'
```
Checando os dados no elastic :
![img14](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2014.png)


#### 5.8.4 Checagem no Kibana
[voltar para sumário](#sum%C3%A1rio)

No Kibana, foi verificado que o tópico nele criado (```obitoscvd19```) tem um dcoumento indexado, conforme feito no passo 5.8.3 :
![img15](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2015.png)

E também é possível verificar os dados pelo Kibana > Discover:
![img16](https://github.com/cinthialet/spark-hive-elastic_semantix/blob/main/img/IMAGEM%2016.png)


## Conclusão
[voltar para sumário](#sum%C3%A1rio)

Ao longo do projeto, foram utilizadas diversas tecnologias e ferramentas essenciais em engenharia de dados para coletar, processar, armazenar e visualizar informações relacionadas à pandemia de COVID-19. Através dessas tecnologias, foi possível desenvolver um pipeline de dados completo, permitindo o tratamento eficiente de grandes volumes de informações e a disponibilização de insights valiosos para análise.

Durante o desenvolvimento do projeto, o Docker Compose desempenhou um papel fundamental ao facilitar a orquestração e a execução de múltiplos serviços e contêineres necessários para o pipeline de dados. Com o uso do Docker Compose, foi possível definir e gerenciar a infraestrutura necessária para o ecossistema de big data com apenas um arquivo de configuração. Essa abordagem baseada em contêineres permitiu que todo o ambiente de desenvolvimento e testes fosse reproduzido de forma consistente em diferentes máquinas.

A linguagem de programação Python foi fundamental para desenvolver scripts e processos de manipulação de dados, além de facilitar a interação com diversas bibliotecas e ferramentas.

O Apache Spark demonstrou ser uma escolha poderosa para o processamento distribuído de dados em grande escala. Com sua abordagem de computação em memória e capacidade de trabalhar com várias fontes de dados, o Spark permitiu a realização de transformações complexas e agregações em velocidade otimizada.

O Hadoop Distributed File System (HDFS) foi utilizado como sistema de arquivos distribuído, garantindo a confiabilidade, escalabilidade e tolerância a falhas no armazenamento dos dados coletados.

O Apache Hive proporcionou uma camada de abstração para consultar e analisar os dados armazenados no HDFS, permitindo uma integração suave com o ecossistema Hadoop e facilitando a execução de consultas SQL complexas.

O Apache Kafka possibilitou a criação de um sistema de mensagens distribuído, permitindo o streaming de eventos em tempo real, garantindo a integridade dos dados e a comunicação eficiente entre diferentes componentes do pipeline.

A integração entre o Elasticsearch e o Kibana permitiu a criação de visualizações interativas para analisar e monitorar os dados em tempo real. O Elasticsearch proporcionou um mecanismo de indexação eficiente e pesquisa de dados, enquanto o Kibana ofereceu ferramentas poderosas para a criação de dashboards e gráficos interativos.

Durante o desenvolvimento do projeto, foram enfrentados desafios significativos, como a configuração e integração das diversas tecnologias em um pipeline de dados coeso. Através de pesquisa, experimentação e soluções criativas, esses desafios foram superados, permitindo a implementação bem-sucedida de cada componente do projeto.

As habilidades adquiridas ao longo do projeto incluem o conhecimento prático de tecnologias de big data e processamento distribuído, a capacidade de criar e gerenciar pipelines de dados escaláveis, a habilidade de trabalhar com dados em tempo real e a competência para projetar visualizações interativas para análise.

Como pontos de melhoria para projetos futuros, destaca-se a possibilidade de aprimorar a integração do Jupyter Notebook com o Kafka e Elastic para um envio mais eficiente de dados.

## Contato

Para qualquer dúvida ou interesse em contribuir com o projeto, entre em contato através do seguinte email: [cithsantos@gmail.com](mailto:cithsantos@gmail.com)

Contato linkedin : [https://www.linkedin.com/in/cinthialpsantos/](https://www.linkedin.com/in/cinthialpsantos/))
