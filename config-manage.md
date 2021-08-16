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


# Managing your {{site.data.keyword.satelliteshort}} Config resources
{: #satcon-manage}

To create and roll out new versions of your applications, update your {{site.data.keyword.satelliteshort}} Config configurations and subscriptions. You can also use {{site.data.keyword.satelliteshort}} Config to review an inventory of all the resources that are managed by your configurations across clusters.
{: shortdesc}

## Reviewing resources that are managed by {{site.data.keyword.satelliteshort}} Config
{: #satconfig-resources}

You can use {{site.data.keyword.satelliteshort}} Config to review the Kubernetes resources that run in your registered clusters.
{: shortdesc}

Before you begin, make sure that you have the following permissions. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
-  The **Administrator** platform role, **Reader** service role, or **Manager** service role in {{site.data.keyword.cloud_notm}} IAM for the **Resource** resource type in {{site.data.keyword.satellitelong_notm}}.
-  The appropriate permissions to enable the {{site.data.keyword.satelliteshort}} Config watchkeeping capability, such as one of the following options.
    * The [permissions](/docs/satellite?topic=satellite-satcon-create) to create a configuration version and subscribe clusters to the version.
    * The **Writer** service role in {{site.data.keyword.cloud_notm}} IAM to the **Kubernetes Service** clusters that you want to watch resources for.

### Enabling watchkeeper collection methods
{: #satconfig-enable-watchkeeper}

Review the [watchkeeper collection methods](https://github.com/razee-io/WatchKeeper#collection-methods){: external} to decide how to set up watchkeeping for your resources. Common use cases include,

#### Watch all of the resources that my {{site.data.keyword.satelliteshort}} subscription creates
{: #satconfig-enable-watchkeeper-all}

1. [Add a configmap](https://github.com/razee-io/WatchKeeper#watch-by-resource){: external} to the YAML file of your {{site.data.keyword.satelliteshort}} configuration version. 
2. In the `metadata.namespace` field of the configmap, set the value to `razeedeploy`.
3. In the `data` section of the configmap, add all of the resources that you want {{site.data.keyword.satelliteshort}} Config to watch.
4. Subscribe your clusters to this version from the [console](/docs/satellite?topic=satellite-satcon-create#create-satconfig-ui) or [CLI](/docs/satellite?topic=satellite-satcon-create#create-satconfig-cli).

#### Watch a particular resource in my {{site.data.keyword.satelliteshort}} Config version
{: #satconfig-enable-watchkeeper-specific}

1. In the `metadata.labels` field of the Kubernetes resource in your {{site.data.keyword.satelliteshort}} Config version, set the value to `razee/watch-resource=lite`.
2. Subscribe your clusters to this version from the [console](/docs/satellite?topic=satellite-satcon-create#create-satconfig-ui) or [CLI](/docs/satellite?topic=satellite-satcon-create#create-satconfig-cli).

#### Watch a particular resource that I label in my cluster
{: #satconfig-enable-watchkeeper-label}

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).
2. Individually label the resource that you want {{site.data.keyword.satelliteshort}} Config to watch. For example, the following command watches a deployment that is called `nginx`.

    ```
    kubectl label deployment nginx razee/watch-resource=lite
    ```
    {: pre}
            
After you enable watchkeeping for a resource, wait about an hour for the resources to display.

### Reviewing the resources from {{site.data.keyword.satelliteshort}} Config
{: #satconfig-review-resources}

**From the console**

You can review resources in several areas in the console as follows.

* From the [**Cluster resources** page](https://cloud.ibm.com/satellite/resources){: external}. 
* From the [**Configurations** page](https://cloud.ibm.com/satellite/configuration){: external}, click a configuration. Then, click a subscription and review the **Resources** tab.
* From the [**Clusters** page](https://cloud.ibm.com/satellite/clusters){: external}, click a cluster. Then, review the **Resources** tab.

**From the CLI** 

Use the **`ibmcloud sat resource ls`** [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls) and its options to list resources. To view the details of a particular resource, use the `ibmcloud sat resource get` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get).



