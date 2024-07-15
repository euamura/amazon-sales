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

<details>
  <summary>1.2.1- Identificar e tratar valores nulos usando SQL:</summary>

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
  ```
</details>

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

1.2.2-  A variável rating_count é usada como ponto importante da análise, portanto não é possível manter suas linhas nulas.

<details>
  <summary>Tratar e atualizar tabela sem as linhas nulas usando SQL</summary>

  ```sql
  --atualizar nulos
SELECT
  *
FROM `amazon_sales.amazon_review_clean`
WHERE rating_count IS NULL;


--tabela teste de atualização de nulo
WITH rating_nulls AS(
  SELECT
    *
  FROM `amazon_sales.amazon_review_clean`
  WHERE rating_count IS NOT NULL
)
SELECT * FROM rating_nulls

--atualizando tabela
CREATE OR REPLACE TABLE `amazon_sales.amazon_review_clean` AS(
    SELECT
    *
  FROM `amazon_sales.amazon_review_clean`
  WHERE rating_count IS NOT NULL
);
  ```
</details>

### 1.3. Identificar e tratar valores duplicados

<details>
  <summary>1.3.1- Identificar e tratar dados duplicados usando comandos SQL:</summary>

  ```sql
  ---- Identificar duplicados

---amazon_review
--visualizar tabela
SELECT * FROM `amazon_sales.amazon_review`
----criar tabela temporaria com contagem para identificar duplicados
WITH review_duplicates AS(
  SELECT
    r.*,
    ROW_NUMBER() OVER (PARTITION BY review_id ORDER BY review_id) AS row_num
  FROM `amazon_sales.amazon_review` AS r
)
SELECT
  *
FROM
  review_duplicates
WHERE
  row_num = 1;


---amazon_product
--visualizar tabela
SELECT * FROM `amazon_sales.amazon_product`
----criar tabela temporaria com contagem para identificar duplicados
WITH product_duplicates AS(
  SELECT
    r.*,
    ROW_NUMBER() OVER (PARTITION BY product_id ORDER BY product_id) AS row_num
  FROM `amazon_sales.amazon_product` AS r
)
SELECT
  *
FROM
  product_duplicates
WHERE
  row_num = 1;
  ```
</details>

### 1.4. Identificar e gerenciar dados fora do escopo de análise

1.4.1- A análise é focada na categoria do produto e suas classificações, então ao selecionar os dados o foco está nas que podemos trazer segmentação de produtos. As colunas em verde foram as selecionadas como importantes pra análise, as amarelas, são colocadas como possíveis extras para complementação da análise, e, as vermelhas, são as que não trazem objetivos claros para o escopo desta análise específica:

![tabela-1](https://github.com/user-attachments/assets/8b17b9bd-7cfe-4176-988c-c8f5235da231)

![tabela-2](https://github.com/user-attachments/assets/ac30a25f-4725-4a4d-93a3-3e92d4f8c31a)

### 1.5. Identificar e tratar dados discrepantes em variáveis categóricas

1.5.1- Na coluna rating há um valor discrepante, onde se espera um número há um caractere “|”, o dado foi retirado da análise como uma exceção, mas é um alerta pois o rating_count dessa linha tem um valor considerável de 992.

<details>
  <summary>Identificar e tratar usando SQL:</summary>

  ```sql
  ---identificando dados discrepantes em rating
SELECT   
  MIN(rating) AS min_rating,
  MAX(rating) AS max_rating
FROM `amazon_sales.amazon_review_clean`

---selecionando linha com dado discrepante
SELECT
  *
FROM `amazon_sales.amazon_review`
WHERE rating = '|'

---consultando tabela retirando a linha com dado discrepante
SELECT
    *
FROM `amazon_sales.amazon_review_clean`
WHERE rating != '|'

---substituir tabela sem a linha do dado discrepante
CREATE OR REPLACE TABLE `amazon_sales.amazon_review_clean` AS
 SELECT
    *
  FROM `amazon_sales.amazon_review_clean`
  WHERE rating != '|'
  ```
</details>

![tabela-3](https://github.com/user-attachments/assets/346a84b8-362a-42a7-9776-cda48be93345)

1.5.2- São encontradas nas colunas review_title e review_content, emojis e vários formatos de frase. Para tratá-los, todos devem ser transformados em letras minúsculas, sem os emojis e com espaços após as pontuações.

![tabela-4](https://github.com/user-attachments/assets/a5cbc818-00c6-48ab-a345-31b01779a475)

### 1.6. Identificar e tratar dados discrepantes em variáveis numéricas

1.6.1- Não há identificação de dados discrepantes em variáveis numéricas. Ao considerar que se trata de inúmeros varejos, a análise respeita a forma autônoma como cada varejista atribui o preço do seu produto bem como os descontos.

<details>
  <summary>Identificar dados usando SQL</summary>

  ```sql
  -Verificar valores discrepantes--
SELECT
MAX (discounted_price),
MIN (discounted_price),
AVG (discounted_price),
FROM
projeto-4-caso-consultoria.Amazon.amazon - amazon_product;

SELECT
MAX (actual_price),
MIN (actual_price),
AVG (actual_price),
FROM
projeto-4-caso-consultoria.Amazon.amazon - amazon_product;

SELECT
MAX (discount_percentage),
MIN (discount_percentage),
AVG (discount_percentage),
FROM
projeto-4-caso-consultoria.Amazon.amazon - amazon_product;

SELECT
MAX (rating_count),
MIN (rating_count),
AVG (rating_count),
FROM
projeto-4-caso-consultoria.Amazon.amazon - amazon_review;

SELECT
MAX (rating),
MIN (rating),
AVG (rating),
FROM
projeto-4-caso-consultoria.Amazon.amazon - amazon_review
  ```
</details>

