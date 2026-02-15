# social-anxiety-analysis

## 📝 Descrição

Este projeto apresenta uma análise da relação entre características individuais (como hábitos, gênero, ocupação e qualidade da alimentação) e níveis de ansiedade social. O estudo foi conduzido a partir do [*Social Anxiety Dataset*](https://www.kaggle.com/datasets/natezhang123/social-anxiety-dataset/) retirado da plataforma [*Kaggle*](https://www.kaggle.com/), que reúne mais de 10.000 amostras sintéticas projetadas para simular padrões realistas de indivíduos com diferentes graus de ansiedade social.

Para a realização do estudo, foram adotados modelos de classificação (aprendizado supervisionado) visando à predição do nível de ansiedade social a partir dos atributos comportamentais modelados.

## 🧪 Metodologia
1. **Análise inicial** do dataset (`data/data.csv`) e das **distribuições das features**.
2. Para enriquecer e análise, foram criados **4 níveis de labels**:
   1. **Nível 1**: pouco ansioso (1 e 2) vs muito ansioso (9 e 10) -> `data/data_level_1.csv`
   2. **Nível 2**: pouco/medio (<= 5) vs medio/muito (> 5) -> `data/data_level_2.csv`
   3. **Nível 3**: pouco (1 e 2), moderada (4 a 6), muito (9 e 10) -> `data/data_level_3.csv`
   4. **Nível 4**: categorias originais (1 a 10) -> `data/data_level_4.csv`
3. A partir do `data/data_level_1.csv` realizou-se uma **análise de *feature importance*** através:
   1. Gráficos da distribuição de cada feature para cada classe
   2. Gráfico de informação mútua
4. **Pré-processamento**:
   1. **One-hot** encoding com `drop_first`.
   2. Split treino/teste com **25% para teste**.
   3. Escalonamento com `MinMaxScaler` (features em $[0,1]$).
5. Redução de dimensionalidade (2D) para visualização usando *PCA*, *t-SNE* e *MDS*
6. **Aplicação de modelos de classificação**:
   - Foram avaliados dois modelos de classificação: **KNN** e **Random Forest**. Cada modelo foi testado considerando os quatro níveis de *labels* definidos na Etapa 2. Para cada nível, os modelos passaram por três configurações de treinamento:
     1. utilizando os dados originais;
     2. utilizando os dados reduzidos para três dimensões por meio do **PCA**;
     3. utilizando os dados originais com a remoção de features de baixa relevância.
   - O processo de treinamento e seleção de hiperparâmetros foi conduzido por meio do `GridSearchCV`, visando identificar a melhor configuração de cada modelo.
   - A avaliação de desempenho foi realizada utilizando o `classification_report`, complementada pela análise da matriz de confusão.

## 📊 Resultados gerais
- Visualização (PCA/t-SNE/MDS): não há clusters totalmente separados, mas altas ansiedades tendem a ficar próximas e com baixa sobreposição com classes distintas.
- Distinguir instâncias com níveis de ansiedade extremos (dataset `data/data_level_1.csv`) é uma tarefa de classificação relativamente simples. Nesse cenário, o KNN obteve valores de precisão e revocação próximos de 100%, mesmo após a redução de dimensionalidade.
- Em contrapartida, a classificação de níveis de ansiedade adjacentes ou semanticamente próximos revelou-se mais complexa, possivelmente devido ao caráter subjetivo das classes.
  - Para esse cenário, o Random Forest apresentou desempenho significativamente superior ao KNN, evidenciado principalmente pelos resultados obtidos no dataset de nível 3.
- Por fim, embora não seja uma prática comum, a redução de dimensionalidade e a remoção de features irrelevantes contribuíram para a melhora do desempenho geral do KNN na maior parte dos cenários avaliados. Esse efeito, contudo, não foi observado de forma significativa no Random Forest, uma vez que, devido ao seu mecanismo de construção baseado em divisões sucessivas do espaço de atributos, o modelo é naturalmente mais robusto à presença de features ruidosas/"prejudiciais".

## 📂 Estrutura do repositório
- `data/`: dataset original e versões com labels por nível
- `train_data/` e `test_data/`: dados escalonados
- `src.ipynb`: notebook com análises, modelos e resultados
- `requirements.txt`: bibliotecas necessárias utilizadas pelo notebook