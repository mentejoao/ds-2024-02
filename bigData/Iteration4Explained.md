# Iteration 4

Na quarta iteração, tendo como pressuposto o nosso padrão de arquitetura Lambda, é hora de decompor a Serving Layer (ou Camada de Serviço).
Levando em conta o contexto ao qual estamos trabalhando, ela é a responsável por disponibilizar resultados processados de forma rápida para que os sistemas consumidores (como dashboards, APIs, aplicações e etc) possam acessar informações sem atrasos.

Para realizar a decomposição desse elemento, vamos escolher uma carta de família (azul) e uma de tecnologia (vermelha), analisando as constraints e 
requisitos para chegar na melhor escolha possível de acordo com o nosso contexto de sistema.

![lambdaarq](https://github.com/user-attachments/assets/37053cbb-5a65-4b60-856b-9f52a7c53a4a)

# Premissas
### Architecture Drivers
Para essa iteração em específico, como drivers de arquitetura, temos:
* Quality Atributes
```cpp  
Performance -> Q2: "The system shall provide static reports over historical data
                    (< 5 sec report load time) for Product and IT Managers."


Performance -> Q3: "The system shall provide ad-hoc analysis over historical data 
                    with human-time queries (< 1 min query execution time) historical
                    for Data Analysts" 
```
* Constraints
```cpp  
Environment -> C3: "The system shall use corporate BI tool with SQL interface
                    for static reports (e.g. MicroStrategy, QlikView, Tableau)"
```
Sendo assim, para design iremos priorizar Ad-hoc Analysis e Performance, enquanto que para tecnologia vamos priorizar apenas Performance.
Como possíveis opções de desing temos Column-Family, Document-Oriented e Interactive Query Engine.

# Design
### 1. Column-Family (Blue Card)
Column-Family é um tipo de modelagem de dados usada em bancos de dados NoSQL, caracterizada por conter um conjunto de linhas,
onde cada uma delas é um conjunto de pares chave-valor chamados colunas. Podem ser apresentados como matrizes bidimensionais em que
cada chave possui um ou mais pares de valores-chave anexados a ela.

![Column-Family design preview](https://studio3t.com/wp-content/uploads/2017/12/cassandra-column-family-example-1024x608.png)

![column-family_card](https://github.com/user-attachments/assets/c9a8e586-ba59-4eae-8e68-e12d8f1d6dcb)

Em relação aos drivers analisados nessa iteração, a carta de Column-Family é avaliada como:
```python
★★★ Performance – is extremely fast due to absence of schema definition, relational,
                    transactional or referential integrity functionality

★ Ad-hoc Analysis – supports secondary indexing, but no aggregate functions
```

### 2. Document-Oriented (Blue Card)
Document-oriented é um tipo de modelagem de dados também usada em bancos de dados NoSQL. Funciona de maneira semelhante ao design
Column-Family, mas permite um aninhamento de dados muito mais profundo e armazenamento de estruturas mais complexas, isto é,
documentos, que podem ser aninhados (um documento dentro de outro documento dentro de outro documento...)

![Document oriented design preview](https://devsblog.home.blog/wp-content/uploads/2019/04/9-document-oriented-databases-11-638-1.jpg)

![document-oriented_card](https://github.com/user-attachments/assets/00db24ff-ab0a-48ea-8021-4f8ce869b8e8)

Em relação aos drivers analisados nessa iteração, a carta de Document-Oriented é avaliada como:
```python
★★ Performance – performance varies significantly from one implementation to the next,
                  but overall not as fast as Key-Value databases

★½ Ad-hoc analysis – somewhat better than other NoSQL families, but still not
                      as good as relational databases or interactive query engines
```

### 3. Interactive Query Engine (Blue Card)
O Distributed Query Processor foi desenvolvido para executar consultas analíticas interativas e em lote em fontes de dados de grande tamanho.

![Interactive Query Engine design preview](https://github.com/user-attachments/assets/dbeabd23-4914-4ed9-97b1-96053ee31e3b)

![interactive-query-engine_card](https://github.com/user-attachments/assets/728e2329-fdff-4807-a6ed-85a2cf33dd5b)

Em relação aos drivers analisados nessa iteração, a carta de Interactive Query Engine é avaliada como:
```python
★★ Performance – can query large amounts of data in human-time (2-30 seconds),
                  however still not as fast as relational data warehouse

★½ Ad-hoc analysis – somewhat better than other NoSQL families, but still not
                      as good as relational databases or interactive query engines
```

### Smart Decision 1 - Design

...

# Tecnologias
### 1. Impala (Red Card)
Nossa primeira opção de tecnologia é o Apache Impala, ele é um mecanismo de consulta SQL de processamento massivamente paralelo de código aberto para dados armazenados em um cluster de computador executando o Apache Hadoop.

![Impala_Architecture](https://github.com/user-attachments/assets/1c2dc786-0a6e-4a65-8d29-b2bd0c2765dc)

![Impala_Card](https://github.com/user-attachments/assets/394a2267-1d44-4646-b030-e2649c30afa5)

Em relação aos drivers de interesse dessa iteração, o Impala é avaliado como:
```python
★★★ Performance – considered as one of fastest technologies at the 
                    moment, significantly faster than Hive 
```

### 2. Spark SQL (Red Card)
Já em nossa segunda opção temos o Spark SQL, ele é um módulo da plataforma Apache Spark que facilita a processamento de dados estruturados e consultas SQL. Ele permite que os usuários executem consultas SQL sobre dados armazenados em diferentes fontes, como Hadoop Distributed File System (HDFS), bancos de dados SQL e data lakes. Além disso, o Spark SQL pode combinar processamento em lote e em tempo real com o suporte a grandes volumes de dados.

![Spark_SQL_Architecture](https://github.com/user-attachments/assets/978db9be-b282-4715-bfec-2f3c9d76ea06)

![Spark_SQL Card](https://github.com/user-attachments/assets/30195636-e324-43bc-976e-d4165895c109)

Em relação aos drivers de interesse dessa iteração, o Spark SQL é avaliado como:
```python
★★★ Performance – considered as one of fastest technologies at the 
                    moment, significantly faster than Hive
```

### 3. Apache Hive (Red Card)
E como nossa terceira e última alternativa temos o Apache Hive, que é um software de Data Warehouse desenvolvido em cima do Apache Hadoop para consulta e análise de dados. O Hive oferece uma interface semelhante ao SQL para consulta de dados em diferentes bancos de dados e sistemas de arquivos integrados ao Hadoop.

![Apache_Hive_Architecture](https://github.com/user-attachments/assets/73dd8920-3932-413e-8999-56c3ede291de)

![Apache_Hive Card](https://github.com/user-attachments/assets/dcf4ba2e-4c64-496d-b296-acb29f115968)


Em relação aos drivers de interesse dessa iteração, o Apache Hive é avaliado como:
```python
★½ Performance – even with Stinger initiative Hive is still slow compared 
                  to other alternatives such as Impala or Spark SQL
```

### Smart Decision 2 - Tecnologia

...
