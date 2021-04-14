---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-14"

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

With {{site.data.keyword.satellitelong_notm}}, you can create a hybrid environment that brings the scalability and on-demand flexibility of public cloud services to the applications and data that run in your secure private cloud. To achieve this distributed cloud architecture, {{site.data.keyword.satelliteshort}} provides an API-based suite of tools that let you represent your on-premises data center, another cloud provider, or an edge network as a {{site.data.keyword.satelliteshort}} location. You fill the {{site.data.keyword.satelliteshort}} location with your own host machines that meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system). Then, these hosts provide the compute power to run {{site.data.keyword.cloud_notm}} services, such as workloads in managed {{site.data.keyword.openshiftshort}} clusters or data and artificial intelligence (AI) tools like {{site.data.keyword.watson}}.

Your {{site.data.keyword.satelliteshort}} location includes tools like {{site.data.keyword.satelliteshort}} Link and {{site.data.keyword.satelliteshort}} config to provide additional capabilities for securing and auditing network connections in your location and consistently deploying, managing, and controlling your apps and policies across clusters in the location.

For more information, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## Benefits of using {{site.data.keyword.satelliteshort}}
{: #about-benefits}

Review some of the key benefits for using {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

|Benefits|Description|
|----------|----------------------------------|
|Bring your own infrastructure to {{site.data.keyword.cloud_notm}}.|With {{site.data.keyword.satellitelong_notm}}, you can bring your own infrastructure that is in your on-premises data center, in other cloud providers, or in edge environments to {{site.data.keyword.cloud_notm}} by creating a {{site.data.keyword.satelliteshort}} location. With this setup, you can run your workloads in the physical location of your choice to meet legal requirements, compliance standards, data speeds, and network latency requirements while securely using {{site.data.keyword.cloud_notm}} services. For more information, see [Setting up {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-locations). |
|Securely access {{site.data.keyword.cloud_notm}} services.|Every {{site.data.keyword.satelliteshort}} location is securely connected to {{site.data.keyword.cloud_notm}} through an encrypted TLS tunnel that is provided with the {{site.data.keyword.satelliteshort}} Link component. By using this TLS tunnel, you can securely access {{site.data.keyword.cloud_notm}} services in your location and use them with the same security and compliance standards as in {{site.data.keyword.cloud_notm}}.  For more information, see [Connecting {{site.data.keyword.satelliteshort}} locations with services outside of locations by using endpoints](/docs/satellite?topic=satellite-link-location-cloud).  |
|Control and monitor network traffic to services.|With {{site.data.keyword.satelliteshort}} Link, you can control network access to apps, services, and servers that run in {{site.data.keyword.cloud_notm}}, other cloud providers, or in your on-premises data center. Configure these apps, services, and servers as sources for your {{site.data.keyword.satelliteshort}} endpoints. All network traffic that is sent through an endpoint is automatically captured and can be reviewed by the user.  |
|Consistently deploy Kubernetes resources across multiple locations.|Use a single dashboard to manage the deployment of Kubernetes resources across cloud, on-premises, and edge environments with {{site.data.keyword.satelliteshort}} Config and gain global visibility into your apps and Kubernetes operations. For more information, see [Deploying Kubernetes resources across clusters](/docs/satellite?topic=satellite-cluster-config). |
|Centralize your monitoring and logging.|{{site.data.keyword.satellitelong_notm}} is integrated with {{site.data.keyword.mon_full_notm}}, {{site.data.keyword.la_full_notm}}, and {{site.data.keyword.at_full_notm}}. You can view the metrics and logs for the apps that run in your location, the {{site.data.keyword.cloud_notm}} services that you use, or the events that happen in your location from a single location. |
{: caption="Reasons to use {{site.data.keyword.satellitelong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the benefit of using {{site.data.keyword.satellitelong_notm}}. The second column is a description of the benefit."}

For more information about {{site.data.keyword.satelliteshort}}, how it works and the service benefits, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

<br />

## {{site.data.keyword.satelliteshort}} components
{: #components}

Review the following {{site.data.keyword.satelliteshort}} components.
{: shortdesc}

| Component | Description |
| --- | --- |
| {{site.data.keyword.satelliteshort}} config | Based on the [Razee open source project](https://github.com/razee-io/Razee){: external}, {{site.data.keyword.satelliteshort}} config is a continuous delivery tool that you can use to consistently roll out versions of your apps across clusters in {{site.data.keyword.satelliteshort}} locations. For more information, see [Deploying {{site.data.keyword.openshiftshort}} resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config).|
| {{site.data.keyword.satelliteshort}} hosts | Hosts are machines that reside in your infrastructure provider, across at least three separate zones, and must [meet the minimum host requirements](/docs/satellite?topic=satellite-host-reqs). After attaching the hosts to a {{site.data.keyword.satelliteshort}} location, you assign the hosts to {{site.data.keyword.satelliteshort}}-enabled services such as clusters to provide the computing power to run the service workloads. For more information, see [Setting up {{site.data.keyword.satelliteshort}} hosts](/docs/satellite?topic=satellite-hosts). |
| {{site.data.keyword.satelliteshort}} Link | {{site.data.keyword.satelliteshort}} Link securely connects your {{site.data.keyword.satelliteshort}} location to the {{site.data.keyword.cloud_notm}} region that your location is managed from. All communication that leaves and enters your location is proxied by the Link tunnel server, and network traffic on this connection can be monitored and audited. For more information, see [Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints](/docs/satellite?topic=satellite-link-location-cloud).|
| {{site.data.keyword.satelliteshort}} location | A location is a representation of an environment in your infrastructure provider, such as an on-prem data center or cloud, that you want to bring {{site.data.keyword.cloud_notm}} services to so that you can run workloads in your own environment. You create the location based off at least three separate zones of your backing infrastructure environment, and attach host machines from across these zones in your infrastructure to the location. The [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} provides a single pane of glass to manage the workloads that run across the infrastructure in your locations. For more information, see [Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-infrastructure-plan) and [Setting up {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-locations).|
| {{site.data.keyword.satelliteshort}}-enabled service | An {{site.data.keyword.cloud_notm}} service that you can set up in a {{site.data.keyword.satelliteshort}} location, such as a {{site.data.keyword.openshiftlong_notm}} cluster. The service is managed from the {{site.data.keyword.cloud_notm}} region that your location is managed from, but you provide the infrastructure hosts to run the service's resources in your location. For more information, see [What {{site.data.keyword.cloud_notm}} services can I use with my {{site.data.keyword.satelliteshort}} location?](/docs/satellite?topic=satellite-faqs#supported-services). |
| {{site.data.keyword.satelliteshort}} storage | {{site.data.keyword.satelliteshort}} storage uses {{site.data.keyword.satelliteshort}} config to provide a convenient way to install various storage drivers in {{site.data.keyword.openshiftlong_notm}} clusters across your {{site.data.keyword.satelliteshort}} locations, by using storage templates. The storage templates are provided and tested by the vendors. After you install {{site.data.keyword.satelliteshort}} storage, your cluster users can use Kubernetes persistent volume claims (PVCs) to order and save their application data in persistent storage. For more information, see [Understanding {{site.data.keyword.satelliteshort}} storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov). |
{: caption="{{site.data.keyword.satelliteshort}} terms" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the component. The second column is a brief description of the component."}

<br />

## {{site.data.keyword.satelliteshort}} architecture
{: #architecture}

The following image shows the main components in {{site.data.keyword.satellitelong_notm}} and how they interact with each other.
{: shortdesc}

![{{site.data.keyword.satellitelong_notm}} service architecture](/images/sat_architecture.png)

|Master components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link tunnel|The {{site.data.keyword.satelliteshort}} Link tunnel server creates a secure TLS connection to the {{site.data.keyword.satelliteshort}} Link connector that runs on the control plane worker nodes in your {{site.data.keyword.satelliteshort}} location. One tunnel server is created for each cluster in your {{site.data.keyword.satelliteshort}} location, including clusters that run {{site.data.keyword.satelliteshort}}-enabled services and {{site.data.keyword.openshiftshort}} clusters that you create. All communication that leaves and enters your location is proxied by the Link tunnel server, and the connection metadata captured and monitored by IBM to detect malicious activity. To control the network connections between the workloads that run in your location and the {{site.data.keyword.cloud_notm}} multizone metro that manages your location, you can set up {{site.data.keyword.satelliteshort}} Link endpoints. For more information, see [Connecting your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} with endpoints](/docs/satellite?topic=satellite-link-location-cloud).  |
|IBM Monitoring|The IBM Monitoring component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, IBM Site Reliability Engineers are automatically informed for further investigation. |
|{{site.data.keyword.satelliteshort}} Config|With {{site.data.keyword.satelliteshort}} Config, you can consistently deploy Kubernetes resource configurations across {{site.data.keyword.openshiftlong}} clusters that run on the infrastructure of your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. You can monitor the health of these resources by using the location dashboard. For more information, see [Deploying Kubernetes resources across {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=satellite-cluster-config).|
{: summary="Overview of the {{site.data.keyword.satelliteshort}} control plane master components"}
{: class="simple-tab-table"}
{: caption="Overview of {{site.data.keyword.satelliteshort}} control plane master components." caption-side="top"}
{: #master-components}
{: tab-title="Master components"}
{: tab-group="satellite-components"}

|Worker node components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link connector|The {{site.data.keyword.satelliteshort}} link connector component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. All workloads that run in your location and that must connect to an app, service or server that runs in {{site.data.keyword.cloud_notm}} must send their requests to the {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector securely forwards your request to the {{site.data.keyword.satelliteshort}} Link tunnel server where the request is proxied and forwarded to the destination target. To enable DNS resolution between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}, you must create a {{site.data.keyword.satelliteshort}} Link endpoint. For more information, see [Connecting your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} with endpoints](/docs/satellite?topic=satellite-link-location-cloud). By default, all network traffic that enters and leaves your location is automatically captured to allow network monitoring and auditing. |
|IBM Monitoring|The IBM Monitoring component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, IBM Site Reliability Engineers are automatically informed for further investigation. |
|Cluster master|When you create a {{site.data.keyword.satelliteshort}} cluster in your location, the master of this cluster is deployed onto your {{site.data.keyword.satelliteshort}} control plane worker nodes to allow communication to {{site.data.keyword.cloud_notm}} and monitoring through IBM. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters).|
{: summary="Overview of the {{site.data.keyword.satelliteshort}} control plane worker node components"}
{: class="simple-tab-table"}
{: caption="Overview of {{site.data.keyword.satelliteshort}} control plane worker node components." caption-side="top"}
{: #worker-node-components}
{: tab-title="Worker node components"}
{: tab-group="satellite-components"}

<br />
