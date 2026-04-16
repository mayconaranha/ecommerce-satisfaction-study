# Determinantes da Insatisfação no E-commerce: Um Estudo de Caso de Modelagem Preditiva

Este repositório apresenta um estudo detalhado sobre o comportamento do consumidor, utilizando dados públicos da **Olist** (Brazilian E-Commerce Public Dataset). O objetivo é identificar e quantificar os fatores operacionais e logísticos que determinam as avaliações negativas dos clientes (Scores 1 e 2) por meio de modelagem estatística avançada.

## Descobertas Centrais

*   **Logística e Performance**: Cada dia de atraso na entrega aumenta a taxa de insatisfação em **4,45%**.
*   **Impacto do Frete**: Cada R$ 1,00 adicional no custo do frete eleva a probabilidade de um review negativo em **0,54%**.
*   **Variabilidade por Categoria**: As categorias de Telefonia e Móveis/Decoração apresentam taxas de insatisfação significativamente superiores à média do marketplace.

---

## Resultados e Significância Estatística

| Variável | Efeito na Taxa de Insatisfação | Significância (p-valor) |
| :--- | :--- | :--- |
| **Atraso Médio na Entrega** | **+4,45% por dia** | < 0.001 |
| **Custo Médio do Frete** | **+0.54% a cada R$ 1** | < 0.001 |
| **Categoria: Telefonia** | **+35,7%** | < 0.001 |
| **Categoria: Cama/Mesa/Banho** | **+31,1%** | < 0.001 |

> [!NOTE]
> Variáveis como o **Ticket Médio** do pedido e a **Localização do Vendedor** não apresentaram significância estatística no modelo final, indicando que a insatisfação é primariamente impulsionada pela eficiência da malha logística.

---

## Metodologia Científica

### Modelagem de Dados de Contagem
Avaliações de usuários constituem dados de contagem (valores inteiros não negativos). Modelos de regressão linear tradicional não capturam a natureza estocástica desses dados, e a regressão de Poisson requer a condição de equidispersão (Média = Variância). Neste estudo, foi detectada uma forte **sobredispersão** (Variância/Média ≈ 52,9), inviabilizando o uso do modelo de Poisson.

### Implementação: Regressão Binomial Negativa (NB2)
Para contornar a sobredispersão, foi implementado um modelo **Binomial Negativo (NB2)** com um **Offset de Exposição** (logaritmo do total de reviews por vendedor). Esta abordagem permite modelar a **taxa de incidência** de reviews negativos, normalizando a análise entre vendedores com volumes de transação distintos.

---

## Análise Visual dos Resultados

Abaixo apresentamos as principais descobertas extraídas diretamente dos outputs dos notebooks de análise:

### 1. Dinâmica de Variáveis (Correlação)
A matriz de correlação revela a interdependência entre os fatores operacionais. Nota-se uma correlação positiva moderada entre atraso e insatisfação, enquanto o ticket médio apresenta baixa influência direta isolada.

![Matriz de Correlação](images/eda_correlacao.png)

### 2. Impacto Regional: O Caso de Rondônia (RO)
A análise geográfica demonstra uma disparidade significativa. Vendedores baseados em Rondônia apresentam uma densidade de reviews negativos superior à média nacional, evidenciando o gargalo logístico em regiões distantes do Sudeste.

![Densidade por UF](images/eda_negativos_por_estado.png)

### 3. Modelagem Preditiva (Forest Plot - IRR)
Utilizando a **Regressão Binomial Negativa**, quantificamos o risco relativo (*Incidence Rate Ratio*). O gráfico abaixo mostra que categorias como **Telefonia** possuem ~35% mais risco de gerar avaliações negativas do que o grupo de controle, mesmo sob as mesmas condições de frete.

![Forest Plot de IRR](images/interp_coeficientes.png)

### 4. Diagnóstico do Modelo
A análise de resíduos confirma que a escolha pela distribuição Binomial Negativa (NB2) foi acertada para lidar com a **sobredispersão** dos dados (Razão Variância/Média ≈ 52,9), mantendo a integridade das inferências estatísticas.

![Diagnóstico de Resíduos](images/interp_diagnostico_residuos.png)

---

## Estrutura do Repositório

```
.
├── notebooks/                     # Fluxo completo da análise
│   ├── 01_preparacao.ipynb        # ETL e Engenharia de Atributos
│   ├── 02_eda.ipynb               # Análise Exploratória e Visualizações
│   ├── 03_modelagem.ipynb         # Seleção e Ajuste do Modelo NB2
│   └── 04_interpretacao.ipynb     # Diagnóstico e Insights Finais
├── data/                          # Datasets (Agregado e Placeholder)
├── images/                        # Outputs visuais para documentação
├── outputs/                       # Resultados técnicos (CSV/JSON)
├── .gitignore                     # Configurações de exclusão
└── requirements.txt               # Dependências do projeto
```

## Como Executar

1.  Clone o repositório.
2.  Instale as dependências: `pip install -r requirements.txt`.
3.  Execute os notebooks na ordem numérica (`01` a `04`).
4.  O dataset consolidado já está incluso em `data/` para execução imediata do modelo.

## Fonte dos Dados
[Olist Public Dataset (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

---
**Autor:** Maycon Aranha
**Contato:** [LinkedIn](https://www.linkedin.com/in/maycon-aranha/)
