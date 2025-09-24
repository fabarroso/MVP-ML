# MVP-ML

MVP - Machine Learning

2. Ambiente

O ambiente de análise foi configurado para garantir total reprodutibilidade. Para isso, foram fixadas seeds em Python (random, numpy e TensorFlow) e no sistema operacional, garantindo que todas as operações aleatórias, como divisão de dados e inicialização de modelos, produzissem resultados consistentes. Bibliotecas utilizadas incluem:
 - pandas e numpy para manipulação de dados;
 - scikit-learn para pré-processamento, pipelines, treino de modelos clássicos, validação cruzada e métricas de regressão;
 - XGBoost e LightGBM para modelos de boosting;
 - TensorFlow/Keras para redes neurais (Deep Learning).

As pipelines automatizadas garantem que qualquer transformação aplicada ao conjunto de treino seja reaplicada ao conjunto de teste, prevenindo vazamento de dados e mantendo a comparabilidade de resultados.

3. Dados: Carga, Entendimento e Qualidade

O dataset contém informações mensais de homicídios dolosos, indicadores socioeconômicos, demográficos e categóricos de municípios. O carregamento incluiu verificação de codificação e delimitadores, garantindo a integridade dos dados.

Durante o entendimento da base, foram identificados:
 - Variáveis numéricas e categóricas, algumas com valores ausentes;
 - Forte assimetria e dispersão no target, com outliers significativos em grandes centros urbanos;
 - Dependências entre indicadores socioeconômicos (densidade populacional, renda, desemprego) e homicídios.

Tratamentos realizados:
 - Remoção de linhas de anos incompletos para evitar viés temporal;
 - Identificação de tipos de variáveis e tratamento de valores ausentes.

3.1 Análise Exploratória Resumida (EDA)
A EDA combinou análise gráfica e estatística descritiva:
 - Distribuição do target: histograma com densidade e boxplot mostraram valores concentrados próximos a zero e outliers extremos;
 - Evolução temporal: linha de tendência anual destacou ciclos de aumento e diminuição de homicídios, indicando influência de políticas públicas ou eventos específicos;
 - Correlação: matrizes de correlação e heatmaps evidenciaram associações fortes entre homicídios e variáveis como densidade populacional e renda média;
 - Variáveis categóricas: região administrativa e tipo de município apresentaram padrões de concentração distintos, justificando codificação via OneHotEncoder para modelagem.

Essa análise permitiu selecionar features relevantes, reduzir ruído e definir estratégias adequadas para o pré-processamento e modelagem.

4. Definição do Target, Variáveis e Divisão dos Dados

O target foi definido como homicídios dolosos por município e ano, devido à sua relevância social, disponibilidade padronizada e natureza quantitativa. As features incluíram:
 - Variáveis socioeconômicas (renda média, desemprego);
 - Demográficas (população, densidade);
 - Categóricas (região administrativa, tipo de município).

A divisão em treino e teste foi de 80/20, preservando a distribuição do target. Isso permitiu avaliar a capacidade de generalização dos modelos em dados não vistos, considerando heterogeneidade e outliers.

5. Tratamento de Dados e Pipeline de Pré-processamento

O pipeline de pré-processamento implementou transformações robustas e matematicamente fundamentadas:

  1 - Variáveis numéricas:
  
     - Imputação da mediana para lidar com outliers e valores ausentes, evitando distorções;
     - Padronização (Z-score) para normalizar escalas diferentes, essencial para modelos sensíveis à magnitude das features;
     - PowerTransformer (Yeo-Johnson) para reduzir assimetria, aproximando a distribuição à normalidade, favorecendo regressão linear e gradientes em modelos de boosting;
     - Winsorização aplicada para limitar valores extremos e reduzir impacto de outliers na modelagem.

  2 - Variáveis categóricas:
  
     - Imputação da moda para valores ausentes;
     - OneHotEncoder para transformar categorias em representações numéricas, mantendo relação sem impor ordem.

O pipeline garante consistência, reprodutibilidade e prevenção de vazamento, sendo aplicado integralmente tanto em treino quanto em teste.

6. Baseline e Modelos Candidatos

O baseline foi Regressão Linear, escolhida por sua simplicidade e interpretabilidade, permitindo identificar relações globais entre homicídios e variáveis socioeconômicas. Apesar de capturar tendências gerais, apresentou limitações para outliers e padrões não-lineares.

Modelos mais complexos foram aplicados para lidar com dispersão e heterogeneidade do target:
Random Forest: captura relações não-lineares e interações complexas entre features, essencial para modelar efeitos combinados de renda, população e densidade;
XGBoost e LightGBM: algoritmos de boosting que ajustam gradientes de erro de forma iterativa, robustos a outliers e variáveis correlacionadas, modelando nuances que regressão linear não consegue.

Resultados rápidos (baseline vs candidatos):
Linear Regression: R² ≈ 0.60, RMSE ≈ 3.18;
Random Forest: R² ≈ 0.96, RMSE ≈ 1.04;
XGBoost: R² ≈ 0.96, RMSE ≈ 1.05;
LightGBM: R² ≈ 0.96, RMSE ≈ 1.00.

Isso evidencia que modelos de árvore e boosting capturam melhor a complexidade e variabilidade presentes nos dados.

7. Validação e Otimização de Hiperparâmetros

Modelos de boosting passaram por RandomizedSearchCV em amostras reduzidas de treino (40%) para otimizar hiperparâmetros:
n_estimators: número de árvores;
max_depth: profundidade máxima de cada árvore;
learning_rate: taxa de aprendizado do gradiente.

A validação cruzada (3 folds) foi usada para avaliar consistência e robustez, equilibrando desempenho e tempo computacional. Os resultados mostraram melhoria sutil nos modelos otimizados, mantendo alta generalização sem overfitting.

8. Avaliação Final, Análise de Erros e Limitações

O desempenho final foi avaliado com R², RMSE e MAE, além de análise de resíduos e matrizes de confusão adaptadas (regressão categorizada).

Modelos de árvore e boosting apresentaram baixa dispersão residual e R² elevados, evidenciando ajuste adequado a heterogeneidade do target;
Limitações incluem sensibilidade a outliers extremos e ausência de variáveis externas (dinâmicas espaciais, lags temporais).

9. Deep Learning

Foi implementada uma rede neural MLP (Multi-Layer Perceptron):
Estrutura: 2 camadas densas (64 e 32 neurônios), função de ativação ReLU e camada de saída linear;
Otimização: Adam, loss MSE, early stopping para prevenção de overfitting;
Pré-processamento: mesmo pipeline aplicado aos dados numéricos e categóricos.

O modelo capturou interações não-lineares complexas, complementando modelos de boosting, especialmente em municípios com homicídios extremos.
