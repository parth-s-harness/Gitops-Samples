## Overview

In this sample, we will walk through deploying to a new environment using an ApplicationSet in a PR Pipeline. 

## Understanding Application Set in GitOps

ApplicationSet is an Argo CD feature that automates the creation and management of multiple applications from a single configuration. It enables dynamic application creation based on template-driven definitions. In our case, the ApplicationSet will detect the new environment configuration in the repo and automatically instantiate a new application with the desired configuration.

This approach ensures consistency, reduces manual effort, and enables streamlined deployments across environments using a GitOps-driven workflow.

Learn more about [Application Set](https://developer.harness.io/docs/continuous-delivery/gitops/applicationsets/appset-basics).


## PR Pipeline Workflow

We have multiple environments, and we want to deploy an application with a unique configuration for each new environment. Using a PR pipeline, we will:

- **Create a new configuration file** for the target environment using **Update Release Repo**, specifying any necessary application-specific changes.
- **Approve the changes** using **Harness Approval Step**.
- **Merge the PR** using **Merge PR step** to merge this configuration into the repository.
- **Approve the deployment** of AppSet.
- **Sync the ApplicationSet** using **GitOpsSync step**, which detects the new configuration and dynamically generates a new Harness GitOps application for the environment.
- **Fetch the Linked Application** using **Fetch Linked Apps step** to check the new application created.

## Using a Git Generator in the ApplicationSet

We use a **Git Generator** to dynamically create applications for different environments based on the `config.json` files stored in the repository.  

Git generator file for this sample: [here](/ApplicationSet-Sample/appset/applicationset.yaml)

## Steps

We are using the sample repository: **[GitOps-Samples: ApplicationSet-Sample](https://github.com/harness-community/Gitops-Samples/tree/main/ApplicationSet-Sample)**.

We will deploy the **Guestbook application** to a new environment by adding a new configuration file and using **Harness GitOps PR Pipeline**.

Below are the steps to create a **Harness GitOps PR Pipeline** to deploy to a new environment using **ApplicationSet**.

### Prerequisites

Before creating a PR pipeline, ensure the following prerequisites are met:

#### 1. Install Harness GitOps Agent

A **Harness GitOps Agent** is a worker process that runs in your environment, makes secure outbound connections to Harness SaaS, and performs all the GitOps tasks you request in Harness.

Learn more about installing a [GitOps Agent](https://developer.harness.io/docs/continuous-delivery/gitops/connect-and-manage/install-a-harness-git-ops-agent/) in Harness.

#### 2. Create a GitOps Cluster

A **GitOps Cluster** is the target deployment cluster that is compared to the desired state. Clusters are synced with the source manifests you add as GitOps Repositories.

In this sample, we have used the credentials of a specific **GitOps Agent** we installed earlier for authentication. You can also authenticate by specifying the **Kubernetes Cluster URL** and credentials.

Learn more about creating a [GitOps Cluster](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-cd-git-ops-quickstart#step-3-add-a-harness-gitops-cluster) in Harness.

#### 3. Create a GitOps Repository

A **Harness GitOps Repository** is a repo containing the declarative description of a desired state. The declarative description can be in Kubernetes manifests, Helm Charts, Kustomize manifests, etc.

Check out the [Interactive Guide](https://app.tango.us/app/embed/589df13a-b1d7-4538-aef3-fcea4f179add) to create a **GitOps Repository** for this sample.

- Fork and clone this [Git repository](https://github.com/harness-community/Gitops-Samples).
- Navigate to **GitOps: Settings**  
  ![](/static/gitops-setting.png)

- In **Overview**, select the **GitOps Agent** you created in Step 1 and provide the **Git Repository URL**.  
  ![](/static/gitops-repo.png)

- In **Credentials**, specify the **Connection Type**, and select the **Authentication Method**.  
  ![](/static/gitops-repo-1.png)

- In the next step, **Verify the Connection**.

Learn more about creating a [GitOps Repository](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#repository) in Harness.

#### 4. Create a GitOps Application

**GitOps Applications** manage GitOps operations for a given desired state and its live instantiation.

A **GitOps Application** collects the **Repository** (what you want to deploy), **Cluster** (where you want to deploy), and **Agent** (how you want to deploy). You define these entities and then select them when setting up your **Application**.

Check out the [Interactive Guide](https://app.tango.us/app/embed/18d662e4-5c08-4d63-95a5-20ebac87b42e) to create a **GitOps Application** for this sample.

- Navigate to **Applications**  
  ![](/static/Gitops-application.png)

- Click on **+ New Application**.
- In **Overview**, enter the **Application Name** (in this case, **appset**), select your **GitOps Operator** (in this sample, we are using **ArgoCD**), and select the **GitOps Agent** you created in Step 1.  
  ![](/static/gitops-application-setting.png)

- In **Service**, click on **+ New Service**.  
  - Enter **Service Name**.  
  - Under **How do you want to set up your service?**, select **Inline** if you want to store your service in Harness or **Remote** if you want to store your service in a Git Repository.  
  - Under **Service Definition**, select the deployment type as **Kubernetes** and choose **GitOps**.  
  - Under **Manifest**, click on **+ Add Release Repo Manifest**.  
    - Select **Release Repo Store** as **GitHub**.  
    - Provide the **GitHub Connector** to authenticate with the repo containing your release repo manifest. Learn more about creating a [GitHub Connector](https://developer.harness.io/docs/platform/connectors/code-repositories/ref-source-repo-provider/git-hub-connector-settings-reference/). For this sample, we are using the same [GitOps Sample](https://github.com/harness-community/Gitops-Samples) repo.  
    - In **Manifest Details**, provide **Manifest Name** (let's keep it as **config**), set **Git Fetch Type** as **Latest from Branch**, select branch **main**, and under **File Path**, change its value to **Runtime Input** for now. We will change it back to an expression later and provide a path to dynamically fetch the manifest file for the environment.
- Click on **Deployment Repo**, where we will define our **ApplicationSet YAML file**.
    - Select **Deployment Repo Store** as **GitHub**.  
    - Provide the **GitHub Connector** to authenticate with the repo containing your release repo manifest.  
    - In **Manifest Details**, provide **Manifest Identifier** (let's keep it as **appset**), set **Git Fetch Type** as **Latest from Branch**, select branch **main**, and under **File Path**, add **ApplicationSet-Sample/appset/applicationset.yaml**. This is the path to the **Git Generator file** that will be used to deploy our **GitOps Application** to the new environment.

  - Click **Save**.  
- In **Environment**, click on **+ New Environment**.  
  - Enter **Name** (let's keep it as **Appset_Environment**).  
  - Choose **Environment Type**.  
  - Under **How do you want to set up your Environment?**, select **Inline** if you want to store your environment in Harness or **Remote** if you want to store it in a Git Repository.  
  - Click **Save**.  
- Under **Sync Policy**, select **Manual** and click **Continue**.  
- Under **Source**, select **Repo URL**, and provide the **Repo URL** (for this sample, we are using [GitOps Sample](https://github.com/harness-community/Gitops-Samples) repo).  
  - Under **Target Revision**, choose **Branch** and set it to **main**.  
  - Under **Path**, enter **ApplicationSet-Sample/appset**.  
- Under **Destination**, choose the **GitOps Cluster** you created in Step 2 and provide the **Namespace** where you installed your **GitOps Agent**.  
- Click **Finish**.  

Learn more about creating a [GitOps Application](https://developer.harness.io/docs/continuous-delivery/gitops/get-started/harness-git-ops-basics#application) in Harness.

#### 5. Create an Environment

In this step, we will create an **environment** where we want to deploy our **GitOps Application**. When the **PR Pipeline** performs a sync operation on the new environment, it will fetch the environment name linked to our **GitOps Application** from our **config file**.

- In **Environment**, click on **+ New Environment**.  
  - Enter **Name** (let's keep it as **Prod1**).  
  - Choose **Environment Type** as **Production**.  
  - Under **How do you want to set up your Environment?**, select **Inline** if you want to store your environment in Harness or **Remote** if you want to store it in a Git Repository.  
  - Click **Save**.  

### Creating a PR Pipeline

- Under **Pipelines**, click on **+ Create a Pipeline**, give a name to your pipeline, and under **How do you want to set up your Pipeline?**, select **Inline** if you want to store your pipeline in Harness or **Remote** if you want to store your pipeline in a Git repository.  

- Click on **Add Stage**, select **Stage Type** as **Deploy**.  
- Provide a **Stage Name**, select **Deployment Type** as **Kubernetes**, and select **GitOps**.  
- Under **Service**, select the service you created as part of your **GitOps Application**.  
- Under **Environment**, select the environment you created as part of your **GitOps Application**.  
- Under **Specify GitOps Cluster**, select the GitOps cluster from the same GitOps agent as your AppSet that manages your pipeline.  

For the cluster to be populated under **Specify GitOps Cluster**, navigate to the environment you created (during the creation of the GitOps Application), go to the **GitOps Cluster** tab, click on **+ Select Cluster(s)**, and select the GitOps cluster from the same GitOps agent as your AppSet that manages your pipeline.  

![](/static/env_gitops_cluster.png)  

- Under **Execution**, we have steps as **Update Release Repo**, **Merge PR**, and **Fetch Linked Apps**.  

  1. Click on **Update Release Repo**. This step fetches JSON or YAML files, updates them with your changes, performs a commit and push, and then creates the PR.
     - Provide a name for your step **Update Release Repo**.
     - Under **Optional Configuration**, provide **PR Title (optional)** that will be the PR title for the PR that will be created for creating a new `config.json`.
     - Under **Variables (optional)**, we will add values for our `config.json`.
       
       For this sample, we will be using the following values. Add each value as an individual variable, and it will create a new `config.json` file with these configurations:
       
       ```yaml
       name: cluster_name
       type: String
       value: in-cluster
       name: harness_service
       type: String
       value: service_gitops_pr
       name: environment
       type: String
       value: prod1
       name: server_address
       type: String
       value: https://kubernetes.default.svc
       name: replicas
       type: String
       value: "3"
       ```

       ![](/static/variable_appset.png)  

       Here, `server_address` is your [GitOps Cluster](#2-create-a-gitops-cluster) address URL.  
     - Click on **Apply Changes**.  

     Learn more about [Update Release Repo](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#update-release-repo-step) in the PR pipeline in Harness.

  2. Now, we will add an **Approval Step** before the **Merge PR** step to ask for approval before merging the PR into the main branch.  
     - Click on **Add Step**, select **Harness Approvals**, provide a step name, then under **User Group**, select the User Group of users you want approval from. Click on **Apply Changes**.  

  3. Click on **Merge PR**. This step merges the new PR created by the preceding **Update Release Repo** step.
     - Provide a name for your step **Merge PR**.
     - Click on **Apply Changes**.  

     Learn more about the [Merge PR step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#merge-pr-step) in the PR pipeline in Harness.

  4. Now, we will again add an **Approval Step** before the **GitOps Sync** step to ask for approval before deploying the application to a new environment.
     - Click on **Add Step**, select **Harness Approvals**, provide a step name, then under **User Group**, select the User Group of users you want approval from. Click on **Apply Changes**.  

  5. Click on **Add Step**, select **GitOps Sync**, provide a step name, and under **Advanced Configuration**, provide **Application Name** and choose the **GitOps Application**—in this case, the [GitOps Application for AppSet](#4-create-a-gitops-application) you created. Click **Apply Changes**. This step will sync the GitOps application with the merged changes.

     Learn more about the [GitOps Sync step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/gitops-pipeline-steps#gitops-sync-step) in the PR pipeline in Harness.

  6. Now, we add a **Fetch Linked Apps** step to fetch all the Applications linked to the ApplicationSet. This step outputs all the applications linked along with their URL.

- Click **Save**.  

### Adding a Pipeline Variable  

Before running the pipeline, add a pipeline variable that will help propagate the environment name from the `config.json` file added in the **Update Release Repo** step.  

In **Pipeline Studio**, add a pipeline variable that we will use in our matrix.  

Under **Custom Variables**, add a variable named **environment_name**, set the variable type as **Expression**, and provide the value as an expression pointing to the variable defined in **Update Release Repo**:  

```yaml
<+pipeline.stages.<STAGE_ID>.spec.execution.steps.updateReleaseRepo.spec.variables.environment>
```
We will use this pipeline variable **environment_name** in the Service variable as well as the Manifest.


We will use this pipeline variable **environment_name** in Service variable as well as manifest.

Navigate to your service that you created as part of [Step 4](#4-create-a-gitops-application), move to the **Advanced** section and add a variable named as **environment** change it type to an Expression and provide expression as `<+pipeline.variables.environment_name>`, the same pipeliine variable but as expressions that you created above.

Under **Manifest**, click on **Edit** icon for the **Release repo** and move to **Manifest Details** setting and in the **File path** change it's value from **Runtime Input** to **Expression** and the add the expression as `ApplicationSet-Sample/cluster-config/<+serviceVariables.environment>/config.json`.

Check the complete service yaml [here](/Application-Set/service.yaml).

This setup will dynamically update the environment name in the manifest file, fetch the manifest file for the correct path, and if not present, it will create the file path.

This is how your pipeline will look in the end:-

![](/static/pipeline_view.png)

- Click on **Run**.

In the execution logs of **Update Release Repo** step we see that it tries to fetch the manifest file `ApplicationSet-Sample/cluster-config/prod1/config.json`. Since the file is not found, it will create a new file and a PR.

![](/static/update_appset.png)

Once done, it will move to the **Approval Step** and ask for approval before proceeding to merge the PR. Once approved, it will move to the **Merge PR step**.

![](/static/approval_appset.png)

Now, the PR created in the **Update Release Repo** step will get merged in the **Merge PR** step.

![](/static/merge_appset.png)

Once the PR is merged, it will move to another **Approval Step** and ask for approval before proceeding to deploy the AppSet. Once approved, it will move to the **GitOps Sync step**, which deploys the application to a new environment and creates a new **Harness GitOps application**.

![](/static/synced_appset.png)


After the Application is Synced, you will see in the Fetch Linked App step the link to your GitOps Application that was created via ApplicationSet. Clicking the URL confirms that it has been deployed in the `prod1` environment.

![](/static/fetch_appset.png)


We have successfully deployed our Guestbook Application to a new environment using ApplicationSet via Harness PR Pipelines! 🚀

You can also view the complete pipeline YAML for above sample [here](/Application-Set/pipeline.yaml). 


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
10. [Pipeline Variable and Expression](https://developer.harness.io/docs/platform/variables-and-expressions/harness-variables/)
11. [ApplicationSet basics](https://developer.harness.io/docs/continuous-delivery/gitops/applicationsets/appset-basics)
12. [Fetch Linked Apps step](https://developer.harness.io/docs/continuous-delivery/gitops/pr-pipelines/#fetch-linked-apps-step)

