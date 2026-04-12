# 📊 Customer Personality Analysis
### Segmentação Comportamental, LTV e Propensão à Conversão em Campanhas de Marketing

> **MVP — Análise Exploratória e Pré-processamento de Dados · PUC-Rio**
> Entrega: 12 de Abril de 2026

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/luskao92/customer-personality-analysis/blob/main/notebook_customer_personality.ipynb)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=flat&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-visualização-4c72b0?style=flat)
![SciPy](https://img.shields.io/badge/SciPy-estatística-8CAAE6?style=flat)
![Status](https://img.shields.io/badge/Status-Concluído-4CAF50?style=flat)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## 🎯 Problema de Negócio

Times de **Marketing de Performance e CRM** enfrentam um desafio recorrente: a base de clientes é heterogênea, mas os recursos de mídia e campanhas são limitados. Investir igualmente em todos os segmentos é ineficiente — o retorno concentra-se em perfis específicos.

Este projeto analisa dados de **2.240 clientes** de uma empresa de varejo multicanal para responder:

> *Quais características demográficas e comportamentais definem os segmentos com maior LTV e maior propensão à conversão em campanhas de marketing — e como o perfil familiar modera essas relações?*

**Tipo de problema:**
- **Supervisionado (implícito):** `Response` (aceitou última campanha: 0/1) como variável-alvo
- **Não supervisionado:** segmentação RFM e por perfil familiar (`HasChildren`)

---

## 💡 Principais Achados

| # | Achado | Impacto de Negócio |
|---|--------|--------------------|
| 1 | Top 20% dos clientes concentram ~70% do faturamento | Supera Pareto 80/20 — retenção é prioridade absoluta |
| 2 | Não-pais gastam 2.6× mais em vinhos e 2.4× mais em carnes | Portfólio premium deve ser direcionado ao segmento sem filhos |
| 3 | Canal catálogo supera digital em correlação com conversão | Revisão de alocação de budget de mídia recomendada |
| 4 | Visitas ao site sem compra correlacionam negativamente com conversão | KPI de engajamento deve ser compras, não visitas |
| 5 | Pais mais sensíveis a descontos; não-pais convertem mais em campanhas formais | CRM deve usar estratégias distintas por segmento |

---

## 🔬 Hipóteses Testadas

| # | Hipótese | Teste Aplicado | Veredicto |
|---|----------|---------------|-----------|
| H1 | Renda e escolaridade determinam o volume de gastos | Pearson + ANOVA | ✅ Confirmada — r = 0.79, p < 0.05 |
| H2 | Não-pais concentram gasto em produtos premium | Teste T independente (6 categorias) | ✅ Confirmada — todos p < 0.05 |
| H3 | Menor recência = maior taxa de conversão | Comparação de médias + correlação | ✅ Confirmada com refinamento |
| H4 | Engajamento digital correlaciona com conversão | Correlação por canal + WebEngagement | ✅ Confirmada com achado contraintuitivo |
| H5 | Super compradores concentram o faturamento (Pareto) | Curva de Lorenz + Coeficiente de Gini | ✅ Confirmada |

---

## 📦 Dataset

**Customer Personality Analysis** · Kaggle · [imakash3011](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis)

| Dimensão | Valor |
|----------|-------|
| Instâncias | 2.240 clientes |
| Atributos originais | 29 variáveis |
| Separador | `\t` (TSV) |
| Variável-alvo | `Response` — desbalanceada (85% / 15%) |

```python
URL = (
    "https://raw.githubusercontent.com/luskao92/customer-personality-analysis/refs/heads/main/data/marketing_campaign.csv"
)
df = pd.read_csv(URL, sep="\t")
```

---

## ⚙️ Features Derivadas

| Feature | Lógica | Papel |
|---------|--------|-------|
| `Age` | `2026 - Year_Birth` | Segmentação demográfica |
| `Seniority_Days` | `2026-01-01 - Dt_Customer` | Maturidade do cliente |
| `HasChildren` | `Kidhome + Teenhome > 0` | **Eixo central da análise** |
| `TotalSpend` | Soma de todos `MntXxx` | Proxy de LTV |
| `TotalPurchases` | Soma de todos `NumXxxPurchases` | Volume de engajamento |
| `TotalCampaignsAccepted` | Soma de `AcceptedCmp1–5` | Histórico de conversão |
| `WebEngagement` | `NumWebPurchases / NumWebVisitsMonth` | Qualidade do engajamento digital |
| `RFM_Score` | Quartis R + F + M | Segmentação de valor |
| `RFM_Segmento` | Score → Campeões / Leais / Potencial / Em risco / Inativos | Label analítico |

---

## 🗂️ Estrutura do Notebook (9 Seções)

```
Seção 1  — Descrição do Problema
           1.1 Contexto · 1.2 Tipo de aprendizado · 1.3 Hipóteses
           1.4 Restrições · 1.5 Dicionário de Atributos (29 variáveis)

Seção 2  — Importação das Bibliotecas e Carga de Dados

Seção 3  — Análise Descritiva
           3.1 Dimensões e tipos · 3.2 Primeiras linhas
           3.3 Valores faltantes e inconsistências · 3.4 Resumo estatístico

Seção 4  — Feature Engineering
           4.1 Variáveis derivadas (9 features) · 4.2 Segmentação RFM

Seção 5  — Visualizações Exploratórias  ← 12 gráficos com análise textual
           5.1 Distribuição numérica · 5.2 Distribuição categórica
           5.3 Variável-alvo · 5.4 Correlações (heatmap)
           5.5 Análise por perfil familiar · 5.6 Análise por canal

Seção 6  — Pré-processamento            ← 8 passos + 4 visões salvas
           6.1 Limpeza (8 passos justificados)
           6.2–6.4 Normalização (MinMax) e Padronização (Standard)
           6.5 Discretização · 6.6 One-Hot Encoding · 6.7 Dataset Encoded

Seção 7  — Análise Pós-Pré-processamento (verificação de integridade)

Seção 8  — Respondendo às Hipóteses    ← H1–H5 com testes estatísticos formais

Seção 9  — Conclusão
           9.1 Síntese · 9.2 Achados de negócio
           9.3 Recomendações · 9.4 Limitações
```

---

## 🧹 Tratamento de Qualidade dos Dados

| Problema identificado | Campo | Decisão |
|----------------------|-------|---------|
| Valores nulos (~1%) | `Income` | Imputação pela mediana por grupo de `Education` |
| Outlier extremo ($666k) | `Income` | Remoção (> 13σ acima da média) |
| Corte em $150k | `Income` | Preserva cauda legítima, remove único ponto anômalo |
| Nascimentos implausíveis | `Year_Birth` < 1920 | Remoção |
| Categorias inválidas | `Marital_Status` = "Absurd", "YOLO" | Remoção |
| Colunas constantes | `Z_CostContact`, `Z_Revenue` | Drop na carga |

---

## 📁 Estrutura do Repositório

```
customer-personality-analysis/
│
├── README.md
├── LICENSE
├── notebook_customer_personality_-_Final_Version.ipynb
│
├── marketing_campaign.csv     ← dataset principal - Customer Personality Analysis · TSV · 2.240 linhas
├── marketing_campaign_clean.csv          ← visão 1 · pós-limpeza · escala original
├── marketing_campaign_normalized.csv     ← visão 2 · MinMaxScaler [0, 1]
├── marketing_campaign_standardized.csv   ← visão 3 · StandardScaler (μ=0, σ=1)
└── marketing_campaign_encoded.csv        ← visão 4 · OHE + discretização
```

---

## 🛠️ Stack

`Python 3.10+` · `Pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `Scikit-learn` · `SciPy` · `Google Colab`

---

## 📋 Checklist MVP — PUC-Rio

- [x] Definição do problema com contexto de negócio
- [x] Tipo de aprendizado declarado e justificado
- [x] 5 hipóteses formuladas e testadas formalmente com testes estatísticos
- [x] Dicionário de atributos completo (29 variáveis em 4 blocos)
- [x] Análise descritiva completa (dimensões, tipos, nulos, estatísticas)
- [x] 12 visualizações exploratórias com parágrafo de análise após cada gráfico
- [x] Pré-processamento com 8 passos, cada um justificado textualmente
- [x] Normalização, padronização, discretização e OHE aplicados
- [x] 4 visões do dataset salvas
- [x] Dataset carregado via URL raw do GitHub
- [x] Repositório público

---

## 👤 Autor

**Lucas Alves Medeiros** · [@luskao92](https://github.com/luskao92)
PUC-Rio

---

## 📄 Licença

[MIT](LICENSE)
