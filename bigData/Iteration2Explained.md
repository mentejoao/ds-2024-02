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

Como possíveis opções de design temos o Data Collector e o Distributed Message Broker, aqui explanarei melhor um pouco sobre os dois.

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

Apesar da melhor performance do Distributed Message Broker, escolheremos o Data Collector para nossa iteração, já que neste trade-off entre Performance e Compatibilidade, o Data Collector oferece uma compatibilidade melhor para uma não tão ruim performance, comparada ao Distributed Message Broker.

