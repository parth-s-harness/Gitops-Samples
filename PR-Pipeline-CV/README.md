## Overview

Now that we have created a PR Pipeline to update, merge, and sync the application, we will add a **Continuous Verification (CV)** step and use **Datadog** to monitor the number of pods used in our GitOps cluster.

## Prerequisites

Make sure you have completed all the [prerequisites](/PR-Pipeline/README.md#prerequisites) before adding a failure strategy to the GitOps PR Pipeline.

## GitOps PR Pipeline

Follow this guide to [create a GitOps PR Pipeline](/PR-Pipeline/README.md#creating-a-pr-pipeline) before proceeding.

## Adding a Continuous Verification (CV) Step to the PR Pipeline

1. Click on **+ Add Step**, then under **Continuous Verification**, select **Verify**.
2. Provide a name for your **Verify** step.
3. Under **Continuous Verification Type**, select the type that matches your deployment strategy.
4. Click on **+ Add** under **Health Sources**.
5. In the **Edit Health Source** tab, select health source type as **Datadog**.
6. Provide a **Health Source Name**.
7. Under **Connect Health Source**, add a Datadog Connector. Learn more about creating a [**Datadog Connector**](https://developer.harness.io/docs/continuous-delivery/verify/configure-cv/health-sources/datadog) in Harness.
8. Under **Select Dashboards**, you can either select an existing **Datadog Dashboard** or click **+ Manually input query**.
9. We will manually enter our query, so click on **+ Manually input query**.

   ![](/static/manual_query_cv.png)

10. After clicking **Submit**, the **Query Specification** page will open:

    - Provide a **Metric Name**.
    - Provide a **Group Name**.
    - Provide the **Metric**, for example:  
      `kubernetes_state.deployment.replicas_available`
    - Optionally, provide an **Aggregator**.
    - Under **Metrics Tag**, fetch the replica count for your app instance `gitops-sample-application`.  
      Example:  
      `kube_app_instance:gitops-sample-application`  
      > This will auto-populate all metric tags from your cluster.

    - You can also manually enter a Datadog query.  
      Example:  
      `avg:kubernetes_state.deployment.replicas{kube_app_instance:gitops-sample-application}`

    ![](/static/metrics_tag.png)

    - Under **Select the services you want to apply the metric**, choose the relevant services.
      - If you select **Continuous Verification**, you will need to provide a **Service Instance Identifier (SII)**.
        - The SII is a key field used to uniquely identify individual instances of a service within your infrastructure.
        - It enables consistent query configuration across multiple Harness functionalities like SRM and Verify steps.
        - It ensures accurate analysis and tracking by associating metrics and logs with specific service instances.
        - Example: `kube_app_instance`  
          > It will automatically fetch all the available SIIs in the dropdown.

      ![](/static/sii.png)

    - Click on **Submit**.

In DataDog dashbaord, the above query result will look like:-
![](/static/ddg_metrics.png)

11. When you **Run** the Pipeline, the **Verify** step will show a similar graph.

---

You have successfully set up Continuous Verification for your PR Pipeline! 🚀

Check out the full YAML [here](/PR-Pipeline-CV/pipeline.yaml) for this sample.


## Resources

1. [Harness Gitops Basic](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics)
2. [Harness CD Gitops Tutorial](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-cd-git-ops-quickstart)
3. [GitOps Agent](https://developer.harness.io/docs/continuous-delivery/gitops/connect-and-manage/install-a-harness-git-ops-agent/) 
4. [GitOps Repository](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#repository)
5. [GitOps Application](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#application)
6. [Update Release Repo](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#update-release-repo-step)
7. [Merge PR step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#merge-pr-step)
8. [GitOps Sync Step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step)
9. [PR Pipeline in Harness](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/)
10. [Continuous Verification](https://developer.harness.io/docs/category/configure-cv)

      




