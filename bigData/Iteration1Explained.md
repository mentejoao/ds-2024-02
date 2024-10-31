
# Iteration 1

Após ter sido feito a análise de inputs (Input Review) em que analisamos as informações a serem consideradas em nosso cenário de jogo, deveremos agora decompor o nosso cerne, nosso core do jogo. 
Devemos decompor um sistema de BIG DATA. Dito isso, o contexto em que nos encontramos é:  O sistema deve ser capaz de coletar e processar dados semi-estruturados em grande escala, 
possibilitando tanto o monitoramento em tempo real quanto análises históricas e ad-hoc. A solução precisa atender a quatro casos de uso principais:

### Use Cases:
 * UC1: Monitorar serviços online – Fornecer dashboards em tempo real para a Equipe de Operações acompanhar o estado dos serviços e infraestrutura.
 * UC2: Solucionar problemas de serviços online – Facilitar a análise de logs para diagnóstico e resolução de problemas.
 * UC3: Fornecer relatórios de gestão – Permitir que gerentes tenham acesso a relatórios históricos sobre desempenho e uso do sistema.
 * UC4: Análise de dados ad-hoc – Habilitar Cientistas de Dados a realizar consultas exploratórias para descobrir padrões e melhorar o planejamento.

### Desafios:
 * Análise Ad-Hoc: Cientistas de Dados e Analistas precisam realizar consultas exploratórias não planejadas em tempo real ou próximo disso. 
 Eles têm acesso a dados brutos e agregados e precisam de flexibilidade para investigar padrões inesperados.

 * Análise em Tempo Real: A Equipe de Operações e Desenvolvedores precisam de monitoramento em tempo real da infraestrutura para identificar problemas e realizar manutenções preventivas ou corretivas rapidamente.

 * Processamento de Dados Não Estruturados: Os registros provenientes de diversas fontes podem ser semi-estruturados ou não estruturados, como logs de servidores,
   o que exige um sistema que consiga lidar com esses formatos de dados de maneira eficiente.

 * Escalabilidade: O sistema precisa ser escalável para lidar com o crescimento constante de dados à medida que o número de servidores e a quantidade de informações aumentam,
   mantendo a performance e a eficiência.

 * Economia de Custo: A solução deve ser economicamente viável, proporcionando flexibilidade sem custos excessivos.

![image](https://github.com/user-attachments/assets/b8c8241e-6f22-4247-9002-df1fdfd07ba3)


# Premissas

Tomamos como premissa o que interpretamos em nosso cenário de jogo, não temos nesse momento atributos de qualidade pré-definidos, mas apenas o que nos foi informado em game scenerario, 
tendo por disposição algumas cartas para definição da melhor arquitetura para os drivers/desafios propostos incialmente.

# Design

#### 1. Pure Non-relational (Blue Cards)
A arquitetura Pure Non-relational é baseada em tecnologias que não seguem os princípios do modelo relacional, como NoSQL e Hadoop. 
Ela é projetada para lidar eficientemente com dados semi-estruturados e não estruturados, permitindo o armazenamento e a análise de grandes volumes de dados de forma flexível.

A carta de Pure Non-relational em relação aos nossos drivers, no jogo:
```python
★★ Ad-hoc analysis:  O suporte a consultas ad-hoc em tempo real é mais difícil do que 
                      em arquiteturas relacionais, pois os sistemas NoSQL ou Hadoop não são
                      otimizados para este tipo de consulta de forma nativa.

★★½ Real-time analysis: Suporta processamento em tempo real com processamento de um dado por vez,
                         o que limita um pouco a performance para análises mais complexas.

★★★ Unstructured data processing: Armazenamento e processamento de dados semi-estruturados e não
                                    estruturados é fácil e eficiente, uma das principais vantagens desta
                                    arquitetura.

★★★ Scalability: Capaz de escalar horizontalmente para lidar com petabytes de dados,
                  facilitado pela natureza distribuída do NoSQL e Hadoop.

★★★ Cost economy: O custo é minimizado, pois se baseia amplamente em tecnologias
                   open-source, o que reduz o custo com licenciamento.

resultado: 13 ★ e ½ 
```

### 2. Extended Relational (Blue Cards)
Esta arquitetura baseia-se completamente em princípios relacionais e em DBMS baseados em SQL, utilizando intensivamente técnicas de processamento 
massivo em paralelo (MPP) e memória in-memory para melhorar a escalabilidade.

A carta de Extended Relational em relação aos nossos drivers, no jogo:
```python
★★★ Ad-hoc analysis: Oferece suporte para consultas ad-hoc complexas em tempo real,
                     ideal para análises em grande escala.

★★ Real-time analysis: Suporta análises em quase tempo real com técnicas de micro-batch,
                       o que gera uma leve latência no processamento.

★★ Unstructured data processing: Suporta ingestão e consulta de dados semi-estruturados,
                                 como JSON e XML, mas não é tão eficiente quanto sistemas não-relacionais.

★★ Scalability: Pode escalar para volumes na ordem de terabytes utilizando MPP e capacidades de cluster,
                embora tenha limitações para volumes maiores como petabytes.

★ Cost economy: O custo é elevado devido às licenças caras dos sistemas MPP RDBMS.

resultado: 10 ★ 
```

### 3. Data Refinery (Blue Cards)
Esta arquitetura combina técnicas relacionais e não relacionais, utilizando a parte não relacional como um ETL (Extract, Transform, Load) 
para refinar dados semi-estruturados e não estruturados antes de carregá-los em um data warehouse relacional para análise.

A carta de Data Refinery em relação aos nossos drivers, no jogo:
```python
★★★ Análise ad-hoc – suporta consultas de leitura em tempo real complexas e ad-hoc

★ Análise em tempo real – a latência dos dados é alta devido ao processamento em batch

★★★ Processamento de dados não estruturados – suporta o processamento de dados semi-estruturados e não estruturados

★★ Escalabilidade – pode operar com terabytes, utilizando capacidades de MPP e clustering

★ Economia de custo – o custo da licença do MPP RDBMS é bastante caro

resultado: 10 ★ 
```

### 4. Lambda Architecture (Blue Cards)
A arquitetura Lambda combina processamento em batch e em tempo real, permitindo análises históricas e operacionais na mesma solução. 
A camada de batch é baseada em técnicas não relacionais, enquanto a camada de velocidade utiliza streaming para análises em tempo real.

A carta de Lambda Architecture em relação aos nossos drivers, no jogo:
```python
★★ ½ Análise ad-hoc - o suporte a consultas em tempo real ad-hoc é mais difícil do que na arquitetura relacional

★★★ Análise em tempo real – abordagem de streaming com baixa latência de dados

★★★ Processamento de dados não estruturados – suporta o processamento de dados semi-estruturados e não estruturados

★★★ Escalabilidade – pode escalar mantendo petabytes

★★★ Economia de custo – custo minimizado devido a tecnologias open-source

resultado: 14 ★ e ½
```

#### Smart Decision
Considerando [Análise  Ad-Hoc,  Análise  em  tempo  real,  Processamento  de  dados  não  estruturados,  Escalabilidade,  Economia  de  custos] se torna irrefutável que a melhor opção é a arquitetura lambda,
que contempla da melhor forma todos os requisitos do cenário de jogo, além de saber que se conecta com a proposta relatada incialmente:
 * Inteligência em Tempo Real: Permite que a equipe de operações monitore serviços online em tempo real, com baixa latência, para detectar e responder rapidamente a problemas.

 * Descoberta de Dados: Facilita análises ad-hoc por Cientistas de Dados e Analistas, permitindo a exploração de dados históricos e a descoberta de padrões que melhoram a infraestrutura e a satisfação do cliente.

 * Relatórios Empresariais: Suporta a geração de relatórios históricos, agregando dados de forma eficiente para mostrar tendências de uso e qualidade das versões.


