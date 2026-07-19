# Projeto de Precificação de Imóveis em King County

## Visão Geral do Projeto
Este projeto visa desenvolver um modelo de Inteligência Artificial para prever o preço de imóveis no condado de King County, EUA. Utilizando o dataset `kc_house_data.csv`, o objetivo principal é construir um pipeline preditivo capaz de estimar o valor de venda de um imóvel com base em suas características físicas e de localização.

### Problema Preditivo
A variável-alvo que estamos tentando prever é `price` (valor numérico contínuo em dólares). Prever com precisão o preço dos imóveis é crucial para imobiliárias, compradores e vendedores, auxiliando em decisões de compra, venda, avaliação e financiamento.

## Fases do Projeto
O projeto foi estruturado em 6 fases principais, seguindo as melhores práticas de Ciência de Dados:

1.  **Análise Exploratória de Dados (EDA)**: Compreensão da estrutura dos dados, identificação de tipos de variáveis, estatísticas descritivas e visualização de distribuições e correlações.
2.  **Tratamento e Limpeza (Data Prep)**: Lidar com linhas duplicadas, valores ausentes (imputação por mediana) e outliers (contenção por limite).
3.  **Feature Engineering (Coluna Calculada)**: Criação de novas variáveis como `idade_imovel`, `foi_reformado` e `preco_por_m2`.
4.  **Preparação para Modelagem**: Codificação de variáveis categóricas (One-Hot Encoding para `zipcode`), tratamento de multicolinearidade (remoção de `sqft_above` e `sqft_living15`), divisão dos dados em treino/teste e escalonamento (`StandardScaler`).
5.  **Modelagem, Validação e Diagnóstico de Overfitting**: Treinamento de um modelo de Regressão Linear, avaliação de desempenho e diagnóstico de overfitting.
6.  **Avaliação, Interpretação e Versionamento do Modelo**: Apresentação de métricas finais, análise gráfica de resíduos, veredito de negócios e salvamento do modelo e suas métricas para versionamento.

## Tecnologias e Bibliotecas Utilizadas
-   **Python**: Linguagem de programação principal.
-   **Pandas**: Manipulação e análise de dados.
-   **NumPy**: Operações numéricas.
-   **Scikit-learn**: Ferramentas de pré-processamento, modelagem (Regressão Linear) e avaliação.
-   **Matplotlib**: Geração de gráficos.
-   **Seaborn**: Visualização de dados estatísticos.
-   **Statsmodels**: Análise estatística (utilizado para VIF).
-   **Joblib**: Serialização e desserialização de modelos Python.

## Desempenho do Modelo (Versão v1)
O modelo de Regressão Linear (v1) foi salvo em `models/v1/modelo_regressao_v1.pkl` e suas métricas de desempenho no conjunto de teste são:
-   **MAE (Mean Absolute Error)**: `mae_test:.2f`
-   **RMSE (Root Mean Squared Error)**: `rmse_test:.2f`
-   **R2 Score (Coeficiente de Determinação)**: `r2_test:.2f`

As métricas detalhadas e as variáveis explicativas utilizadas podem ser encontradas em `models/v1/metricas_v1.json`.

## Conclusão e Próximas Melhorias
A primeira versão do modelo alcançou um R² de 0.77, explicando 77% da variabilidade do preço. No entanto, o MAE de aproximadamente 100 mil dólares é considerável para o contexto de negócio. Para futuras iterações (v2, v3, etc.), as seguintes melhorias são recomendadas:

1.  **Explorar Modelos Mais Complexos**: Avaliar algoritmos como Random Forest, Gradient Boosting ou redes neurais para capturar relações não-lineares.
2.  **Engenharia de Features Adicional**: Criar features mais elaboradas, como interações entre variáveis ou a inclusão de dados geográficos mais detalhados.
3.  **Otimização de Hiperparâmetros**: Realizar tunagem fina dos hiperparâmetros dos modelos utilizando técnicas como Grid Search ou Random Search.
4.  **Inclusão de Dados Externos**: Considerar a adição de dados socioeconômicos, índices de criminalidade, ou proximidade a pontos de interesse.

## Como Executar o Sistema
1.  Certifique-se de ter as dependências listadas em `requirements.txt` instaladas.
2.  Baixe o dataset `kc_house_data.csv` e coloque-o na pasta `data/raw/`.
3.  Execute o notebook `[Nome do seu Notebook.ipynb]` sequencialmente.

Para fins de avaliação, este projeto contém um vídeo explicativo que aborda os pontos solicitados na Seção 5.4 do roteiro do projeto.
""".format(mae_test=mae_test, rmse_test=rmse_test, r2_test=r2_test)

with open('README.md', 'w') as f:
    f.write(readme_content)

print("Arquivo 'README.md' criado com sucesso na raiz do projeto.")
