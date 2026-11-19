```mermaid
flowchart TD

    A[CSV SUSEP]
    B[Dados Sintéticos]

    A --> C[PySpark Ingestion]
    B --> C

    C --> D[Delta Bronze]

    D --> E[dbt Staging]

    E --> F[dbt Marts]

    F --> G[Star Schema]

    G --> H[Power BI]

    H --> I[Executivos]
    H --> J[Analistas]
```
