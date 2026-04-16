# Determinantes da Insatisfação no E-commerce: Um Estudo de Caso de Modelagem Preditiva

Estudo de caso aplicado utilizando dados públicos da **Olist** (Brazilian E-Commerce Public Dataset). O objetivo é identificar e quantificar os fatores operacionais e logísticos que determinam as avaliações negativas dos clientes (Scores 1 e 2) por meio de regressão para dados de contagem.

## Descobertas Centrais

- **Logística e Reputação**: cada dia de atraso na entrega aumenta a taxa de insatisfação em **4,45%**.
- **Gestão de Frete**: cada R$ 1,00 adicional no custo do frete eleva a taxa de reviews negativos em **0,54%**.
- **Sensibilidade por Categoria**: Telefonia (+35,7%), Cama/Mesa/Banho (+31,1%) e Móveis/Decoração (+27,0%) apresentam taxas significativamente superiores ao grupo de referência.
- **Modelo Binomial Negativo** reduz o AIC em **6,75%** em relação ao Poisson, com LR Test χ²=492,0 (p<0,001).

---

## Resultados do Modelo Final

| Variável | IRR | Efeito na Taxa | p-valor |
| :--- | :---: | :--- | :---: |
| Atraso médio na entrega (dias) | 1,0445 | +4,45% por dia | < 0,001 |
| Frete médio (R$) | 1,0054 | +0,54% por R$ 1 | < 0,001 |
| Categoria: Telefonia | 1,3568 | +35,7% | < 0,001 |
| Categoria: Cama/Mesa/Banho | 1,3108 | +31,1% | < 0,001 |
| Categoria: Móveis e Decoração | 1,2696 | +27,0% | < 0,001 |

> **IRR = Incidence Rate Ratio = exp(β).** Valores acima de 1,0 indicam aumento na taxa de avaliações negativas em relação ao grupo de referência, mantidas as demais variáveis constantes.

---

## Metodologia

### Por que Binomial Negativa?

Avaliações de clientes são dados de contagem (inteiros não negativos). A regressão de Poisson exige equidispersão (Variância = Média), condição violada neste dataset: o teste de Cameron-Trivedi confirmou sobredispersão com t=6,295 (p<0,001) e o parâmetro α=0,1147 (Pearson χ²/df: 1,789 no Poisson vs 1,061 no BN).

### Offset de Exposição

O modelo inclui `log(n_reviews)` como offset, normalizando a análise pela exposição de cada vendedor. Isso permite comparar a **taxa de insatisfação** entre vendedores com volumes de transação distintos, e não apenas o volume bruto de reviews negativos.

### Seleção de Variáveis

Partindo de um modelo completo com 29 variáveis (estados, categorias, métricas operacionais), o modelo reduzido foi selecionado por critério AIC com VIF < 5 para todos os preditores retidos.

| Métrica | Poisson | BN Reduzido |
| :--- | :---: | :---: |
| AIC | 7.488,7 | **6.983,0** |
| BIC | 7.647,3 | **7.015,8** |
| Pearson χ²/df | 1,789 | **1,061** |
| Parâmetros | 29 | **6** |

---

## Estrutura do Repositório

```
.
├── notebooks/
│   ├── 01_preparacao.ipynb    # ETL e engenharia de atributos
│   ├── 02_eda.ipynb           # Análise exploratória e visualizações
│   ├── 03_modelagem.ipynb     # Ajuste e seleção do modelo NB2
│   └── 04_interpretacao.ipynb # Diagnóstico, IRR e insights finais
├── data/                      # Dataset consolidado por vendedor
├── requirements.txt           # Dependências do projeto
└── .gitignore
```

## Como Executar

1. Clone o repositório.
2. Instale as dependências: `pip install -r requirements.txt`
3. Execute os notebooks na ordem numérica (`01` → `04`).
4. O dataset consolidado já está incluso em `data/` para execução imediata.

## Fonte dos Dados

[Olist Brazilian E-Commerce Public Dataset — Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

---

**Autor:** Maycon Aranha | [LinkedIn](https://www.linkedin.com/in/maycon-aranha/) | MBA em Data Science e Analytics — USP/ESALQ
