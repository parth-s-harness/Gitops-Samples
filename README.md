# GitOps Sample  

This repository contains various **GitOps Sample Pipelines** using **Harness GitOps** to help you automate deployments, sync applications, and handle failure scenarios efficiently. These samples cover different aspects of GitOps workflows, including syncing applications, automating PR-based deployments, handling failures, and setting up notifications for pipeline completion.

## 🚀 Samples Covered  

Below are the different GitOps workflows covered in this repository:

### 1️⃣ [Fetching App Details and Syncing App using Harness Pipeline](https://github.com/harness-community/Gitops-Samples/tree/gitops-1/Sample-1)  
Learn how to create a **Harness GitOps pipeline** that fetches application details and syncs it with the desired state defined in Git. 
This sample covers:  
✔ Setting up a **GitOps pipeline** in Harness  
✔ Fetching application details from the Git repository  
✔ Syncing the application using Harness GitOps  

---

### 2️⃣ [PR Pipeline: Update Values, Approve, Merge & Sync GitOps App](https://github.com/harness-community/Gitops-Samples/tree/main/Sample-2)  
This sample demonstrates a **PR-based GitOps pipeline**, where application updates go through a **Pull Request (PR) workflow** before being deployed. 
Key highlights:  
✔ Updating `values.yaml` in a GitOps repository  
✔ Raising a **PR to review changes** before deployment  
✔ Obtaining **manual approval** before merging  
✔ Merging the PR and **syncing the GitOps application**  

---

### 3️⃣ [GitOps PR Pipeline with Failure Strategy](https://github.com/harness-community/Gitops-Samples/tree/main/Sample-3)  
This sample builds on the PR pipeline by adding a **Failure Strategy** to ensure the application remains in a **healthy state** in case of sync failures.  
✔ If **GitOps sync fails**, the pipeline will **revert the merged PR**  
✔ This restores the application to a **previous healthy state**  
✔ After reverting, the pipeline **re-syncs** the GitOps application  

---

### 4️⃣ [Adding Notifications to the GitOps PR Pipeline](https://github.com/harness-community/Gitops-Samples/tree/main/Sample-4)  
This sample focuses on **adding notifications** to a GitOps PR pipeline. It ensures that users are informed when a pipeline completes successfully.  
✔ Configuring **email notifications**  
✔ Selecting **pipeline events** to trigger notifications  
✔ Adding **recipients and user groups**  

---

## 🎯 Why Use These GitOps Samples?  
✅ **Automate deployments** using GitOps principles  
✅ **Improve visibility** into GitOps sync failures and rollback strategies  
✅ **Enhance control** over application updates with PR-based workflows  
✅ **Get notified** when deployments complete successfully  

These examples provide a step-by-step guide to setting up Harness GitOps pipelines efficiently. Whether you're new to GitOps or looking to enhance your workflows, these samples will help streamline your deployments! 🚀
