---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-19"

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



# About {{site.data.keyword.satelliteshort}}
{: #about}

Learn about {{site.data.keyword.satellitelong_notm}} terminology, service architecture, and components.
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

## {{site.data.keyword.satelliteshort}} concepts
{: #location-concept}

Review the following information to understand {{site.data.keyword.satelliteshort}} components and terminology.
{: shortdesc}

**{{site.data.keyword.satelliteshort}} location**: A {{site.data.keyword.satelliteshort}} location represents a data center that you can fill with your own infrastructure resources to run and extend {{site.data.keyword.cloud_notm}} services in your location.

The following diagram presents the concept of setting up your own {{site.data.keyword.satelliteshort}} locations in the Asia Pacific metro, and running the same application in your location as well as in {{site.data.keyword.cloud_notm}}.

![Concept overview of Satellite locations in Asia Pacific](/images/location-ov.png){: caption="Figure 1. A conceptual overview of setting up {{site.data.keyword.satelliteshort}} locations." caption-side="bottom"}

**{{site.data.keyword.satelliteshort}} control plane master**: When you create a location, a highly available control plane master is automatically created in one of the {{site.data.keyword.cloud_notm}} multizone metros that you selected during location creation. The control plane master securely connects your location to {{site.data.keyword.cloud_notm}} and stores the configuration of your location. If service updates are available, the control plane master automatically makes these updates available to the control plane worker nodes. All location metadata is automatically backed up to an {{site.data.keyword.cos_full_notm}} instance in your account.

**{{site.data.keyword.satelliteshort}} control plane worker**: After you create the location, you must attach hosts in groups of 3 to the location control plane as worker nodes. The minimum of 3 hosts for the location control plane is for demonstration purposes only, so plan to have at least 6 hosts to run production workloads. Then, the location control plane worker is ready to manage your location resources. Other components are also set up, such as IBM monitoring components to monitor the health of your location and hosts and automatically resolve issues. For more information, see the [service architecture](/docs/satellite?topic=satellite-service-architecture).

**{{site.data.keyword.satelliteshort}} Link**: A {{site.data.keyword.satelliteshort}} Link connector component is automatically created in the location control plane worker to securely connect the location back to the control plane master in {{site.data.keyword.cloud_notm}}. You can use the Link component to control and audit network traffic in and out of the location.

**Hosts**: To fill the {{site.data.keyword.satelliteshort}} location with your own infrastructure, you attach hosts such as on-prem bare metal, virtual machines in other cloud providers, edge computing devices, and more. All hosts must meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs). The hosts become the compute capacity in your location. You must assign at least three of these hosts as worker nodes to the {{site.data.keyword.satelliteshort}} control plane. All other hosts can be used to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters on your own infrastructure.

**{{site.data.keyword.openshiftlong_notm}} clusters**: You can use host capacity in your location to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters. Your hosts become the worker nodes of your cluster in a `upi` (or user-provided infrastructure) worker pool. The cluster master runs on and is managed from your {{site.data.keyword.satelliteshort}} control plane worker nodes. {{site.data.keyword.openshiftshort}} is automatically set up for these clusters, and you can run any Kubernetes workload that you want on the clusters.

**{{site.data.keyword.satelliteshort}} configurations**: To help you deploy and manage workloads across {{site.data.keyword.openshiftshort}} clusters in your location and in {{site.data.keyword.cloud_notm}}, you can use {{site.data.keyword.satelliteshort}} configurations. Simply upload a Kubernetes resource YAML file as a version to your configuration and subscribe the clusters where you want to deploy the resource. Your Kubernetes resources and subsequent version updates are automatically deployed to the subscribed clusters, and you also can view an inventory of all the Kubernetes resources that are managed by a {{site.data.keyword.satelliteshort}} configuration.

**{{site.data.keyword.cloud_notm}} services**: You can use host capacity in your location to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services on your own infrastructure. Simply select the service that you want to run in your location from the {{site.data.keyword.cloud_notm}} catalog, and start creating service instances in your location. Additionally, you use the same {{site.data.keyword.cloud_notm}} platform tools to manage identity and access, key management, certificate management, logging and monitoring, and other security and compliance controls for these services.
The master control plane is automatically spread across zones in the {{site.data.keyword.cloud_notm}} region for you. Unlike the zones in your {{site.data.keyword.satelliteshort}} location, these zones do not represent the zones in your infrastructure provider. Instead, these zones are managed by IBM and available to services that run in {{site.data.keyword.cloud_notm}} only.

<br />

## {{site.data.keyword.satelliteshort}} architecture
{: #architecture}

The following image shows the main components in {{site.data.keyword.satellitelong_notm}} and how they interact with each other.
{: shortdesc}

![{{site.data.keyword.satellitelong_notm}} service architecture](/images/sat_architecture.png)

|Master components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link tunnel|The {{site.data.keyword.satelliteshort}} Link tunnel creates a secure TLS connection to the {{site.data.keyword.satelliteshort}} Link connector that runs on the control plane worker nodes in your {{site.data.keyword.satelliteshort}} location. All communication that leaves and enters your location is proxied by the Link tunnel server, and is captured and monitored by IBM to detect malicious activity. To control the network connections between the workloads that run in your location and the {{site.data.keyword.cloud_notm}} multizone metro that manages your location, you can set up {{site.data.keyword.satelliteshort}} Link endpoints.  |
|IBM Monitoring|The IBM Monitoring component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, IBM Site Reliability Engineers are automatically informed for further investigation. |
|{{site.data.keyword.satelliteshort}} Config|With {{site.data.keyword.satelliteshort}} Config, you can consistently deploy Kubernetes resource configurations across {{site.data.keyword.openshiftlong}} clusters that run on the infrastructure of your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. You can monitor the health of these resources by using the location dashboard.|
{: summary="Overview of the {{site.data.keyword.satelliteshort}} control plane master components"}
{: class="simple-tab-table"}
{: caption="Overview of {{site.data.keyword.satelliteshort}} control plane master components." caption-side="top"}
{: #master-components}
{: tab-title="Master components"}
{: tab-group="satellite-components"}

|Worker node components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link connector|The {{site.data.keyword.satelliteshort}} link connector component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. All workloads that run in your location and that must connect to an app, service or server that runs in {{site.data.keyword.cloud_notm}} must send their requests to the {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector securely forwards your request to the {{site.data.keyword.satelliteshort}} Link tunnel server where the request is proxied and forwarded to the destination target. To enable DNS resolution between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}, you must create a {{site.data.keyword.satelliteshort}} Link endpoint. By default, all network traffic that enters and leaves your location is automatically captured to allow network monitoring and auditing. |
|IBM Monitoring|The IBM Monitoring component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, IBM Site Reliability Engineers are automatically informed for further investigation. |
|Cluster master|When you create a {{site.data.keyword.satelliteshort}} cluster in your location, the master of this cluster is deployed onto your {{site.data.keyword.satelliteshort}} control plane worker nodes to allow communication to {{site.data.keyword.cloud_notm}} and monitoring through IBM. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters).|
{: summary="Overview of the {{site.data.keyword.satelliteshort}} control plane worker node components"}
{: class="simple-tab-table"}
{: caption="Overview of {{site.data.keyword.satelliteshort}} control plane worker node components." caption-side="top"}
{: #worker-node-components}
{: tab-title="Worker node components"}
{: tab-group="satellite-components"}

<br />
