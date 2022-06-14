---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-14"

keywords: satellite architecture, satellite components, satellite workload isolation, satellite tenant isolation, satellite dependencies

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Learning about {{site.data.keyword.satelliteshort}} architecture, workload isolation, and dependencies
{: #service-architecture}

Learn about the {{site.data.keyword.satellitelong_notm}} service architecture, its components, how your workloads are isolated from other tenants, and what other {{site.data.keyword.cloud_notm}} and third-party services {{site.data.keyword.satelliteshort}} depends on.
{: shortdesc}

## {{site.data.keyword.satelliteshort}} architecture
{: #architecture}

The following image shows the main components in {{site.data.keyword.satellitelong_notm}} and how they interact with each other.
{: shortdesc}



![Satellite for IBM Cloud service architecture](/images/sat_architecture.png "Satellite service architecture"){: caption="Figure 1. Satellite service architecture" caption-side="bottom"}

| Legend | Description |
|-----------------|-----------------------|
| ![Legend1](/images/sat_architecture_legend1.png "Satellite service architecture legend 1") | Manages the deployment of Kubernetes resources across {{site.data.keyword.satelliteshort}} clusters by creating {{site.data.keyword.satelliteshort}} configurations and subscribing your clusters to it. |
| ![Legend2](/images/sat_architecture_legend2.png "Satellite service architecture legend 2") | Monitors the health and performance of your {{site.data.keyword.satelliteshort}} location, automatically resolves issues if possible, and informs the IBM Site Reliability Engineers (SREs) to allow further investigation. |
| ![Legend3](/images/sat_architecture_legend3.png "Satellite service architecture legend 3") | Integrates with Identity and Access Management to control {{site.data.keyword.satelliteshort}} location access and {{site.data.keyword.cos_full_notm}} to back up {{site.data.keyword.satelliteshort}} control plane data. In addition, you can connect to other {{site.data.keyword.cloud_notm}} services to enhance your location capabilities. |
| ![Legend4](/images/sat_architecture_legend4.png "Satellite service architecture legend 4") | Securely connects the Satellite location to the {{site.data.keyword.cloud_notm}} region that the location is managed from. Incoming and outgoing network traffic on this connection is monitored and can be reviewed and analyzed by the location administrator. |
| ![Legend5](/images/sat_architecture_legend5.png "Satellite service architecture legend 5") | Latency requirements between {{site.data.keyword.satelliteshort}} components such as the hosts that you use for the control plane and clusters, in milliseconds. As latency increases, you might see service interruptions. |
{: caption="Legend and description for Satellite architecture image." caption-side="top"}

### Master and worker node components
{: #architecture-master-worker}

Review a description of the main components of the {{site.data.keyword.satelliteshort}} location control plane, including the {{site.data.keyword.IBM_notm}}-managed master from the {{site.data.keyword.cloud_notm}} region and the worker nodes that you set up in your location.
{: shortdesc}

|Master components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link tunnel|The {{site.data.keyword.satelliteshort}} Link tunnel server creates a secure TLS connection to the {{site.data.keyword.satelliteshort}} Link connector that runs on the control plane worker nodes in your {{site.data.keyword.satelliteshort}} location. Three tunnels are created between the tunnel server and the connector to support redundancy across the three availability zones of your location. All communication that leaves and enters your location is proxied by the Link tunnel server, and the connection metadata captured and monitored by {{site.data.keyword.IBM_notm}} to detect malicious activity. To control the network connections between the workloads that run in your location and the {{site.data.keyword.cloud_notm}} multizone metro that manages your location, you can set up {{site.data.keyword.satelliteshort}} Link endpoints. For more information, see [Connecting your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} with endpoints](/docs/satellite?topic=satellite-link-location-cloud).  |
|{{site.data.keyword.monitoringfull_notm}}|The {{site.data.keyword.monitoringfull_notm}} component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, {{site.data.keyword.IBM_notm}} Site Reliability Engineers are automatically informed for further investigation. |
|{{site.data.keyword.satelliteshort}} Config|With {{site.data.keyword.satelliteshort}} Config, you can consistently deploy Kubernetes resource configurations across {{site.data.keyword.openshiftlong}} clusters that run on the infrastructure of your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. You can monitor the health of these resources by using the location dashboard. For more information, see [Deploying Kubernetes resources across {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig).|
| {{site.data.keyword.secrets-manager_full_notm}} | With {{site.data.keyword.secrets-manager_full_notm}}, you can create secrets dynamically and lease them to applications while you control access from a single location. Built on open source HashiCorp Vault, {{site.data.keyword.secrets-manager_full_notm}} helps you get the data isolation of a dedicated environment with the benefits of a public cloud. For more information, see [Getting started with {{site.data.keyword.secrets-manager_full_notm}}](/docs/secrets-manager?topic=secrets-manager-getting-started).|
{: summary="Overview of the Satellite control plane master components"}
{: class="simple-tab-table"}
{: caption="Overview of Satellite control plane master components." caption-side="top"}
{: #master-components}
{: tab-title="Master components"}
{: tab-group="satellite-components"}

|Worker node components|Description|
|-----------------|-----------------------|
|{{site.data.keyword.satelliteshort}} Link connector|The {{site.data.keyword.satelliteshort}} link connector component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. All workloads that run in your location and that must connect to an app, service or server that runs in {{site.data.keyword.cloud_notm}} must send their requests to the {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector securely forwards your request to the {{site.data.keyword.satelliteshort}} Link tunnel server where the request is proxied and forwarded to the destination target. To enable DNS resolution between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}, you must create a {{site.data.keyword.satelliteshort}} Link endpoint. For more information, see [Connecting your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} with endpoints](/docs/satellite?topic=satellite-link-location-cloud). By default, all network traffic that enters and leaves your location is automatically captured to allow network monitoring and auditing. |
|{{site.data.keyword.monitoringfull_notm}}|The {{site.data.keyword.monitoringfull_notm}} component monitors the compute capacity in your {{site.data.keyword.satelliteshort}} location and the components that run in your {{site.data.keyword.satelliteshort}} control plane to detect issues and automatically resolve them if possible. These actions can include assigning additional hosts to the control plane or restart components that keep on failing. For issues that cannot be resolved, {{site.data.keyword.IBM_notm}} Site Reliability Engineers are automatically informed for further investigation. |
|Cluster master|When you create a {{site.data.keyword.satelliteshort}} cluster in your location, the master of this cluster is deployed onto your {{site.data.keyword.satelliteshort}} control plane worker nodes to allow communication to {{site.data.keyword.cloud_notm}} and monitoring through {{site.data.keyword.IBM_notm}}. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters).|
{: summary="Overview of the Satellite control plane worker node components"}
{: class="simple-tab-table"}
{: caption="Overview of Satellite control plane worker node components." caption-side="top"}
{: #worker-node-components}
{: tab-title="Worker node components"}
{: tab-group="satellite-components"}

### Latency requirements
{: #architecture-latency}

Review the network latency requirements for the hosts that you add to your {{site.data.keyword.satellitelong_notm}} location.
{: shortdesc}

#### {{site.data.keyword.IBM_notm}}-managed master to customer-provided worker nodes for the {{site.data.keyword.satelliteshort}} location control plane
{: #architecture-latency-master-worker}

The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).

#### Customer-provided worker nodes in the {{site.data.keyword.satelliteshort}} location control plane to worker nodes that run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services like {{site.data.keyword.redhat_openshift_notm}} clusters in the same location
{: #architecture-latency-host}

Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.

#### Customer-provided worker nodes that are assigned to the same {{site.data.keyword.satelliteshort}} resource, like the {{site.data.keyword.satelliteshort}} location control plane or a cluster
{: #architecture-latency-cluster}

Your host infrastructure setup must have a low latency connection of less than or equal to 10 milliseconds (`<= 10ms`) round-trip time (RTT) among all the hosts that are assigned to the same {{site.data.keyword.satelliteshort}} resource, such as the {{site.data.keyword.satelliteshort}} location control plane, a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service, or cluster. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services like databases or cluster application failures.

## Workload isolation in {{site.data.keyword.satellitelong_notm}}
{: #workload-isolation}

Learn how your workloads are isolated from other {{site.data.keyword.IBM_notm}} customers in {{site.data.keyword.satelliteshort}}.
{: shortdesc}

### Workload isolation in the {{site.data.keyword.cloud_notm}} multizone metro that manages your location
{: #workload-isolation-cloud}

When you create a {{site.data.keyword.satelliteshort}} location, a {{site.data.keyword.satelliteshort}} control plane master is automatically created in the {{site.data.keyword.cloud_notm}} multizone metro that manages your location. The master and all the components that run inside the master are dedicated to your {{site.data.keyword.satelliteshort}} location only and are managed by {{site.data.keyword.IBM_notm}} in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cloud_notm}} account.

One of the components that is set up in the {{site.data.keyword.satelliteshort}} control plane master is an etcd database where {{site.data.keyword.IBM_notm}} stores personal and sensitive data of the customer account that created the location. The etcd database is not shared across locations or with other {{site.data.keyword.IBM_notm}} customers. For more information, see [What information is stored with {{site.data.keyword.IBM_notm}} when using {{site.data.keyword.satelliteshort}}?](/docs/satellite?topic=satellite-data-security#sat-sensitive-data) and [How is my information stored and encrypted?](/docs/satellite?topic=satellite-data-security#sat-data-encryption).

### Workload isolation in your {{site.data.keyword.satelliteshort}} location
{: #workload-isolation-location}

Because you manage the host infrastructure that you bring to your {{site.data.keyword.satelliteshort}} location, you are responsible to isolate app workloads that run on your infrastructure. If you host and run {{site.data.keyword.cloud_notm}} services in your location, such as {{site.data.keyword.openshiftlong_notm}}, you can leverage the tools and features that this service provides to isolate your workloads. For more information about available options, see the service documentation.
