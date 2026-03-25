# 📊 Customer Personality Analysis
### Segmentação Comportamental, LTV e Propensão à Conversão em Campanhas de Marketing

> **MVP — Análise Exploratória e Pré-processamento de Dados**
> PUC-Rio · Entrega: 12 de Abril de 2026

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ZOiC7Z-4pTtv903pb_ldQl5Dep0Vo4Xh?usp=sharing)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=flat&logo=python&logoColor=white)
![Status](https://img.shields.io/badge/Status-Em%20desenvolvimento-F9AB00?style=flat)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

---

## 🎯 Problema de Negócio

Uma empresa de varejo multicanal deseja otimizar sua estratégia de **Marketing de Performance e CRM** identificando os segmentos de clientes com maior **LTV (Lifetime Value)** e maior **propensão à conversão em campanhas**, com foco especial no impacto do **perfil familiar** (presença ou ausência de filhos) no comportamento de compra e na preferência por canal de mídia.

**Pergunta central:**
> *Quais características demográficas e comportamentais definem os segmentos de clientes com maior LTV e maior propensão à conversão em campanhas de marketing — e como o perfil familiar modera essas relações?*

**Tipo de problema:** Supervisionado (classificação binária — `Response`) + Não supervisionado (segmentação RFM e perfil familiar)

---

## 🔬 Hipóteses Testadas

| # | Hipótese | Variáveis |
|---|----------|-----------|
| H1 | Renda e escolaridade determinam o volume de gastos | `Income`, `Education`, `TotalSpend` |
| H2 | Não-pais concentram gasto em produtos premium | `MntWines`, `MntMeatProducts`, `MntGoldProds`, `HasChildren` |
| H3 | Menor recência = maior taxa de conversão em campanhas | `Recency`, `Response`, `TotalCampaignsAccepted` |
| H4 | Engajamento digital correlaciona com resposta a campanhas | `NumWebPurchases`, `NumWebVisitsMonth`, `Response` |
| H5 | Segmento "super compradores" concentra o faturamento (Pareto) | `TotalSpend`, Segmento RFM |

---

## 📦 Datasets

### Principal — Customer Personality Analysis
| Item | Detalhe |
|------|---------|
| Fonte | [Kaggle — imakash3011](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis) |
| Instâncias | 2.240 clientes |
| Atributos | 29 variáveis |
| Separador | `\t` (TSV) |
| Variável-alvo | `Response` — aceitou a última campanha (0/1) |

```python
URL_PRINCIPAL = (
    "https://raw.githubusercontent.com/luskao92/customer-personality-analysis"
    "/refs/heads/main/Customer%20Personality%20Analysis.csv"
)
df = pd.read_csv(URL_PRINCIPAL, sep="\t")
```

### Complementar — E-Commerce Customer Behavior
| Item | Detalhe |
|------|---------|
| Fonte | [Kaggle — uom190346a](https://www.kaggle.com/datasets/uom190346a/e-commerce-customer-behavior-dataset) |
| Uso | Validação cruzada de padrões comportamentais digitais (Seção 8) |

```python
URL_COMPLEMENTAR = (
    "https://raw.githubusercontent.com/luskao92/customer-personality-analysis"
    "/refs/heads/main/data/ecommerce_behavior.csv"
)
df_ecom = pd.read_csv(URL_COMPLEMENTAR)
```

---

## ⚙️ Features Derivadas

| Feature | Lógica | Seção |
|---------|--------|-------|
| `Age` | `2024 - Year_Birth` | Sprint 1 |
| `Seniority_Days` | `data_ref - Dt_Customer` | Sprint 1 |
| `TotalSpend` | Soma de todos os `MntXxx` | Sprint 1 |
| `TotalPurchases` | Soma de todos os `NumXxxPurchases` | Sprint 1 |
| `TotalCampaignsAccepted` | Soma de `AcceptedCmp1` a `AcceptedCmp5` | Sprint 1 |
| `HasChildren` | `Kidhome + Teenhome > 0` | Sprint 1 |
| `ChildrenTotal` | `Kidhome + Teenhome` | Sprint 1 |
| `WebEngagement` | `NumWebPurchases / NumWebVisitsMonth` | Sprint 2 |
| `RFM_Score` | Segmentação por Recência, Frequência e Valor | Sprint 2 |

---

## 🗂️ Estrutura do Notebook (10 Seções)

```
1.  Descrição do Problema
    1.1 Tipo de Problema
    1.2 Hipóteses
    1.3 Seleção e Restrições dos Dados
    1.4 Dicionário de Atributos

2.  Importação das Bibliotecas e Carga de Dados

3.  Análise Descritiva
    3.1 Dimensões e Tipos de Dados
    3.2 Primeiras Linhas e Inspeção Inicial
    3.3 Valores Faltantes e Inconsistências
    3.4 Resumo Estatístico

4.  Visualizações Exploratórias
    4.1 Distribuição dos Atributos Numéricos
    4.2 Distribuição dos Atributos Categóricos
    4.3 Distribuição da Variável-Alvo (Response)
    4.4 Análise Bivariada e Correlações
    4.5 Análise por Perfil Familiar (HasChildren)
    4.6 Análise por Canal de Compra

5.  Feature Engineering
    5.1 Criação de Variáveis Derivadas
    5.2 Segmentação RFM

6.  Pré-processamento
    6.1 Limpeza e Tratamento de Nulos
    6.2 Remoção de Outliers e Inconsistências
    6.3 Normalização e Padronização
    6.4 Discretização
    6.5 One-Hot Encoding
    6.6 Salvamento das Visões do Dataset

7.  Análise Pós-Pré-processamento

8.  Contextualização com Mercado
    8.1 Comportamento Omnichannel — NielsenIQ 2023
    8.2 Validação Cruzada — Dataset E-Commerce Kaggle

9.  Respondendo às Hipóteses

10. Conclusão e Próximos Passos
```

---

## ⚠️ Problemas de Qualidade Identificados

| Problema | Campo | Tratamento planejado (Sprint 3) |
|---------|-------|-------------------------------|
| Valores nulos | `Income` (~24 registros) | Imputação pela mediana por grupo de escolaridade |
| Outlier extremo | `Income` = 666.666 e > 150.000 | Remoção |
| Nascimentos inválidos | `Year_Birth` < 1920 | Remoção |
| Categorias inválidas | `Marital_Status` = `"Absurd"`, `"YOLO"` | Remoção |
| Colunas constantes | `Z_CostContact` = 3, `Z_Revenue` = 11 | Drop na carga |

---

## 📅 Progresso das Sprints

| Sprint | Período | Status | Entregável |
|--------|---------|--------|------------|
| Sprint 0 | 25/03 | ✅ Concluída | Dataset, hipóteses, GitHub, README |
| Sprint 1 | 26–29/03 | 🔄 Em andamento | Seções 1–3 do notebook |
| Sprint 2 | 30/03–05/04 | ⏳ Pendente | Seções 4–5 (EDA visual + Feature Eng.) |
| Sprint 3 | 06–10/04 | ⏳ Pendente | Seções 6–8 (Pré-proc. + Contextualização) |
| Sprint 4 | 11–12/04 | ⏳ Pendente | Seções 9–10 + revisão final + entrega |

---

## 🛠️ Tecnologias

`Python 3.10+` `Pandas` `NumPy` `Matplotlib` `Seaborn` `Scikit-learn` `Google Colab`

---

## 👤 Autor

**Lucas Alves Medeiros** · [@luskao92](https://github.com/luskao92)

---

## 📄 Licença

Este projeto está sob a licença [MIT](LICENSE).
