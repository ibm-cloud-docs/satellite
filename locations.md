---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-12"

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
{:step: data-tutorial-type='step'}


# Setting up {{site.data.keyword.satelliteshort}} locations
{: #locations}

Set up an {{site.data.keyword.satellitelong}} location to represent a data center that you fill with your own infrastructure resources, and start running {{site.data.keyword.cloud_notm}} services on your own infrastructure.
{: shortdesc}

{{site.data.keyword.satellitelong}} is available as a tech preview and subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: preview}

**Steps to set up your first location**
1. [Create a location](#location-create).
2. [Add 3 hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
3. [Assign at least 3 hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
4. [Add more hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
5. [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) and assign the hosts to the cluster, so that you can run OpenShift workloads in your location.

<br />


## Understanding {{site.data.keyword.satelliteshort}} locations
{: #location-concept}

A {{site.data.keyword.satelliteshort}} location represents a data center that you can fill with your own infrastructure resources to run and extend {{site.data.keyword.cloud_notm}} services in your location.
{: shortdesc}

The following diagram presents the concept of setting up your own {{site.data.keyword.satelliteshort}} locations in the Asia Pacific region, and running the same application in your location as well as in {{site.data.keyword.cloud_notm}}.

![Concept overview of Satellite locations in Asia Pacific](/images/location-ov.png){: caption="Figure 1. A conceptual overview of setting up {{site.data.keyword.satelliteshort}} locations." caption-side="bottom"}

**{{site.data.keyword.satelliteshort}} control plane master**: When you create a location, a highly available control plane master is automatically created in one of the {{site.data.keyword.cloud_notm}} multizone regions that you selected during location creation. The control plane master securely connects your location to {{site.data.keyword.cloud_notm}} and stores the configuration of your location. If service updates are available, the control plane master automatically makes these updates available to the control plane worker nodes. All location metadata is automatically backed up to an {{site.data.keyword.cos_full_notm}} instance in your account.

**{{site.data.keyword.satelliteshort}} control plane worker**: After you create the location, you must add at least 3 hosts to the location control plane as worker nodes. Then, the location control plane worker is ready to manage your location resources. Other components are also set up, such as IBM monitoring components to monitor the health of your location and hosts and automatically resolve issues. For more information, see the [service architecture](/docs/satellite?topic=satellite-service-architecture).

**{{site.data.keyword.satelliteshort}} Link**: A {{site.data.keyword.satelliteshort}} Link connector component is automatically created in the location control plane worker to securely connect the location back to the control plane master in {{site.data.keyword.cloud_notm}}. You can use the Link component to control and audit network traffic in and out of the location.

**Hosts**: To fill the {{site.data.keyword.satelliteshort}} location with your own infrastructure, you attach hosts such as on-prem bare metal, virtual machines in other cloud providers, edge computing devices, and more. All hosts must the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host). The hosts become the compute capacity in your location. You must assign at least three of these hosts as worker nodes to the {{site.data.keyword.satelliteshort}} control plane. All other hosts can be used to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters on your own infrastructure.

**{{site.data.keyword.openshiftlong_notm}} clusters**: You can use host capacity in your location to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters. Your hosts become the worker nodes of your cluster in a `upi` (or user-provided infrastructure) worker pool. The cluster master runs on and is managed from your {{site.data.keyword.satelliteshort}} control plane worker nodes. {{site.data.keyword.openshiftshort}} is automatically set up for these clusters, and you can run any Kubernetes workload that you want on the clusters.

**{{site.data.keyword.satelliteshort}} configurations**: To help you deploy and manage workloads across {{site.data.keyword.openshiftshort}} clusters in your location and in {{site.data.keyword.cloud_notm}}, you can use {{site.data.keyword.satelliteshort}} configurations. Simply upload a Kubernetes resource YAML file as a version to your configuration and subscribe the clusters where you want to deploy the resource. Your Kubernetes resources and subsequent version updates are automatically deployed to the subscribed clusters, and you also can view an inventory of all the Kubernetes resources that are managed by a {{site.data.keyword.satelliteshort}} configuration.

**{{site.data.keyword.cloud_notm}} services**: You can use host capacity in your location to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services on your own infrastructure. Simply select the service that you want to run in your location from the {{site.data.keyword.cloud_notm}} catalog, and start creating service instances in your location. Additionally, you use the same {{site.data.keyword.cloud_notm}} platform tools to manage identity and access, key management, certificate management, logging and monitoring, and other security and compliance controls for these services. 

<br />


## Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you add to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

1. Set up a highly available location control plane worker with enough compute capacity to manage the resources in your {{site.data.keyword.satelliteshort}} location.
   *  **Minimum size**: To get started, you must add and assign at least 3 hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host) of 4 CPU, 16 GB memory, and 100 GB storage. You assign these hosts to 3 separate zones.
   *  **High availability**: When you assign hosts to the location control plane, you must assign at least 1 host to each of the 3 available zones of your multizone metro city that you selected during location creation. To make the location control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might use 3 hosts that run in separate availability zones in your cloud provider, or that run in three separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable. For more information, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha). 
   *  **Compute capacity**: {{site.data.keyword.satelliteshort}} monitors your location for capacity. When the location reaches 70% capacity, you see a warning status to notify you to add more hosts to the location. If the location reaches 80% capacity, the state changes to **critical** and you see another warning that tells you to add more hosts to the location.
2. Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then IBM can assign the hosts to your location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.
3. To decide on the size and amount of hosts to add to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations:
   * How many resources does my app require?
   * What else besides my app might use resources in the cluster?
   * What type of availability do I want my workload to have?
   * How many worker nodes (hosts) do I need to handle my workload?
   * How do I monitor resource usage and capacity in my cluster?

<br />


## Creating {{site.data.keyword.satelliteshort}} locations
{: #location-create}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north and south of the country. A {{site.data.keyword.satellitelong}} location represents a data center that you fill with your own infrastructure resources to run your own workloads and {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click **Create location**.
2. Enter a name and an optional description for your location.
3. Select the {{site.data.keyword.cloud_notm}} multizone metro city that you want to use to manage your location. For more information about why you must select a multizone metro city, see [Understanding supported {{site.data.keyword.cloud_notm}} regions in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the city that is closest to where your host machines physically reside that you plan to add to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. 
4. Click **Create location**. When you create the location, a location control plane master is deployed to one of the zones that are located in the multizone metro city that you selected. That process might take a few minutes to complete.
5. Wait for the master to be fully deployed and the location **State** to change to `Action required`.
6. Continue with [adding hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts) to finish the setup of your  {{site.data.keyword.satelliteshort}} control plane. 

<br />


## Setting up the {{site.data.keyword.satelliteshort}} control plane for the location
{: #setup-control-plane}

The location control plane runs resources that are managed by {{site.data.keyword.satelliteshort}} to help manage the hosts, clusters, and other resources that you add to the location. To create the control plane, you must add at least 3 compute hosts to your location that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
{: shortdesc}

**Before you begin**:
- Make sure that you [added at least 3 hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane.
- Verify that your location is in an **Action required** state.

**To add hosts as worker nodes to the control plane**:

1. From the {{site.data.keyword.satelliteshort}} [**Locations** dashboard](https://cloud.ibm.com/satellite/locations), select the location where you want to finish the setup of your control plane.
2. From the **Hosts** tab, identify at least 3 hosts that you can assign as worker nodes to your control plane. All hosts must be in an **Unassigned** status.
3. From the actions menu of each host, click **Assign host**.
4. Select **Control plane** as your cluster and choose one of the available zones. Make sure that you assign each host to a different zone so that you spread all 3 hosts across all 3 zones in US South (`us-south-1`, `us-south-2`, and `us-south-3`). When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
5. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} control plane. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.
6. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Prometheus is not yet initialized` or `Verify that alb steps have been completed for this cluster`.
   {: note}

### What's next?
{: #location-control-plane-next}

Now that your location control plane is set up, you can choose among the following options:
- [Add more compute capacity to your location to run {{site.data.keyword.openshiftlong_notm}} clusters](/docs/satellite?topic=satellite-hosts#add-hosts) on your own infrastructure.
- [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
- [Learn more about the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

<br />


## Removing locations
{: #location-remove}

You can remove a {{site.data.keyword.satelliteshort}} location if you no longer need the clusters and other resources that run in the location.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts and clusters that run in the location. Note that the underlying host infrastructure is not automatically deleted when you delete a location because you manage the infrastructure yourself.
{: important}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.
2. [Remove all hosts](/docs/satellite?topic=satellite-hosts#host-remove) from your location.
3. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external} **Locations** table, hover over the location that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
4. Click **Remove location**, enter the location name to confirm the deletion, and click **Remove**.
