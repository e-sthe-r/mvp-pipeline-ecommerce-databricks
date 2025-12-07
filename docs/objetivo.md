# Objetivo do MVP – Pipeline de Dados de E-commerce

# 1. Objetivo Geral

Desenvolver um pipeline completo, estruturado nas camadas **Bronze → Silver → Gold**, com o objetivo de:

- Organizar e transformar dados brutos de e-commerce
- Criar um modelo dimensional (Esquema Estrela)  
- Avaliar a qualidade dos dados  
- Realizar análises exploratórias e de negócio  
- Identificar padrões de comportamento de clientes  
- Descobrir produtos com sazonalidade de vendas  

# 2. Perguntas de Negócio

O MVP busca responder às seguintes questões:

## 2.1 Vendas e Países
1. Quais países geram maior receita?  
2. Qual país possui o maior ticket médio?  
3. Qual é o impacto das transações canceladas no faturamento?

## 2.2 Produtos
4. Quais produtos são os mais vendidos e os mais lucrativos?  
5. Existem produtos com comportamento sazonal ao longo do ano?  
6. Qual é a curva de vendas mensal dos principais produtos?

## 2.3 Clientes
7. Quem são os clientes mais valiosos segundo as métricas de **RFM (Recency, Frequency, Monetary)**?
8. Como essas métricas ajudam a entender o comportamento dos clientes ao longo do tempo?

# 3. Escopo Técnico do MVP

O projeto engloba:

### 3.1 Busca e Coleta dos Dados
Utilização do dataset público *Ecommerce Data (Kaggle)*.

### 3.2 Pipeline em Databricks (SQL + Python)
Implementação das camadas:
- **Bronze:** ingestão dos dados brutos  
- **Silver:** limpeza, padronização, enriquecimento  
- **Gold:** modelagem dimensional (DW) e tabelas analíticas  

### 3.3 Modelagem
Criação das tabelas:
- `fact_sales`
- `dim_customer`
- `dim_product`
- `dim_date`
- `dim_country`

### 3.4 Análises Avançadas
- RFM para clientes recorrentes  
- Sazonalidade por produto  
- Métricas por país e por produto  

### 3.5 Documentação
Inclui:
- Catálogo de dados  
- Modelagem  
- Análises  
- Autoavaliação  


# 4. Dataset Utilizado

**Ecommerce Data — Kaggle**  
URL: https://www.kaggle.com/datasets/carrie1/ecommerce-data  
Licença: CC0 (público)


