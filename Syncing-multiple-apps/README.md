## Overview

Now, that we have [fetched and sync single Gitops Application](/Fetch-App-Sync/README.md), let's move to a scenerio where we have multiple Gitops Application and we want to perform a sync with them using Harness Pipelines.

## Prerequisites

1. Ensure you have all the [prerequisites](/Fetch-App-Sync/README.md#prerequisites) before syncing multiple apps in Harness Pipeline.
2. Create multiple Gitops Application, follow this guide to guide tp create [Gitops Application](/Fetch-App-Sync/README.md#4-create-a-gitops-application). For this sample we are using [Gitops Multiple Application 1](/Gitops-multiple-apps/Application1/) and [Gitops Multiple Application 2](/Gitops-multiple-apps/Application2/) and the same Gitops Agent that we have used to sync the GitOps Application in this [sample](/Fetch-App-Sync/README.md#syncing-your-application).
   - In Path of Application1

3. We will be using same [Gitops Repository](/Fetch-App-Sync/README.md#3-create-a-gitops-repository) used in this [sample](https://github.com/harness-community/Gitops-Samples).



## Syncing multiple apps

### Option1: Manually selecting via UI

### Option2: Using matrix looping strategy to dynamically sync multiple Apps.