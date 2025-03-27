# GitOps PR Pipeline with Notification  

## Overview  

Now that we have created a [PR pipeline](/Sample-2/README.md) that updates `values.yaml`, obtains approval before merging the PR, and then syncs the GitOps application with the merged changes, let's add a notification mechanism. This will send an email notification when the pipeline completes successfully.  

## Prerequisites  

Ensure you have all the [prerequisites](/Sample-2/README.md#prerequisites) before adding a notification mechanism to the GitOps PR Pipeline.  

## GitOps PR Pipeline  

Follow this guide to [create the GitOps PR Pipeline](/Sample-2/README.md#creating-a-pr-pipeline) before adding notifications.  

## Adding Notification to the GitOps Pipeline  

Once you have the [PR pipeline](/Sample-2/README.md#creating-a-pr-pipeline) ready, let's add a notification rule.  

1. In the **Pipeline Studio**, click on **Notify** and then click on **+ Notifications**.  
   ![](/static/add_notification.png)  

2. Under **Overview**, give your notification a name.  
   ![](/static/notify_users.png)  

3. Under **Pipeline Events**, select **Pipeline Success** to trigger a notification when the pipeline completes successfully.  
   ![](/static/notification_pipeline_events.png)  

4. Under **Notification Method**, choose **Email** as the Channel Type. Harness supports other Channel Types such as Microsoft Teams, Slack, PagerDuty, and Webhooks.  
   ![](/static/notification_method.png)   

5. Under **Type emails of your recipients**, add the email addresses or select user groups that should receive the notification. Individual users within a user group will receive the same notification.  
![](/static/email.png) 

6. Click **Finish** to apply the notification method.    

Learn more about [Notifications](https://developer.harness.io/docs/platform/notifications/notification-settings) in Harness.

You can also view the complete pipeline YAML for fetching app details and syncing the GitOps application [here](/Sample-4/pipeline.yaml).

You have successfully set up a notification rule to receive alerts when your GitOps PR pipeline completes! 🚀

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
10. [Notifications](https://developer.harness.io/docs/platform/notifications/notification-settings)





