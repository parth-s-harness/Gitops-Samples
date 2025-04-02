## Overview

Now that we have [fetched and synced a single GitOps Application](/Fetch-App-Sync/README.md), let's move to a scenario where we have multiple GitOps Applications and we want to perform a sync on them using Harness Pipelines.

## Prerequisites

1. Ensure you have all the [prerequisites](/Fetch-App-Sync/README.md#prerequisites) before syncing multiple apps in a Harness Pipeline.
2. Create multiple GitOps Applications. Follow this guide to create a [GitOps Application](/Fetch-App-Sync/README.md#4-create-a-gitops-application). For this example, we are using [GitOps Multiple Application 1](/Gitops-multiple-apps/Application1/) and [GitOps Multiple Application 2](/Gitops-multiple-apps/Application2/) with the same GitOps Agent used in this [sample](/Fetch-App-Sync/README.md#syncing-your-application).
   - In the path of Application1, add **Path** as `Gitops-multiple-apps/Application1`.
   - In the path of Application2, add **Path** as `Gitops-multiple-apps/Application2`.
3. We will use the same [GitOps Repository](/Fetch-App-Sync/README.md#3-create-a-gitops-repository) as in this [sample](https://github.com/harness-community/Gitops-Samples).

## Syncing Multiple Apps

Now that we have created multiple applications, we will fetch their details and sync them using a Harness Pipeline.

All steps will be the same as those provided in [Fetch App Details and Sync App](/Fetch-App-Sync/README.md#option-2-using-a-harness-pipeline), but we will make changes in **GitOps Get App Details** and **GitOps Sync App**.

### Option 1: Manually Selecting Multiple Apps in GitOps Get App Details and GitOps Sync App

- In the **GitOps Get App Details** step, select **Application Names**. There is a dropdown to select multiple applications. Click **Apply Changes**.

  ![](/static/gitops-multiple-app-fetch.png)

- In the **GitOps Sync App** step, under **Advanced Configuration**, select **Application Names**. There is a dropdown to select multiple applications. Click **Apply Changes**.

  ![](/static/gitops-multiple-app-sync.png)

- Click **Save** and **Run**.

We can see in the console log for **GitOps Get App Details** that it is fetching details of the applications added under Application Names.

  ![](/static/fetch-app-details-multiple-logs.png)

We can see in the console log for **GitOps Sync App** that it is syncing the applications added under Application Names.

  ![](/static/sync-app-multiple-logs.png)

You have successfully fetched and synced multiple GitOps Applications! 🚀

### Option 2: Using a Matrix Looping Strategy to Dynamically Sync Multiple Apps

- In **Pipeline Studio**, add a pipeline variable that we will use in our matrix. Check more details on adding a [Pipeline Variable](https://developer.harness.io/docs/platform/variables-and-expressions/harness-variables/) in Harness. Under **Custom Variables**, add a variable named **applications** and choose the variable type as **Runtime Input**.

  ![](/static/pipeline-variable.png)

Now, we will add a matrix looping strategy in our deploy stage where we have **GitOps Get App Details** and **GitOps Sync App** steps.

- In the **Deploy** stage, under **Advanced** and **Looping Strategy**, select **Matrix** and add:

```yaml
matrix:
  applications: <+pipeline.variables.applications.split(',')>
```

Learn more about [Looping Strategy](https://developer.harness.io/docs/platform/pipelines/looping-strategies/looping-strategies-matrix-repeat-and-parallelism/) in Harness.

When running the pipeline, provide comma-separated values of application names for the pipeline variable **applications**. The split function will handle this automatically.

- In the **GitOps Get App Details** step, under **Advanced Configuration**, select **Application Regex**. Set its value to **Expression** and enter `<+matrix.applications>`. Learn more about [Expressions](https://developer.harness.io/docs/platform/variables-and-expressions/harness-variables/) in Harness. Click **Apply Changes**.

  ![](/static/application-regex-fetch.png)

- In the **GitOps Sync App** step, select **Application Regex**, set its value to **Expression**, and enter `<+matrix.applications>`. Click **Apply Changes**.

- Click **Save** and **Run** the pipeline.

- Provide application names for the variable **applications** in comma-separated format.

  ![](/static/run_pipeline_matrix.png)

- Click **Run Pipeline**.

We can see that the deploy stage started running in a matrix loop and that we successfully fetched application names.

  ![](/static/application_1.png)
  ![](/static/application_2.png)

You have successfully fetched and synced multiple GitOps Applications using the matrix looping strategy in Harness! 🚀

You can also view the complete pipeline YAML for this example [here](/Syncing-multiple-apps/pipeline.yaml).

## Resources

1. [Harness GitOps Basics](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics)
2. [Harness CD GitOps Tutorial](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-cd-git-ops-quickstart)
3. [GitOps Agent](https://developer.harness.io/docs/continuous-delivery/gitops/connect-and-manage/install-a-harness-git-ops-agent/)
4. [GitOps Repository](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#repository)
5. [GitOps Application](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#application)
6. [GitOps Sync Step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step)
7. [Get App Details Step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-get-app-details-step)
8. [Pipeline Variable and Expression](https://developer.harness.io/docs/platform/variables-and-expressions/harness-variables/)
9. [Looping Strategy](https://developer.harness.io/docs/platform/pipelines/looping-strategies/looping-strategies-matrix-repeat-and-parallelism/)