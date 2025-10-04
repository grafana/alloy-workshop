---
---

# 3.5. Cutting Through The Noise

Not every log is important — some are just static. Your mission: filter out the noise from your Kubernetes logs so the real signals shine through. Think of it as giving your observability stack a pair of noise-cancelling headphones.

## Dropping Logs

Health checks, readiness probes, and liveness endpoints can flood your system with repetitive noise, making it harder to see what really matters. Next you will use Alloy's log processing components to drop these unhelpful lines and let the meaningful signals stand out.

Your goal is to drop request logs for **/healthz**, **/ready**, and **/metrics**.

:::tip[Documentation Reference]

Alloy's [loki.process](https://grafana.com/docs/alloy/latest/reference/components/loki/loki.process/) component has a variety of stages that can be used to manipulate logs as they pass through a pipeline.

:::

1.  Navigate to **Drilldown -> Logs** and open one of the QuickPizza web services like `catalog`

1.  Take note of how the log line is structured and where the request paths are logged.

1.  From Fleet Management, open the **lab_scrape_telemetry** pipeline to get started.

1.  Create a `loki.process` component to drop the request logs for **/healthz**, **/ready**, and **/metrics**.
    <details>
        <summary>Need a hint?</summary>

        Try creating 2 stages to accomplish this task.
        1.  Use the [json stage](https://grafana.com/docs/alloy/latest/reference/components/loki/loki.process/#stagejson) to extract the request path from the log message.
        2.  Use the [drop stage](https://grafana.com/docs/alloy/latest/reference/components/loki/loki.process/#stagedrop) to drop log lines where the request path matches the ones we want to filter out.

        <details>
            <summary>Regex making you mad again?</summary>

            Here is a pattern that will match the 3 paths we're trying to drop:
            ```
            expression = `\/metrics|\/healthz|\/ready`
            ```
        </details>
    </details>

1.  **Save** the pipeline and confirm to roll out changes.

1.  Try using Alloy's live debugging UI on your `loki.process` component to watch how it processes your log lines. Note this will output `[IN]` and `[OUT]` for each line (omitting the output if it's dropped).

1.  Back in Grafana, check out **Drilldown -> Logs** to confirm you're no longer seeing these logs.
