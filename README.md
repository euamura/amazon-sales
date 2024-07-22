# Otimizando a Análise de Produtos para Estratégias de Dropshipping Eficazes na Amazon

## Link de nteresse

https://app.powerbi.com/view?r=eyJrIjoiNzUwYzgxNjktYzQ4Ni00ZDRlLTkwODctMDkyNjRhNzZkOTJkIiwidCI6IjRhNWRiNDI3LTllNTgtNDQ5MC04ZDY4LWYxOWJlYjRiNzlmMCJ9

## Equipe
- Amanda Mendonça
- Keila Del Re

## Introdução

Diante do crescimento exponencial e da alta demanda por comércio eletrônico, a Amazon, junto com seus fornecedores de dropshipping, enfrentam o desafio de aprimorar suas estratégias para maximizar o sucesso em um mercado cada vez mais competitivo. Para alcançar esse objetivo, o time de produto precisa da implementação de um modelo robusto de análise de produtos que otimize a tomada de decisões e impulsione a rentabilidade.

Para esta análise, pontua-se os pilares e dados importantes como:

1. Rentabilidade do Produto: Avaliando o preço do produto e descontos disponíveis na plataforma.
2. Popularidade do Produto: Medindo o interesse do público através do número de avaliações e classificação média.
3. Satisfação do Cliente: Monitorando o nível de contentamento dos consumidores através da pontuação média das avaliações.

## Case

É indiscutível que a tomada de decisão informada é fundamental para o sucesso nos negócios. Entendendo essa necessidade nasceu a Datalab, uma empresa de consultoria inovadora especializada em análise de dados, fundada com a visão de fornecer soluções analíticas de ponta.

A Datalab tornou-se uma parceira confiável para diversas empresas em diferentes setores e complexidade. Seu modelo de consultoria exclusiva permite que seus analistas de dados escolham projetos que estejam alinhados com seus interesses e conhecimentos, buscando maximizar a aplicação de suas habilidades técnicas para contextos que possam despertar seu maior potencial.

A possibilidade de escolher os projetos e/ou clientes com quem deseja colaborar não apenas capacita os analistas de dados, mas também contribui para um ambiente de trabalho dinâmico e em constante evolução.

A Datalab incentiva a criatividade e a inovação, fornecendo um espaço onde os profissionais podem explorar novas ideias e abordagens na análise de dados.

Além disso, esta flexibilidade na escolha dos projetos permite desenvolver relacionamentos fortes com os clientes, graças ao fato de que os analistas entendem perfeitamente suas necessidades e desafios. Esta conexão mais próxima não só beneficia os clientes ao receber soluções mais personalizadas, mas também fornece aos analistas de dados uma experiência enriquecedora e gratificante.

DATASET ESCOLHIDO: AMAZON-SALES

Num ambiente globalizado e altamente competitivo, cresce cada vez mais o número de vendas online, mas as empresas também vêm enfrentando uma quantidade maior de reclamações dos seus clientes, diante disso, os diretores de produto da Amazon estão em busca de novas estratégias comerciais para aumentar as vendas e melhorar a satisfação dos clientes. Para alcançar esse objetivo, o time de produto precisa da implementação de um modelo robusto de análise de produtos que otimize a tomada de decisões e impulsione a rentabilidade.

## Objetivos

Elaborar uma análise abrangente que forneça insights valiosos para os diretores de produto, permitindo-lhes entender melhor o desempenho dos produtos e subcategorias, e auxiliar na formulação de estratégias comerciais mais eficazes. E implementar um modelo de análise de produtos robusto e estratégico para que a Amazon e seus fornecedores de dropshipping podem tomar decisões mais assertivas, otimizar seus investimentos. 

Isso levantou algumas hipóteses sobre a base de produtos:
- Hipótese 1:  Quanto maior o desconto, melhor será a pontuação.
- Hipótese 2: Quanto maior o número de pessoas que avaliaram o produto, melhor será a classificação.
- Hipótese 3: Categorias com produtos mais caros têm maiores pontuações.
- Hipótese 4: Quanto maior o desconto, mais avaliações a categoria recebe.

## Ferramentas
- Bigquery
- Power BI
  
## Linguagens
- SQL

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
 
- **2. Fazer uma análise exploratória**
    - 2.1. Calcular correlação entre variáveis
    - 2.2. Calcular percentis e aplicar medidas de tendência central
    - 2.3. Agrupar e visualizar dados de acordo com variáveis categóricas
    - 2.4. Aplicar medidas de dispersão
      
- **3. Aplicar técnica de análise**
    - 3.1. Validar hipótese
    - 3.2. Aplicar segmentação

- **4. Resumir informações e apresentar resultados em um dashboard**
    - 4.1. Dashboard
      
- **5. Conclusões e próximos passos**
    
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

## 1.7. Verificar e alterar o tipo de dados

<details>
  <summary>1.7.1- A coluna rating está em string, e é preciso alterar para tipo de número, usando SQL:</summary>

  ```sql
 --teste de mudança de tipo de variavel
WITH test AS(
  SELECT 
    * EXCEPT(rating,row_num),
      CAST(rating AS FLOAT64) AS rating
  FROM 
    `amazon_sales.amazon_review_clean`
)

SELECT * FROM test

--atualizando tabela
CREATE OR REPLACE TABLE `amazon_sales.amazon_review_clean` AS(
    SELECT 
    * EXCEPT(rating,row_num),
      CAST(rating AS FLOAT64) AS rating
  FROM 
    `amazon_sales.amazon_review_clean`
);
  ```
</details>

## 1.8. Criar novas variáveis

<details>
  <summary>1.8.1- Criação de 2 novas variáveis a partir das variáveis da coluna category usando SQL:</summary>

  ```sql
  --tabela teste de novas variaveis
WITH category_array AS(
  SELECT
    * EXCEPT(row_num),
    SPLIT(category, '|')[OFFSET(0)] AS main_category,
    SPLIT(category, '|')[OFFSET(1)] AS sub_category
  FROM `amazon_sales.amazon_product_clean`
)
SELECT * FROM category_array

--atualizar tabela
CREATE OR REPLACE TABLE `amazon_sales.amazon_product_clean` AS(
    SELECT
    * EXCEPT(row_num),
    SPLIT(category, '|')[OFFSET(0)] AS main_category,
    SPLIT(category, '|')[OFFSET(1)] AS sub_category
  FROM `amazon_sales.amazon_product_clean`
);
  ```
</details>

<details>
  <summary>1.8.2- Limpeza de texto das novas colunas</summary>

  ```sql

-- limpeza de textos
WITH limpeza_textos AS (
  SELECT
    * EXCEPT(sub_category, main_category),
    REGEXP_REPLACE(
      REGEXP_REPLACE(
        REGEXP_REPLACE(sub_category, r'([a-z])([A-Z])', r'\1 \2'),  -- Espaço antes de letras maiúsculas
        r'\s*&\s*', r' & '  -- Espaço antes e depois de &
      ),
      r',', r', '  -- Espaço após a vírgula
    ) AS sub_category,
    REGEXP_REPLACE(
      REGEXP_REPLACE(
        REGEXP_REPLACE(main_category, r'([a-z])([A-Z])', r'\1 \2'),  -- Espaço antes de letras maiúsculas
        r'\s*&\s*', r' & '  -- Espaço antes e depois de &
      ),
      r',', r', '  -- Espaço após a vírgula
    ) AS main_category
  FROM 
    `amazon_sales.amazon_product_clean`
)

SELECT * 
FROM limpeza_textos;

---atualização de tabela
CREATE OR REPLACE TABLE `amazon_sales.amazon_product_clean` AS(
   SELECT
    * EXCEPT(sub_category, main_category),
    REGEXP_REPLACE(
      REGEXP_REPLACE(
        REGEXP_REPLACE(sub_category, r'([a-z])([A-Z])', r'\1 \2'),  -- Espaço antes de letras maiúsculas
        r'\s*&\s*', r' & '  -- Espaço antes e depois de &
      ),
      r',', r', '  -- Espaço após a vírgula
    ) AS sub_category,
    REGEXP_REPLACE(
      REGEXP_REPLACE(
        REGEXP_REPLACE(main_category, r'([a-z])([A-Z])', r'\1 \2'),  -- Espaço antes de letras maiúsculas
        r'\s*&\s*', r' & '  -- Espaço antes e depois de &
      ),
      r',', r', '  -- Espaço após a vírgula
    ) AS main_category
  FROM 
    `amazon_sales.amazon_product_clean`
);
  ```
</details>

## 1.9. Unir tabelas

<details>
  <summary>Unir tabelas com SQL:</summary>

  ```sql
 -- visualizar tabela review
SELECT *
FROM `amazon_sales.amazon_review_clean`;

-- visualizar tabela product
SELECT *
FROM `amazon_sales.amazon_product_clean`;

--- CRIAR NOVA TABELA
--CTE
WITH unida AS(
  SELECT
    p.product_id,
    p.main_category,
    p.sub_category,
    p.actual_price,
    p.discount_percentage,
    r.user_id,
    r.review_id,
    r.rating,
    r.rating_count
FROM `amazon_sales.amazon_product_clean` AS p
LEFT JOIN `amazon_sales.amazon_review_clean` AS r ON p.product_id = r.product_id
), 

--retirando duplicados da CTE de união de tabelas
tratados AS(
  SELECT
    * EXCEPT(rating,rating_count),
    COALESCE(rating,0) AS rating,
    COALESCE(rating_count,0) AS rating_count,
    ROW_NUMBER() OVER(PARTITION BY product_id) AS rownum
  FROM unida
)

SELECT * EXCEPT(rownum) FROM tratados
WHERE rownum = 1;
  ```
</details>

# 2. Fazer uma análise exploratória

### 2.1. Calcular correlação entre variáveis

<details>
  <summary>2.1.1- Calcular correlação com SQL:</summary>

  ```sql
 
---visualizar tabela
SELECT * FROM `amazon_sales.amazon_principal`;

----calcular correlação
SELECT
  CORR(actual_price,rating) AS corr_price_rating,
  CORR(actual_price,rating_count) AS corr_price_rating_count,
  CORR(discount_percentage, rating) AS corr_per_discount_rating,
  CORR(discount_percentage, rating_count) AS corr_per_discount_rating_count,
  CORR(rating,rating_count) AS corr_rating_rating_count
FROM `amazon_sales.amazon_principal`;

  ```
</details>

2.1.2- Resultados:
![tabela-5](https://github.com/user-attachments/assets/03ea0345-3734-4caf-a4d2-0de9882ddf77)

### 2.2. Calcular percentis e aplicar medidas de tendência central

<details>
  <summary>2.2.1- Dividir base de dados por quintil, com variáveis com SQL:</summary>

  ```sql
  ---dividir quintis
CREATE OR REPLACE TABLE `amazon_sales.amazon_principal` AS(
WITH quintis AS(
SELECT
  *,
  NTILE(5) OVER(ORDER BY rating) AS rating_ntile,
  NTILE(5) OVER(ORDER BY rating_count) AS rating_count_ntile,
  NTILE(5) OVER(ORDER BY actual_price) AS actual_price_ntile,
  NTILE(5) OVER(ORDER BY discount_percentage) AS discount_percentage_ntile
FROM `amazon_sales.amazon_principal`
)

SELECT * FROM quintis
);
  ```
</details>

![Quintil_rating](https://github.com/user-attachments/assets/84a4b270-1f3a-45a3-8258-18d1e294df20)


![Quintil_rating_count](https://github.com/user-attachments/assets/4f1d3491-7db3-4f14-a4a2-c6588a972366)


![Quintil_actual_price](https://github.com/user-attachments/assets/53269ae9-f95c-4adf-830a-36ab27ab25f2)


![Quintil_discount_percentage](https://github.com/user-attachments/assets/9094f435-dff5-41b9-a7aa-27b123ad9e7f)


### 2.3. Agrupar e visualizar dados de acordo com variáveis categóricas


![rating e rating_count categoria](https://github.com/user-attachments/assets/c1ec6c28-d3fc-4a88-8fb1-4c13df960a3c)


![rating e rating_count subcategoria](https://github.com/user-attachments/assets/4464e280-bed7-4732-bc9f-950f44928ef1)


![rating e rating_count categoria_produto](https://github.com/user-attachments/assets/71abd93d-68ec-4c08-856e-c8ac54c0f121)


![Nulos](https://github.com/user-attachments/assets/592613cb-a062-4f6e-8779-da5d0bc07b80)


![Nulos  2](https://github.com/user-attachments/assets/5e81d959-e239-4378-a619-8f6d82a7dcd8)




### 2.4. Aplicar medidas de dispersão


![Dispersão 1](https://github.com/user-attachments/assets/599615fc-ca71-4d3d-a3ea-bb37f48f9e70)



![Dispersão 2](https://github.com/user-attachments/assets/a9bd52fd-87ef-4b12-b6ce-ec45eac44149)




![Dispersão 3](https://github.com/user-attachments/assets/6234b4d2-a0e7-4ab6-be06-1fc3c6eacfbc)



![Dispersão 4](https://github.com/user-attachments/assets/a074c6b9-8991-46ae-bde5-aaaa6228cbe7)




   
## 3. Aplicar técnica de análise


### 3.1. Validar hipótese
 
 
![Hipótese 1](https://github.com/user-attachments/assets/14a1e7db-187d-4593-9ac1-f941efc564e1)



![Hipótese 2](https://github.com/user-attachments/assets/b7208118-b78d-4f1a-9485-751b51ad70fc)



![Hipótese 3](https://github.com/user-attachments/assets/09c105e2-eb5e-4d52-a929-d1e17b5d9ff0)



![Hipótese 4](https://github.com/user-attachments/assets/3dac41bb-d31e-4249-9783-f95af7af0beb)



### 3.2. Aplicar Segmentação 



  ```sql
    --Criando uma coluna com segmentação das avaliações--
  WITH CategorizedRatings AS (
  SELECT
  product_id,
  product_name,
  category,
  discounted_price,
  actual_price,
  discount_percentage,
  about_product,
  main_category,
  sub_category,
  user_id,
  user_name,
  review_id,
  review_title,
  review_content,
  rating_count,
  rating,
  rating_quintile,
  rating_count_quintile,
  actual_price_quintile,
  discount_percentage_quintile,
    CASE
      WHEN rating = 0 THEN '0'
      WHEN rating >= 0 AND rating < 1 THEN '0-1'
      WHEN rating >= 1 AND rating < 2 THEN '1-2'
      WHEN rating >= 2 AND rating < 3 THEN '2-3'
      WHEN rating >= 3 AND rating < 4 THEN '3-4'
      WHEN rating >= 4 AND rating < 5 THEN '4-5'
      WHEN rating = 5 THEN '5'
    END AS rating_range
  FROM
    `projeto-4-caso-consultoria.Amazon.uniao_tabelas`
)
SELECT
  *
FROM
  CategorizedRatings
ORDER BY
  main_category, rating_range;

--Atualizando a tabela com a coluna com segmentação das avaliações--
  CREATE OR REPLACE TABLE `projeto-4-caso-consultoria.Amazon.uniao_tabelas` AS
  WITH CategorizedRatings AS (
  SELECT
  product_id,
  product_name,
  category,
  discounted_price,
  actual_price,
  discount_percentage,
  about_product,
  main_category,
  sub_category,
  user_id,
  user_name,
  review_id,
  review_title,
  review_content,
  rating_count,
  rating,
  rating_quintile,
  rating_count_quintile,
  actual_price_quintile,
  discount_percentage_quintile,
    CASE
      WHEN rating = 0 THEN '0'
      WHEN rating >= 0 AND rating < 1 THEN '0-1'
      WHEN rating >= 1 AND rating < 2 THEN '1-2'
      WHEN rating >= 2 AND rating < 3 THEN '2-3'
      WHEN rating >= 3 AND rating < 4 THEN '3-4'
      WHEN rating >= 4 AND rating < 5 THEN '4-5'
      WHEN rating = 5 THEN '5'
    END AS rating_range
  FROM
    `projeto-4-caso-consultoria.Amazon.uniao_tabelas`
)
SELECT
  *
FROM
  CategorizedRatings
ORDER BY
  main_category, rating_range;
```

Após realizar a segmentação das avaliações para que fosse possível analisar a satisfação dos clientes de acordo com as categorias de produtos, realizamos uam nova classificação com o objetivo de verificar o desempenho de cada categoria, de acordo com as variáveis: rating, rating_count, actual_price e discount_percentage, para ter uma visão do desempenho do produto na visão da empresa.



  ```sql
    --coluna com a média da soma dos quintis das variáveis, rating, rating_count, actual_price e discount_percentage---
WITH Quintile AS (
  SELECT
  product_id,
  product_name,
  category,
  discounted_price,
  actual_price,
  discount_percentage,
  about_product,
  main_category,
  sub_category,
  user_id,
  user_name,
  review_id,
  review_title,
  review_content,
  rating_count,
  rating,
  rating_quintile,
  rating_count_quintile,
  actual_price_quintile,
  discount_percentage_quintile,
  rating_range,
  ROUND((rating_quintile + rating_count_quintile + actual_price_quintile + discount_percentage_quintile) / 4.0) AS average_quintile
  FROM
    `projeto-4-caso-consultoria.Amazon.uniao_tabelas`
)
SELECT
  *
FROM
  Quintile;
--Atualizando a tabela---
CREATE OR REPLACE TABLE `projeto-4-caso-consultoria.Amazon.uniao_tabelas` AS
WITH Quintile AS (
  SELECT
  product_id,
  product_name,
  category,
  discounted_price,
  actual_price,
  discount_percentage,
  about_product,
  main_category,
  sub_category,
  user_id,
  user_name,
  review_id,
  review_title,
  review_content,
  rating_count,
  rating,
  rating_quintile,
  rating_count_quintile,
  actual_price_quintile,
  discount_percentage_quintile,
  rating_range,
  ROUND((rating_quintile + rating_count_quintile + actual_price_quintile + discount_percentage_quintile) / 4.0) AS average_quintile
  FROM
    `projeto-4-caso-consultoria.Amazon.uniao_tabelas`
)
SELECT
  *
FROM
  Quintile;
```


## 4. Resumir informações e apresentar resultados em um dashboard

![Dashboard](https://github.com/user-attachments/assets/6225d1c6-64d7-4d74-9bc0-8e96e0980757)



**Tabela de Resumo das Categorias**


![Resumo Categorias](https://github.com/user-attachments/assets/f84941be-259e-4d7a-a2ff-70b182c21744)



## 5. Conclusões e próximos passos

**Conclusão: Rumo ao Sucesso no Dropshipping**

Ao implementar um modelo de análise de produtos robusto e estratégico, a Amazon e seus fornecedores de dropshipping podem tomar decisões mais assertivas, otimizar seus investimentos e alcançar um sucesso duradouro no mercado dinâmico do comércio eletrônico. A Matriz BCG, como ferramenta complementar, auxilia na identificação de oportunidades e tomada de decisões estratégicas para cada produto da carteira.

Podemos verificar na tabela com resumo das informações das categorias, que o desempenho em satisfação dos clientes deixa a categoria Casa e Cozinha em 1º lugar seguida, de Computadores e acessórios em 2º lugar, Eletrônicos apesar de ser a categoria com mais produtos no seu portfólio e 8,6 milhões de avaliações, esta em 3º lugar, e em 4º lugar Produtos de escritório que apesar de ter a melhor média e possuir 100% dos seus produtos com classificação entre 4 e 5, possui um número de apenas 31 produtos, representando apenas 2,29% do total de produtos.

**Eletrônicos em Foco**

No contexto da Amazon, com foco na categoria de eletrônicos, a análise de produtos e a Matriz BCG podem ser utilizadas para:

- Priorizar investimentos em produtos com alto potencial de crescimento (smartphones, notebooks, TVs).
- Maximizar a lucratividade de produtos estáveis e populares (eletrodomésticos, tablets).
- Identificar produtos com baixo desempenho que precisam ser otimizados ou descontinuados (acessórios obsoletos, periféricos de baixa qualidade).

  **Próximos Passos:**

Para construir um modelo de desempenho de produto eficaz, é essencial analisar as seguintes variáveis:

- Porcentagem de Desconto e Preço: Indicadores da rentabilidade do produto e da competitividade no mercado.
- Avaliações e Pontuação: Revelam a popularidade do produto e a percepção do cliente.

E as seguintes, adicionais:

- Data e Valor de Venda: Permitem identificar tendências de mercado e oportunidades de crescimento.
- Investimento do Fornecedor: Essencial para calcular a margem de lucro real e negociar melhores preços.

Com a completude das informações. A análise terá os pilares já citados, com bases mais elaboradas para resultados mais precisos:

1. Rentabilidade do Produto: Avalia o potencial de lucro através do preço de venda e descontos disponíveis na plataforma *e margem de lucro.*
2. Popularidade do Produto: Mede o interesse do público através do número de avaliações, classificação média *e tendências de pesquisa ao longo do tempo.*
3. Satisfação do Cliente: Monitora o nível de contentamento dos consumidores através da pontuação média das avaliações *e de uma análise de sentimento a partir de PNL.*

**Impulsionando o produto para o sucesso**

Com a complementação dos dados também é possível desenvolver uma Matriz BCG automatizada para que os produtos sejam categorizados com praticidade e estratégias de mercado possam ser traçadas, analisando a participação de mercado e taxa de crescimento do produto.
