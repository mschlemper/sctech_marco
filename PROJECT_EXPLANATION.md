
# Explicação Detalhada do Projeto de Precificação de Imóveis em King County

Este documento serve como um guia detalhado sobre o desenvolvimento do modelo de Inteligência Artificial para prever preços de imóveis em King County, EUA, utilizando o dataset `kc_house_data.csv`. O projeto foi estruturado em 6 fases principais, cada uma com seus objetivos, implementações e resultados.

## Visão Geral e Problema Preditivo
O objetivo central é construir um pipeline preditivo capaz de estimar o valor de venda de um imóvel (`price`, variável numérica contínua em dólares) com base em suas características. A previsão precisa do preço é vital para diversas partes interessadas no mercado imobiliário.

---

## Fase 1: Análise Exploratória de Dados (EDA)

**Objetivo:** Compreender a estrutura do dataset, identificar padrões, anomalias e relações entre as variáveis que guiarão as fases seguintes.

**Ações Realizadas:**
1.  **Carregamento e Dimensões:** O dataset `kc_house_data.csv` foi carregado, revelando 21613 linhas e 25 colunas.
2.  **Tipos de Dados:** Foi verificado que a maioria das colunas é numérica, com exceção da coluna `date` (`object`). A coluna `sqft_above` apresentou 2 valores nulos.
3.  **Estatísticas Descritivas:** O método `.describe()` revelou a grande variação na variável `price` (média de $540.000, desvio padrão de $367.000), e distribuições assimétricas para algumas variáveis.
4.  **Visualização da Variável Alvo (`price`):** Um histograma da distribuição do `price` foi plotado, mostrando uma assimetria positiva de **4.02**. Isso indica que a maioria dos imóveis tem preços mais baixos, com uma cauda longa de imóveis mais caros.
5.  **Gráficos de Dispersão:** Plotamos `price` vs. `sqft_living` e `price` vs. `grade`. Ambos mostraram correlações positivas claras: imóveis maiores e de melhor qualidade tendem a ser mais caros.
6.  **Mapa de Calor de Correlação:** Um mapa de calor de Pearson destacou: 
    *   `sqft_living` (0.70), `grade` (0.67) e `sqft_above` (0.69) como as variáveis mais correlacionadas com `price`.
    *   Alta multicolinearidade entre variáveis explicativas como `sqft_living` e `sqft_above` (0.88).

**Implicações para o Projeto:**
*   A assimetria de `price` sugere a necessidade de transformação da variável alvo ou uso de modelos robustos. 
*   A multicolinearidade identificada exigirá tratamento para evitar distorção dos coeficientes do modelo.

---

## Fase 2: Tratamento e Limpeza (Data Prep)

**Objetivo:** Preparar os dados para a modelagem, garantindo sua qualidade e consistência.

**Ações Realizadas:**
1.  **Linhas Duplicadas:** Foi verificado que **0** linhas duplicadas. Nenhuma duplicata foi encontrada ou foram removidas, garantindo a unicidade dos registros.
2.  **Valores Ausentes (`sqft_above`):** A coluna `sqft_above` apresentava valores ausentes. Devido à sua distribuição assimétrica (assimetria de 0.91), os valores ausentes foram imputados com a **mediana** (1560.00) para garantir robustez a outliers.
3.  **Gerenciamento de Outliers:** Boxplots revelaram outliers em várias variáveis (`price`, `bedrooms`, `bathrooms`, `sqft_living`, `sqft_lot`, `sqft_above`, `sqft_basement`). Adotamos a estratégia de **contenção por limite (capping)**, utilizando o IQR, para mitigar o impacto desses valores extremos sem removê-los completamente, o que é importante para modelos de regressão linear que são sensíveis a outliers.

**Resultados:** Dataset limpo e tratado, com valores ausentes preenchidos e outliers gerenciados, pronto para a próxima etapa.

---

## Fase 3: Feature Engineering (Coluna Calculada)

**Objetivo:** Criar novas variáveis a partir das existentes para enriquecer o poder preditivo do modelo.

**Ações Realizadas:**
1.  **`idade_imovel`:** Calculada como `yr_sale - yr_built`. Esta variável representa a idade do imóvel no momento da venda, capturando um fator importante para a precificação. Valores negativos (erros de dados) foram corrigidos para 0.
2.  **`foi_reformado`:** Uma variável binária (`0` ou `1`) indicando se o imóvel foi reformado, derivada de `yr_renovated`. Imóveis reformados geralmente têm maior valor.
3.  **`preco_por_m2`:** Calculada como `price / sqft_living`. Esta feature foi criada para fins de **Análise Exploratória** e **não foi utilizada como preditora no modelo** para evitar vazamento de dados, pois `price` é a variável alvo.

**Resultados:** O dataset foi enriquecido com novas variáveis contextuais que podem ajudar o modelo a aprender padrões mais complexos.

---

## Fase 4: Preparação para Modelagem

**Objetivo:** Estruturar o dataset final para o treinamento do modelo, incluindo codificação, tratamento de multicolinearidade, divisão e escalonamento.

**Ações Realizadas:**
1.  **Remoção de Colunas Irrelevantes/Redundantes:** As colunas `id`, `date`, `yr_sale`, `yr_renovated` e `preco_por_m2` foram removidas, pois já foram usadas para criar novas features ou não são adequadas para modelagem direta.
2.  **Codificação (`zipcode`):** A variável `zipcode` (nominal) foi convertida para tipo categórico e One-Hot Encoding foi aplicado, resultando em 69 novas colunas binárias para representar cada CEP.
3.  **Multicolinearidade (VIF):** A análise do VIF (Variance Inflation Factor) revelou alta multicolinearidade entre `sqft_living`, `sqft_above` e `sqft_living15`. Para resolver isso, as colunas `sqft_above` e `sqft_living15` foram removidas, mantendo `sqft_living` como a principal medida de área por ser mais abrangente.
4.  **Divisão Amostral (`train_test_split`):** O dataset foi dividido em conjuntos de treino (80%) e teste (20%) para as variáveis explicativas (X) e a variável alvo (y).
    *   `X_train`: (17290, 85)
    *   `X_test`: (4323, 85)
    *   `y_train`: (17290,)
    *   `y_test`: (4323,)
5.  **Escalonamento (`StandardScaler`):** `StandardScaler` foi aplicado a `X_train` (`fit_transform`) e `X_test` (`transform`) para padronizar as features, essencial para a Regressão Linear.

**Resultados:** Dados prontos para o treinamento do modelo, com multicolinearidade reduzida e features escalonadas, otimizando o desempenho do modelo.

---

## Fase 5: Modelagem, Validação e Diagnóstico de Overfitting

**Objetivo:** Treinar o modelo de regressão linear, avaliar seu desempenho e diagnosticar se há overfitting ou underfitting.

**Ações Realizadas:**
1.  **Modelo de Regressão Linear:** Um modelo `LinearRegression` do `scikit-learn` foi inicializado e treinado com os dados escalonados de `X_train_scaled` e `y_train`.
2.  **Predições:** Foram realizadas predições nos conjuntos de treino (`y_train_pred`) e teste (`y_test_pred`).
3.  **Cálculo de Métricas:** As métricas MAE, RMSE e R² Score foram calculadas para ambos os conjuntos:
    *   **TREINO:** MAE: 97079.09, RMSE: 172174.55, R² Score: 0.77
    *   **TESTE:** MAE: 100835.82, RMSE: 186014.20, R² Score: 0.77
4.  **Diagnóstico de Overfitting:** A comparação das métricas de treino e teste revelou que os valores estão próximos, indicando que o modelo apresenta uma **boa generalização** e não está sofrendo de overfitting significativo.

**Resultados:** Um modelo de Regressão Linear treinado e validado, com boa capacidade de generalização.

---

## Fase 6: Avaliação, Interpretação e Versionamento do Modelo

**Objetivo:** Consolidar as métricas finais, interpretar os resultados no contexto de negócio e salvar o modelo para futuras utilizações e auditorias.

**Ações Realizadas:**
1.  **Métricas Técnicas Finais (Conjunto de Teste):**
    *   **MAE (Mean Absolute Error):** US$ 100835.82
    *   **RMSE (Root Mean Squared Error):** US$ 186014.20
    *   **R² Score (Coeficiente de Determinação):** 0.77
2.  **Análise Gráfica:**
    *   **Valores Reais vs. Previstos:** Um gráfico de dispersão mostrou a relação entre os valores reais e as predições do modelo. A linha de 'Ajuste Perfeito' (`y=x`) ajuda a visualizar a qualidade das predições. 
    *   **Distribuição dos Resíduos:** Um histograma dos resíduos (`y_test - y_test_pred`) foi plotado, idealmente centrada em zero e com distribuição normal (embora aqui tenhamos observado uma assimetria de 5.14, indicando que o modelo pode ter uma tendência a superestimar/subestimar em certos casos).
3.  **Veredito de Negócios:** O modelo explica 77% da variabilidade do preço, o que é razoável para uma primeira versão. Contudo, o MAE de aproximadamente US$ 100 mil é um erro considerável para decisões de compra/venda, especialmente para imóveis de menor valor. Isso indica a necessidade de melhorias para que o modelo seja mais aplicável no contexto de negócio.
4.  **Versionamento do Modelo:** O modelo treinado (`modelo_regressao_v1.pkl`) e suas métricas de desempenho (`metricas_v1.json`) foram salvos na pasta `models/v1/` para garantir reprodutibilidade e controle de versão.

**Recomendações para Próximas Etapas:**
*   Explorar modelos mais complexos (Random Forest, Gradient Boosting).
*   Realizar engenharia de features adicional.
*   Otimizar hiperparâmetros do modelo.
*   Considerar a inclusão de dados externos (socioeconômicos, infraestrutura).

**Resultados:** O projeto foi concluído, com um modelo funcional e documentado, e um plano claro para futuras iterações.
