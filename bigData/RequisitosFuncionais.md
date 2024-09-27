# Requisitos Funcionais
### 1. Consulta e Compreensão Geral dos Documentos do Jogo
O time deve obter os arquivos no site ```https://smartdecisionsgame.com/```, na seção de download. Após efetuar o download, ser capaz de acessar e compreender as informações contidas nas duas pastas principais (no contexto do Big Data):

      * Cards:
            - Blue Cards (design concepts)
            - Red Cards (tecnologias).
      * Instructions: 
            - Game Board (tabuleiro),
            - Game Instruction (instruções gerais)
            - Game Scenario (explica o contexto do problema, os atributos de qualidade e constraints para cada iteração)
            - Game Scorecard (a ser preenchido),
            - Game Scoring Tables (Instrução detalhada)
        
### 2. Consulta das Blue Cards:
O time deve ser capaz de consultar as Blue Cards para identificar conceitos de arquitetura de Big Data, como "Lambda Architecture" ou "Pure Non-Relational". Cada Blue Card, de modo mais importante, deve apresentar os seguintes detalhes:
* Título do conceito arquitetural.
* Consequências de uso em termos de escalabilidade, custo, e outros quality attribute, avaliadas em estrelas.

### 3. Consulta das Red Cards
O time deve ser capaz de consultar as Red Cards para selecionar as tecnologias que implementam os conceitos das Blue Cards. Cada Red Card, de modo mais importante, deve apresentar:

* Tecnologia associada (ex: Apache Kafka, Logstash).
* Consequências em termos de performance, confiabilidade e economia de custos, avaliadas em estrelas.
### 4. Seleção de Drivers para a Iteração
Para cada iteração, o jogador deve analisar os drivers a serem considerados no documento de Game Scoring Tables, que são as prioridades e requisitos da arquitetura naquela fase (ex: Ad-Hoc Analysis, Real-time Analysis, Unstructured Data Processing, Scalability, Cost Economy). Nesse sentido, apenas estrelas destes requisitos devem ser consideradas. Exemplo: se a carta possui outros requisitos que estão fora do escopo da iteração, suas estrelas não devem ser consideradas a título de pontuação.

### 5.Decomposição do Elemento da Iteração
O time deve ser capaz de identificar o elemento a ser decomposto em cada iteração (ex: Big Data na primeira iteração). Isso envolve dividir o sistema em partes menores e mais gerenciáveis, com base nos drivers e nas Blue Cards selecionadas.

### 6. Avaliação e Escolha das Blue Cards para a Iteração
O time deve escolher as Blue Cards mais adequadas para cada iteração, com base nos drivers e nas estrelas que cada Blue Card recebeu para os atributos necessários. O objetivo é maximizar a eficiência da arquitetura de acordo com os quality attributes prioritários, respeitando as constraints.

### 7. Implementação com as Red Cards
A dependar da iteração, após a escolha das Blue Cards, o time deve selecionar as Red Cards (tecnologias) que implementam os conceitos das Blue Cards escolhidas. A seleção deve ser feita considerando as consequências de uso de cada tecnologia para os objetivos da iteração.

### 8. Avaliação de Consequências das Decisões
O time deve ser capaz de analisar as consequências das suas decisões arquiteturais, considerando o impacto nos atributos como escalabilidade, custo e performance.

### 9. Iterações e Smart Decisions
O jogo avança em iterações. O time deve tomar decisões arquiteturais a cada iteração, otimizando os input drivers exigidos. Cada iteração pode trazer novos drivers ou desafios, e o time deve adaptar suas decisões de acordo. As decisões da equipe devem ser anotadas no Score Board para pontuação.

### 10. Colaboração em Equipe
O jogo permite que os jogadores trabalhem em time. O time deve ser capaz de discutir as opções das Blue Cards e Red Cards, avaliar os impactos das decisões e fazer escolhas arquiteturais colaborativas para cada iteração.



