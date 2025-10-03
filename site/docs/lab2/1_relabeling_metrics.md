---
---

# 2.1. Relabeling metrics

While we've used relabeling in the context of service discovery, it is also useful for manipulating scraped metrics. This allows us to perform operations like drop metrics, drop labels, combine labels, etc. This is the equivalent of `metric_relabel_configs` in Prometheus. We'll look at a few examples with the metrics we've collected.

```mermaid
flowchart LR
    A(Discovery) --> B(Discovery Relabeling)
    B --- C(Scrape)
    C --> D(Prometheus Relabeling)
    D --> E(Remote Write)
```

## Dropping Metrics

One common use case for metric relabeling is dropping metrics that are not needed.

:::tip[Documentation Reference]

These Grafana Alloy docs may prove useful:
* [prometheus.scrape](https://grafana.com/docs/alloy/latest/reference/components/prometheus/prometheus.scrape/) component reference
* [prometheus.relabel](https://grafana.com/docs/alloy/latest/reference/components/prometheus/prometheus.relabel/) component reference
:::

1.  Navigate to **Connections -> Collector -> Fleet Management** in the left-hand menu.

1.  Click the **Remote Configuration** tab to list the pipelines we have configured.

1.  Click the **Edit** (pencil icon) button next to the **lab_scrape_telemetry** pipeline to open the partially built pipeline we'll be modifying.

1.  **Paste the contents below** below the **prometheus.scrapes** component in the pipeline.

    This code includes a partial `prometheus.relabel` component.
    ```
    prometheus.relabel "app" {
      // TODO: forward to Grafana Cloud
      forward_to = []

      rule {
        // TODO: specify the action to drop the metric
        action = ""

        source_labels = ["__name__"]
        // TODO: specify a pattern to match the `go_info` metric
        regex = ""
      }

    }
    ```

1.  In the `prometheus.relabel` component, fill in the missing pieces as described by the comments.
    
    - Forward the output of the relabeling component to the `prometheus.remote_write` block

    - Specify the correct action to drop the metric

    - Specify the pattern to match the metric name `go_info`
    
    <details>
        <summary>See the solution</summary>
        ```
        prometheus.relabel "app" {
          // forward to Grafana Cloud
          forward_to = [prometheus.remote_write.grafana_cloud.receiver]

          rule {
            // specify the action to drop the metric
            action = "drop"

            source_labels = ["__name__"]
            // specify a pattern to match the `go_info` metric
            regex = "go_info"
          }

        }
        ```
    </details>

1.  Now that we have a `prometheus.relabel` block, we need to feed it some metrics!
    In the `prometheus.scrape` component, update `forwards_to` to the newly created component in the previous step.
    <details>
        <summary>See the solution</summary>
        ```
          forward_to = [prometheus.relabel.app.receiver]
        ```
    </details>

1.  Click the **Test configuration pipeline** button to validate the config.

1.  Click the **Save** button to apply it.

1.  In the modal that pops up warning that your pipeline is active, click **Save** again.

### Check your work

With metrics flowing, let's see what Alloy's live debugging shows.

1.  Open the browser tab for the Grafana Alloy UI and click **Remote Configuration** in the top navigation.

1.  Click the **View** button next to the **lab_scrape_telemetry.default** pipeline.

1.  Click the blue **Graph** button just below the name of the pipeline on the details page.

1.  **Verify** you see the boxes for the newly added Prometheus components.

1.  **Click the box** for the `prometheus.relabel` component.

1.  Click the **Live Debugging** button near the top of the page.

1.  Type **go_info** into the Filter Data textbox.

1.  Watch for data to appear as it flows through this component.

1.  This UI will begin displaying matched samples and the resulting relabeling (`{before} => {after}`). **Verify that the metric is dropped** (i.e. the line ends in `=> {}`)


Click **Next** to continue to the next module.
