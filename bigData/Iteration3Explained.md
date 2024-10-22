# Iteration 3

Conforme o padrão arquitetural Lambda, a terceira fase envolve a decomposição da Batch Layer (ou Camada de Lote).

![image](https://github.com/user-attachments/assets/64dd573c-011b-41cb-b74c-c91a471520d0)

Ela é a responsável por processar grandes volumes de dados de forma periódica, geralmente em ciclos pré-definidos, como diários, semanais ou mensais. Ela lida com dados históricos ou de longo prazo, realizando o processamento em lote para criar um conjunto mestre de dados. Esses dados são então pré-computados para gerar visões agregadas ou "views", que são posteriormente disponibilizadas para outras camadas do sistema, como a Serving Layer.

![image](https://github.com/user-attachments/assets/43769f69-2f2a-432e-a079-20a6d0352861)

## Premissas
### Architecture Drivers
Os Architecture Drivers para a terceira iteração, excluindo Extensibility, Cost e Environment, que se aplicam a todas as iterações, são os seguintes:
* Quality Atributes
```cpp  
Scalability - > Q9: O sistema armazenará dados brutos dos últimos 60 dias (~1 TB de dados brutos por dia, ~60 TB no total)

Availability -> Q10: O sistema sobreviverá e continuará operando se algum de seus nós ou componentes falhar
```
Vamos priorizar os seguintes fatores para a iteração:
 - Escalabilidade: É fundamental garantir que o sistema possa crescer e se adaptar a um aumento na carga de trabalho e no volume de dados sem comprometer seu desempenho.
- Disponibilidade: Precisamos assegurar que o sistema esteja sempre acessível e funcional, minimizando o tempo de inatividade e garantindo uma experiência contínua para os usuários.

Além disso, vamos buscar uma opção que ofereça melhor extensibilidade, facilitando o armazenamento de novos formatos de dados. Essa flexibilidade será crucial para acomodar futuras necessidades e garantir que o sistema permaneça atualizado e eficiente.

## Design
### 1. Column-Family (Blue Card)
Column-Family é um tipo de modelagem de dados usada em bancos de dados NoSQL, caracterizada por conter um conjunto de linhas,
onde cada uma delas é um conjunto de pares chave-valor chamados colunas. Podem ser apresentados como matrizes bidimensionais em que
cada chave possui um ou mais pares de valores-chave anexados a ela.

![Column-Family design preview](https://studio3t.com/wp-content/uploads/2017/12/cassandra-column-family-example-1024x608.png)

![column-family_card](https://github.com/user-attachments/assets/c9a8e586-ba59-4eae-8e68-e12d8f1d6dcb)

Em relação aos drivers analisados nessa iteração, a carta de Column-Family é avaliada como:
```python
★★★ Scalability – pode ser dimensionado linearmente dividindo os dados entre servidores 
                    usando valor hash calculado com base em uma chave de linha

★★★ Availability – alta disponibilidade é fornecida por clustering e 
                     sistema de arquivos distribuído (por exemplo, HDFS)
```

### 2. Document-Oriented (Blue Card)
Document-oriented é um tipo de modelagem de dados também usada em bancos de dados NoSQL. Funciona de maneira semelhante ao design
Column-Family, mas permite um aninhamento de dados muito mais profundo e armazenamento de estruturas mais complexas, isto é,
documentos, que podem ser aninhados (um documento dentro de outro documento dentro de outro documento...)

![Document oriented design preview](https://devsblog.home.blog/wp-content/uploads/2019/04/9-document-oriented-databases-11-638-1.jpg)

![document-oriented_card](https://github.com/user-attachments/assets/00db24ff-ab0a-48ea-8021-4f8ce869b8e8)

Em relação aos drivers analisados nessa iteração, a carta de Document-Oriented é avaliada como:
```python
★★★ Scalability – mais de 100 organizações executam clusters com mais de 100 nós. 
                    Alguns clusters excedem 1.000 nós

★★★ Availability – alta disponibilidade é fornecida por clustering e 
                     replicação

```

### 3. Distributed File System (Blue Card)
Os modernos sistemas de arquivos distribuídos são altamente tolerantes a falhas e projetados para serem executados em hardware de baixo custo. Código aberto  implementações como HDFS (Hadoop Distributed File System) e CFS (Cassandra File System) fornecem acesso de alto rendimento aos dados do aplicativo e são adequados para aplicativos que processam grandes conjuntos de dados.

![image](https://github.com/user-attachments/assets/b9c557ff-8555-4e1e-9d11-3cfba2b89c8c)

![image](https://github.com/user-attachments/assets/a66bd94f-9df2-4f4a-9473-662e0b7a5df9)

Em relação aos drivers analisados nessa iteração, a carta de Distributed File System é avaliada como:
```python
★★★ Scalability – massivamente e linearmente escalável, número de nós 
                    teoricamente é ilimitado, clusters de produção existentes com até 10.000 
                    nós

★★★ Availability – replicação padrão de dados para 3 nós, rack e 
                     reconhecimento do datacenter, sem pontos únicos de falha
```

### Smart Decision 1 - Design
Para a Batch Layer (Master Dataset), onde o objetivo é armazenar grandes volumes de dados imutáveis e processá-los em lotes, a melhor escolha entre as três opções seria um Distributed File System como HDFS, pois ele é ideal para processamento em lote devido à sua capacidade de leitura/gravação sequencial rápida, o que é essencial para tarefas que exigem alta performance no processamento de dados em massa. Além disso, seu nível de escalabilidade é extremamente elevado, suportando clusters com milhares de nós, o que permite que o sistema cresça conforme a necessidade de armazenamento de dados aumenta. A alta disponibilidade, garantida pela replicação automática e pela tolerância a falhas, assegura que o sistema seja robusto e confiável, características importantes para um Master Dataset. Em comparação, um banco de dados Column-Family (e.g., Cassandra, HBase) também oferece desempenho extremamente rápido e alta escalabilidade, mas é mais adequado para operações de leitura/gravação frequente de dados em chave-valor, e sua limitação no suporte a agregações e análises ad-hoc torna-o menos ideal para processamento em lote. Já o banco de dados Document-Oriented (e.g., MongoDB, CouchDB) possui uma estrutura flexível e suporta análises um pouco melhores do que o Column-Family, porém seu desempenho é mais lento, o que pode ser um gargalo quando se trata de lidar com grandes volumes de dados em lotes. Assim, o Distributed File System se destaca por ser mais otimizado e eficiente para o propósito de processamento em lotes e armazenamento de grandes datasets.


## :arrow_right: [Iteration 4](https://github.com/mentejoao/ds-2024-02/blob/main/bigData/Iteration4Explained.md)
