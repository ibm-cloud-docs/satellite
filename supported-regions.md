---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-29"

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



# Supported {{site.data.keyword.cloud_notm}} locations
{: #sat-regions}

Review the {{site.data.keyword.cloud}} regions that you can choose from to manage your {{site.data.keyword.satelliteshort}} location. Your infrastructure provider must have a low latency connection of less than 100 milliseconds (`< 100ms`) round-trip time (RTT) between the hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location and the {{site.data.keyword.cloud_notm}} region that the location is managed from. For more information, see [Latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-reqs#host-latency-mzr).
{: shortdesc}

|Geography|Country|Multizone Metro|Location|Region|Zone|
|---------|-------|---------------|--------|------|----|
|North America|United States|Washington DC| `wdc`| `us-east`|`us-east-1` </br> `us-east-2` </br> `us-east-3`|
|Europe|United Kingdom|London| `lon` | `eu-gb`|`eu-gb-1` </br> `eu-gb-2` </br> `eu-gb-3`|
{: caption="Supported {{site.data.keyword.cloud_notm}} locations to manage your {{site.data.keyword.satelliteshort}} location." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the geography. The second column is the country. The third column is the multizone metro. The fourth column is the region. The fifth column has the possible zones."}

## About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}
{: #understand-supported-regions}

Review some frequently asked questions about why and how you choose an {{site.data.keyword.cloud_notm}} region to manage your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

**Why is my location managed by an {{site.data.keyword.cloud_notm}} region?**

Running {{site.data.keyword.cloud_notm}} services on your own infrastructure requires a secure connection to {{site.data.keyword.cloud_notm}}. The connection is controlled, monitored, and managed by IBM to ensure that security and compliance standards for each of the services are met and to roll out updates to these services.

Every {{site.data.keyword.satelliteshort}} location is set up with a control plane that establishes the secure connection back to {{site.data.keyword.cloud_notm}}. The control plane consists of a highly available control plane master that runs in the {{site.data.keyword.cloud_notm}} region that you choose and that is controlled and managed by IBM. The control plane worker nodes run on your own compute hosts that you added to your {{site.data.keyword.satelliteshort}} location.

IBM uses this connection to monitor your {{site.data.keyword.satelliteshort}} location, automatically detect and resolve capacity issues, monitor malicious activity, and roll out updates to the {{site.data.keyword.cloud_notm}} services that you run on your infrastructure.

For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture).

**What {{site.data.keyword.cloud_notm}} multizone metro do I choose for my {{site.data.keyword.satelliteshort}} location?**

You can choose any of the supported {{site.data.keyword.cloud_notm}} region to manage your {{site.data.keyword.satelliteshort}} location. The metro determines where the master of your {{site.data.keyword.satelliteshort}} control plane runs. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture). To reduce latency between the {{site.data.keyword.cloud_notm}} region and your {{site.data.keyword.satelliteshort}} location, choose the region that is closest to where your physical compute infrastructure is.

**Is there a limitation where my compute hosts can reside?**

Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. Hosts can be in your own on-premises data center, other cloud providers, or edge computing devices if they meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) for {{site.data.keyword.satelliteshort}}.

**What about latency requirements?**

As you select your infrastructure provider, consider the following latency requirements. Environments that do not meet the latency requirements experience degraded performance.
* **Between {{site.data.keyword.cloud_notm}} and the location**: Your infrastructure provider must have a low latency connection of less than 100 milliseconds (`< 100ms`) round-trip time (RTT) between the hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location and the {{site.data.keyword.cloud_notm}} region that the location is managed from. For more information, see [Latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-reqs#host-latency-mzr).
* **Between hosts in your location**: Your host infrastructure setup must have a low latency connection of less than 10 milliseconds (`< 10ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane and the hosts that are used for other resources in the location, like clusters or services. For example, in cloud providers such as AWS, this setup typically means that the all of the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`.
