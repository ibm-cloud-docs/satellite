---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-19"

keywords: satellite, hybrid, multicloud

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
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
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
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
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


# Logging for {{site.data.keyword.satelliteshort}}
{: #health}

Integrate {{site.data.keyword.satelliteshort}} and other {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.la_full}} to get a comprehensive view and tools to manage all your resources.
{: shortdesc}

By default, the following logs are automatically generated for your {{site.data.keyword.satelliteshort}} location:
* Messages and more detailed information from the IBM Monitoring component regarding certain alerts for issues with your location setup and host infrastructure
* Health check status of the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint

Logging for your {{site.data.keyword.satelliteshort}} location and for the {{site.data.keyword.cloud_notm}} services that run in your location must be set up separately. For example, to collect logs for your {{site.data.keyword.satelliteshort}} location setup, you enable a {{site.data.keyword.la_short}} instance to collect platform logs in the same region that your location is managed from. Then, to collect logs for a {{site.data.keyword.openshiftlong_notm}} cluster that runs in your {{site.data.keyword.satelliteshort}} location, you create a LogDNA agent in your cluster to automatically collect and forward pod logs to a {{site.data.keyword.la_short}} instance. Note that you can use the same LogDNA instance to collect logs for both your {{site.data.keyword.satelliteshort}} location and services that run in your {{site.data.keyword.satelliteshort}} location.

## Setting up LogDNA for {{site.data.keyword.satelliteshort}} location platform logs
{: #setup-logdna}

Forward and view logs that are automatically generated for your {{site.data.keyword.satelliteshort}} location setup in an {{site.data.keyword.la_full_notm}} instance that is enabled for platform-level logs.
{: shortdesc}

If you already have a {{site.data.keyword.la_short}} instance in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from, and the LogDNA instance is configured to collect platform logs, the logs that are generated for your {{site.data.keyword.satelliteshort}} location are automatically forwarded to this LogDNA instance. If you do not, first contact your account administrator to verify that a {{site.data.keyword.la_short}} instance with platform logs does not exist in the region. Otherwise, follow these steps to set up LogDNA for your {{site.data.keyword.satelliteshort}} location.

1. [Provision an {{site.data.keyword.la_full_notm}} instance](https://cloud.ibm.com/catalog/services/ibm-log-analysis-with-logdna){: external} in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.
2. [Enable the instance for platform-level log collection](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-config_svc_logs). Note that within one region, only one {{site.data.keyword.la_short}} instance can be enabled for platform logs collection.
3. In the **Logging** dashboard, click **View LogDNA** for your {{site.data.keyword.la_short}} instance.
4. In the LogDNA dashboard, review `satellite` logs for your location, which you can identify by searching for your location's ID.
5. Review how you can [search and filter logs in the LogDNA dashboard](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-view_logs).

<br />



## Setting up logging for clusters
{: #setup-clusters}

To understand and set up logging for {{site.data.keyword.openshiftshort}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the tutorials in the [{{site.data.keyword.la_full_notm}} documentation](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-kube#kube).
{: shortdesc}

You cannot currently use the {{site.data.keyword.openshiftlong_notm}} console or the observability plug-in CLI (`ibmcloud ob`) to enable logging for {{site.data.keyword.satelliteshort}} clusters. You must manually deploy LogDNA agents to your cluster to forward logs to {{site.data.keyword.la_short}}.
{: note}
