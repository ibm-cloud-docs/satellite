---

copyright:
  years: 2020, 2020
lastupdated: "2020-07-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}



# Supported {{site.data.keyword.cloud_notm}} regions
{: #sat-regions}

Review the {{site.data.keyword.cloud_notm}} regions that you can choose from to manage your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

|Geography|County|Location/ Metro|Region|Zone|
|---------|--------------|-------------|----------|-------------|
|North America|United States|Washington DC|`us-east`|`us-east-1` </br> `us-east-2` </br> `us-east-3`|
|Europe|United Kingdom|London|`eu-gb`|`eu-gb-1` </br> `eu-gb-2` </br> `eu-gb-3`|

## Understanding supported {{site.data.keyword.cloud_notm}} regions in {{site.data.keyword.satelliteshort}}
{: #understand-supported-regions}

Review some frequently asked questions about why and how you choose an {{site.data.keyword.cloud_notm}} region to manage your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

**Why is my location managed by an {{site.data.keyword.cloud_notm}} region?** </br>
Running {{site.data.keyword.cloud_notm}} services on your own infrastructure requires a secure connection to {{site.data.keyword.cloud_notm}} that is controlled, monitored, and managed by IBM to ensure that security and compliance standards for each of the services are met and to roll out updates to these services.

Every {{site.data.keyword.satelliteshort}} location is set up with a control plane that establishes the secure connection back to {{site.data.keyword.cloud_notm}}. The control plane consists of a highly available control plane master that runs in the {{site.data.keyword.cloud_notm}} region that you choose and that is controlled and managed by IBM. The control plane worker nodes run on your own compute hosts that you added to your {{site.data.keyword.satelliteshort}} location.

IBM uses this connection to monitor your {{site.data.keyword.satelliteshort}} location, automatically detect and resolve capacity issues, monitor malicious activity, and roll out updates to the {{site.data.keyword.cloud_notm}} services that you run on your infrastructure.

For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture).

**What {{site.data.keyword.cloud_notm}} region should I choose for my {{site.data.keyword.satelliteshort}} location?** </br>
You can choose any of the supported {{site.data.keyword.cloud_notm}} regions to manage your {{site.data.keyword.satelliteshort}} location. The region determines where the master of your {{site.data.keyword.satelliteshort}} control plane runs. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture). To reduce latency between the {{site.data.keyword.cloud_notm}} region and your {{site.data.keyword.satelliteshort}} location, choose the region that is closest to where your physical compute infrastructure resides.

**Is there a limitation where my compute hosts can reside?** </br>
No. Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. This can include hosts in your own on-premises data center, hosts that you set up with cloud providers, or edge computing devices as long as they meet the [minimum host requirements](/docs/satellite?topic=satellite-limitations#limits-host-system) for {{site.data.keyword.satelliteshort}}.
