---
---

# 3.4. Dishing out DB Metrics

Databases are full of hidden signals, and it’s your job to uncover them. In this lab, you’ll configure the Postgres exporter, scrape its metrics, and transform raw data into actionable observability. Consider it telemetry straight from the source.

## Observing Postgres

QuickPizza is configured to run from a single Postgres database running in Kubernetes. It can be monitored via the connection string below:
```
postgres://quickpizza:quickpizza@postgres.quickpizza:5432/quickpizza?sslmode=disable
```

:::tip[Documentation Reference]

Alloy has a native [prometheus.exporter.postgres](https://grafana.com/docs/alloy/latest/reference/components/prometheus/prometheus.exporter.postgres/) component.

:::

1.  Edit the **lab_scrape_telemetry** pipeline to get started.

1.  Create the components needed to monitor the QuickPizza Postgres database.
    <details>
        <summary>Need a hint?</summary>

        The [first example](https://grafana.com/docs/alloy/latest/reference/components/prometheus/prometheus.exporter.postgres/#collect-metrics-from-a-postgresql-server) in the docs is a great starting point. We already have a `prometheus.remote_write` component, but the exporter and scrape components will be needed to wire up metrics from Postgres. Use the connection string documented above.

    </details>

1.  **Save** the pipeline and confirm to roll out changes.

1.  Try using Alloy's live debugging UI to confirm if your updates worked. Try live debugging the `prometheus.scrape` component.

1.  Back in Grafana, check out **Drilldown -> Metrics** to see what Postgres metrics were picked up.
