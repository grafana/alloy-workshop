---
---

# 3.1. Forge your Fame

import Question from '@site/src/components/Question';

We’ve reached the final stretch: a series of self-guided missions where you’ll put everything you’ve learned into practice. Each challenge is designed to test your skills, spark some creativity, and maybe even spark a little friendly competition. Ready to begin your first mission?

## Catching Up

The success of this final lab requires having working telemetry pipelines for metrics, logs, and traces. You can use these completed pipelines if you haven't fully finished lab 1 and lab 2.

-  [lab_scrape_telemetry](https://raw.githubusercontent.com/grafana/alloy-workshop/refs/heads/main/finished_pipelines/lab_scrape_telemetry.alloy)
-  [lab_receive_telemetry](https://raw.githubusercontent.com/grafana/alloy-workshop/refs/heads/main/finished_pipelines/lab_receive_telemetry.alloy)

## Find your Metal Moniker

No mission is complete without a proper codename. Claim your Metal Moniker: a shiny nickname that will follow you through the challenges ahead (and maybe onto the leaderboard).

1.  From the Fleet Management page, open the **Inventory** tab to list your connected Alloy instance.

1.  In the **Attributes** column, look for the value of the **moniker** attribute on your collector.

<Question id={'moniker'} title={'Jot down your Metal Moniker here in case you need it later.'}></Question>


## Join the Leaderboard (optional)

The leaderboard will track everyone’s progress across missions and be on display for the rest of the workshop. It’s all in good fun—hop on if you’re up for bragging rights, or stay off the board if you’d rather fly under the radar. Leaderboard or not, you'll still be able to complete the challenges.

:::tip[Documentation Reference]

In addition to the Grafana Alloy reference docs, these pages may prove useful:
* [Pipeline Attribute Injection](https://grafana.com/docs/grafana-cloud/send-data/fleet-management/set-up/configuration-pipelines/pipeline-attribute-injection/)

:::

1.  From the Fleet Management **Remote Configuration** page, open the **self_monitoring_metrics** pipeline. This pipeline is provided out-of-box to report the health of your Alloy instances to Grafana Cloud and is one of the signals powering the `Collector Health` view in Fleet Management.

1.  Update the pipeline so that the value of the `moniker` attribute is added as a label `leaderboard_name` on the metrics scraped by Alloy.
    <details>
        <summary>Need a hint?</summary>

        Try adding a rule in the `discovery.relabel` component. The first rule in this block is doing something very similar to what you need to accomplish.
    </details>

1.  Check the Alloy UI to see if the metrics flowing through the pipeline contain your newly added label.

1.  Congrats! You should be on the leaderboard shortly. Your facilitator may already be displaying this dashboard, but you can also access it from the home page of your Grafana Cloud stack.
