# 🌍 Análise Global - Expectativa de Vida, Felicidade, PIB e Suicídio

> **Por que países mais felizes e ricos ainda têm altas taxas de suicídio?**

Projeto completo de Data Science e Machine Learning com 132 países, explorando o **paradoxo da felicidade** e agrupando nações por indicadores socioeconômicos.

[Dashboard Métricas](metricas_dashboard.png)

## 📊 Dataset

- **Life_expectancy.csv** - Expectativa de vida (homens/mulheres), Happiness Score, Fertilidade
- **Suicide_Rate.csv** - PIB per capita e taxa de suicídio
- **Merge final:** 132 países (inner join de 142 + 140)

**Features criadas:**
- `Life Expectancy Avg` - média homens/mulheres
- `Gender Gap` - diferença mulher - homem (importante!)
- `Suicide_High` - classe binária (mediana 14.35)

| Indicador | Média | Min | Max |
|---|---|---|---|
| Expectativa | 74.4 anos | 54.2 | 86.7 |
| Felicidade | 5.58 | 3.35 | 7.77 |
| Fertilidade | 2.79 | 1.1 | 7.6 |
| PIB per capita | $15,267 | $300 | $120k |
| Suicídio | 15.85 | 3.5 | 39.7 |

Exemplo **Brasil:** 75.55 anos, Felicidade 6.22, PIB $9,581, Suicídio 18.6 (alto), Fertilidade 1.8, Gap 5.3

## 🔍 Principais Insights - EDA

### Correlações que importam
- **Expectativa x Felicidade: 0.89** - forte positiva
- **Expectativa x Fertilidade: -0.85** - forte negativa
- **Felicidade x Fertilidade: -0.76**
- **Felicidade x PIB: 0.75**
- **Felicidade x Suicídio: 0.01** <- **PARADOXO** - ser feliz não protege
- **PIB x Suicídio: -0.15** - correlação quase nula
- **Gender Gap x Suicídio: 0.33** - a maior com suicídio

> Conclusão EDA: Indicadores macroeconômicos explicam bem felicidade e longevidade, mas **não explicam suicídio**.

## 🧩 Clustering - 4 Arquétipos de Países

K-means com StandardScaler, k=4 escolhido por interpretabilidade (silhueta k=2: 0.386, k=4: 0.378). PCA explica 82.5% da variância.

- **Cluster 0 - Vulnerável (42 países):** Exp 63/69 anos, Felicidade 4.41, Fertilidade 4.47, PIB $2,369, Suicídio 13.87 - Ex: Afeganistão, Angola, Níger
- **Cluster 1 - Intermediário LATAM (51):** Exp 74/78.8, Felicidade 5.79, Fertilidade 2.23, PIB $8,925, Suicídio 15.32 - **Brasil está aqui**, EUA, México
- **Cluster 2 - Cluster do Suicídio (13):** Exp 72.9/78.7, Felicidade 5.66, Fertilidade 1.75, PIB $9,200, **Suicídio 29.25**, Gap 5.81 - Albânia, Armênia, Belarus, Letônia (ex-URSS)
- **Cluster 3 - Rico & Longevo (26):** Exp 80.7/84.4, Felicidade 7.02, Fertilidade 1.69, PIB $51,574, Suicídio 13.37, Gap 3.71 - Suíça, Japão, Noruega

## 🤖 Machine Learning

### Bloco 13 - Regressão (prever valor exato)
**Objetivo:** Prever taxa exata de suicídio

```
Linear Regression -> RMSE: 3.96 | R²: -0.28
Random Forest (200 árvores, depth 8) -> RMSE: 3.35 | R²: 0.08
CV 5-fold R²: 0.25 (+/- 0.28)
```

**Feature Importance Regressão:**
1. Gender Gap 24.8%
2. Fertilidade 23.6%
3. PIB 16.1%
4. Felicidade apenas 7.5%

> Aprendizado: Modelo mostra que suicídio é complexo e não é bem explicado só por PIB e felicidade. Precisa de variáveis de saúde mental, cultura, álcool, etc.

### Bloco 14 - Classificação (Alto vs Baixo) - MELHOR RESULTADO

Transformação binária usando mediana 14.35 para balancear: 66 baixo, 66 alto.

```
Random Forest Classifier
Acurácia: 76%
AUC: 83%
Precision Baixo: 70% | Recall Baixo: 94%
Precision Alto: 90% | Recall Alto: 56%
Logistic Regression: 73% acurácia
```

**Matriz de Confusão (33 países teste):** 16 acertos baixo, 9 acertos alto, 7 FN, 1 FP

**Feature Importance Classificação:**
1. Gender Gap 17.7%
2. PIB 16.4%
3. Happiness Score 14.5%
4. Fertilidade 13.9%

**Brasil:** Taxa 18.6 (classe Alto) - Modelo previu 56.9% de chance de ser Alto - acerto no limite.

### Dashboard Final
```
REGRESSÃO: RMSE 3.35 | R² 8% (holdout) - difícil prever
CLASSIFICAÇÃO: 76% Acurácia | 83% AUC - bom discriminador
```

## 🛠️ Tecnologias

- Python, Pandas, NumPy
- Matplotlib, Seaborn, Plotly
- Scikit-learn: StandardScaler, KMeans, PCA, RandomForestRegressor/Classifier, LogisticRegression
- Train/Test Split, Cross-Validation, Silhouette Score

## 📁 Como Rodar

```bash
git clone https://github.com/seu-usuario/analise-suicidio-global
cd analise-suicidio-global
pip install pandas numpy matplotlib seaborn scikit-learn plotly
jupyter notebook portfolio_suicidio.ipynb
```

1. Coloque `Life_expectancy.csv` e `Suicide_Rate.csv` na mesma pasta do `.ipynb`
2. Run All - todos os gráficos são gerados com `plt.show()`
3. O dashboard de métricas é salvo como `metricas_dashboard.png`

## 📂 Estrutura

```
.
├── Life_expectancy.csv
├── Suicide_Rate.csv
├── dataset_completo.csv (gerado)
├── dataset_com_cluster.csv (gerado)
├── portfolio_suicidio.ipynb
├── metricas_dashboard.png
├── README.md
```

## 💡 Conclusão para Portfólio

Este projeto demonstra:
- **Limpeza de dados** - PIB como string com vírgula, merge de 2 fontes
- **EDA profundo** - paradoxo da felicidade (r=0.01)
- **Clustering não-supervisionado** - 4 arquétipos interpretáveis
- **Regressão honesta** - R² baixo como insight, não como falha
- **Classificação eficiente** - 76% acurácia e 83% AUC
- **Storytelling** - Brasil como exemplo concreto

> Suicídio não é explicado por riqueza ou felicidade média. Gender Gap e fertilidade são preditores mais fortes, indicando fatores sociais e culturais profundos.

---

Feito com dados reais | 132 países | 2026
