## Overview

This pipeline sample fetches application details and syncs the selected application using the Harness GitOps Pipeline.

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

Checkout the [Interactive Guide](https://app.tango.us/app/embed/589df13a-b1d7-4538-aef3-fcea4f179add) to create Gitops Repository for this sample.

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

Checkout the [Interactive Guide](https://app.tango.us/app/embed/18d662e4-5c08-4d63-95a5-20ebac87b42e) to create Gitops Application for this sample.

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
    - In **Manifest Details**, provide **Manifest Name**, set **Git Fetch Type** as **Latest from Branch**, select Branch **main**, and specify File Path as **gitops-sample-app/charts/Python-App/Chart.yaml**.  
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

## Syncing Your Application  

#### Option 1: Manual Sync  

1. Under **GitOps**, go to **Applications** and select the GitOps application created in Step 4.  
2. Click **Sync**.  
   ![](/static/gitopps-sync-app.png)  
3. Under **Synchronize Resources**, select all resources for service, deployment, and ingress.  
   ![](/static/gitops-sync-app-1.png)  
4. Click **Synchronize**.  

You can view **Recent Deployment Activities** under the Deployment Tab.  
![](/static/gitops-deployment-view.png)  

Under **Resource View**, you can see the hierarchical structure of the GitOps-managed application.  
![](/static/gitopps-sync-app.png)  

Under **Sync Status**, you can view the sync status of all resources.  
![](/static/sync-status.png)  

If your sync status fails, view the error under **Sync Status**, make changes to your resource file, and sync again.  
![](/static/gitops-sync-fail.png)  

Learn more about [Syncing GitOps applications](https://developer.harness.io/docs/continuous-delivery/gitops/use-gitops/sync-gitops-applications) in Harness.

#### Option 2: Using a Harness Pipeline  

Checkout the [Interactive Guide](https://app.tango.us/app/embed/90e3760e-9e4e-46ad-b301-864012ffb5ce) to create a Harness Pipeline to fetch Gitops App details and sync Gitops App for this sample.

- Under **Pipelines**, click on **+ Create a Pipeline**, give a name to your pipeline, and under **How do you want to set up your Pipeline?**, select **Inline** if you want to store your pipeline in Harness or **Remote** if you want to store your pipeline in a Git repository.  

- Click on **Add Stage**, select **Stage Type** as **Deploy**.  
- Provide a **Stage Name**, select **Deployment Type** as **Kubernetes**, and select **GitOps**.  
- Under **Service**, select the service you created as part of your **GitOps Application** in Step 4.  
- Under **Environment**, select the environment you created as part of your **GitOps Application** in Step 4.  
- Under **Specify GitOps Cluster**, select the GitOps cluster from the same GitOps agent as your AppSet that manages your pipeline, which you created in Step 2.  

For the cluster to be populated under **Specify GitOps Cluster**, navigate to the environment you created in Step 4 (during the creation of the GitOps Application), go to the **GitOps Cluster** tab, click on **+ Select Cluster(s)**, and select the GitOps cluster from the same GitOps agent as your AppSet that manages your pipeline, which you created in Step 2.  

![](/static/env_gitops_cluster.png)  

- Under **Execution**, since we are only fetching app details and syncing your GitOps application, remove the first three steps: **Update Release Repo**, **Merge PR**, and **Fetch Linked Apps**.  
- Click on **Add Step**, select **GitOps Get App Details**, provide a step name, then select **Application Name** and choose the **GitOps Application** you created in Step 4. Click **Apply Changes**.  
Learn more about [GitOps Sync Step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step) in Harness.

- Click on **Add Step**, select **GitOps Sync**, provide a step name, and under **Advanced Configuration**, provide **Application Name** and choose the **GitOps Application** you created in Step 4. Click **Apply Changes**.  
Learn more about [Get App Details step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-get-app-details-step) in Harness.

- Click **Save** and **Run** the pipeline.  

In the execution logs for the **GitOps Get App Details** step, you can see the details of the GitOps application.  
![](/static/gitops-fetch-app.png)  

In the execution logs for the **GitOps Sync** step, you can see the sync details of the GitOps application.  
![](/static/gitops-sync-step.png)  

You can also view the complete pipeline YAML for above sample [here](/Fetch-App-Sync/pipeline.yaml).  

You have successfully fetched and synced your first GitOps Application! 🚀

## Resources

1. [Harness Gitops Basic](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics)
2. [Harness CD Gitops Tutorial](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-cd-git-ops-quickstart)
3. [GitOps Agent](https://developer.harness.io/docs/continuous-delivery/gitops/connect-and-manage/install-a-harness-git-ops-agent/) 
4. [GitOps Repository](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#repository)
5. [GitOps Application](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#application)
6. [GitOps Sync Step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step)
7. [Get App Details step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-get-app-details-step)