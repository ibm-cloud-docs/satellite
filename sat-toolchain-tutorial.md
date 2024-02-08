---


copyright:
  years: 2022, 2024
lastupdated: "2024-02-07"

keywords: satellite, toolchain, satellite config, Kubernetes, cluster

subcollection: satellite

content-type: tutorial
services: satellite, ContinuousDelivery
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}



# Deploying apps to clusters with {{site.data.keyword.contdelivery_short}} and {{site.data.keyword.satelliteshort}} Config
{: #sat_toolchain_tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite, ContinuousDelivery"}
{: toc-completion-time="30m"}

Deploy Kubernetes resources, like deployments, from your GitHub or GitLab repository to multiple clusters with {{site.data.keyword.contdelivery_full}} and {{site.data.keyword.satelliteshort}} Config. 
{: shortdesc}

## Objectives
{: #sat_toolchain_objectives}

In this tutorial, you learn how to create an open toolchain by using {{site.data.keyword.contdelivery_short}} to deploy your app to multiple clusters. You also learn how toolchains are implemented in the {{site.data.keyword.contdelivery_short}} service and how to deploy a simple web app by using a {{site.data.keyword.contdelivery_short}}-only toolchain template.

With {{site.data.keyword.satelliteshort}} Config, you create a configuration that specifies which Kubernetes resources you want to deploy to a cluster group. Then, set up a {{site.data.keyword.contdelivery_short}} pipeline to deploy these resources across a group of clusters whenever a change is made to the configuration.

The toolchain that is used in this tutorial implements standard DevOps practices around continuous delivery capabilities by managing your deployments in {{site.data.keyword.satelliteshort}} locations. After you create clusters and associate them with a {{site.data.keyword.satelliteshort}} cluster group, you can create a toolchain to change your deployment source code and push the change to the GitHub repo. When you push changes to your repo, the Tekton-based delivery pipeline automatically deploys the code to your clusters.

## Audience
{: #sat_toolchain_audience}

This tutorial is for administrators who are using {{site.data.keyword.contdelivery_short}} toolchains and {{site.data.keyword.satelliteshort}} Config to deploy resources for the first time.
{: shortdesc}

## Prerequisites
{: #toolchain-prereq}

Before you start this tutorial, make sure that you have the following resources in place.

* An [{{site.data.keyword.cloud_notm}} account](https://{DomainName}/registration){: external}. Depending on your {{site.data.keyword.cloud_notm}} account type, access to certain resources might be limited. Depending on your account plan limits, certain capabilities that are required by some deployment strategies might not be available. For more information about {{site.data.keyword.cloud_notm}} accounts, see [Setting up your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-account-getting-started) and [Upgrading your account](/docs/account?topic=account-upgrading-account).

* A {{site.data.keyword.satelliteshort}} cluster or an {{site.data.keyword.redhat_openshift_full}} cluster on {{site.data.keyword.cloud_notm}} that is registered with your {{site.data.keyword.satelliteshort}} location.

If you don't have a {{site.data.keyword.satelliteshort}} cluster, you can [register a {{site.data.keyword.redhat_openshift_notm}} cluster with {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-register-openshift-clusters).
{: tip}

* A [{{site.data.keyword.satelliteshort}} cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig) that contains the cluster that is required by the toolchain. The toolchain in this tutorial supports a {{site.data.keyword.satelliteshort}} cluster group that contains only {{site.data.keyword.redhat_openshift_notm}} clusters. 

* Image Pull Secrets. Make sure that you configure the image pull secrets that are required to deploy application images in your cluster namespace. For more information, see [understanding how to authorize your cluster to pull images from a private registry](/docs/openshift?topic=openshift-registry#cluster_registry_auth).

* A {{site.data.keyword.satelliteshort}} location for deploying the resources.

* An instance of the [{{site.data.keyword.contdelivery_short}}](/docs/ContinuousDelivery?topic=ContinuousDelivery-getting-started) service.

## Getting started
{: #welcome}
{: step}

Start to create a toolchain in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

1. Log in to [{{site.data.keyword.cloud_notm}} Toolchains](https://cloud.ibm.com/devops/toolchains) with your {{site.data.keyword.cloud_notm}} credentials. 
2. Select the Resource Group and Location for your toolchain.
3. Click **Create toolchain**.
4. On the **Create a Toolchain** page, select **{{site.data.keyword.satelliteshort}}** under **Filters** to display templates that are applicable for {{site.data.keyword.satelliteshort}}.
5. Select the **Deploy Kubernetes resources to multiple clusters** template.
6. Read through the information to understand how toolchains work and then click **Start**.

## Defining toolchain settings
{: #toolchain_settings}
{: step}

Define the name, region, and resource group for the toolchain. 
{: shortdesc}

1. Provide the name for the toolchain.
2. Select the region where you want the toolchain to be created.
3. Select a resource group where you want the toolchain to be created.
4. Click **Continue**. 

## Defining your source repository
{: #source_repository}
{: step}

Specify details of the source repository you want this toolchain to listen to. You can specify this info by using either standard configuration options or advanced configuration options.
{: shortdesc}

1. Select your source provider. Depending on the source provider you choose, options specific to that source provider are displayed. For example, you can select **Github Enterprise Whitewater** as the source provider.

2. Define the source repository.
     - To use a new repository for this toolchain, click **Create new source repository** and name the new repository. 
     - To use an existing repository for this toolchain, click **Use existing source repository** and provide the repository URL. Select the **Source Provider** and enter the URL of your repo, for example: `https://github.com/my-org/my-repo`. 
   
3. Click **Continue**.  
   
If you want to access more advanced source repository settings such as using a custom server and changing the cloning behavior, select **Switch to advanced configuration**. 
{: tip}

## Setting up your delivery pipeline
{: #pipeline_settings}
{: step}

Name the delivery pipeline for deploying your applications to multiple clusters in your {{site.data.keyword.satelliteshort}} cluster group.
{: shortdesc}

1. Enter the pipeline name, which is displayed in your toolchain. 

2. Click **Continue**.

## Providing an API key
{: #secrets}
{: step}

Provide the {{site.data.keyword.cloud_notm}} API key info that the toolchain can use to access your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

If you have an existing API key or multiple existing API keys, follow these steps.

1. Select the secret provider or providers that you use. You can select one or more.

2. Click **Continue**. 

3. Provide the necessary info for accessing your secret provider. 

If you don't have an existing API key, you can ignore the options on this page and click **Continue** to proceed to the next page, where you can create an {{site.data.keyword.cloud_notm}} API key and save it to a secret provider.

## Setting up {{site.data.keyword.satelliteshort}} Config
{: #satellite_config}
{: step}

Specify the settings that the {{site.data.keyword.satelliteshort}} Config tool can use when deploying resources.
{: shortdesc}


1. Provide {{site.data.keyword.cloud_notm}} API key. If you did not provide an existing {{site.data.keyword.cloud_notm}} API key on the previous page, you can create a new {{site.data.keyword.cloud_notm}} API key by clicking **New**. 
   
    You can save this key to a secret provider of your choice for reuse by selecting that option when you create the new key or by clicking the key icon.
    {: tip}

2. Define the namespace that you want your resources to be deployed to. This must be an existing namespace.

3. Specify the branch to deploy the resource to; for example, `v1`.

4. Provide the path to deploy the resource to; for example, `deployments`.

5. Select the cluster group to receive the resources that you are deploying.

6. Provide the displayed name of the {{site.data.keyword.satelliteshort}} configuration.

7. Click **Continue**. 

## Reviewing the summary
{: #summary}
{: step}

Review the summary and confirm to create your toolchain.
{: shortdesc}

- To change any parameters you provided earlier, click **Back**.

- To create the toolchain with the provided parameters, click **Create toolchain**.

## Testing the toolchain
{: #test_toolchain}
{: step}

When the toolchain is created, the toolchain Overview page is displayed. On this page, you can view the repositories, pipelines, and any {{site.data.keyword.cloud_notm}} tools that are associated with this toolchain. 
{: shortdesc}

Under **Repositories** is your source repository as well as additional repositories for your Tekton catalog and pipeline tasks. If you are comfortable with using Tekton, you can go to these Tekton repositories to make changes to your toolchain. 

Under **Delivery pipelines**, you can see the status of your pipeline runs. You can click the name of the pipeline to see details about your pipeline and set From this pipeline overview, you can set the triggers for your pipeline. Triggers define when the pipeline starts.

You can start the delivery pipeline in either of the following ways.

- Trigger the delivery pipeline manually by setting a manual trigger. 
- Push a commit to the deployment source repo as defined by a Git repository trigger. To set a Git repository trigger, see [Deploying a new version of your app from a different branch](#modify_trigger).

To start a manual pipeline run, follow these steps.

1. On the **Overview** of your toolchain under **Delivery pipelines**, select the delivery pipeline to view its status.
2. Click **Run pipeline**. 
3. Review the trigger properties to make sure that they match your configuration.
4. Click **Run** to run the toolchain and deploy your app.
5. Optional: Click the name of the run to see the status, logs, and details.
6. Optional: After the app is running, go to [{{site.data.keyword.satelliteshort}} Configuration](https://cloud.ibm.com/satellite/configuration) or run `ibmcloud sat config` command to see your {{site.data.keyword.satelliteshort}} Config resources.
    
## Verifying that the sample app is running
{: #verify_app}
{: step}

After your toolchain is set up and the {{site.data.keyword.deliverypipeline}} successfully completes, run the following steps to check your app.
{: shortdesc}

1. From the [Clusters](https://cloud.ibm.com/satellite/clusters){: external} page, click the cluster that you used to deploy your app.
2. Click **OpenShift web console**.
3. In the **Workloads** > **Pod** section, filter by the project or the cluster namespace, and verify that the pods are running.
4. In the **Networking** > **Routes** section, filter by the project or the cluster namespace, and locate the app URL.
5. Verify that the app is running by visiting the app URL with your browser.
6. Optional: You can view your {{site.data.keyword.satelliteshort}} configuration [in the console](https://cloud.ibm.com/satellite/configuration), or by running the `ibmcloud sat config ls` [command](/docs/satellite?topic=satellite-satellite-cli-reference#config-ls-cli).

## Deploying a new version of your app from a different branch
{: #modify_trigger}
{: step}

If you have multiple branches in your repository representing multiple versions of the app, you can deploy a different branch to see a new version of your app running. Follow these steps to modify the toolchain to deploy a different branch of your source repository.
{: shortdesc}

1. On the **Overview** of your toolchain, select the delivery pipeline to modify.
2. Click **Triggers**.
3. Click the action menu for the **commit-push** trigger.
4. Select **Edit**.
5. In the **Branch** field, select the branch you want to deploy. For example, `v2`.
6. In the **deployment-branch** line under **Properties**, click the **Edit** icon.
7. Modify the value of this property to the branch name you want to deploy. For example, `v2`.
8. Save the changes.
9. Check your app to see if it is running the new version by visiting the app URL with your browser.
10. Optional: View the new version of your configuration in the {{site.data.keyword.satelliteshort}} configurations [console](https://cloud.ibm.com/satellite/configuration) or by running running the `ibmcloud sat config get --config CONFIG` [command](/docs/satellite?topic=satellite-satellite-cli-reference#config-get-cli).

Now you can deploy Kubernetes resources to multiple clusters with a single toolchain.


