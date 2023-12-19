---

copyright:
  years: 2022, 2023
lastupdated: "2023-12-19"

keywords: satellite, satellite config, Kubernetes, cluster

subcollection: satellite

content-type: tutorial
services: satellite
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}


# Deploying apps to clusters with {{site.data.keyword.satelliteshort}} Config
{: #begin-sat-config-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite"}
{: toc-completion-time="30m"}

Set up a {{site.data.keyword.satelliteshort}} configuration to automatically deploy your Kubernetes resources to multiple clusters.
{: shortdesc}

## Objectives
{: #begin-sat-config-tutorial-objectives}

With {{site.data.keyword.satelliteshort}} Config UI or CLI, you create a configuration that specifies which Kubernetes resources you want to deploy to a cluster group. Then, the system can automatically deploy these resources across the clusters.

In this tutorial, you create a configuration to deploy a food ordering app, which includes several micro services such as front end, back end, route, and database. After you deploy the version 1 of the food delivery app, you then create a version of app which changes the appearance to use a dark theme.

## Audience
{: #begin-sat-config-tutorial-audience}

This tutorial is for Location administrators who are using {{site.data.keyword.satelliteshort}} Config to deploy resources for the first time.
{: shortdesc}

## Prerequisites
{: #begin-sat-config-tutorial-prereq}

Before you start this tutorial, make sure that you have the following resources in place.

* A {{site.data.keyword.satelliteshort}} location for deploying the resources. 

* A {{site.data.keyword.satelliteshort}} cluster or a {{site.data.keyword.redhat_openshift_full}} cluster on {{site.data.keyword.cloud_notm}} that is registered with your {{site.data.keyword.satelliteshort}} location. 

* A {{site.data.keyword.satelliteshort}} cluster group that contains the clusters you want to deploy resources to.

* Make sure that {{site.data.keyword.satelliteshort}} Config has admin access to your clusters. You can either enable admin access when creating the cluster or [grant {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig) after you create the cluster. 



## Beginner tutorial: Deploying a hello world app
{: #begin-sat-config-tutorial-deploy-app-ui}
{: ui}

Follow these steps if you are new to {{site.data.keyword.satelliteshort}} Config and want to try out the process by deploying some sample resources in an existing IBM Git repo to your clusters.

1. Log in to the [{{site.data.keyword.satelliteshort}} Config UI](https://cloud.ibm.com/satellite/configuration) with your {{site.data.keyword.cloud_notm}} credentials. 

1. Click **Create configuration**.

1. Select the **GitOps - Sample application** template and follow the prompts.

## Advanced tutorial: Deploying an app and updating it with new versions
{: #begin-sat-config-tutorial-advanced-helloworld-ui}
{: ui}

Before you begin, delete the subscription created previously in the [Beginner tutorial](/docs/satellite?topic=satellite-begin-sat-config-tutorial&interface=ui#begin-sat-config-tutorial-deploy-app-ui).

1. Log in to GitHub and fork `https://github.com/IBM/satellite-config-example` including all branches. Make sure the **Copy the main branch only** option is not selected when you create the fork.

2. Log in to the [{{site.data.keyword.satelliteshort}} Config UI](https://cloud.ibm.com/satellite/configuration){: external} with your {{site.data.keyword.cloud_notm}} credentials. 

3. Click **Create configuration**.

4. Select the **GitOps** template.

5. On the **Configuration** page:
  
    1. Enter `hello-world-gitops` as the configuration name
  
    2. Select `GitHub` as the provider.
  
    3. Click **Next**.
  
6. On the **Subscription** page:
  
    1. For repository URL, enter the URL of your fork.
  
    2. For Git ref type, select `Branch`.
  
    3. For branch name, enter `config-sample-prod`.
  
    4. For path, enter `deployments/*.yaml`.
  
    5. Select a cluster group or groups to deploy to.
  
    6. Click **Next**.

7. On the **Summary** page, confirm that the displayed information is correct and then click **Complete**. 

8. Observe the hello world sample version 1.0 being deployed to the clusters. You can click the configuration name to see the rollout status. This sample creates a Hello world app in the `satellite-config-sample` namespace, which serves a simple web page with the current version of the app. Browse to the route of the app to see `Hello world 1.0!`. You can get the URL of the app from the OpenShift web console or by using the following command.
    ```sh
    oc get routes helloworld-route -n satellite-config-sample -o go-template --template='http://{{.spec.host}}{{.spec.path}}{{println}}'`.
    ```
    {: pre}

9. In GitHub, create a pull request from branch `config-sample-dev` to `config-sample-prod`.
  
10. Merge the pull request created in the previous step.

11. In a few minutes, observe the hello world sample version 1.1 being deployed to the clusters.


    

