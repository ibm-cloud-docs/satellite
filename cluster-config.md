---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-15"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} Config 
{: #cluster-config}

With {{site.data.keyword.satelliteshort}} Config, you create a configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

You can create either GitOps-based configurations or Direct Upload configurations.

## Understanding GitOps-based configurations
{: #satcon-understand-gitops}

Store your Kubernetes resource definitions in a Git repository. Clusters automatically pull changes directly from your repository and Git references. Your resources stay in your source repository and automatically deploy when the referenced source changes. To use this method, select the **GitOps** template when you create your configuration. If you are using GitOps-based configuration for the first time and want to see how this flow works, you can select the **GitOps - Sample application** template, which guides you to set up and deploy a sample application from the IBM Git repository to your clusters.  
{: shortdesc}
  
![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon-gitops.svg){: caption="Figure 1. How GitOps configurations work" caption-side="bottom"}

A GitOps-based configuration involves the follow high-level steps.

1. Create a {{site.data.keyword.satelliteshort}} configuration and specify the repository, Git reference type, Git reference name, and path.
2. Create a subscription to associate the source repository of resource files with one or more cluster groups.
3. The resource files in the source repository is automatically deployed to the clusters that belong to the selected cluster group.
  
## Understanding Direct Upload configurations
{: #satcon-understand-direct-upload}

Store your Kubernetes resource definitions in {{site.data.keyword.satelliteshort}} Config by either manually uploading your YAML files or pushing YAML files from your CI/CD process. Manage changes directly through {{site.data.keyword.satelliteshort}} Config GUI, CLI, or from your CI/CD process. To use this method, select the **Direct upload** template when you create your configuration.
{: shortdesc}

![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon-direct-upload.svg){: caption="Figure 2. How Direct upload configurations work" caption-side="bottom"}

A Direct Upload configuration involves the follow high-level steps.  

1. Create a {{site.data.keyword.satelliteshort}} configuration and create versions by uploading Kubernetes resource files. Each Kubernetes resource file that you upload represents a version within the configuration.
2. Create a subcription to associate a version with one or more cluster groups. 
3. The version of the Kubernetes resource file that is specified in the subscription is automatically deployed to the clusters that belong to the selected cluster group.  

## Key concepts for {{site.data.keyword.satelliteshort}} Config
{: #satcon-terminology}

Review the following key concepts that are used when you create a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

|Term|Description|
|---------|-------------------|
|Configuration|With {{site.data.keyword.satelliteshort}} configuration, you can upload or create Kubernetes resource YAML file versions to deploy to a group of clusters. The version that you upload is not applied to your cluster until you add a subscription to your configuration. |
|Subscription|A {{site.data.keyword.satelliteshort}} subscription specifies which Kubernetes resource is deployed to one or more cluster groups. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource that you specified and starts applying it across all clusters that belong to the cluster group. This process takes a few minutes to complete.  \n The clusters in your cluster group can exist in your {{site.data.keyword.satelliteshort}} or in {{site.data.keyword.cloud_notm}}. To include clusters that you run in {{site.data.keyword.cloud_notm}}, you must register the cluster with the {{site.data.keyword.satelliteshort}} Config component and install the {{site.data.keyword.satelliteshort}} Config agent on this cluster.|
|Version|A version represents a Kubernetes resource YAML file that you uploaded or manually created for a {{site.data.keyword.satelliteshort}} configuration. You can include any Kubernetes resource in your version and upload as many versions to a configuration as you like.  (This concept applies to **Direct Upload** configurations only.)|
|Cluster groups|A cluster group specifies a set of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and that are included in a {{site.data.keyword.satelliteshort}} configuration. Clusters that run in your location are automatically registered and can be added to a cluster group. Clusters that run in {{site.data.keyword.cloud_notm}} must be manually registered with the {{site.data.keyword.satelliteshort}} Config component before you can add them to a cluster group. |
{: caption="Satellite config key concepts." caption-side="bottom"}

