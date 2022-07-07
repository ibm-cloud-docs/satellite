---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} Config
{: #cluster-config}

With {{site.data.keyword.satelliteshort}} Config, you create a configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## How {{site.data.keyword.satelliteshort}} configurations work
{: #satcon-flow}

The following image shows how you can use a {{site.data.keyword.satelliteshort}} configuration to consistently deploy Kubernetes resources across {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon.png){: caption="Figure 1. How Satellite configurations work" caption-side="bottom"}

1. Create a {{site.data.keyword.satelliteshort}} configuration and upload Kubernetes resource files to it. Each Kubernetes resource file that you upload represents a version within the configuration.
2. Create a {{site.data.keyword.satelliteshort}} subscription and specify which Kubernetes resource file version is deployed to a group of clusters.
3. The version of the Kubernetes resource file that is specified in the subscription is automatically deployed to the clusters that belong to the selected cluster group. The clusters in the cluster group can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.

## Key concepts for {{site.data.keyword.satelliteshort}} Config
{: #satcon-terminology}

Review the following key concepts that are used when you create a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

|Term|Description|
|---------|-------------------|
|Configuration|With {{site.data.keyword.satelliteshort}} configuration, you can upload or create Kubernetes resource YAML file versions to deploy to a group of clusters. The version that you upload is not applied to your cluster until you add a subscription to your configuration. For more information, see [Creating {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-satcon-create).|
|Version|A version represents a Kubernetes resource YAML file that you uploaded or manually created for a {{site.data.keyword.satelliteshort}} configuration. You can include any Kubernetes resource in your version and upload as many versions to a configuration as you like. For help developing a Kubernetes YAML file, see [Developing apps to run on {{site.data.keyword.redhat_openshift_notm}}](/docs/openshift?topic=openshift-openshift_apps). To create a version, see [Creating {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-satcon-create). |
|Subscription|A {{site.data.keyword.satelliteshort}} subscription is created for a {{site.data.keyword.satelliteshort}} configuration and specifies which version of the Kubernetes resource that you uploaded is deployed to one or more cluster groups. To create a subscription, see [Creating {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-satcon-create).  \n The clusters in your cluster group can exist in your {{site.data.keyword.satelliteshort}} or in {{site.data.keyword.cloud_notm}}. To include clusters that you run in {{site.data.keyword.cloud_notm}}, you must [register the cluster](/docs/satellite?topic=satellite-satcon-existing) with the {{site.data.keyword.satelliteshort}} Config component and install the {{site.data.keyword.satelliteshort}} Config agent on this cluster.|
|Cluster groups|A cluster group specifies a set of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and that are included in a {{site.data.keyword.satelliteshort}} configuration. Clusters that run in your location are automatically registered and can be [added to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups). Clusters that run in {{site.data.keyword.cloud_notm}} must be [manually registered](/docs/satellite?topic=satellite-satcon-existing) with the {{site.data.keyword.satelliteshort}} Config component before you can add them to a cluster group. |
{: caption="Satellite config key concepts." caption-side="top"}

