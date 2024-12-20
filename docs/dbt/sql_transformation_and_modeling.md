Module 3: SQL Transformations and Modeling

Writing effective dbt models
Understanding materialization types

Tables
Views
Incremental models


Handling complex transformations
Writing clean, modular SQL
Performance optimization techniques
Working with common data transformation patterns


1 Materialization Types
Materialization defines how your models are created and stored in the data warehouse:<br>

- Table Materialization
    - Creates a full table in the database
    - Best for complex transformations or when you need fast query performance
    - Rebuilds the entire table each time

```
{{ config(materialized='table') }}

SELECT 
    customer_id,
    SUM(order_amount) as total_revenue,
    COUNT(DISTINCT order_id) as order_count
FROM 
    {{ ref('raw_orders') }}
GROUP BY 
    customer_id
```

- View Materialization
    - Creates a virtual table (saved query)
    - Lightweight, always reflects latest source data
    - Good for simple transformations

```
{{ config(materialized='view') }}

SELECT 
    date,
    SUM(sales_amount) as daily_sales
FROM 
    {{ ref('sales_data') }}
GROUP BY 
    date
```

- Incremental Materialization
    - Only processes new or updated records
    - Ideal for large datasets and frequent updates

```
{{ config(
    materialized='incremental',
    unique_key='order_id'
) }}

SELECT 
    order_id,
    customer_id,
    order_date,
    total_amount
FROM 
    {{ ref('raw_orders') }}
{% if is_incremental() %}
    WHERE order_date > (SELECT MAX(order_date) FROM {{ this }})
{% endif %}
```

2 Writing Clean, Modular SQL

- Modularization Techniques
    - Use ref() to reference other models
    - Break complex transformations into multiple models
    - Create reusable CTEs

```
WITH customer_orders AS (
    SELECT 
        customer_id,
        COUNT(order_id) as total_orders
    FROM 
        {{ ref('orders') }}
    GROUP BY 
        customer_id
),

customer_revenue AS (
    SELECT 
        customer_id,
        SUM(order_total) as lifetime_revenue
    FROM 
        {{ ref('orders') }}
    GROUP BY 
        customer_id
)

SELECT 
    c.customer_id,
    c.total_orders,
    r.lifetime_revenue
FROM 
    customer_orders c
JOIN 
    customer_revenue r ON c.customer_id = r.customer_id
```

3 Performance Optimization Techniques

- Use incremental models for large datasets
    - Partition large tables
    - Use appropriate clustering keys
    - Avoid unnecessary JOINs and complex subqueries
```
{{ config(
    materialized='incremental',
    partition_by='order_date',
    cluster_by=['customer_segment']
) }}

SELECT 
    order_date,
    customer_segment,
    SUM(order_total) as segment_revenue
FROM 
    {{ ref('orders') }}
{% if is_incremental() %}
    WHERE order_date > (SELECT MAX(order_date) FROM {{ this }})
{% endif %}
GROUP BY 
    order_date, 
    customer_segment
```

4 Common Data Transformation Patterns
Slowly Changing Dimensions (SCD)
Track historical changes in dimension tables
```
WITH current_data AS (
    SELECT 
        customer_id,
        name,
        email,
        CURRENT_TIMESTAMP as valid_from
    FROM 
        {{ ref('customer_source') }}
),

historical_data AS (
    SELECT 
        *,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id 
            ORDER BY valid_from DESC
        ) as rn
    FROM 
        {{ this }}
)

SELECT * FROM (
    SELECT 
        customer_id,
        name,
        email,
        valid_from
    FROM 
        current_data
    
    UNION ALL
    
    SELECT 
        customer_id,
        name,
        email,
        valid_from
    FROM 
        historical_data
    WHERE 
        rn > 1
)
```
Key Takeaways for dbt Beginners:

Start with simple models
Use ref() to create dependencies
Choose the right materialization type
Keep transformations modular
Focus on readability and performance

Pro Tips:

Use dbt's built-in testing
Leverage dbt documentation
Learn to use dbt Cloud or dbt Core
Practice with sample datasets

Remember, dbt is about making your data transformations more maintainable, transparent, and efficient!