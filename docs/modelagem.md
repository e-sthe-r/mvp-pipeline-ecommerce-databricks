# Modelagem dos Dados – MVP de Pipeline de Dados de Vendas

Este documento descreve a modelagem analítica definida para o MVP, construída a partir do banco de dados transacional **AdventureWorks2017**. O objetivo da modelagem é transformar dados operacionais, originalmente normalizados, em um modelo dimensional adequado para análises de negócio.


## 1. Contexto da Modelagem

O banco AdventureWorks2017 é um sistema transacional (OLTP), projetado para suportar operações do dia a dia, como registro de pedidos, clientes e produtos. Esse tipo de modelagem não é adequado para análises de caráter analítico e históricas, pois envolve múltiplas tabelas normalizadas e consultas complexas. Dessa forma, foi definido um **modelo dimensional em Esquema Estrela**, com foco no processo de vendas, a fim de facilitar análises agregadas e temporais.

---

## 2. Granularidade do Modelo

A granularidade definida para a tabela fato é:

> **Uma linha por item de pedido vendido**

Cada registro da tabela fato representa um produto vendido em um pedido específico, em uma determinada data, para um cliente e território definidos.

---

## 3. Modelo Dimensional Proposto

### 3.1 Tabela Fato

#### `fact_sales`

Tabela central do modelo, responsável por armazenar as métricas de vendas.

Principais atributos:
- SalesOrderID
- SalesOrderDetailID
- OrderDateKey
- ProductKey
- CustomerKey
- TerritoryKey
- OrderQty
- UnitPrice
- LineTotal
- OrderStatus  
- IsCancelled

O campo **IsCancelled** é derivado do status do pedido, permitindo identificar pedidos cancelados diretamente na tabela fato, conforme a seguinte regra lógica:
- `IsCancelled = 1` quando `OrderStatus = 6 (Cancelled)`
- `IsCancelled = 0` caso contrário

Origem dos dados:
- `Sales.SalesOrderHeader`
- `Sales.SalesOrderDetail`

### 3.2 Tabelas Dimensão

#### `dim_customer`

Dimensão responsável por armazenar informações descritivas dos clientes.

Principais atributos:
- CustomerKey (chave substituta)
- CustomerID (chave de negócio)
- Nome completo
- Tipo de cliente
- Dados de contato

Origem:
- `Sales.Customer`
- `Person.Person`
- `Person.EmailAddress`


#### `dim_product`

Dimensão que descreve os produtos comercializados.

Principais atributos:
- ProductKey  
- ProductID  
- Nome do produto  
- ProductNumber  
- Color  
- StandardCost  
- ListPrice  
- SubcategoryName  
- CategoryName 

Origem:
- `Production.Product`
- `Production.ProductSubcategory`
- `Production.ProductCategory`


#### `dim_date`

Dimensão de tempo, utilizada para análises temporais e agregações por período.

Principais atributos:
- DateKey
- Data completa
- Ano
- Mês
- Nome do mês
- Trimestre

Origem:
- Datas derivadas do campo `OrderDate` da tabela de pedidos.


#### `dim_territory`

Dimensão geográfica e territorial.

Principais atributos:
- TerritoryKey
- Nome do território
- País
- Região

Origem:
- `Sales.SalesTerritory`
- `Person.CountryRegion`

---

## 4. Relacionamentos

O modelo apresenta relacionamentos do tipo **N:1**, partindo da tabela fato para as dimensões:

- `fact_sales.ProductKey` → `dim_product.ProductKey`
- `fact_sales.CustomerKey` → `dim_customer.CustomerKey`
- `fact_sales.DateKey` → `dim_date.DateKey`
- `fact_sales.TerritoryKey` → `dim_territory.TerritoryKey`

---

## 5. Justificativa da Modelagem

- Melhor desempenho em consultas analíticas
- Facilidade de entendimento do modelo
- Adequação às perguntas de negócio definidas no objetivo do MVP
- Separação clara entre métricas (tabela fato) e atributos descritivos (dimensões)

---

## 6. Observações

Este modelo analítico serve como base para a construção das tabelas da camada **Gold** do pipeline.

Detalhes adicionais sobre colunas, domínios e qualidade dos dados estão apresentados no **Catálogo de Dados**.
