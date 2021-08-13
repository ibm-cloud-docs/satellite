---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Understanding {{site.data.keyword.satelliteshort}} Config
{: #cluster-config}

With {{site.data.keyword.satelliteshort}} Config, you create a configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## How {{site.data.keyword.satelliteshort}} configurations work
{: #satcon-flow}

The following image shows how you can use a {{site.data.keyword.satelliteshort}} configuration to consistently deploy Kubernetes resources across {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon.png)

1. A user creates a {{site.data.keyword.satelliteshort}} configuration and uploads Kubernetes resource files to it. Each Kubernetes resource file that you upload represents a version within the configuration.
2. The user creates a {{site.data.keyword.satelliteshort}} subscription and specifies which Kubernetes resource file version is deployed to a group of {{site.data.keyword.openshiftlong_notm}} clusters.
3. The version of the Kubernetes resource file that is specified in the subscription is automatically deployed to the {{site.data.keyword.openshiftlong_notm}} clusters that belong to the selected cluster group. The clusters in the cluster group can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.

## Key concepts
{: #satcon-terminology}

Review the following key concepts that are used when you create a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

|Term|Description|
|---------|-------------------|
|Configuration|A {{site.data.keyword.satelliteshort}} configuration lets you upload or create Kubernetes resource YAML file versions that you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters. The version that you upload is not applied to your cluster until you add a subscription to your configuration. For more information, see [Creating {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui).|
|Version|A version represents a Kubernetes resource YAML file that you uploaded or manually created for a {{site.data.keyword.satelliteshort}} configuration. You can include any Kubernetes resource in your version and upload as many versions to a configuration as you like. For help developing a Kubernetes YAML file, see [Developing apps to run on {{site.data.keyword.openshiftshort}}](/docs/openshift?topic=openshift-openshift_apps). To create a version, see [Creating {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui). |
|Subscription|A {{site.data.keyword.satelliteshort}} subscription is created for a {{site.data.keyword.satelliteshort}} configuration and specifies which version of the Kubernetes resource that you uploaded is deployed to one or more cluster groups. To create a subscription, see [Creating {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui).<br><br>The {{site.data.keyword.openshiftlong_notm}} clusters in your cluster group can exist in your {{site.data.keyword.satelliteshort}} or in {{site.data.keyword.cloud_notm}}. To include {{site.data.keyword.openshiftlong_notm}} clusters that you run in {{site.data.keyword.cloud_notm}}, you must [register the cluster](#existing-openshift-clusters) with the {{site.data.keyword.satelliteshort}} Config component and install the {{site.data.keyword.satelliteshort}} Config agent on this cluster.|
|Cluster groups|A cluster group specifies a set of {{site.data.keyword.openshiftlong_notm}} clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and that are included in a {{site.data.keyword.satelliteshort}} configuration. {{site.data.keyword.openshiftlong_notm}} clusters that run in your location are automatically registered and can be [added to a cluster group](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-groups). Clusters that run in {{site.data.keyword.cloud_notm}} must be [manually registered](#existing-openshift-clusters) with the {{site.data.keyword.satelliteshort}} Config component before you can add them to a cluster group. |
{: caption="{{site.data.keyword.satelliteshort}} Config key concepts." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the term for the key concept. The second column is a description of the key concept."}

