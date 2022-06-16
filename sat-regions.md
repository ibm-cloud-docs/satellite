---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-16"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Supported {{site.data.keyword.cloud_notm}} locations
{: #sat-regions}

Review the {{site.data.keyword.cloud}} regions that you can choose from to manage your {{site.data.keyword.satelliteshort}} location. The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).
{: shortdesc}


Red Hat CoreOS is available only in the Dallas (`us-south`), Frankfurt (`eu-de`), Tokyo (`jp-tok`), London (`eu-gb`), and Washington D.C. (`us-east`) regions and for only {{site.data.keyword.redhat_openshift_notm}} version 4.9 and 4.10.
{: important}


| Geography | Country | Multizone Metro | Location | Region | Zone |
| --- | --- | --- | --- | --- | --- |
| Asia Pacific | Australia | Sydney | `syd` | `au-syd` | `au-syd-1`  \n `au-syd-2`  \n `au-syd-3`|
| Asia Pacific | Japan | Tokyo | `tok` | `jp-tok` | `jp-tok-1`  \n `jp-tok-2`  \n `jp-tok-3`|
| Asia Pacific | Japan | Osaka | `osa` | `jp-osa` | `jp-osa-1`  \n `jp-osa-2`  \n `jp-osa-3`|
| North America | Canada | Toronto | `tor`| `ca-tor`|`tor-1`  \n `tor-4`  \n `tor-5`|
| North America | United States | Dallas | `dal`| `us-south`|`us-south-1`  \n `us-south-2`  \n `us-south-3`|
| North America | United States | Washington DC | `wdc`| `us-east`|`us-east-1`  \n `us-east-2`  \n `us-east-3`|
| Europe | Germany | Frankfurt | `fra` | `eu-de` | `eu-de-1`  \n `eu-de-2`  \n `eu-de-3`|
| Europe | United Kingdom | London | `lon` | `eu-gb`|`eu-gb-1`  \n `eu-gb-2`  \n `eu-gb-3`|
| South America | Brazil | Sao Paulo | `sao` | `br-sao` | `br-sao-1`  \n `br-sao-2`  \n `br-sao-3` |
{: caption="Supported {{site.data.keyword.cloud_notm}} locations to manage your {{site.data.keyword.satelliteshort}} location." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the geography. The second column is the country. The third column is the multizone metro. The fourth column is the region. The fifth column has the possible zones."}

## About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}
{: #understand-supported-regions}

Review some frequently asked questions about why and how you choose an {{site.data.keyword.cloud_notm}} region to manage your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### Why is my location managed by an {{site.data.keyword.cloud_notm}} region?
{: #supported-regions-why-location}

Running {{site.data.keyword.cloud_notm}} services on your own infrastructure requires a secure connection to {{site.data.keyword.cloud_notm}}. The connection is controlled, monitored, and managed by {{site.data.keyword.IBM_notm}} to ensure that security and compliance standards for each of the services are met and to roll out updates to these services.

Every {{site.data.keyword.satelliteshort}} location is set up with a control plane that establishes the secure connection back to {{site.data.keyword.cloud_notm}}. The control plane consists of a highly available control plane master that runs in the {{site.data.keyword.cloud_notm}} region that you choose and that is controlled and managed by {{site.data.keyword.IBM_notm}}. The control plane worker nodes run on your own compute hosts that you added to your {{site.data.keyword.satelliteshort}} location.

{{site.data.keyword.IBM_notm}} uses this connection to monitor your {{site.data.keyword.satelliteshort}} location, automatically detect and resolve capacity issues, monitor malicious activity, and roll out updates to the {{site.data.keyword.cloud_notm}} services that you run on your infrastructure.

For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture).

### What {{site.data.keyword.cloud_notm}} multizone metro do I choose for my {{site.data.keyword.satelliteshort}} location?
{: #supported-regions-what-multizone-metro}

You can choose any of the supported {{site.data.keyword.cloud_notm}} region to manage your {{site.data.keyword.satelliteshort}} location. The metro determines where the master of your {{site.data.keyword.satelliteshort}} control plane runs. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture). To reduce latency between the {{site.data.keyword.cloud_notm}} region and your {{site.data.keyword.satelliteshort}} location, choose the region that is closest to where your physical compute infrastructure is.

### Is there a limitation where my compute hosts can reside?
{: #supported-regions-limitations}

Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. Hosts can be in your own on-premises data center, public cloud providers, or edge computing devices if they meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}.

### What about latency requirements?
{: #supported-regions-latency}

As you select your infrastructure provider, consider the following latency requirements. Environments that do not meet the latency requirements experience degraded performance.

Between {{site.data.keyword.cloud_notm}} and the location
:   The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).

Between hosts in your location
:   Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.

