# Previsão de vendas de rede farmacêutica

<img src="https://github.com/pedroachagas/rossmann_webapp/blob/main/img/rossmann.jpg" width=70% height=70%/>

## Contextualização

A Rossmann é uma das maiores redes de farmácias da Europa, possuindo mais de 4.000 lojas, e 56 mil colaboradores até 2020.

Os dados utilizados neste projeto são reais, e foram disponibilizados pela própria Rossmanm através do site Kaggle, para uma competição de ciência de dados.

Foram disponibilizados 1.017.209 registros de vendas, contendo 18 características de cada venda.

O contexto de negócios é fictício, porém descreve um problema real de uma grande varejista: prever com assertividade suas vendas.


## 1. Problema de negócios

### 1.1 Problema

Em uma reunião mensal de resultados da Rossmann, o CFO solicitou aos gerentes das lojas a previsão de vendas (faturamento) para as próximas 6 semanas, pois ele precisa saber quanto cada loja pode contribuir financeiramente para uma reforma na rede, que está padronizando suas lojas.

### 1.2 Objetivos

- Criar um modelo de machine learning capaz de fornecer previsões de venda para as próximas 6 semanas de cada uma das lojas da rede.
- Desenvolver uma interface pela qual o CFO possa interagir com o modelo, solicitando de qual das lojas deseja prever as vendas.


## 2. Premissas de negócio

- A consulta da previsão de vendas estará disponível 24/7, e será acessível via dispositivos móveis.
- O planejamento da solução será validado com os gerentes, visando garantir que o seu conhecimento de negócios seja aproveitado ao máximo.
- Dias sem vendas não foram incluídos
- Para lojas sem informações sobre a distância do competidor mais próximo, foi adotada a maior distância registrada no conjunto de dados.

### As variáveis do dataset original são

Variável | Definição
------------ | -------------
|store | id único de cada loja.|
|day_of_week | indica o dia da semana que era aquele dia (assumindo 1-Sun -> 7-Sat).|
|date | data do registro.|
|sales | faturamento da loja naquele dia.|
|customers | número de clientes na loja naquele dia.|
|open | loja aberta ou fechada: (0 = closed, 1 = open).|
|state_holiday | feriado nacional (a = public holiday, b = Easter holiday, c = Christmas, 0 = Dia Comum).|
|school_holiday | indica se a loja naquele dia foi afetada pelo fechamento das escolas públicas.|
|store_type | indica qual dos 4 modelos distintos é esta loja: (a, b, c, d).|
|assortment | indica o nível de sortimento da loja: (a = basic, b = extra, c = extended).|
|competition_distance | indica a distancia em metros do competidor mais próximo.|
|competition_open_since_month | indica mês aproximado da abertura do competidor mais próximo.|
|competition_open_since_year | indica ano aproximado da abertura do competidor mais próximo.|
|promo | indica se a loja está com uma promoção ativa naquele dia.|
|promo2 | é uma promoção contínua e consecutiva: (0 = store not participating, 1 = store participating).|
|promo2_since_week | indica a semana do calendário onde a loja entrou em Promo2.|
|promo2_since_year | indica o ano onde a loja entrou em Promo2.|
|promo_interval | indica os meses de início anual onde Promo2 é iniciada (ex: "Feb,May,Aug,Nov").|

As variáveis derivadas na etapada de Feature Engineering são:
Variável | Definição
------------ | -------------
|competition_since | data desde que existem competidores. |
|competition_time_month | número de meses desde que a competição iniciou. |
|promo2_since | data desde que a Promo2 está ativa. |
|promo2_time_week | números de semanas em que a Promo2 ficou ativa. |


## 3. Planejamento da solução

### 3.1. Produto final

O que será entregue efetivamente?

- Um bot (robô) no aplicativo de mensagens Telegram, que recebe o código da loja, e retorna em tempo real qual a sua previsão de vendas (faturamento) para as próximas 6 semanas.

### 3.2. Ferramentas

Quais ferramentas serão usadas no processo?

- Python 3.8.12;
- Jupyter Notebook;
- Git e Github;
- Heroku Cloud;
- Algoritmos de Classificação e Regressão;
- Pacotes de Machine Learning Sklearn e Scipy;
- Técnicas de Seleção de Atributos e Redução de Dimensionalidade
- Flask e Python API's

### 3.3 Processo

#### 3.3.1 Estratégia de solução

<img src="https://github.com/pedroachagas/rossmann_webapp/blob/main/img/crisp.png" width=100% height=100%/>

A estratégia utilizada na resolução do problema, baseada:

**Passo 1 -** Compreender com clareza o modelo e o problema de negócios, através da estatística descritiva;

**Passo 2 -** Tratar os dados (formatos, dados faltantes, outliers), realizando a sua limpeza.

**Passo 3 -** Levantar junto ao time de negócio as variáveis que impactam nas vendas, formular e validar hipóteses gerando insights de negócio, e perceber quais variáveis são insumos relevantes para o algoritmo de previsão de vendas.

**Passo 4 -** Preparar os dados para a criação do modelo de previsão de vendas, realizando transformações, separação do dataframe entre treino e teste, e seleção de features através de algoritmo com esta finalidade.

**Passo 5 -** Treinar 5 algoritmos de aprendizado de máquina (lineares e não lineares), comparar sua performance, e selecionar o que melhor desempenha.

**Passo 6 -** Encontrar o conjunto de parâmetros que maximiza o aprendizado do modelo selecionado, reduzindo o seu erro nas previsões.

**Passo 7 -** Interpretar o erro do modelo e traduzir em resultado financeiro para a empresa.

**Passo 8 -** Avaliar se a previsão de vendas construída já entrega valor ao time de negócios, publicando em produção em caso positivo, ou realizando um novo ciclo de melhorias pontuais em caso negativo.

**Passo 9 -** Após a publicação, criar robô no Telegram que acesse a previsão em tempo real, de qualquer lugar.

**Passo 10 -** Apresentar e disponibilizar o bot do Telegram aos gerentes e CFO, detalhando o funcionamento do modelo e esclarecendo as suas dúvidas.

## 4. Os 3 principais insights dos dados

Durante a análise exploratória de dados, foram gerados insights ao time de negócio.

Insights são informações novas, ou que contrapõe crenças até então estabelecidas do time de negócios. São também acionáveis: possibilitam ação para direcionar resultados futuros.

### Lojas com promoções ativas por mais tempo vendem menos

- Ação sugerida: Descontinuar as promoções ativas por tempo estendido, visto que constatou-se queda nas vendas após o período promocional normal.

### Lojas vendem menos durante os feriados escolares, exceto nos meses de agosto

- Ação sugerida: Considerar esta particularidade do mês de agosto na elaboração de promoções envolvendo clientes em faixas etárias escolares.

### Lojas vendem menos no segundo semestre do ano

- Ação sugerida: Considerar o declínio sazonal histórico de vendas entre os meses de agosto a novembro, compensando este fenômeno como ações de marketing.  

## 5. Escolha e resultados do modelo

Durante o desenvolvimento do projeto foram testados 4 modelos de machine learning através de uma estratégia de Time-Series Cross-Validation, com 6 folds. Seus desempenhos foram avaliados pelas métricas MAE, MAPE e RMSE

|     Modelo         |           MAE           |          MAPE          |        RMSE         |
|--------------------|-------------------------|------------------------|---------------------|
|  Linear Regression |    2055.56 +/- 323.74   |      0.29 +/- 0.01     |  2921.75 +/- 489.18 |
|  Lasso Regression    |    2070.72 +/- 337.78   |      0.29 +/- 0.01     |  2964.9 +/- 499.26  |  
|  Random Forest Regressor   |    854.23 +/- 230.73    |      0.12 +/- 0.03     |  1294.43 +/- 355.29 |
|  XGBoost Regressor |    863.12 +/- 135.32    |      0.12 +/- 0.01     |  1276.11 +/- 210.31 |

Apesar do Random Forest ter tido um desempenho ligeiramente superior, o modelo escolhido foi o XGBoost Regressor por requerer muito menos espaço de memória. Além disso, após a etapa de Fine Tuning dos hiperâmetros do modelo, foi possível melhorar seu desempenho.

|    Modelo (Tuned)  |       MAE        |       MAPE         |     RMSE     |
|--------------------|------------------|--------------------|--------------|
|  XGBoost Regressor |    792.24 +/- 124.6    |       0.11 +/- 0.01       | 1156.23 +/- 186.87  |

## 5. Resultados financeiros para o negócio

As previsões de vendas da Rossmann eram, até antes deste projeto, realizadas por meio de planilhas de histórico de venda, através de uma média móvel. A taxa de erros da previsão de vendas de toda a rede ficava na média de 36%, chegando a até 60% nas lojas mais recentes.

Após a implementação deste modelo de previsão de vendas, a taxa de erro média das previsões em toda a rede passou para 11%.

A figura abaixo ilustra o desempenho geral do modelo.

    error = sales - predictions

    error_rate = prediction/sales

<img src="https://github.com/pedroachagas/rossmann_webapp/blob/main/img/model_performance.png" width=100% height=100%/>

## 6. Implementação do modelo em produção

O modelo foi finalmente colocado em produção e operado através de um chatbot do Telegram. Para isto, além do modelo final treinado, foi criada uma classe em python com todo o pipeline de processamento de dados, um manipulador de API e uma aplicação para gerenciar as mensagens. Todos os arquivos foram hospedados no Heroku (https://www.heroku.com/); os dados de produção também foram armazenados na nuvem do Heroku.

<img src="https://github.com/pedroachagas/rossmann_webapp/blob/main/img/app.png" width=100% height=100%/>

## 7. Conclusão

O objetivo do projeto foi alcançado, resolvendo não só o problema inicial de previsibilidade de faturamento do CFO, bem como melhorando a gestão financeira da Rossmann como um todo, trazendo consigo ganhos financeiros consideráveis para o negócio.

O funcionamento da previsão de vendas via bot do Telegram pode ser visto no gif abaixo:

<img src="https://github.com/pedroachagas/rossmann_webapp/blob/main/img/telegram_bot.gif" width=100% height=100%/>

Para utilizar o bot clique [aqui](http://t.me/RossmannSalesPredBot)

## 8. Próximos passos

- Reavaliar o conjunto de parâmetros utilizados para maximizar o aprendizado do modelo, incluindo mais parâmetros na estratégia Random Search, e avaliando a viabilidade de uso da estratégia Bayesian Search.

## 9. Referências

- Este Projeto de Previsão de vendas é parte do curso "DS em Produção", da [Comunidade DS](https://www.comunidadedatascience.com/comunidade-ds/)
- O Dataset foi obtido no [Kaggle](https://www.kaggle.com/c/rossmann-store-sales)
- A imagem utilizada é de uso livre e foi obtida no [Pexels](https://www.pexels.com/pt-br/foto/mulher-adulta-elegante-usando-smartphone-na-rua-3774903/)
