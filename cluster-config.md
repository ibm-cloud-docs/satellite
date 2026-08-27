---


copyright:
  years: 2020, 2026
lastupdated: "2026-08-27"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} Config 
{: #cluster-config}

With {{site.data.keyword.satelliteshort}} Config, create a configuration to deploy Kubernetes resources to {{site.data.keyword.openshiftlong_notm}} clusters in your {{site.data.keyword.satelliteshort}} location or {{site.data.keyword.cloud_notm}}.
{: shortdesc}

You can create either GitOps-based configurations or Direct Upload configurations.

## Understanding GitOps-based configurations
{: #satcon-understand-gitops}

Store Kubernetes resource definitions in a Git repository. Clusters automatically pull and deploy changes from your repository. Select the **GitOps** template when you create your configuration.
{: shortdesc}
  
![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon-gitops.svg){: caption="How GitOps configurations work" caption-side="bottom"}

A GitOps-based configuration involves the follow high-level steps.

1. Create a {{site.data.keyword.satelliteshort}} configuration and specify the repository, Git reference type, Git reference name, and path.
2. Create a subscription to associate the source repository of resource files with one or more cluster groups.
3. The resource files in the source repository is automatically deployed to the clusters that belong to the selected cluster group.
  
## Understanding Direct Upload configurations
{: #satcon-understand-direct-upload}

Upload YAML files manually or from your CI/CD process and manage them through the {{site.data.keyword.satelliteshort}} Config GUI or CLI. Select the **Direct upload** template when you create your configuration.
{: shortdesc}

![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon-direct-upload.svg){: caption="How Direct upload configurations work" caption-side="bottom"}

A Direct Upload configuration involves the follow high-level steps.  

1. Create a {{site.data.keyword.satelliteshort}} configuration and create versions by uploading Kubernetes resource files. Each Kubernetes resource file that you upload represents a version within the configuration.
2. Create a subscription to associate a version with one or more cluster groups. 
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
