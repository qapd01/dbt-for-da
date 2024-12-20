## Materializations

### Overview
Materializations are strategies for persisting dbt models in a warehouse. There are five types of materializations built into dbt. They are:

- Table
- View
- Incremental
- Ephemeral
- Materialized View

### Table
- Pros: Table are fast to query.
- Cons:
    - Tables can take a long time to rebuild, especially for complex transformations
    - New records in underlying source data are not automatically added to the table

### View
When using the <strong>view</strong> materialization, your model is rebuilt as a view on each run, via a create view as statement.

- Pros: No additional data is stored, views on top of source data will always have the latest records in them.
- Cons: Views that perform a significant transformation, or are stacked on top of other views, are slow to query.

### Incremental
- Pros: You can significantly reduce the build time by just transforming new records
- Cons: Incremental models require extra configuration and are an advanced usage of dbt

### Ephemeral
- Pros:
You can still write reusable logic
Ephemeral models can help keep your data warehouse clean by reducing clutter (also consider splitting your models across multiple schemas by using custom schemas).
- Cons:
You cannot select directly from this model.
Operations (for example, macros called using dbt run-operation cannot ref() ephemeral nodes)
Overuse of ephemeral materialization can also make queries harder to debug.
Ephemeral materialization doesn't support model contracts.

### Materialized View
The <strong>materialized_view</strong> materialization allows the creation and maintenance of materialized views in the target database. Materialized views are a combination of a view and a table, and serve use cases similar to incremental models.

- Pros:
    - Materialized views combine the query performance of a table with the data freshness of a view
    - Materialized views operate much like incremental materializations, however they are usually able to be refreshed without manual interference on a regular cadence (depending on the database), forgoing the regular dbt batch refresh required with incremental materializations
    - dbt run on materialized views corresponds to a code deployment, just like views
- Cons:
    - Due to the fact that materialized views are more complex database objects, database platforms tend to have fewer configuration options available; see your database platform's docs for more details
    - Materialized views may not be supported by every database platform  