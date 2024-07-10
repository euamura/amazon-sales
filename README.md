# amazon-sales

## Introdução

## Case

## Objetivos

## Ferramentas

## Linguagens

## Insumos

## Escopo do projeto

- **1. Processar e preparar a base de dados**
    - 1.1. Conectar/importar dados para ferramentas
    - 1.2. Identificar e tratar valores nulos
    - 1.3. Identificar e tratar valores duplicados
    - 1.4. Identificar e gerenciar dados fora do escopo de análise
    - 1.5. Identificar e tratar dados discrepantes em variáveis categóricas
    - 1.6. Identificar e tratar dados discrepantes em variáveis numéricas
    - 1.7. Verificar e alterar o tipo de dados
    - 1.8. Criar novas variáveis
    - 1.9. Unir tabelas
    - 1.10. Construir tabelas auxiliares

## **1. Processar e preparar a base de dados**

### 1.1. Conectar/importar dados para ferramentas

1.1.1- Baixar insumos, descompactar, importar arquivos no BigQuery e criar 2 tabelas. Nome do projeto no BigQuery: amazon-sales-lab-proj4; Nome da pasta de conjuto de dados: amazon_sales; Nome das tabelas:amazon_product, amazon_review.

### **1.2. Identificar e tratar valores nulos**

1.2.1- Identificar e tratar valores nulos usando SQL:

```sql
--- tabela: amazon_product
-- visualizando tabela
SELECT *
FROM `amazon_sales.amazon_product`;
-- criando CTE de contagem de nulos
WITH product_nulls AS(
  SELECT
    SUM(CASE WHEN product_id IS NULL THEN 1 ELSE 0 END) AS product_id_nulls,
    SUM(CASE WHEN product_name IS NULL THEN 1 ELSE 0 END) AS product_name_nulls,
    SUM(CASE WHEN category IS NULL THEN 1 ELSE 0 END) AS category_nulls,
    SUM(CASE WHEN discounted_price IS NULL THEN 1 ELSE 0 END) AS discounted_price_nulls,
    SUM(CASE WHEN actual_price IS NULL THEN 1 ELSE 0 END) AS actual_price_nulls,
    SUM(CASE WHEN discount_percentage IS NULL THEN 1 ELSE 0 END) AS discount_percentage_nulls,
    SUM(CASE WHEN about_product IS NULL THEN 1 ELSE 0 END) AS about_product_nulls
  FROM `amazon_sales.amazon_product`
)
-- visualizando tabela de contagem de nulos
SELECT * FROM product_nulls;

--- tabela: amazon_review
-- visualizando tabela
SELECT *
FROM `amazon_sales.amazon_review`;
-- criando CTE de contagem de nulos
WITH review_nulls AS(
  SELECT
    SUM(CASE WHEN user_id IS NULL THEN 1 ELSE 0 END) AS user_id_nulls,
    SUM(CASE WHEN user_name IS NULL THEN 1 ELSE 0 END) AS user_name_nulls,
    SUM(CASE WHEN review_id IS NULL THEN 1 ELSE 0 END) AS review_id_nulls,
    SUM(CASE WHEN review_title IS NULL THEN 1 ELSE 0 END) AS review_title_nulls,
    SUM(CASE WHEN review_content IS NULL THEN 1 ELSE 0 END) AS review_content_nulls,
    SUM(CASE WHEN img_link IS NULL THEN 1 ELSE 0 END) AS img_link_nulls,
    SUM(CASE WHEN product_link IS NULL THEN 1 ELSE 0 END) AS product_link_nulls,
    SUM(CASE WHEN product_id IS NULL THEN 1 ELSE 0 END) AS product_id_nulls,
    SUM(CASE WHEN rating IS NULL THEN 1 ELSE 0 END) AS rating_nulls,
    SUM(CASE WHEN rating_count IS NULL THEN 1 ELSE 0 END) AS rating_count_nulls
  FROM `amazon_sales.amazon_review`
)
-- visualizando tabela de contagem de nulos
SELECT * FROM review_nulls;
````

| product_nulls |  |  |
| --- | --- | --- |
|  | product_id_nulls | 0 |
|  | product_name_nulls | 0 |
|  | category_nulls | 0 |
|  | discounted_price_nulls | 0 |
|  | actual_price_nulls | 0 |
|  | discount_percentage_nulls | 0 |
|  | about_product_nulls | 4 |

| review_nulls |  |  |
| --- | --- | --- |
|  | user_id_nulls | 0 |
|  | user_name_nulls | 0 |
|  | review_id_nulls | 0 |
|  | review_title_nulls | 0 |
|  | review_content_nulls | 0 |
|  | img_link_nulls | 466 |
|  | product_link_nulls | 466 |
|  | product_id_nulls | 0 |
|  | rating_nulls | 0 |
|  | rating_count_nulls | 2 |
