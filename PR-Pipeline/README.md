## Overview

This pipeline sample creates a PR pipeline that updates `values.yaml`, obtains approval before merging the PR, and then syncs the GitOps application with the merged changes and then sync the GitOps application.

## Prerequisites

Before using this pipeline, ensure the following prerequisites are met:

### 1. Install Harness GitOps Agent

A Harness GitOps Agent is a worker process that runs in your environment, makes secure outbound connections to Harness SaaS, and performs all the GitOps tasks you request in Harness.

Learn more about installing a [GitOps Agent](https://developer.harness.io/docs/continuous-delivery/gitops/connect-and-manage/install-a-harness-git-ops-agent/) in Harness.

### 2. Create a GitOps Cluster

A cluster is the target deployment cluster that is compared to the desired state. Clusters are synced with the source manifests you add as GitOps Repositories.

In this sample, we have used the credentials of a specific GitOps Agent we installed earlier for authentication. You can also authenticate by specifying the Kubernetes Cluster URL and credentials.

Learn more about creating a [GitOps Cluster](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-cd-git-ops-quickstart#step-3-add-a-harness-gitops-cluster) in Harness.

### 3. Create a GitOps Repository

A Harness GitOps Repository is a repo containing the declarative description of a desired state. The declarative description can be in Kubernetes manifests, Helm Charts, Kustomize manifests, etc.

Check out the [Interactive Guide](https://app.tango.us/app/embed/589df13a-b1d7-4538-aef3-fcea4f179add) to create Gitops Repository for this sample.

- Fork and clone this [Git repository](https://github.com/harness-community/Gitops-Samples).
- Navigate to **GitOps: Settings**  
  ![](/static/gitops-setting.png)

- In **Overview**, select the GitOps Agent you created in Step 1 and provide the Git Repository URL.  
  ![](/static/gitops-repo.png)

- In **Credentials**, specify the **Connection Type**, and select the **Authentication method**.  
  ![](/static/gitops-repo-1.png)

- In the next step, **Verify the Connection**.

Learn more about creating a [GitOps Repository](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#repository) in Harness.

### 4. Create a GitOps Application

GitOps Applications manage GitOps operations for a given desired state and its live instantiation.

A GitOps Application collects the Repository (what you want to deploy), Cluster (where you want to deploy), and Agent (how you want to deploy). You define these entities and then select them when setting up your Application.

Check out the [Interactive Guide](https://app.tango.us/app/embed/18d662e4-5c08-4d63-95a5-20ebac87b42e) to create Gitops Application for this sample.

- Navigate to **Applications**  
  ![](/static/Gitops-application.png)

- Click on **+ New Application**.
- In **Overview**, enter the **Application Name**, select your **GitOps Operator** (in this sample, we are using **ArgoCD**), and select the **GitOps Agent** you created in Step 1.
![](/static/gitops-application-setting.png)

- In **Service**, click on **+ New Service**.  
  - Enter **Service Name**.  
  - Under **How do you want to set up your service?**, select **Inline** if you want to store your service in Harness or **Remote** if you want to store your service in a Git Repository.  
  - Under **Service Definition**, select the deployment type as **Kubernetes** and choose **GitOps**.  
  - Under **Manifest**, click on **+ Add Release Repo Manifest**.  
    - Select **Release Repo Store** as **GitHub**.  
    - Provide the GitHub Connector to authenticate with the repo containing your release repo manifest. Learn more about creating a [GitHub Connector](https://developer.harness.io/docs/platform/connectors/code-repositories/ref-source-repo-provider/git-hub-connector-settings-reference/). For this sample, we are using the same [GitOps Sample](https://github.com/harness-community/Gitops-Samples) repo.  
    - In **Manifest Details**, provide **Manifest Name**, set **Git Fetch Type** as **Latest from Branch**, select Branch **main**, and specify File Path as **gitops-sample-app/charts/Python-App/values.yaml**.  
  - Under **Artifacts**, click on **+ Add Artifact Source**.  
    - Select **Docker Registry** under **Specify Artifact Repository Type** and choose **Docker Registry Connector**. Learn more about creating a [Docker Connector](https://developer.harness.io/docs/platform/connectors/cloud-providers/ref-cloud-providers/docker-registry-connector-settings-reference) in Harness.  
    - Under **Artifact Details**, provide **Artifact Source Identifier**, enter the **Image Path** (for this sample, we are using a public image **krishi0408/python-helm-native-canary-blue**), specify a tag (for this sample, we use **9**), and click **Submit**.  
  - Click **Save**.  
- In **Environment**, click on **+ New Environment**.  
  - Enter **Name**.  
  - Choose **Environment Type**.  
  - Under **How do you want to set up your Environment?**, select **Inline** if you want to store your Environment in Harness or **Remote** if you want to store your Environment in a Git Repository.  
  - Click **Save**.  
- Under **Sync Policy**, select **Manual** and click **Continue**.  
- Under **Source**, select **Repo URL**, and provide the Repo URL (for this sample, we are using [GitOps Sample](https://github.com/harness-community/Gitops-Samples) repo).  
  - Under **Target Revision**, choose **Branch** and set it to **main**.  
  - Under **Path**, enter **gitops-sample-app/charts/Python-App**.  
  - Under **Helm**, choose the Values file as **values.yaml**.  
- Under **Destination**, choose the **GitOps Cluster** you created in Step 2 and provide the **Namespace** where you installed your **GitOps Agent**.  
- Click **Finish**.  

Learn more about creating a [GitOps Application](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#application) in Harness.

## Creating a PR Pipeline

Check out the [Interactive Guide](https://app.tango.us/app/embed/91bc8d1d-64fb-4523-b218-b86297deae85) to create a GitOps PR Pipeline for this sample.

- Under **Pipelines**, click on **+ Create a Pipeline**, give a name to your pipeline, and under **How do you want to set up your Pipeline?**, select **Inline** if you want to store your pipeline in Harness or **Remote** if you want to store your pipeline in a Git repository.  

- Click on **Add Stage**, select **Stage Type** as **Deploy**.  
- Provide a **Stage Name**, select **Deployment Type** as **Kubernetes**, and select **GitOps**.  
- Under **Service**, select the service you created as part of your **GitOps Application** in Step 4.  
- Under **Environment**, select the environment you created as part of your **GitOps Application** in Step 4.  
- Under **Specify GitOps Cluster**, select the GitOps cluster from the same GitOps agent as your AppSet that manages your pipeline, which you created in Step 2.  

For the cluster to be populated under **Specify GitOps Cluster**, navigate to the environment you created in Step 4 (during the creation of the GitOps Application), go to the **GitOps Cluster** tab, click on **+ Select Cluster(s)**, and select the GitOps cluster from the same GitOps agent as your AppSet that manages your pipeline, which you created in Step 2.  

![](/static/env_gitops_cluster.png)  

- Under **Execution**, we have steps as **Update Release Repo**, **Merge PR**, and **Fetch Linked Apps**.  
  1. Click on **Update Release Repo**. This step fetches JSON or YAML files, updates them with your changes, performs a commit and push, and then creates the PR.
   - Provide name for your step **Update Release Repo**.
   - Under **Optional Configuration**, Provide **PR Title (optional)** that will be the PR title for the PR that will be created for changes done in the `values.yaml` file.
   - Under **Variables (optional)**, we will update the `replicaCount` in Values YAML to `3` from `1`.
   - Click on **Apply Changes**.
   Learn more about [Update Release Repo](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#update-release-repo-step) in PR pipeline in Harness.

  2. Now, we will add an Approval Step before **Merge PR** step to ask for approval before merging the PR in the main branch. 
    - Click on **Add Step**, select **Harness Approvals**, provide a step name, then under **User Group** select the User Group of users you want approval from. Click on **Apply Changes**.  
  3. Click on **Merge PR**, This step merges the new PR created by the preceding Update Release Repo step.
    - Provide name for your step **Update Release Repo**.
    - Click on **Apply Changes**.
    Learn more about [Merge PR step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#merge-pr-step) in PR pipeline in Harness.

  4. Click on **Add Step**, select **GitOps Sync**, provide a step name, and under **Advanced Configuration**, provide **Application Name** and choose the **GitOps Application** you created in Step 4. Click **Apply Changes**, this step will sync the GitOps application with the merged changes.
  Learn more about [GitOps Sync step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step) in PR pipeline in Harness.

- Click **Save** and **Run** the pipeline.  

You can also view the complete pipeline YAML for fetching app details and syncing the GitOps application [here](/Sample-2/pipeline.yaml).  

In the execution logs for the **Update Release Repo** step, you can see the details of the GitOps application and inside the step under **Update GitOps Configuration files**, we can see that `replicaCount` is getting updated to `3` and under **Create PR** we can see that a PR is created.
![](/static/Update%20release%20repo.png)


In the execution logs for the **Approval_before_merging_pr** step, you can **Approve or Reject** the step before merging the PR. 
![](/static/approval_step.png)

Once you approve the PR **Merge PR** step will start. In the execution logs you can see that the PR created in **Update Release Repo** step is merged into main branch. 
![](/static/merge_pr.png)

Please note that, for merging the PR, you need to ensure that the token used for authentication in your Git repository has the necessary permissions to merge the PR.

Once the PR is merged, the **GitOpsSync_Application** Once the PR is merged, the **GitOpsSync_Application** step will be triggered, updating the replica count from 3 to 1.
![](/static/Gitops_sync_app_step.png)

You can either select the **URL** from the execution logs, which will redirect you to the GitOps Application, and then you can check the **Sync Status** and **Resource View** of your GitOps Application or manually navigate to Gitops Application Page and select your GitOps Application to check the Sync Status and logs.

Click on the Deployment resource kind, then navigate to the Events tab, where you can see that the ReplicaSet has been scaled down from 3 to 1.
![](/static/gitops_dep_resource.png)

Learn more about creating [PR Pipeline](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/) in Harness.

You can also view the complete pipeline YAML for above sample [here](/PR-Pipeline/pipeline.yaml).  

You have successfully updated `values.yaml`, obtained approval, merged the PR, synced your GitOps application! 🚀

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