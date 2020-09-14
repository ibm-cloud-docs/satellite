---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-14"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Supported {{site.data.keyword.cloud_notm}} locations
{: #sat-regions}

Review the {{site.data.keyword.cloud_notm}} multizone metros that you can choose from to manage your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

|Geography|Country|Multizone Metro|Region|Zone|
|---------|--------------|-------------|----------|-------------|
|North America|United States|Washington DC|`us-east`|`us-east-1` </br> `us-east-2` </br> `us-east-3`|
|Europe|United Kingdom|London|`eu-gb`|`eu-gb-1` </br> `eu-gb-2` </br> `eu-gb-3`|
{: caption="Supported {{site.data.keyword.cloud_notm}} locations to manage your {{site.data.keyword.satelliteshort}} location." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the geography. The second column is the country. The third column is the multizone metro. The fourth column is the region. The fifth column has the possible zones."}

## Understanding supported {{site.data.keyword.cloud_notm}} multizone metros in {{site.data.keyword.satelliteshort}}
{: #understand-supported-regions}

Review some frequently asked questions about why and how you choose an {{site.data.keyword.cloud_notm}} multizone metro to manage your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

**Why is my location managed by an {{site.data.keyword.cloud_notm}} multizone metro?** </br>
Running {{site.data.keyword.cloud_notm}} services on your own infrastructure requires a secure connection to {{site.data.keyword.cloud_notm}}. The connection is controlled, monitored, and managed by IBM to ensure that security and compliance standards for each of the services are met and to roll out updates to these services.

Every {{site.data.keyword.satelliteshort}} location is set up with a control plane that establishes the secure connection back to {{site.data.keyword.cloud_notm}}. The control plane consists of a highly available control plane master that runs in the {{site.data.keyword.cloud_notm}} multizone metro that you choose and that is controlled and managed by IBM. The control plane worker nodes run on your own compute hosts that you added to your {{site.data.keyword.satelliteshort}} location.

IBM uses this connection to monitor your {{site.data.keyword.satelliteshort}} location, automatically detect and resolve capacity issues, monitor malicious activity, and roll out updates to the {{site.data.keyword.cloud_notm}} services that you run on your infrastructure.

For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture).

**What {{site.data.keyword.cloud_notm}} multizone metro do I choose for my {{site.data.keyword.satelliteshort}} location?** </br>
You can choose any of the supported {{site.data.keyword.cloud_notm}} multizone metros to manage your {{site.data.keyword.satelliteshort}} location. The metro determines where the master of your {{site.data.keyword.satelliteshort}} control plane runs. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture). To reduce latency between the {{site.data.keyword.cloud_notm}} region and your {{site.data.keyword.satelliteshort}} location, choose the region that is closest to where your physical compute infrastructure is.

**Is there a limitation where my compute hosts can reside?** </br>
No. Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. Hosts can be in your own on-premises data center, other cloud providers, or edge computing devices if they meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) for {{site.data.keyword.satelliteshort}}.

