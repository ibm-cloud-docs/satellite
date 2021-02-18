---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-18"

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

Review the following diagram and explanation to understand how {{site.data.keyword.satelliteshort}} concepts build off your infrastructure to bring {{site.data.keyword.cloud_notm}} to your locations.
{: shortdesc}

Review the following parts of your {{site.data.keyword.satellitelong_notm}} environment.
* [Your infrastructure environment](#concept-infra)
* [Your {{site.data.keyword.satelliteshort}} locations](#concept-satloc)
* [{{site.data.keyword.cloud_notm}} resources](#concept-ibm-cloud)

![Concept overview of Satellite locations](/images/satellite-ov.png){: caption="Figure 1. A conceptual overview of setting up {{site.data.keyword.satelliteshort}} locations that are based on your infrastructure." caption-side="bottom"}

### Infrastructure
{: #concept-infra}

Your {{site.data.keyword.satelliteshort}} location starts with your actual infrastructure that runs somewhere outside {{site.data.keyword.cloud_notm}}, such as another cloud provider or on-prem. Your infrastructure provides the basis for the hosts and zones that you use to build out your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

**Hosts**

The host instances in your infrastructure provider become the compute hosts to run the resources in your {{site.data.keyword.satelliteshort}} location. To add hosts to {{site.data.keyword.satelliteshort}}, the hosts must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) and any [provider-specific requirements](/docs/satellite?topic=satellite-providers), such as running the RHEL 7 operating system.

**Zones**

Your infrastructure environment might have multiple zones to increase [high availability](/docs/satellite?topic=satellite-ha). Typically, you have three zones that are physically separate to spread out hosts evenly across zones. For example, your cloud provider might have three different zones within the same region, or you might use three racks with three separate networking and power supply systems in an on-prem environment. You can represent these zone names when you create your {{site.data.keyword.satelliteshort}} location.

### {{site.data.keyword.satelliteshort}} locations
{: #concept-satloc}

Your {{site.data.keyword.satelliteshort}} locations are representations of your infrastructure environment, with added functionality to help manage the location, deploy your workloads, and bring {{site.data.keyword.cloud_notm}} services to the location.
{: shortdesc}

**{{site.data.keyword.satelliteshort}} location control plane**

Each {{site.data.keyword.satelliteshort}} location must have a location control plane that runs the necessary components to manage the location.
* **Hosts**: You must assign hosts to run the control plane, typically at least 6 hosts that are spread evenly across 3 zones. As your location grows, you might need to add more hosts evenly in groups of 3 to the control plane. For sizing and setup information, see the [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations) documentation.
* **{{site.data.keyword.satelliteshort}} link**: To audit and secure cloud connections, including the connection back to {{site.data.keyword.cloud_notm}}, each location control plane is set up with [{{site.data.keyword.satelliteshort}} link](/docs/satellite?topic=satellite-link-location-cloud).
* **IBM monitoring**: The {{site.data.keyword.satelliteshort}} location control plane includes monitoring components that can automatically detect, resolve, or report location issues back to {{site.data.keyword.cloud_notm}}.
* **Cluster masters**: The {{site.data.keyword.satelliteshort}} location control plane runs the masters for the {{site.data.keyword.openshiftshort}} clusters that run in the location, including {{site.data.keyword.satelliteshort}}-enabled service clusters. These masters are automatically updated and managed by {{site.data.keyword.cloud_notm}} through the {{site.data.keyword.satelliteshort}} link connection. However, the masters run on hosts in your infrastructure, so you share responsibility to make sure that the masters have enough compute resources to run.

**{{site.data.keyword.satelliteshort}}-enabled service**

One of the main benefits to using {{site.data.keyword.satellitelong_notm}} is the ability to get {{site.data.keyword.cloud_notm}} services in any of your compatible infrastructure environments. To consume these {{site.data.keyword.satelliteshort}}-enabled services, the services often set up a control plane cluster to run in your location. After the service cluster is set up, you can start using the service across that location. An example {{site.data.keyword.satelliteshort}}-enabled service is {{site.data.keyword.openshiftlong_notm}}.

**{{site.data.keyword.satelliteshort}}-enabled service: {{site.data.keyword.openshiftlong_notm}} cluster**

You can [create clusters in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=openshift-satellite-clusters) by using the {{site.data.keyword.openshiftlong_notm}} suite of API, CLI, or UI tools, such as the `ibmcloud oc cluster create satellite` command. Use these clusters to run any Kubernetes-compatible workload in your location. You also have a few tools to help you deploy and manage your workloads. This {{site.data.keyword.satelliteshort}}-enabled service uses the location control plane, so you do not need a separate {{site.data.keyword.openshiftlong_notm}} cluster in a {{site.data.keyword.satelliteshort}} location control plane cluster.
* **{{site.data.keyword.openshiftshort}}**: Each cluster that you create installs OpenShift Container Platform for a simplified Kubernetes developer experience. With {{site.data.keyword.openshiftshort}}, you get a built-in image registry and continuous integration, continuous delivery (CI/CD) pipeline for deploying your apps, strict app security settings by default, and other benefits like the ability to run {{site.data.keyword.cloud_notm}} Paks.
* **{{site.data.keyword.satelliteshort}} config**: With [{{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-cluster-config), you can group clusters in your location to create the same Kubernetes configurations and updates across clusters for consistent workload deployments.

**Hosts**

[Attach as many hosts](/docs/satellite?topic=satellite-hosts) as you need to your {{site.data.keyword.satelliteshort}} location, and consider having a few extra in case resources such as services, clusters, or the control plane request extra capacity. You can use host labels to help manage capacity requests and automatically assign hosts to your resources. The hosts become the user-provided infrastructure (`upi`) worker nodes in your clusters, providing the compute capacity that is needed to run the cluster workloads.

**Zones**

You must create at least 3 zones to spread hosts evenly across the {{site.data.keyword.satelliteshort}} location control plane, as well as for the worker pools in your clusters throughout the location. You can name these zones to represent the actual, physically separate zones in your infrastructure provider.

### {{site.data.keyword.cloud_notm}}
{: #concept-ibm-cloud}

{{site.data.keyword.cloud_notm}} manages the master components for your {{site.data.keyword.satelliteshort}} locations. You might also run services in {{site.data.keyword.cloud_notm}}, such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

**{{site.data.keyword.satelliteshort}} master control plane**

Each {{site.data.keyword.satelliteshort}} location is managed from an {{site.data.keyword.cloud_notm}} region, where the master control plane resides. IBM manages the master control plane. From the master control plane, components such as version updates, {{site.data.keyword.satelliteshort}} Config and Link, and {{site.data.keyword.cloud_notm}} services are pushed out to your {{site.data.keyword.satelliteshort}} location, to make those components available to all of your resources in the location. Additionally, the master control plane enables you to use the same {{site.data.keyword.cloud_notm}} platform tools to manage identity and access, key management, certificate management, logging and monitoring, and other security and compliance controls for your {{site.data.keyword.satelliteshort}} location.

**Cluster**

You might [create {{site.data.keyword.openshiftshort}} clusters](/docs/openshift?topic=openshift-getting-started) in {{site.data.keyword.cloud_notm}} single or multizone regions. Even though these clusters are not in your {{site.data.keyword.satelliteshort}} location, you can add them to cluster groups in {{site.data.keyword.satelliteshort}} config so that you can deploy the same workloads across clusters in {{site.data.keyword.satelliteshort}} or {{site.data.keyword.cloud_notm}}.

**Zones**

The master control plane is automatically spread across zones in the {{site.data.keyword.cloud_notm}} region for you. Unlike the zones in your {{site.data.keyword.satelliteshort}} location, these zones do not represent the zones in your infrastructure provider. Instead, these zones are managed by IBM and available to services that run in {{site.data.keyword.cloud_notm}} only.

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
