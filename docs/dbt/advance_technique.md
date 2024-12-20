Module 4: dbt Best Practices and Advanced Techniques

DRY (Don't Repeat Yourself) principles
Jinja templating in dbt
Macros and reusable code
Package management
Understanding and creating ref() and source() functions
Complex model dependencies


# Advance Technique

### What is Jinja in dbt

> Jinja is a templating engine used by dbt to dynamically generate SQL queries. It allows you to write code that is modular, reusable, and flexible by embedding Python-like expressions and variables into your SQL code. Jinja is built into dbt, so you can use it to customize queries based on parameters, environment variables, or metadata from your project.

Basics of Jinja in dbt

1. Expressions

    - Jinja expressions are placed inside {{ ... }}
    - These expressions are evaluated when dbt render the query.

    ```SELECT * FROM {{ ref('my_table') }}```

2. Control Structures

    - Use Jinja’s {% ... %} for control logic like loops and conditionals.

    ```{% if target.name == 'prod' %}
        SELECT * FROM production_table
    {% else %}
        SELECT * FROM staging_table
    {% endif %}
    ```

3. Variables

    - Define variables in dbt_project.yml or pass them at runtime.
    - Access variables in your SQL files using {{ var('variable_name') }}

    ```vars:
    latest_date: 2024-11-04
    ```

    ```SELECT * FROM ... WHERE  processing_date = DATE(" {{var('latest_date')}} ")```

4. Macro

    - A marco is a reusable block of jinja code.
    - It is defined in .sql files in the macros/ folder.

    marcos/cents_to_dollars.sql

    ```{% macro cents_to_dollars(column_name, scale=2) %}
    ({{ column_name }} / 100)::numeric(16, {{ scale }})
    {% endmacro %}
    ```

    A model which uses this macro might look like:

    ```select
    id as payment_id,
    {{ cents_to_dollars('amount') }} as amount_usd,
    ...
    from app_data.payments
    ```

    This would be compiled to:

    ```select
    id as payment_id,
    (amount / 100)::numeric(16, 2) as amount_usd,
    ...
    from app_data.payments
    ```
