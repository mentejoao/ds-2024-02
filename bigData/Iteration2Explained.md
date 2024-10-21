# Iteration 2

Dada a nossa escolha na primeira iteração, a respeito da Lambda Architecture, nesta iteração o elemento a ser decomposto é o Data Stream (ou Fluxo de Dados).
No nosso contexto, este fluxo de dados são os dados de logs de diferentes fontes (que são armazenados em centenas de servidores) da companhia de Internet que contratou nosso serviço (como arquitetos... chique né?).
Nesta iteração devemos escolher qual o design e a tecnologia a fim de implementar um projeto de fluxo de dados.

![image](https://github.com/user-attachments/assets/a4289cfd-0eb5-4c48-848e-f0c06aed99ed)


# Premissas
#### Architecture Drivers
Como Architecture Drivers para a segunda iteração, **desconsiderando Extensibility, Cost e Environment que são drivers para todas as iterações**, temos:

* Quality Atributes
```cpp  
Performance -> Q1: "The system shall collect 10000 raw events/sec in average from up to 300 web servers."

Compatibility -> Q6: "The system shall be composed of components that peferably integrate to each other with no

                    or miniumum custom coding." 

Reliability -> Q7: "The data collection and event delivery mechanism shall be reliable (no message loss)"
```

Como possíveis opções de design para a resolução do Fluxo de Logs temos o Data Collector e o Distributed Message Broker, aqui explanarei melhor um pouco sobre os dois.

# Design

#### 1. Data Colector (Blue Cards)
Um Data Collector é responsável por coletar, transformar e enviar dados de várias fontes para um destino centralizado (como um banco de dados, sistema de armazenamento ou um message broker). Ele funciona como um ponto de entrada para os dados e pode ser configurado para realizar tarefas como:

* Capturar dados em tempo real ou em lote.
* Realizar transformações básicas nos dados, como limpeza e formatação.
* Enviar os dados para diferentes destinos (armazenamento, processamento ou sistemas de monitoramento).
Em geral, o Data Collector é utilizado em pipelines de ETL (Extract, Transform, Load) e em sistemas de monitoramento de logs (nosso caso), sensores IoT, e outros cenários onde é necessário agregar e centralizar dados de **diversas** origens, o que está de acordo com nosso driver ```Compatibility -> Q6```.

A carta de Data Collector em relação aos nossos drivers, no jogo:
```python
★★ Performance – can handle large amounts of data in real time
★★★ Compatibility – can be plugged with popular event sources and 
destinations
```

### 2. Distributed Message Broker (Blue Cards)
Um Distributed Message Broker é um sistema que gerencia a troca de mensagens entre produtores e consumidores em um ambiente distribuído. É um descendente do Message Broker, mas de forma distribuída. Ele serve como intermediário entre serviços ou componentes de um sistema, permitindo que eles se comuniquem de maneira assíncrona e escalável.

* Mensageria Assíncrona: Permite que produtores enviem mensagens e consumidores as processem em momentos diferentes, desacoplando os sistemas.
* Distribuição de Mensagens: Redireciona e entrega mensagens para vários consumidores com base em regras e configurações (ex.: filas, tópicos).
* Tolerância a Falhas e Escalabilidade: Geralmente, esses sistemas são projetados para funcionar de forma distribuída, garantindo que as mensagens sejam entregues mesmo se algum nó falhar.

A carta de Distributed Message Broker em relação aos nossos drivers, no jogo:
```python
★★★ Performance – can handle high throughput with low latency
★ Compatibility – in most cases requires writing of custom code to be 
plugged with event producers and consumers
```

#### Smart Decision 1
Apesar da melhor performance do Distributed Message Broker, escolheremos o Data Collector para nossa iteração, já que neste trade-off entre Performance e Compatibilidade, o Data Collector oferece uma compatibilidade melhor para uma não tão ruim performance, comparada ao Distributed Message Broker. Agora, escolhido o design Data Collector, de que modo vamos implementá-lo?

# Tecnologia
### 1. Apache Flume (Red Cards)
* O Apache Flume é um serviço que permite coletar, agregar e mover grandes quantidade de dados em um ambiente distribuído. De diferentes fontes de dados, como em social media, e-mails, logs e qualquer fonte de dados possível.
* O Apache Flume foi desenvolvido pela Cloudera e é escrito em Java.
* É Open Source e é licenciado pela Apache 2.0.
  
A carta de Apache Flume em relação aos nossos drivers, no jogo:
```python
★★ Performance – can handle high throughput with low latency, depends on
channel configuration (memory, file etc.), number of sinks, threads etc.
Supports horizontal scaling for performance (throughput) improvement.
★★ Reliability – uses a transactional approach to guarantee the reliable
delivery of events (via a file channel). Usage of a memory channel improves
performance, but can lead to message loss.
```
* Arquitetura de uma implementação com o Apache Flume
  
![image](https://github.com/user-attachments/assets/da06a2d5-f470-4b79-988a-d90e04cb1d1d)

Refs.:
[Apache Flume](https://flume.apache.org/FlumeUserGuide.html)
[Medium - Introdução. O que é o Apache Flume?](https://medium.com/apache-flume/introdu%C3%A7%C3%A3o-b5c0c97b5634)
[Oracle - Usando o Apache Flume](https://docs.oracle.com/pt-br/iaas/Content/bigdata/hadoop-odh-flume.htm)
### 2. Logstash (Red Cards)
* O Logstash é um pipeline gratuito e aberto de processamento de dados do lado do servidor que faz a ingestão de dados de inúmeras fontes, transforma-os e envia-os para o seu “esconderijo” favorito.
* O Logstash faz dinamicamente ingestões, transformações e envios dos dados independentemente do formato e da complexidade.
* O Logstash foi desenvolvimento pela Elastic e é escrito em JRuby, sendo necessário uma JVM para rodá-lo.
* É Open Source e é licenciado pela Apache 2.0.

A carta do Logstash em relação aos nossos drivers, no jogo:
```python
★★ Performance – can handle high throughput with low latency (via
leveraging multiple threads for filter workers, input/output workers etc.,
adding more instances)
★★ Reliability – depends on broker implementation, reported issues with
losing messages
```
  
  * Arquitetura de uma implementação com o Logstash:
    
![image](https://github.com/user-attachments/assets/527ec72d-f8f7-42ad-aaa2-f90d0646b457)

Refs.:
[Logstash](https://www.elastic.co/pt/logstash)

### 3. Fluentd (Red Cards)
* Fluentd é um Data Collector que unifica a coleção e o consumo dos dados para um melhor uso dos dados.
* Utiliza JSON como um formato de dados interno para unificar toda a coleta, filtros, buffers e mecanismos de output para levar os dados da fonte até o destino.
* O Fluentd foi escrito em C e em Ruby
* É Open Source e é licenciado pela Apache 2.0

* Alguns casos de uso disponíveis na [documentação oficial](https://docs.fluentd.org/quickstart)
  1. Data Search
  2. Data filtering and Alerting
  3. Data Analytics with Treasure Data
  4. Data Collection to MongoDB
  5. Data Collection to HDFS (caso de uso mais comum entre os 3)
  6. Data Archiving to Amazon S3
 
A carta do Fluentd em relação aos nossos drivers, no jogo:
```python
★★ Performance – requires very little system resources. The vanilla 
instance runs on 30-40MB of memory and can process 13,000 
events/second/core.
★★★ Reliability – supports memory- and file-based buffering to prevent 
inter-node data loss. The buffering logic is highly tunable and can be 
customized for various throughput/latency requirements.
```

 * Arquitetura do caso de uso 5 (Data Collection to HDFS)

   
![image](https://github.com/user-attachments/assets/c7f4130d-e341-4e38-9301-19444ece9388)


Casos de Usos Gerais do Fluentd
![image](https://github.com/user-attachments/assets/337aa14b-6a1a-4c34-8c25-ee9a4ce845a7)


#### Smart Decision 2
...

