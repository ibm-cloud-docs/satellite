---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-04"

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


# Setting up {{site.data.keyword.satelliteshort}} locations
{: #locations}

Set up an {{site.data.keyword.satellitelong}} location to represent a data center that you fill with your own infrastructure resources, so that you can control and extend your resources with {{site.data.keyword.cloud_notm}}.
{: shortdesc}

{{site.data.keyword.satellitelong}} is available as a tech preview and subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: preview}

**Steps to set up your first location**
1. [Create a location](#location-create).
2. [Add 3 hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
3. [Set up the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) with the 3 hosts.
4. [Add more hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
5. [Create a cluster](/docs/openshift?topic=openshift-satellite-clusters) and assign the hosts to the cluster, so that you can run Kubernetes workloads in your location.

<br />


## Understanding {{site.data.keyword.satelliteshort}} locations
{: #location-concept}

A {{site.data.keyword.satelliteshort}} location represents a data center that you can fill with your own infrastructure resources as hosts and extend {{site.data.keyword.cloud_notm}} services to your workloads that run in the location.
{: shortdesc}

The following diagram presents the concept of setting up your own {{site.data.keyword.satelliteshort}} locations in the Asia Pacific region, and running the same application across all of your locations, both {{site.data.keyword.satelliteshort}} and {{site.data.keyword.cloud_notm}}.

![Concept overview of Satellite locations in Asia Pacific](/images/location-ov.png){: caption="Figure 1. A conceptual overview of setting up {{site.data.keyword.satelliteshort}} locations." caption-side="bottom"}

**{{site.data.keyword.satelliteshort}} control plane master**: When you create a location, you get a highly available control plane in one of the {{site.data.keyword.cloud_notm}} multizone regions, that runs the master components of the location control plane, to store the location configuration and provide updates and management from {{site.data.keyword.cloud_notm}} to the resources that you set up later in your location. The location metadata is automatically backed up to an {{site.data.keyword.cos_full_notm}} instance in your account.

**{{site.data.keyword.satelliteshort}} control plane worker**: After you create the location, you must add at least 3 hosts to the location control plane as worker nodes. Then, the location control plane worker is ready to manage your location resoruces. Other components are also set up, such as IBM monitoring components to monitor the health of your location and hosts and automatically resolve certain issues. For more information, see the [service architecture](/docs/satellite?topic=satellite-service-architecture).

**{{site.data.keyword.satelliteshort}} Link**: A {{site.data.keyword.satelliteshort}} Link is automatically created in the location control plane worker to connect the location back to the control plane master in {{site.data.keyword.cloud_notm}}. You can use Link to control and audit network traffic in and out of the location.

**Hosts**: To fill the {{site.data.keyword.satelliteshort}} location with your own infrastructure, you attach hosts such as on-prem bare metal, virtual machines in other cloud providers, edge computing devices, and more, as long as the hosts meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host). The hosts become the computing capacity in the location. You assign at least three of these hosts to run the location control plane operations and the rest of the hosts are available to assign to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters for your workloads to run on.

**Clusters from {{site.data.keyword.openshiftlong_notm}}**: After you add hosts your locations, you assign the hosts to {{site.data.keyword.satelliteshort}} clusters, which are clusters in the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.openshiftlong_notm}} service. The hosts become the worker nodes in a `upi` (or user-provided infrastructure) worker pool. The cluster masters are managed in your {{site.data.keyword.satelliteshort}} location control plane worker that runs on your own infrastructure. {{site.data.keyword.openshiftshort}} is automatically set up for these clusters, and you can run any Kubernetes workload that you want on the clusters.

**{{site.data.keyword.satelliteshort}} configuration**: To help you run workloads across clusters and locations, you can use {{site.data.keyword.satelliteshort}} configuration. You can upload configuration versions of any Kubernetes resource and subscribe clusters that run anywhere in a {{site.data.keyword.satelliteshort}} or {{site.data.keyword.cloud_notm}} location to the configuration. Your Kubernetes resources and subsequent version updates are automatically deployed to the subscribed clusters, and you also can view an inventory of all the Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} configuration.

**{{site.data.keyword.cloud_notm}} services**: Your {{site.data.keyword.satelliteshort}} location is automatically an extension of {{site.data.keyword.cloud_notm}}. From the catalog, you can pick {{site.data.keyword.satelliteshort}}-enabled, {{site.data.keyword.cloud_notm}} services such as databases or artificial intelligence tools and create instances in your {{site.data.keyword.satelliteshort}} location. Additionally, you use the same {{site.data.keyword.cloud_notm}} platform tools to manage identity and access, key management, certificate management, logging and monitoring, and other security and compliance controls. These {{site.data.keyword.cloud_notm}} services might run in {{site.data.keyword.cloud_notm}} (such as platform tools) or might require you to assign hosts as compute capacity for the service to run in your {{site.data.keyword.satelliteshort}} location.

<br />


## Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you add to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

1. Set up a highly available location control plane worker with enough compute capacity to manage the resources in your {{site.data.keyword.satelliteshort}} location.
   *  **Minimum size**: To get started, you must add and assign at least 3 hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host) of 4 CPU, 16 GB memory, and 100 GB storage. You assign these hosts to 3 separate zones.
   *  **High availability**: When you create the location control plane, you must assign at least 1 host to each of 3 separate zones. To make the location control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might use 3 hosts that run in separate availability zones in your cloud provider, or that run in three separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable.
   *  **Compute capacity**: {{site.data.keyword.satelliteshort}} monitors your location for capacity. When the location reaches 70% capacity, you see a warning status to notify you to add more hosts to the location. If the location reaches 80% capacity, the state changes to **critical** and you see another warning that tells you to add more hosts to the location.
2. Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then IBM can assign the hosts to your location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.
3. To decide the size and amount of hosts to add to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations:
   * How many resources does my app require?
   * What else besides my app might use resources in the cluster?
   * What type of availability do I want my workload to have?
   * How many worker nodes (hosts) do I need to handle my workload?
   * How do I monitor resource usage and capacity in my cluster?

<br />


## Creating {{site.data.keyword.satelliteshort}} locations
{: #location-create}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north and south of the country. After you set up your locations, you can bring {{site.data.keyword.cloud_notm}} services and consistent application management to the machines that already exist in your environments in the location.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click **Create location**.
2. Select the {{site.data.keyword.cloud_notm}} multizone metro city that you want to use to manage your location. You can use any metro city, but to reduce latency between {{site.data.keyword.cloud_notm}} and your location, choose the city that is closest to the compute hosts that you plan to add to your location later.
3. Enter a name and an optional description for your location, and click **Create location**. When you create the location, a location master is deployed to one of the zones that are located in the multizone metro city that you selected. During this process, the **State** of the location shows `Unknown`. When the master is fully deployed and you can now add compute capacity to your location to set up the {{site.data.keyword.satelliteshort}} control plane, the **State** changes to `action required`.
4. To finish the set up of your location, you must [add at least 3 compute hosts](/docs/satellite?topic=satellite-hosts#add-hosts) to your location so that you can run the {{site.data.keyword.satelliteshort}} control plane.

<br />


## Setting up the {{site.data.keyword.satelliteshort}} control plane for the location
{: #setup-control-plane}

The location control plane runs resources that are managed by {{site.data.keyword.satelliteshort}} to help manage the hosts, clusters, and other resources that you add to the location. To create the control plane, you must add at least 3 compute hosts to your location that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
{: shortdesc}

**Before you begin**:
- Make sure that you [added at least 3 hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts) to use for your {{site.data.keyword.satelliteshort}} control plane.
- Verify that your location is in an **Action required** state.

**To create the control plane**:

1. Identify at least 3 machines that you want to add to your location as hosts so that you can run the {{site.data.keyword.satelliteshort}} control plane.
2. From the {{site.data.keyword.satelliteshort}} [**Locations** dashboard](https://cloud.ibm.com/satellite/locations), select the location where you want to create your control plane.
3. From the actions menu of each machine, click **Assign host**.
4. Select **Control plane** as your cluster and choose one of the available zones. Make sure that you assign each host to a different zone so that you spread all 3 hosts across all 3 zones in US South (`us-south-1`, `us-south-2`, and `us-south-3`). When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Unknown`.
5. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} control plane. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.
6. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show **action required**, and you might see intermittent errors, such as `Prometheus is not yet initialized` or `Verify that alb steps have been completed for this cluster`.
   {: note}

### What's next?
{: #location-control-plane-next}

Now that your location control plane is set up, you can choose among the following options:
- [Add more compute capacity to your location to create {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=satellite-hosts#add-hosts)
- [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

<br />


## Removing locations
{: #location-remove}

You can remove a {{site.data.keyword.satelliteshort}} location if you no longer need the clusters and other resources that run in the location.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts and clusters that run in the location. Note that the underlying host infrastructure is not deleted because you manage the infrastructure yourself.
{: important}

1. [Remove all hosts](/docs/satellite?topic=satellite-hosts#host-remove) from your location.
2. [Remove all clusters](/docs/openshift?topic=openshift-remove) from your location.
3. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external} **Locations** table, hover over the location that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
4. Click **Remove location**, enter the location name to confirm deletion, and click **Remove**.
