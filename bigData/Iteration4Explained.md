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
Sendo assim, para design iremos priorizar Ad-hoc Analysis e Performance, enquanto para tecnologia vamos priorizar apenas Performance.
Como possíveis opções de desing temos Column-Family, Document-Oriented e Interactive Query Engine.

# Design
