# Iteration 5

Na iteração final, seguindo o padrão de arquitetura Lambda, é hora de decompor a Speed Layer (ou Camada de Velocidade do sistema).
No contexto e cenário de big data para a empresa de internet que estamos trabalhando, ela é principalmente relacionada a visualização de dados em tempo real.
Isto porque essa empresa é responsável por prover conteúdo para milhões de milhões de usuários web.

Para realizar a decomposição desse elemento, vamos escolher uma carta de família (azul) e uma de tecnologia (vermelha), analisando as constraints e 
requisitos para chegar na melhor escolha possível de acordo com o nosso contexto de sistema.

![lambdaarq](https://github.com/user-attachments/assets/48ffc706-0ca2-481a-a150-5ae837abd921)

# Premissas
### Architecture Drivers
Para essa iteração específica, como drivers de arquitetura, temos:
* Quality Atributes
```cpp  
Performance -> Q4: "The system shall provide full-text search and ad-hoc analysis with human-time queries
                    (< 20 seconds query execution time, last 48 hours data) for on-duty Operations,
                    Developers and Support Engineers."


Performance -> Q5: "The system shall automatically refresh real-time monitoring dashboard with new data
                    (< 1 min data latency, last 48 hours data) to on-duty Operations, Developers and
                    Support Engineers." 
```
Sendo assim, para design devemos priorizar a análise Ad-hoc, enquanto para tecnologia vamos priorizar
Real-time Analysis.
Como possíveis opções de desing temos Column-Family, Document-Oriented e Distributed Search Engine.

# Design
### 1. Column-Family
Column-Family é um tipo de modelagem de dados usada em bancos de dados NoSQL, caracterizada por conter um conjunto de linhas,
onde cada uma delas é um conjunto de pares chave-valor chamados colunas. Podem ser apresentados como matrizes bidimensionais em que
cada chave possui um ou mais pares de valores-chave anexados a ela.

![Column-Family design preview](https://studio3t.com/wp-content/uploads/2017/12/cassandra-column-family-example-1024x608.png)


![column-family_card](https://github.com/user-attachments/assets/c9a8e586-ba59-4eae-8e68-e12d8f1d6dcb)

Em relação aos drivers analisados nessa iteração, a carta de Column-Family é avaliada como:
```python
★ Ad-hoc analysis – support secondary indexing, but no aggregate functions.
```

### 2. Document-Oriented
Document-oriented é um tipo de modelagem de dados também usada em bancos de dados NoSQL. Funciona de maneira semelhante ao design
Column-Family, mas permite um aninhamento de dados muito mais profundo e armazenamento de estruturas mais complexas, isto é,
documentos, que podem ser aninhados (um documento dentro de outro documento dentro de outro documento...)

![Document oriented design preview](https://devsblog.home.blog/wp-content/uploads/2019/04/9-document-oriented-databases-11-638-1.jpg)

![document-oriented_card](https://github.com/user-attachments/assets/00db24ff-ab0a-48ea-8021-4f8ce869b8e8)

Em relação aos drivers analisados nessa iteração, a carta de Document-Oriented é avaliada como:
```python
★½ Ad-hoc analysis – somewhat better than other NoSQL families, but still not
                      as good as relational databases or interactive query engines.
```

### 3. Distributed Search Engine
Distributed Search Engine ou Mecanismo de Pesquisa Distribuída trata-se de uma solução escalável de indexação com full-text
e pesquisa interativa. A maioria das implementações fornece uma API que permite executar consultas complexas em dados semi-estruturados
como logs, páginas da web e documentos. Além disso, esse tipo de arquitetura de sistema permite a busca em múltiplos servidores.

![image](https://github.com/user-attachments/assets/8cc58129-78ac-4a5d-95b4-8383110f10d0)

Em relação aos drivers analisados nessa iteração, a carta de Document-Oriented é avaliada como:
```python
★★ Ad-hoc analysis – query languages often include faceted and geospatial search,
                      stat functions and simple joins to query, analyze and visualize data.
```

### Smart Decision 1
Dado o cenário que foi estabelecido de "um sistema que envolve armazenamento e análise de um conjunto de massivo de dados
semi-estruturados vindos de centenas de servidores", a melhor escolha nesse caso é o Distributed Search Engine, que vence
os outros modelos nos drivers de arquitetura escolhidos para avaliação. Além disso, também satisfaz as constraints e
atributos de qualidade, por se tratar de um mecanismo distribuído, com boa capacidade de análise ad-hoc e busca full-text.

