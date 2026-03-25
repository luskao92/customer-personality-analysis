# 📊 Customer Personality Analysis
### Análise de Perfil, Segmentação RFM e Propensão à Conversão em Campanhas de CRM

> **MVP de Análise Exploratória e Pré-processamento de Dados**  
> PUC-Rio · Disciplina: Análise Exploratória e Pré-processamento de Dados · Entrega: 12 de Abril de 2026

---

## 🎯 Problema de Negócio

Uma empresa de varejo multicanal deseja otimizar sua estratégia de **Marketing de Performance e CRM** entendendo quais perfis de clientes geram maior valor (LTV) e têm maior propensão a converter em campanhas de marketing.

**Pergunta central:**
> *Quais características demográficas e comportamentais definem os segmentos de clientes com maior Lifetime Value (LTV) e maior propensão à conversão em campanhas de marketing?*

A resposta permite que times de Growth e CRM priorizem segmentos, personalizem comunicações e otimizem o retorno sobre investimento em campanhas — reduzindo CAC e aumentando LTV da base ativa.

---

## 📂 Datasets Utilizados

### Dataset Principal

| Item | Detalhe |
|------|---------|
| Nome | Customer Personality Analysis |
| Fonte | [Kaggle — imakash3011](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis) |
| Instâncias | 2.240 clientes |
| Atributos | 29 variáveis originais |
| Cobertura | Perfil demográfico, histórico de compras, canais e resposta a campanhas |

```python
import pandas as pd

url = "https://raw.githubusercontent.com/luskao92/customer-personality-analysis/main/data/marketing_campaign.csv"
df = pd.read_csv(url, sep="\t")
```

### Dataset Complementar (Validação Cruzada)

| Item | Detalhe |
|------|---------|
| Nome | E-Commerce Customer Behavior Dataset |
| Fonte | [Kaggle — uom190346a](https://www.kaggle.com/datasets/uom190346a/e-commerce-customer-behavior-dataset) |
| Objetivo | Validar padrões comportamentais digitais encontrados no CRM principal com dados reais de e-commerce |
| Uso no projeto | Seção 8.3 — Validação Cruzada de Perfis |

```python
url_ecomm = "https://raw.githubusercontent.com/luskao92/customer-personality-analysis/main/data/ecommerce_behavior.csv"
df_ecomm = pd.read_csv(url_ecomm)
```

### Fontes Externas de Contextualização de Mercado

| Fonte | Dados Utilizados | Seção no Notebook |
|-------|-----------------|-------------------|
| **IBGE PNAD Contínua 2023** | Distribuição de renda por escolaridade e faixa etária no Brasil | 8.1 — Posicionamento de Renda |
| **NielsenIQ Webshoppers 49 (2023)** | Share de canais de compra, ticket médio e comportamento omnichannel no Brasil | 8.2 — Comportamento Omnichannel |

> As fontes externas são incorporadas como benchmarks numéricos contextualizados nos blocos de análise, simulando a prática real de um analista sênior de CRM que contextualiza os dados internos com referências de mercado.

---

## 🔬 Hipóteses Testadas

| # | Hipótese | Variáveis Envolvidas |
|---|----------|----------------------|
| H1 | Renda e escolaridade são os principais determinantes do volume de gastos totais | `Income`, `Education`, `TotalSpend` |
| H2 | Não-pais concentram gasto em produtos premium (vinho, carne e gold) | `MntWines`, `MntMeatProducts`, `MntGoldProds`, `HasChildren` |
| H3 | Clientes com menor recência têm maior taxa de conversão em campanhas | `Recency`, `Response`, `TotalCampaignsAccepted` |
| H4 | O canal web está positivamente correlacionado com resposta a campanhas | `NumWebPurchases`, `NumWebVisitsMonth`, `Response` |
| H5 | Existe um segmento de "super compradores" que concentra desproporcionalmente o faturamento (regra de Pareto) | `TotalSpend`, segmentos RFM |

---

## ⚙️ Features Derivadas (Feature Engineering)

Além dos 29 atributos originais, as seguintes variáveis são criadas durante a análise para enriquecer o poder analítico do dataset:

| Feature | Fórmula / Lógica | Significado de Negócio |
|---------|-----------------|------------------------|
| `Age` | `ano_referência - Year_Birth` | Idade do cliente |
| `Seniority_Days` | `data_referência - Dt_Customer` | Tempo como cliente em dias |
| `TotalSpend` | Soma de todos os `MntXxx` | Proxy de LTV — gasto total nos últimos 2 anos |
| `TotalPurchases` | Soma de todos os `NumXxxPurchases` | Volume total de transações por canal |
| `TotalCampaignsAccepted` | Soma de `AcceptedCmp1` a `AcceptedCmp5` | Engajamento histórico com campanhas |
| `HasChildren` | `Kidhome + Teenhome > 0` | Flag binária de presença de filhos |
| `ChildrenTotal` | `Kidhome + Teenhome` | Total de dependentes no domicílio |
| `WebEngagement` | `NumWebPurchases / NumWebVisitsMonth` | Taxa de conversão no canal digital |
| `RFM_Score` | Segmentação por Recência, Frequência e Valor | Segmento de valor do cliente (1–5) |

---

## 🗂️ Estrutura do Notebook (Seções)

```
1. Descrição do Problema
   1.1 Tipo de Problema (supervisionado + não supervisionado)
   1.2 Hipóteses
   1.3 Seleção e Restrições dos Dados
   1.4 Dicionário de Atributos

2. Importação das Bibliotecas e Carga de Dados

3. Análise Descritiva
   3.1 Dimensões e Tipos de Dados
   3.2 Primeiras Linhas e Inspeção Inicial
   3.3 Valores Faltantes e Inconsistências
   3.4 Resumo Estatístico

4. Visualizações Exploratórias
   4.1 Distribuição dos Atributos Numéricos
   4.2 Distribuição dos Atributos Categóricos
   4.3 Distribuição da Variável Alvo (Response)
   4.4 Análise Bivariada e Correlações
   4.5 Análise por Perfil de Cliente (HasChildren)
   4.6 Análise por Canal de Compra

5. Feature Engineering
   5.1 Criação de Variáveis Derivadas
   5.2 Segmentação RFM

6. Pré-processamento
   6.1 Limpeza e Tratamento de Nulos
   6.2 Remoção de Outliers e Inconsistências
   6.3 Transformações (Normalização e Padronização)
   6.4 Discretização
   6.5 One-Hot Encoding
   6.6 Salvamento das Visões do Dataset

7. Análise Pós-Pré-processamento
   7.1 Verificação de Distribuições Após Transformações

8. Contextualização com Mercado
   8.1 Posicionamento de Renda — IBGE PNAD 2023
   8.2 Comportamento Omnichannel — NielsenIQ Webshoppers 2023
   8.3 Validação de Perfis — Dataset E-Commerce (Kaggle)

9. Respondendo às Hipóteses

10. Conclusão e Próximos Passos
```

---

## 📋 Checklist de Desenvolvimento (Sprints)

- [x] **Sprint 0** — Kickoff: definição do problema, dataset, hipóteses e fontes externas
- [ ] **Sprint 1** — Análise descritiva: tipos de dados, valores faltantes e resumo estatístico
- [ ] **Sprint 2** — Visualizações exploratórias, EDA completa e insights de negócio
- [ ] **Sprint 3** — Feature engineering, pré-processamento e contextualização com mercado
- [ ] **Sprint 4** — Revisão final, polimento, GitHub e entrega

---

## 🗃️ Estrutura do Repositório

```
customer-personality-analysis/
│
├── README.md                                      ← este arquivo
├── notebook.ipynb                                 ← ou link para Google Colab
│
├── data/
│   ├── marketing_campaign.csv                     ← dataset principal (original)
│   ├── ecommerce_behavior.csv                     ← dataset complementar (Kaggle)
│   ├── marketing_campaign_clean.csv               ← após limpeza (Sprint 3)
│   ├── marketing_campaign_normalized.csv          ← visão normalizada
│   ├── marketing_campaign_standardized.csv        ← visão padronizada
│   └── marketing_campaign_encoded.csv             ← visão com one-hot encoding
│
└── images/
    ├── distribuicao_income.png
    ├── distribuicao_response.png
    ├── gasto_por_categoria.png
    ├── heatmap_correlacao.png
    ├── rfm_segmentation.png
    ├── benchmark_canais_nielsen.png
    └── posicionamento_renda_ibge.png
```

---

## 📊 Resultados e Visualizações

*Seção atualizada progressivamente ao longo das sprints.*

---

## ⚠️ Qualidade dos Dados — Alertas Identificados

Durante a inspeção inicial do dataset, foram identificados os seguintes pontos críticos tratados no pré-processamento:

| Problema | Campo | Ação Planejada |
|---------|-------|---------------|
| Valores ausentes | `Income` | Imputação pela mediana por grupo de escolaridade |
| Outliers extremos de renda | `Income` > 150.000 | Investigação e remoção ou cap |
| Datas de nascimento inválidas | `Year_Birth` < 1920 | Remoção por erro de entrada |
| Categorias inválidas | `Marital_Status` = `"Absurd"`, `"YOLO"` | Reclassificação ou remoção |
| Colunas constantes | `Z_CostContact`, `Z_Revenue` | Remoção (sem variância informativa) |
| Renda anômala | `Income` = 666.666 | Remoção por erro de entrada |

---

## 🛠️ Tecnologias

`Python 3.10+` `Pandas` `NumPy` `Matplotlib` `Seaborn` `Scikit-learn` `Google Colab`

---

## 👤 Autor

**[Seu Nome]**  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/[seu-usuario])

---

## 📄 Licença

Este projeto está sob a licença MIT.
