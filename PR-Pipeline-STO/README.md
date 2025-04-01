## Overview
This PR pipeline extends the existing [PR pipeline](/Failure-Strategy-PR-Pipeline/README.md) by adding a **Security Testing Orchestration (STO) stage** to check for vulnerabilities:
   - Scans the deployed image for vulnerabilities.
   - Scans the Git repository for vulnerabilities.
   - If vulnerabilities are found, performs a rollback.

## Prerequisites

Make sure you have all the [prerequisites](/PR-Pipeline/README.md#prerequisites) before adding a failure strategy to the GitOps PR Pipeline.

## GitOps PR Pipeline with Failure Strategy 

Follow this guide to [create a GitOps PR Pipeline with Failure Strategy](/Failure-Strategy-PR-Pipeline/README.md#adding-a-failure-strategy-to-the-gitops-pipeline) before adding a failure strategy.

## Adding STO (Security Testing Orchestration) Stage to Check for Vulnerabilities

Check out the [Interactive Guide](https://app.tango.us/app/embed/fe5dbf35-9569-48ec-8a3d-b63cda8a5803) to create a GitOps PR Pipeline for this sample.

1. Click on **Add Stage** and **Select Stage Type** as **Security**.
2. Enter **Stage Name** of your security stage, enable **Clone Codebase**, and **Select Git Provider** as **Third-party Git Provider**. Select the **Git Connector** to authenticate with the repo containing your release repo manifest. Learn more about creating a [GitHub Connector](https://developer.harness.io/docs/platform/connectors/code-repositories/ref-source-repo-provider/git-hub-connector-settings-reference/). For this sample, we are using the same [GitOps Sample](https://github.com/harness-community/Gitops-Samples) repo.

   ![](/static/sto_stage.png)

3. Click on **Set Up Stage**.
4. In **Infrastructure**, select the infrastructure where you want your builds to run. For this sample, we are using **Harness Cloud**.
5. In **Platform**, select the Operating System and Architecture.

   ![](/static/sto_infra.png)

6. Now, we are going to add **Security Test**. Harness supports various [Security Tests](https://developer.harness.io/docs/category/scanner-configurations).

   For this sample, we are using **Aqua Trivy** for scanning the primary artifact and **Gitleaks** for scanning the Git repository.

7. From **Step Library**, select **Aqua Trivy**. Under **Configure Aqua Trivy**:
   - Add a name for the **Aqua Trivy** step.
   - Select **Scan Mode** as **Orchestration**.
   - Under **Container Image**:
     - Select **Registry Type** (Harness, Third Party, Local). For this sample, choose **Third Party**.
     - Under **Type**, select **Docker v2**.
     - Under **Domain**, add `docker.io`.
     - Under **Name**, add your image (`krishi0408/python-helm-native-canary-blue`).
     - Under **Tag**, use `latest`.
     - Add **Access Id** and **Access Token** if using a private image.
     - Enable **Generate SBOM** if needed.
     - Under **Fail on Severity**, select **High**.
     - Click on **Apply Changes**.

Check out [Aqua Trivy step configuration](https://developer.harness.io/docs/security-testing-orchestration/sto-techref-category/trivy/aqua-trivy-scanner-reference) for more details.

8. From **Step Library**, select **Gitleaks**. Under **Configure Gitleaks**:
   - Add a name for the **Gitleaks** step.
   - Select **Scan Mode** as **Orchestration**.
   - Under **Fail on Severity**, choose **High**.
   - Click on **Apply Changes**.

   *Please note: We are adding the Gitleaks step in parallel.*
   
   ![](/static/sto_parallel.png)

Check out [Gitleaks step configuration](https://developer.harness.io/docs/security-testing-orchestration/sto-techref-category/gitleaks-scanner-reference/) for more details.


9. Under **Advanced**, go to **Failure Strategy**, click on **All Errors**, and under **Perform Action**, select **Rollback Pipeline**. Learn more about [Failure Strategy](https://developer.harness.io/docs/platform/pipelines/failure-handling/define-a-failure-strategy-on-stages-and-steps) in Harness.

10. Click on **Save**.

11. Click on **Run Pipeline**.

Once you run the pipeline, after the **Deploy** stage, the **Security Test** will start running in parallel.

![](/static/sto-parallel_running.png)

In the screenshot above, the **Aqua Trivy** step is failing. The logs show a high-severity issue, and we have set the pipeline to fail if the severity is **High**.

![](/static/sto_logs.png)

The Gitleaks security step passed, as there are no vulnerabilities in the repository.

We have added a **Failure Strategy**: if there are any failures, the pipeline will **Rollback**.

So after the security step is completed, the rollback begins.

You have successfully added a Security Step to your GitOps workflow! 🚀

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
12. [Security Tests](https://developer.harness.io/docs/category/scanner-configurations)
13. [Aqua Trivy step configuration](https://developer.harness.io/docs/security-testing-orchestration/sto-techref-category/trivy/aqua-trivy-scanner-reference)
14. [Gitleaks step configuration](https://developer.harness.io/docs/security-testing-orchestration/sto-techref-category/gitleaks-scanner-reference/)