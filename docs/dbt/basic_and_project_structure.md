# Project Structure

## Structure our dbt projects

dbt Basics and Project Structure

Understanding dbt project structure
Creating your first dbt project
Basic project configuration
Understanding dbt models
Writing SQL transformations
Best practices for model organization
Intro to dbt Core concepts:

Models
Sources
Seeds
Tests

```
    jaffle_shop
    ├── README.md
    ├── analyses
    ├── seeds
    │   └── employees.csv
    ├── dbt_project.yml
    ├── macros
    │   └── cents_to_dollars.sql
    ├── models
    │   ├── intermediate
    │   │   └── finance
    │   │       ├── _int_finance__models.yml
    │   │       └── int_payments_pivoted_to_orders.sql
    │   ├── marts
    │   │   ├── finance
    │   │   │   ├── _finance__models.yml
    │   │   │   ├── orders.sql
    │   │   │   └── payments.sql
    │   │   └── marketing
    │   │       ├── _marketing__models.yml
    │   │       └── customers.sql
    │   ├── staging
    │   │   ├── jaffle_shop
    │   │   │   ├── _jaffle_shop__docs.md
    │   │   │   ├── _jaffle_shop__models.yml
    │   │   │   ├── _jaffle_shop__sources.yml
    │   │   │   ├── base
    │   │   │   │   ├── base_jaffle_shop__customers.sql
    │   │   │   │   └── base_jaffle_shop__deleted_customers.sql
    │   │   │   ├── stg_jaffle_shop__customers.sql
    │   │   │   └── stg_jaffle_shop__orders.sql
    │   │   └── stripe
    │   │       ├── _stripe__models.yml
    │   │       ├── _stripe__sources.yml
    │   │       └── stg_stripe__payments.sql
    │   └── utilities
    │       └── all_dates.sql
    ├── packages.yml
    ├── snapshots
    └── tests
        └── assert_positive_value_for_total_amount.sql
```

**Guide structure overview**
<br>
**We'll walk through our topics in the same order that our data would move through transformation:**

Dig into how we structure the files, folders, and models for our three primary layers in the models directory, which build on each other:
Staging — creating our atoms, our initial modular building blocks, from source data
Intermediate — stacking layers of logic with clear and specific purposes to prepare our staging models to join into the entities we want
Marts — bringing together our modular pieces into a wide, rich vision of the entities our organization cares about
Explore how these layers fit into the rest of the project:
Review the overall structure comprehensively
Expand on YAML configuration in-depth
Discuss how to use the other folders in a dbt project: tests, seeds, and analyses