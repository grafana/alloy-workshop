---
---

# 3.3. Signals in the Sauce

import Question from '@site/src/components/Question';

A recipe is only as good as its ingredients—and so are your traces. In this lab, you'll extract ingredient names as span attributes for the catalog service. The result? Traces that are flavorful and easy to follow.

## Extracting attributes

We currently receive spans with paths in the name like `GET /api/ingredients/pineapple` from the catalog service. For this challenge, we want to extract the ingredient portion of the URL to a span attribute named `ingredient`.

:::tip[Documentation Reference]

The documentation for the [OpenTelemetry Collector span processor](https://grafana.com/docs/alloy/latest/reference/components/otelcol/otelcol.processor.span/#rename-a-span-name-and-adding-attributes) may prove useful for this challenge, but this doesn't discount the use of other components.

:::

1.  Edit the **lab_receive_telemetry** pipeline and add your new component.

1.  Create your span processor component 
    <details>
        <summary>Need a hint?</summary>

        Check out this [example from the docs](https://grafana.com/docs/alloy/latest/reference/components/otelcol/otelcol.processor.span/#keep-the-original-span-name). By crafting a regular expression for the span names, we can extract the ingredient as an attribute and remove it from the name (reducing cardinality)

        **Tip:** [regex101.com](https://regex101.com/) is a great way to test regular expressions interactively (use the `Golang` flavor).

        <details>
            <summary>Regex making you mad?</summary>

            It happens to the best of us. Here is a pattern that will capture the ingredient from the URL:
            ```
            rules = [`^GET \/api\/ingredients\/(?P<ingredient>.+)$`]
            ```

            Note that using `` `backticks` `` allows us to define string literals in Alloy. This avoids the backslashes in our regular expression from being interpreted by Alloy.
        </details>
    </details>
1.  **Save** the pipeline and confirm to roll out changes.

1.  Try using Alloy's live debugging UI to confirm if your updates worked. You may want to create a few recipes if you're not seeing spans.

1.  We can also confirm this using `Explore Traces` and some TraceQL!
    1.  Navigate to `Explore` in the left navigation of Grafana Cloud.

    1.  Type `traces` into the **Datasources** box and select the one containing your username (i.e. _not_ `demoinfra`).
    
    1.  Run the query below to see a count of requests by ingredient:
        ```
        {.service.name = "catalog" && .ingredient != ""} | count_over_time() by (.ingredient)
        ```
