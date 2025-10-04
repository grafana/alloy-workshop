---
---

# 3.2. The Log of Hidden Secrets

import Question from '@site/src/components/Question';

The QuickPizza security team has received a report that the **inventory-sync** service seems to be leaking secrets in its application logs. You've been tasked with updating the log pipeline to stop this exposure.

## Masking Secrets

The inventory service logs via standard output to Kubernetes like the other QuickPizza services. Its logs are ingested into Grafana Cloud using the pipelune we created earlier, meaning we can manipulate them within that pipeline.

:::tip[Documentation Reference]

The Alloy documentation includes a reference to all of its [Loki components](https://grafana.com/docs/alloy/latest/reference/components/loki/) that could potentially be used.

:::

1.  In Grafana Cloud, navigate to **Drilldown -> Logs** and click the **Show Logs** button for the **inventory-sync** service.

1.  View the logs and identify the leaked token in the log line.

1.  Keep these logs handy, but navigate back to **Grafana Fleet Management** in another browser tab.

1.  Review the Loki components in the docs and identify one that might work for our use case.
    <details>
        <summary>Need a hint?</summary>

        Check out the [loki.secretfilter](https://grafana.com/docs/alloy/latest/reference/components/loki/loki.secretfilter/) component.
    </details>
1.  Edit the **lab_scrape_telemetry** pipeline and add a component between the `loki.source.kubernetes` and `loki.write` components to mask this secet.

1.  **Save** the pipeline and confirm to roll out changes.

1. Try using Alloy's live debugging UI to confirm if your updates worked. Then check the logs in Grafana to be extra sure.
