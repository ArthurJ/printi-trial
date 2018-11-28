# Printi

## Arquitetura
A imagem Printi_arch.png ilustra a arquitetura que proponho.
Para o banco de dados relacional em uso, a migração dos dados poderia ser realizada por duas rotas:

#### Oracle Golden Gate
Essa solução seria ideal para importar dados estruturados para o ambiente Hadoop caso seja necessário manter em funcionamento o banco de dados em questão, por não sobrecarregar o IO do banco de dados e ainda sim manter sincronizado os dados do banco com o ambiente big data.

#### Spark
Caso o objetivo seja substituir o banco de dados, usar o Spark para acessar as tabelas e importar os dados para o ambiente Hadoop é a melhor opção.

Para outras fontes de dados, como web services e páginas web, as estratégias podem variar, eu pensaria nas seguintes ferramentas:

#### Kafka
Tanto no caso de web services quanto páginas web, o Kafka poderia ser uma ferramenta interessante no processo de importação, sendo usado como fila para salvar dados diretamente no HDFS como arquivos.

#### Script Python
Para web services próprios, escrever um código que consuma da API desses serviços é uma boa opção, e usando Kafka como meio é possível que multiplas instâncias desse código trabalhem simultaneamente de forma mais simples.

#### Scrapy
Para páginas web ou mesmo APIs web externas, o scrapy é interessante por possibilitar ajuste fino na concorrência das requisições e definição de estratégias em caso de erros HTTP, além de facilitar a criação de pipelines para tratamento das requisições.

## Código
O código requisitado não tem disponibilização dos dados em ambiente de cloud, nem ajuste de gatilhos para execução do carregamento dos dados.

#### Gatilhos
Como gatilho, eu usaria o Airflow se houver muitas condições a serem avaliadas para iniciar ou não o processo de carregamento.
Caso o carregamento de dados seja uma atividade recorrente baseada apenas no tempo, usar o Crontab do Linux e o logrotate é uma opção mais simples e razoávelmente confiável, apesar de que essa escolha implica em menor confiança, há risco de que o computador pare de funcionar por qualquer motívo e não execute a atividade.

#### Cloud e Acesso aos dados
O ambiente de cloud que eu pensaria é o S3, por já ser a escolha da empresa, juntamente com o Athena da AWS.
