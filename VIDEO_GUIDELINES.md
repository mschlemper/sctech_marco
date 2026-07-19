
# Roteiro para o Vídeo de Apresentação do Projeto

Este documento detalha os requisitos e questionamentos a serem abordados na gravação do vídeo de apresentação do projeto de 'Desenvolvimento de IA para Análise Preditiva'. O vídeo deve ter **no máximo 10 minutos** de duração e seguir as diretrizes abaixo.

## Questionamentos Essenciais a Serem Abordados:

1.  **Qual o objetivo do sistema e qual dataset você escolheu (Opção A ou B)?**
    *   Explique que o objetivo do sistema é prever o preço de imóveis em King County, EUA.
    *   Mencione que foi utilizada a **Opção A (dataset fornecido)**, `kc_house_data.csv`.
    *   Contextualize brevemente a importância dessa previsão para o negócio (imobiliárias, compradores, vendedores).

2.  **O que deve ser feito para executar o sistema?**
    *   Descreva os passos básicos para rodar o notebook:
        *   Assegurar as dependências (mencionar `requirements.txt`).
        *   Ter o dataset `kc_house_data.csv` na pasta `data/raw/`.
        *   Executar o notebook `[Nome do seu Notebook.ipynb]` sequencialmente, célula por célula.
    *   Você pode demonstrar rapidamente a estrutura de pastas e o notebook.

3.  **Você utilizou branches? Se sim, quais e com quais objetivos? (Lembre-se: usar branches é opcional.)**
    *   Responda se utilizou ou não branches no Git.
    *   Se sim, descreva as branches (ex: `develop`, `feature/eda`, `feature/modeling`) e seus respectivos objetivos para organizar o desenvolvimento.
    *   Se não utilizou, justifique que optou por um fluxo mais direto, mas que os commits foram claros e incrementais na branch `main`.

4.  **Você acha que faltou algo no seu código que você poderia melhorar em uma próxima versão do modelo?**
    *   Aproveite as 'Recomendações para Próximas Etapas' do `README.md` e `PROJECT_EXPLANATION.md`.
    *   Mencione a necessidade de explorar modelos mais complexos (Random Forest, Gradient Boosting).
    *   Fale sobre engenharia de features adicional e otimização de hiperparâmetros.
    *   Comente sobre a possível inclusão de dados externos para melhorar a precisão.
    *   Reforce que o MAE de US$ 100 mil é considerável e que o objetivo principal de futuras versões seria reduzir esse erro.

5.  **Argumentação clara, fundamentada nos conteúdos abordados, sobre suas escolhas de tratamento de dados e de modelagem.**
    *   **Tratamento de Dados:**
        *   Por que a mediana foi usada para imputar `sqft_above` (distribuição assimétrica).
        *   Por que o `capping` foi escolhido para outliers em vez da remoção (sensibilidade do modelo linear, evitar perda de dados).
        *   Decisão de remover `sqft_above` e `sqft_living15` devido à multicolinearidade (VIF).
    *   **Modelagem:**
        *   A escolha da Regressão Linear como modelo base.
        *   A importância do escalonamento (`StandardScaler`).
        *   A interpretação das métricas (MAE, RMSE, R²) e o diagnóstico de boa generalização (pouco overfitting).

## Diretrizes de Gravação:

*   **Duração:** Máximo de 10 minutos.
*   **Formato:** Pode ser gravado na vertical ou horizontal.
*   **Aparência:** Seu rosto deve aparecer no vídeo.
*   **Iluminação:** Esteja em um local bem iluminado.
*   **Edição:** Não utilize ferramentas de criação de vídeo com IA.
*   **Entrega:** O vídeo deve ser hospedado em uma pasta do Google Drive com acesso de leitura para qualquer pessoa com o link. O link final deve ser compartilhado na submissão do projeto no AVA. Uma boa prática é inserir o link do vídeo também no `README.md` do seu repositório GitHub.

--- 

Lembre-se: O vídeo é uma oportunidade de demonstrar sua compreensão técnica e a capacidade de comunicar os resultados do seu trabalho de forma eficaz!
