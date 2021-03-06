# Algoritmos de Classificação

## k-Vizinhos Mais Próximos

A motivação fundamental das técnicas baseadas em vizinhança é que dados similares tendem a estar concentrados em uma mesma região no espaço de entrada. Por espaço de entrada, podemos entender os atributos preditivos da base. Na técnica k-vizinhos mais próximos (kNN), cada objeto representa um ponto no espaço definido pelos atributos de entrada. Uma vez definida uma métrica de similaridade, é possível calcular a distância entre cada dois pontos. A métrica mais utilizada é a distância Euclidiana mostrada na fórmula a seguir. Nessa fórmula, <img src="https://render.githubusercontent.com/render/math?math=x_{i}^{l}"> e  <img src="https://render.githubusercontent.com/render/math?math=x_{j}^{l}"> são objetos representados por <img src="https://render.githubusercontent.com/render/math?math=l"> atributos.  

<img src="https://render.githubusercontent.com/render/math?math=d(x_i, x_j) = \sqrt{\sum_{l=1}^{d}(x_{i}^{l} - x_{j}^{l}) }">

A variação mais simples do algoritmo kNN é o 1NN apresentado na sequência. Na fase de treinamento, o algoritmo memoriza os exemplos rotulados da base de dados. Na fase de teste, quando um exemplo não rotulado é apresentado para o algoritmo, é calculada a distância entre esse novo exemplo e cada um dos exemplos rotulados da base de treinamento. O rótulo da classe associado ao exemplo de treinamento mais próximo do exemplo de teste é utilizado para classificar o novo exemplo.

![](knn_alg.png) 
*Algoritmo do 1NN. Adaptado de Katti Faceli et al., (2011)*

A extensão imediata do 1NN é o kNN, aonde *k* objetos mais próximos são considerados. Nessa nova configuração, a previsão dos diferentes vizinhos mais próximos são agregadas de forma a classificar a amostra de teste. A forma mais comum para agregar os votos é por maioria. Para evitar empates na votação é comum escolher valores pequenos e ímpares para *k*. 

A Figura a seguir mostra a aplicação do algoritmo 1NN e 3NN em um problema binário. Nessa base, cada amostra pode ser considerada saudável (círculos vermelhos) e doentes (quadrados azuis). O espaço de entrada é definido pelos dois atributos que estão relacionados ao Exame 1 e 2. O triângulo branco representa o exemplo de teste a ser rotulado. Utilizando o algoritmo 1NN com distância euclidiana, podemos definir o vizinho mais próximo como sendo da classe quadrado azul. Utilizando o algoritmo 3NN com distância euclidiana e voto por maioria, podemos definir o rótulo do exemplo de teste como sendo quadrado azul.

![](knn.png) 
*Exemplo ilustrativo do 1NN e 3NN para uma base binária*

As principais vantagens desse algoritmo são: a fase de treinamento é simples porque armazena o conjunto de treinamento; é aplicável em quase qualquer problema; e o algoritmo pode ser incremental, ou seja, novos exemplos de treinamento podem ser adicionados naturalmente sem retreinamento. As principais desvantagens são: o algoritmo não constrói um modelo explícito sobre os dados; a predição é custosa por exigir o cálculo da distância entre todas as amostradas da base de dados; pode ser influenciado pelos atributos caso eles não sejam normalizados; e a dimensionalidade muito alta pode impactar negativamente o desempenho do algoritmo por conta da função de distância utilizada.

### Links úteis

Versões eficientes desse mesmo algoritmo:
* [kNN](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)
* [BallTree](https://en.wikipedia.org/wiki/Ball_tree)
* [k-d tree](https://en.wikipedia.org/wiki/K-d_tree)

## Naive Bayes 

Uma forma de lidar com dados ruidosos e imprecisos é utilizando algoritmos baseados no Teorema de Bayes. O Teorema de Bayes assume que a probabilidade de um evento A ocorrer dado um outro evento B, depende da relação entre ambos, além da probabilidade de observar esses eventos de forma independentes. Nessa definição, a probabilidade de ocorrência do evento A e B podem ser estimada pela frequência com que esses eventos ocorrem de forma independente P(A) e P(B). De forma semelhante, é possível estimar a probabilidade de um evento ocorrer dado B para cada evento A por meio da probabilidade P(B|A). Com isso podemos estimar a probabilidade de A ocorrer dado B, ou seja, P(A|B). O Teorema de Bayes pode ser definido como:

<img src="https://render.githubusercontent.com/render/math?math=P(A|B) = \frac{P(B|A)*P(A)}{P(B)}">

De forma análoga podemos reescrever esse teorema para calcular a probabidade de ocorrência de cada uma das classes <img src="https://render.githubusercontent.com/render/math?math=y_l"> de uma base de dado para uma amostra <img src="https://render.githubusercontent.com/render/math?math=x">:

<img src="https://render.githubusercontent.com/render/math?math=P(y_l|x) = \frac{P(x|y_l)*P(y_l)}{P(x)}">

Aquela classe <img src="https://render.githubusercontent.com/render/math?math=y_l"> que maximizar a probabilidade *a posteori* deve ser a classe com maior probabilidade. Portanto, temos:

<img src="https://render.githubusercontent.com/render/math?math=y_{MAP} = arg max_l \frac{P(x|y_l)*P(y_l)}{P(x)}">

Como o denominador <img src="https://render.githubusercontent.com/render/math?math=P(x)"> é constante para todas as classes <img src="https://render.githubusercontent.com/render/math?math=y_l">, podemos reescrever a expressão como:   

<img src="https://render.githubusercontent.com/render/math?math=y_{MAP} = arg max_l P(x|y_l)*P(y_l)">

Expandindo a segunda parte da equação temos: 

<img src="https://render.githubusercontent.com/render/math?math=P(x|y_l)*P(y_l)=">
<img src="https://render.githubusercontent.com/render/math?math==P(x_1,...,x_d|y_l)*P(y_l)="> 
<img src="https://render.githubusercontent.com/render/math?math==P(x_1|y_l)*P(x_2,...,x_d|y_l,x_1)*P(y_l)="> <img src="https://render.githubusercontent.com/render/math?math==P(x_1|y_l)*P(x_2|y_l,x_1)*P(x_3,...,x_d|y_l,x_1,x_2)*P(y_l)="> <img src="https://render.githubusercontent.com/render/math?math=..."> 
<img src="https://render.githubusercontent.com/render/math?math==P(x_1|y_l)*P(x_2|y_l,x_1)*P(x_3|y_i,x_1,x_2) *...* P(x_d|y_i,x_1,x_2,...,x_{d-1})*P(y_l)">

Infelizmente é computacionalmente **impraticável calcular todas essas probabilidades**. Pensando nisso, simplificações são propostas. Uma delas, chamada de Naive Bayes (NB), assume que os valores dos atributos é independente portanto, a probabilidade <img src="https://render.githubusercontent.com/render/math?math=P(x|y_l)"> pode ser decomposta em <img src="https://render.githubusercontent.com/render/math?math=P(x_1|y_l)*P(x_2|y_l)*P(x_3|y_l)*...*P(x_d|y_l)">. Assim, podemos definir a probabilidade de uma amostra pertencer a uma classe como:

<img src="https://render.githubusercontent.com/render/math?math=P(y_l|x)=P(y_l)*\prod_{j=1}^{d}P(x_j|y_l)">

As principais vantagens desse algoritmo são: sua eficiência, uma vez que todas as probabilidades podem ser calculadas na etapa de treinamento; a construção do modelo é eficiente além de ser de fácil implementação; o algoritmo também é robusto a ruídos e atributos irrelevantes. As principais desvantagens são: o algoritmo desconsidera a dependência entre os atributos o que pode ser danoso para uma gama de problemas reais; ele traça hiperplanos lineares (o que também pode não ser suficiente dependendo da complexidade do problema); e ele necessita de adaptações quando os atributos são numéricos.   

### Exemplo Ilustrativo

O conjunto de dados *Jogar Tênis* é um problema de classificação binária em que pretende-se classificar se uma pessoa deve ou não, dado certas condições climáticas, jogar tênis. Os atributos de entrada são o *Tempo*, *Temperatura*, *Umidade* e *Vento*. O conjunto tem 14 amostras de treinamento e a última coluna denominada *Joga* representa os rótulos jogar ou não tênis. Os atributos *Tempo* e *Vento* são categóricos e os atributos *Temperatura* e *Umidade* são contínuos.

![](jogatenis.png) 
*Base de dados Jogar Tênis. Adaptado de Katti Faceli et al., (2011)*

Para construir um NB precisamos descobrir as probabilidades associadas dos atributos e das classes para o novo exemplo. Assumindo que o exemplo de teste é (Tempo=Ensolarado, Temperatura=70, Umidade=80 e Vento=Sim), calcule a probabilidade de jogar tênis. 

**1⁰ Passo:**

Probabilidade associada de cada classe:

<img src="https://render.githubusercontent.com/render/math?math=P(Joga = Sim) = \frac{9}{14}">
<img src="https://render.githubusercontent.com/render/math?math=P(Joga = Nao) = \frac{5}{14}">

**2⁰ Passo:**

Estimar a probabilidades de observar os valores do exemplo de teste para cada classe:

<img src="https://render.githubusercontent.com/render/math?math=P(Tempo = Ensolarado | Joga = Sim) = \frac{2}{5}">
<img src="https://render.githubusercontent.com/render/math?math=P(Tempo = Ensolarado | Joga = Nao) = \frac{3}{5}">

Quando temos atributos numéricos como *Temperatura* e *Unidade*, o procedimento consiste em calcular a média geral do atributo e definir esse valor como ponto de corte para o cálculo das probabilidades. Caso a amostra a ser classificada tenha um valor menor do que o ponto de corte, basta calcular a frequência com que isso acontece. 

<img src="https://render.githubusercontent.com/render/math?math=mean(Temperatura) = 73.6">
<img src="https://render.githubusercontent.com/render/math?math=P(Temperatura = 70 | Joga = Sim) = \frac{5}{8}">
<img src="https://render.githubusercontent.com/render/math?math=P(Temperatura = 70 | Joga = Nao) = \frac{3}{8}">

<img src="https://render.githubusercontent.com/render/math?math=mean(Umidade) = 81.6">
<img src="https://render.githubusercontent.com/render/math?math=P(Umidade = 80 | Joga = Sim) = \frac{6}{7}">
<img src="https://render.githubusercontent.com/render/math?math=P(Umidade = 80 | Joga = Nao) = \frac{1}{7}">

<img src="https://render.githubusercontent.com/render/math?math=P(Vento = Sim | Joga = Sim) = \frac{3}{6}">
<img src="https://render.githubusercontent.com/render/math?math=P(Vento = Sim | Joga = Nao) = \frac{3}{6}">

**3⁰ Passo:**

<img src="https://render.githubusercontent.com/render/math?math== P(Joga = Sim) * P(Tempo = Ensolarado | Joga = Sim) * P(Temperatura = 70 | Joga = Sim) * P(Umidade = 80 | Joga = Sim) * P(Vento = Sim | Joga = Sim)">
<img src="https://render.githubusercontent.com/render/math?math== \frac{9}{14} * \frac{2}{5} * \frac{5}{8} * \frac{6}{7} * \frac{3}{6}">
<img src="https://render.githubusercontent.com/render/math?math== 0.06887755">

<img src="https://render.githubusercontent.com/render/math?math==P(Joga = Nao) * P(Tempo = Ensolarado | Joga = Nao) * P(Temperatura = 70 | Joga = Nao) * P(Umidade = 80 | Joga = Nao) * P(Vento = Sim | Joga = Nao)">
<img src="https://render.githubusercontent.com/render/math?math==\frac{5}{14} * \frac{3}{5} * \frac{3}{8} * \frac{1}{7} * \frac{3}{6}">
<img src="https://render.githubusercontent.com/render/math?math== 0.005739796">

**4⁰ Passo:**

<img src="https://render.githubusercontent.com/render/math?math== \frac{0.06887755}{0.06887755 + 0.005739796} = 0.9230769">
<img src="https://render.githubusercontent.com/render/math?math== \frac{0.005739796}{0.06887755 + 0.005739796} = 0.005739796">

Portanto devemos jogar tênis!

### Links úteis

Outras versões desse mesmo algoritmo:
  * [NB](https://scikit-learn.org/stable/modules/naive_bayes.html)
  * [Versões NB](https://en.wikipedia.org/wiki/Naive_Bayes_classifier#Parameter_estimation_and_event_models)

## Árvores de Decisão

As [Árvores de Decisão](https://pt.wikipedia.org/wiki/%C3%81rvore_de_decis%C3%A3o) e Regressão são algoritmos supervisionados. O objetivo principal é induzir um modelo que seja capaz de predizer uma classe/rótulo/valor de uma variável resposta por meio do aprendizado de regras simples inferidas do conjunto de treinamento. Essas regras são geradas por meio da estratégia de [divisão e conquista](https://pt.wikipedia.org/wiki/Divis%C3%A3o_e_conquista) que recursivamente tende a diminuir a complexidade do problema,  tornando-o mais simples. A combinação dessas regras produz uma árvore capaz de gerar uma solução para o problema complexo. Os modelos em árvore são designados Árvores de Decisão (AD) para problemas de classificação e Árvores de Regressão (AR) para problemas de regressão.

### Componentes de uma AD

Formalmente uma AD é um grafo acíclico direcionado em que cada nó é um nó de divisão ou um nó folha:

* **Nó de divisão:** Possui dois ou mais sucessores. Ele contém um teste condicional baseado nos valores dos atributos. Normalmente o teste é univariado, ou seja, em um único atributo. Exemplo: Idade > 18, Profissão &#8712; {professor, estudante}.

* **Nó folha:** É rotulado com uma função que considera valores da variável alvo dos exemplos que chegam na folha. Em uma AD de classificação podemos usar uma função de minimização de custo 0-1 como a moda. Em AR de regressão podemos usar uma função de minimização do erro médio quadrático ou desvio absoluto. Exemplo: média/mediana

### Uma AD genérica

A Figura a seguir representa uma AD e sua divisão no espaço para uma base de dados com dois atributos preditivos  (x<sub>1</sub> e x<sub>2</sub>). Cada nó da árvore corresponde a uma região nesse espaço. Os nós folhas são mutualmente excludentes e a junção de todas as regiões cobre todo o espaço definido pelos atributos. Os hiperplanos gerados são ortogonais aos eixos dos atributos testados e paralelo a todos os outros eixos. Todas as regiões são hiper-retângulos.

![](ad.png) *Exemplo de uma Árvore de Decisão. Adaptado de Katti Faceli et al., (2011)*

### Algoritmo

O algoritmo de AD é mostrado na Figura a seguir. A entrada para a função é o conjunto de treinamento **D** e sua saída é uma AD. Na sequência o critério de parada é avaliado. Se mais divisões do conjunto de treinamento são necessárias, é escolhido um atributo que maximiza alguma medida de impureza. Na sequência a função de geração da árvore é chamada recursivamente e aplicada a uma partição do conjunto de treinamento **D**.

![](ad_alg.png) *Algoritmo de uma Árvore de Decisão. Adaptado de Katti Faceli et al., (2011)*

É ressaltar que a geração de uma árvore minimal é um problema [NP-completo](https://pt.wikipedia.org/wiki/NP-completo). Os algoritmos exploram heurísticas que localmente executam pesquisa um passo a frente. Uma vez que uma decisão é tomada ela nunca é desfeita. Isso pode gerar uma solução ótima localmente, o que pode estar longe do [ótimo global](https://pt.wikipedia.org/wiki/Algoritmo_guloso).

### Regras de Divisão

Considere um nó *t* de divisão de uma AD em que a probabilidade de observar exemplos de classe c<sub>i</sub> é dado por p<sub>i</sub>. Portanto a probabilidade de observar todas as classes é p<sub>1</sub>,p<sub>2</sub>,...p<sub>n</sub> então a impureza sobre um nó *t* é uma função sobre a proporção da classe daquele nó <img src="https://render.githubusercontent.com/render/math?math=i(t) = \phi(p_1,p_2,...p_n)">. Portanto a redução de impureza gerado pela divisão S de um conjuntos de treinamento em dois subconjuntos L e R pode ser medida como:

<img src="https://render.githubusercontent.com/render/math?math=d(S) = \phi(p_1,p_2,...p_n) - P_L * \phi(p_{1L},p_{2L},...p_{nL}) - P_R * \phi(p_{1R},p_{2R},...p_{nR})">

Nessa equação P<sub>L</sub> e P<sub>R</sub> representam a probabilidade de um exemplo quando aplicado a regra de divisão pertencer ao subconjunto L ou R, respectivamente.

A função de impureza apresenta características gerais como: simetria, ter máximo quando <img src="https://render.githubusercontent.com/render/math?math=p_1 = p_2 = ... = p_n"> e ter um mínimo quando <img src="https://render.githubusercontent.com/render/math?math=p_i = 1">. Logo, a proposta natural de uma AD deve tentar maximizar a divisão dos subconjuntos que geram menor erro. Portanto, uma divisão que mantém a proporção de classes em todo o subconjunto não tem utilidade e uma divisão em que cada subconjunto contém somente exemplos de uma classe tem utilidade máxima. Casos intermediários são tratados de forma diferente por cada medida de impureza.

#### Entropia e Ganho de Informação

Existem diversas medidas de impureza como ganho de informação, entropia, distância, ângulo, qui-quadrado e etc. Nessa seção vamos abordar a entropia e ganho de informação, medidas base para o entendimento do algoritmo [C4.5](https://pt.wikipedia.org/wiki/Algoritmo_C4.5) proposto por Ross Quinlan em 1993.

A entropia mede a aleatoriedade de uma variável aleatória em bits. Suponha uma variável aleatória A com domínio <img src="https://render.githubusercontent.com/render/math?math=a_1,a_2,...,a_v">. Suponha que a probabilidade de observar os valores são <img src="https://render.githubusercontent.com/render/math?math=p_1,p_2,...,p_v">. A entropia de A é calculada como:

<img src="https://render.githubusercontent.com/render/math?math=H(A) = -\sum_{i}p_i \ln p_i">

Em uma AD, entropia é usada para medir a aleatoriedade do atributo alvo. A cada nó de decisão da árvore, o atributo que mais reduz a aleatoriedade da variável alvo será escolhido para dividir os dados. O ganho de informação é medido em cada atributo para verificar o quanto eles reduzem a entropia do sistema. Portanto o ganho de informação é medido como a diferença da entropia do conjunto de dados e a soma ponderada da entropia das partições.

Assumindo que temos um problema de classificação binária, ou seja, duas classes e que cada uma delas tenha *p* e *q* exemplos no conjunto de treinamento, respectivamente. Dessa forma podemos calcular a entropia das classes da seguinte forma:

<img src="https://render.githubusercontent.com/render/math?math=H(p,q) = -\frac{p}{p %2B q} \ln \frac{p}{p %2B q} - \frac{q}{p %2B q} \ln \frac{q}{p%2B q}">

Alem disso, essa base tem um atributo A com *v* categorias. Para calcular a entropia desse atributo precisamos considerar a entropia das partições, ou seja, a entropia de cada uma das *v* categorias. A seguinte equação representa a entropia das partições *p* e *q* do atributo A:

<img src="https://render.githubusercontent.com/render/math?math=E(A,p,q) = \sum_{i=1}^{v}\frac{p_i %2B q_i}{p %2B q} H(p_i,q_i)">

Uma vez que sabemos a entropia do conjunto de dados e a entropia do atributo A, podemos calcular o ganho de informação por meio da diferença entre a entropia da base e a entropia das partições. A equação a seguir representa esse conceito:

<img src="https://render.githubusercontent.com/render/math?math=IG(A,p,q) = H(p,q) - E(A,p,q)">

A heurística apresentada deve ser aplicada para todos atributos da base com o objetivo de selecionar aquele que maximiza o ganho de informação para aquele conjunto de dados. Como mencionado, normalmente os testes são realizados em atributos nominais e irá dividir os dados em tantos subconjuntos quantos os valores do atributo.

### Exemplo Ilustrativo

O conjunto de dados *Jogar Tênis* é um problema de classificação binária em que pretende-se classificar se uma pessoa deve ou não, dado certas condições climáticas, jogar tênis. Os atributos de entrada são o *Tempo*, *Temperatura*, *Umidade* e *Vento*. O conjunto tem 14 amostras de treinamento e a última coluna denominada *Joga* representa os rótulos jogar ou não tênis. Os atributos *Tempo* e *Vento* são categóricos e os atributos *Temperatura* e *Umidade* são contínuos.

![](jogatenis.png) *Base de dados Jogar Tênis. Adaptado de Katti Faceli et al., (2011)*

Para construir uma AD precisamos descobrir o atributo que melhor discrimina as classes. Para isso, precisamos calcular as probabilidades associadas de cada classe, a entropia do conjunto de treinamento, a entropia das partições e então estimar o ganho de informação de cada atributo. A seguir iremos calcular o ganho de informação do atributo *Tempo*.

**1⁰ Passo:**

Probabilidade associada de cada classe:

<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Sim) = \frac{9}{14}">
<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Nao) = \frac{5}{14}">

Entropia da classe para todo o conjunto de treinamento:

<img src="https://render.githubusercontent.com/render/math?math=H(Joga) = - \frac{9}{14} * \ln \frac{9}{14} - \frac{5}{14} *  \ln \frac{5}{14} = 0.940 bit">

**2⁰ Passo:**

Estimar a probabilidades de observar as classes dado cada categoria do atributo *Tempo*:

<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Sim | Tempo = Ensolarado) = \frac{2}{5}">
<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Nao | Tempo = Ensolarado) = \frac{3}{5}">
<img src="https://render.githubusercontent.com/render/math?math=H(Joga | Tempo = Ensolarado) = - \frac{2}{5} * \ln \frac{2}{5} - \frac{3}{5} * \ln \frac{3}{5} = 0.971 bit">

<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Sim | Tempo = Nublado) = \frac{4}{4}">
<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Nao | Tempo = Nublado) = \frac{0}{4}">
<img src="https://render.githubusercontent.com/render/math?math=H(Jogar | Tempo = Nublado) = - \frac{4}{4} * \ln \frac{4}{4} - \frac{0}{4} * \ln \frac{0}{4} = 0 bit">

<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Sim | Tempo = Chuvoso) = \frac{3}{5}">
<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Nao | Tempo = Chuvoso) = \frac{2}{5}">
<img src="https://render.githubusercontent.com/render/math?math=H(Jogar | Tempo = Chuvoso) = - \frac{3}{5} * \ln \frac{3}{5} - \frac{2}{5} * \ln \frac{2}{5} = 0.971 bit">

**3⁰ Passo:** Calcular a entropia ponderada para o atributo *Tempo*:

<img src="https://render.githubusercontent.com/render/math?math=H(Tempo) = \frac{5}{14} * 0.971 + \frac{4}{14} * 0 + \frac{5}{14} * 0.971 = 0.693 bit">

**4⁰ Passo:** Calcular o ganho de informação em dividir o conjunto de acordo com os valores do atributo *Tempo*:

<img src="https://render.githubusercontent.com/render/math?math=IG(Tempo) = H(Joga) - H(Joga|Tempo)">
<img src="https://render.githubusercontent.com/render/math?math=IG(Tempo) = 0.940 - 0.693 = 0.247 bit">

Uma vez que foi calculado o ganho de informação gerado pelo atributo *Tempo*, o mesmo precisa ser feito para o atributo *Vento*, *Temperatura* e *Umidade*. No caso dos dois últimos, como eles são atributos contínuos, alguma estratégia precisa ser utilizada para permitir a divisão desse atributo em partições.

As estratégias mais utilizadas é a discretização ou a escolha de um ponto de corte binário. A discretização está relacionada a transformação dos dados contínuos em categóricos por meio de estratégias como o [histograma](https://pt.wikipedia.org/wiki/Histograma) enquanto a escolha de um ponto de corte define um valor do conjunto de treinamento (normalmente utilizando uma função de mérito) que permite a construção de uma partição binária. Usualmente utiliza-se a segunda estratégia.

No exemplo da base *Jogar Tênis*, um ponto de corte interessante para o atributo *Temperatura* é o valor 70.5. Esse valor permite separar as amostrar em partições interessantes. A seguir os passos 2, 3 e 4 são repetidos:

**2⁰ Passo:**

Estimar a probabilidades de observar as classes dado cada categoria do atributo *Temperatura*:

<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Sim | Temperatura \leq 70.5) = \frac{4}{5}">
<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Nao | Temperatura \leq 70.5) = \frac{1}{5}">
<img src="https://render.githubusercontent.com/render/math?math=H(Joga | Temperatura \leq 70.5) = - \frac{4}{5} * \ln \frac{4}{5} - \frac{1}{5} * \ln \frac{1}{5} = 0.721 bit">

<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Sim | Temperatura > 70.5) = \frac{5}{9}">
<img src="https://render.githubusercontent.com/render/math?math=p(Joga = Nao | Temperatura > 70.5) = \frac{4}{9}">
<img src="https://render.githubusercontent.com/render/math?math=H(Joga | Temperatura = Ensolarado) = - \frac{5}{9} * \ln \frac{5}{9} - \frac{4}{9} * \ln \frac{4}{9} = 0.991 bit">

**3⁰ Passo:** Calcular a entropia ponderada para o atributo *Temperatura*:

<img src="https://render.githubusercontent.com/render/math?math=H(Temperatura) = \frac{5}{14} * 0.721 + \frac{9}{14} * 0.991 = 0.895 bit">

**4⁰ Passo:** Calcular o ganho de informação em dividir o conjunto de acordo com os valores do atributo *Temperatura*:

<img src="https://render.githubusercontent.com/render/math?math=IG(Temperatura) = H(Joga) - H(Joga|Temperatura)">
<img src="https://render.githubusercontent.com/render/math?math=IG(Temperatura) = 0.940 - 0.895 = 0.045 bit">

Sumarizando o ganho de informação para todos os atributos temos os seguintes valores:

<img src="https://render.githubusercontent.com/render/math?math=IG(Tempo) = 0.940 - 0.693 = 0.247 bit">
<img src="https://render.githubusercontent.com/render/math?math=IG(Temperatura) = 0.940 - 0.895 = 0.045 bit">
<img src="https://render.githubusercontent.com/render/math?math=IG(Umidade) = 0.940 - 0.789 = 0.151 bit">
<img src="https://render.githubusercontent.com/render/math?math=IG(Vento) = 0.940 - 0.892 = 0.048 bit">

Portanto podemos concluir que o atributo *Tempo* é que gera maior redução no ganho de informação. Logo o nó raiz da AD é composta pelo nó *Tempo* com 3 ramos, cada um relacionado a uma categoria desse atributo categórico (ensolarado, chuvoso e nublado). Como o algoritmo é baseado em dividir para conquistar, cada nó filho da árvore precisa ser construído baseado em sua partição dos dados. O processo se repete até que todos os nós sejam puros ou uma estratégia de poda seja aplicada.

### Estratégia de poda

As estatísticas calculadas em nós mais rasos em uma AD costumam ser os mais importantes enquanto estatísticas de nós mais profundos costumam ter níveis de importância menores. Isso se dá porque os nós mais rasos refletem os conceitos mais gerais enquanto os mais profundos, os conceitos mais específicos e normalmente relacionados ao [superajustamento](https://pt.wikipedia.org/wiki/Sobreajuste). O superajustamento está diretamente relacionado ao tamanho da árvore. Quanto maior, mais superajustada e mais difícil de ser interpretada. Portanto, podar uma árvore, que é trocar nós profundos por nós folhas, pode ajudar a minimizar esse problema.

A troca dos nós mais profundos por folhas pode causar a classificação errônea de alguns exemplos do conjunto de treinamento. Apensar de parecer contra-intuitivo, isso pode melhorar o desempenho para exemplos novos nunca antes vistos. Os métodos de poda mais conhecidos são: pré-poda e pós-poda. Enquanto a pré-poda é realizada durante a construção da árvore, a pós-poda é realizada depois da construção da árvore.


## _Random Forest_

## _XGBoost_

## Redes Neurais Artificiais

