## Overview

Now that we have created a [PR pipeline](/Sample-2/README.md) that updates `values.yaml`, obtains approval before merging the PR, and then syncs the GitOps application with the merged changes, let's add a Failure Strategy. If the application sync fails, we will revert the merged PR to restore the GitOps application to a healthy state and then re-sync it to ensure it remains in a healthy state after the rollback.

## Prerequisites

Make sure you have all the [prerequisites](/Sample-2/README.md#prerequisites) before adding a failure strategy to the GitOps PR Pipeline.

## GitOps PR Pipeline  

Follow this guide to [create a GitOps PR Pipeline](/Sample-2/README.md#creating-a-pr-pipeline) before adding a failure strategy.

## Adding a Failure Strategy to the GitOps Pipeline  

Once you have a [PR pipeline](/Sample-2/README.md#creating-a-pr-pipeline) ready, let's add a failure strategy in the **GitOps Sync** step.

1. Under **GitOps Sync**, go to **Advanced**, add a **Failure Strategy**, click on **All Errors**, and under **Perform Action**, select **Rollback Stage**.  
   ![](/static/rollback_stage.png)  
   Learn more about [Failure Strategy](https://developer.harness.io/docs/platform/pipelines/failure-handling/define-a-failure-strategy-on-stages-and-steps) in Harness.

2. Click on **Rollback** to add a rollback step to initiate the rollback process.  
   Learn more about [Kubernetes Rollback](https://developer.harness.io/docs/continuous-delivery/deploy-srv-diff-platforms/kubernetes/cd-k8s-ref/kubernetes-rollback/) in Harness.

3. Click on **Add Step**, select **GitOps Revert PR**, and under **Commit ID**, use an expression to fetch the commit ID of the merged PR. This will revert the PR to its original state and restore the GitOps application.  

   Use the following expression to fetch the commit ID from the **mergePR** step:  `<+pipeline.stages.STAGE_ID.spec.execution.steps.mergePR.mergePROutcome.commitId>`

![](/static/revert_pr_step.png)  
Learn more about the [Revert PR step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps/#revert-pr-step) in Harness.

4. Add another **GitOps Sync** step to ensure the GitOps application is back to a healthy state after reverting the PR.  
- Click on **Add Step**, select **GitOps Sync**, provide a step name, and under **Advanced Configuration**, choose the **Application Name** you created in Step 4.  
- Click **Apply Changes**.  

This step will re-sync the GitOps application to its last known healthy state.  

Learn more about the [GitOps Sync step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step) in a PR pipeline in Harness.

You can also view the complete pipeline YAML for fetching app details and syncing the GitOps application [here](/Sample-3/pipeline.yaml).

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
10. [Revert PR step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps/#revert-pr-step)
11. [Kubernetes Rollback](https://developer.harness.io/docs/continuous-delivery/deploy-srv-diff-platforms/kubernetes/cd-k8s-ref/kubernetes-rollback/) 






