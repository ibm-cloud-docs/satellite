---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# High availability and recovery
{: #ha}

High availability (HA) is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. For example, you can prepare for the failure of one system by adding redundancy and setting up failover mechanisms. Review what options you have to make your {{site.data.keyword.satellitelong}} location highly available.
{: shortdesc}

What level of availability do I need?
:    You can achieve high availability on different levels in your backing infrastructure for the {{site.data.keyword.satelliteshort}} location, the {{site.data.keyword.satelliteshort}} location control plane, and within the different components of the clusters that you deploy to the location. The level of availability that is right for you depends on several factors, such as your business requirements, the Service Level Agreements that you have with your customers, and the resources that you want to expend.

What level of availability does {{site.data.keyword.cloud_notm}} offer?
:    For {{site.data.keyword.cloud_notm}}, see [How {{site.data.keyword.cloud_notm}} ensures high availability and disaster recovery](/docs/overview?topic=overview-zero-downtime).

:    For {{site.data.keyword.satelliteshort}}, review the following topics.
:    - [High availability of the {{site.data.keyword.satelliteshort}} management plane](#ha-control-plane-master).
     - [High availability of the {{site.data.keyword.satelliteshort}} control plane nodes](#ha-control-plane-worker).
     - [High availability of {{site.data.keyword.cloud_notm}} services](#ha-cloud-services).

Where is the service located?
:    See [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).

What am I responsible to configure backup and recovery options for?
:    See [Your responsibilities](/docs/satellite?topic=satellite-responsibilities#disaster-recovery).

## Understanding high availability in {{site.data.keyword.satellitelong_notm}}
{: #ha-understand}

To understand your high availability options in {{site.data.keyword.satelliteshort}}, it is important to understand the components that make up your {{site.data.keyword.satelliteshort}} location and how you can eliminate points of failure.
{: shortdesc}

The following image shows specific areas to watch in the {{site.data.keyword.satelliteshort}} architecture so you can improve your high availability.

![Highly available {{site.data.keyword.satelliteshort}} architecture](/images/sat_architecture_ha1.svg "Highly available Satellite architecture"){: caption="Figure 1. Highly available Satellite architecture" caption-side="bottom"}

1. [{{site.data.keyword.satelliteshort}} management plane](#ha-control-plane-master)
2. [{{site.data.keyword.satelliteshort}} control plane nodes](#ha-control-plane-worker)
3. [{{site.data.keyword.cloud_notm}} services that run in your {{site.data.keyword.satelliteshort}} location](#ha-cloud-services)

### High availability of the {{site.data.keyword.satelliteshort}} location control plane
{: #ha-control-plane-master}

When you create a {{site.data.keyword.satelliteshort}} location, you must choose an {{site.data.keyword.cloud_notm}} multizone metro that runs and manages the {{site.data.keyword.satelliteshort}} control plane of your location. The control plane is in an {{site.data.keyword.IBM_notm}} account and is managed by {{site.data.keyword.cloud_notm}}.
{: shortdesc}

{{site.data.keyword.IBM_notm}} provides high availability for your {{site.data.keyword.satelliteshort}} location control plane in the following ways.

Multiple instances
:    By default, every {{site.data.keyword.satelliteshort}} control plane is automatically set up with multiple instances to ensure availability and sufficient compute capacity to manage your location. {{site.data.keyword.IBM_notm}} monitors the availability and compute capacity for your {{site.data.keyword.satelliteshort}} management plane and automatically scales the master instances if necessary.

Spread across zones
:    {{site.data.keyword.IBM_notm}} automatically spreads the management plane instances across multiple zones within the same {{site.data.keyword.cloud_notm}} multizone metro. For example, if you choose to manage your location from the `wdc` metro in US East region, your {{site.data.keyword.satelliteshort}} location management plane instances are spread across the `us-east-1`, `us-east-2`, and `us-east-3` zones. This zonal spread ensures that your management plane is available, even if one zone becomes unavailable.

Automatic backups to Object Storage
:    All {{site.data.keyword.satelliteshort}} control plane data is backed up to an {{site.data.keyword.cos_full_notm}} service instance so that you can create a new location with this data after a disaster. Access to this instance is protected by {{site.data.keyword.iamshort}} and all data is automatically encrypted during transit and at rest. Note that when you create a location, you also provide an {{site.data.keyword.cos_short}} service instance that you control for backup of the location control plane nodes. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own.

Because the {{site.data.keyword.satelliteshort}} management plane is managed by {{site.data.keyword.IBM_notm}}, you cannot change the number of master instances or how high availability is configured. However,  you must configure your control plane nodes for high availability. The control plan worker nodes can ensure that the workloads that run in your location have enough compute capacity, even if compute hosts become unavailable. The time to recover a location or cluster is dependent on the size of the location or cluster and the network latency between {{site.data.keyword.cloud_notm}} and your host infrastructure. 
{: note}

### High availability of the {{site.data.keyword.satelliteshort}} control plane nodes
{: #ha-control-plane-worker}

The {{site.data.keyword.satelliteshort}} control plane nodes run on the compute infrastructure that you add to your {{site.data.keyword.satelliteshort}} location. Your compute hosts can be in an on-premises data center, in public cloud providers, or in edge computing environments.

Your control plane nodes run the {{site.data.keyword.satelliteshort}} Link tunnel client component that establishes a secure connection back to {{site.data.keyword.cloud_notm}}. The Link tunnel client component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. Without this connection, your location workloads continue to run, but you cannot make any configuration changes to your location, roll out updates with {{site.data.keyword.satelliteshort}} Config, or change {{site.data.keyword.cloud_notm}} services that are deployed to the location.

Because you manage the compute infrastructure for your {{site.data.keyword.satelliteshort}} location, you must make sure that your compute hosts are set up highly available. A high availability setup ensures that the {{site.data.keyword.satelliteshort}} control plane continues to run, even if your compute hosts experience a power, networking, or storage outage.

For more information, see the [Basic setup](#satellite-basic-setup) and [High availability setup](#satellite-ha-setup).

### High availability of {{site.data.keyword.cloud_notm}} services
{: #ha-cloud-services}

Every {{site.data.keyword.cloud_notm}} service that you run in your {{site.data.keyword.satelliteshort}} location, such as  {{site.data.keyword.openshiftlong_notm}}, comes with a set of options for how to increase service availability. Make sure to review the documentation of each service to find supported options.


## Basic control plane worker setup
{: #satellite-basic-setup}

The following image shows a basic {{site.data.keyword.satelliteshort}} location control plane node setup. This setup ensures that your {{site.data.keyword.satelliteshort}} location control plane has sufficient compute capacity to run basic {{site.data.keyword.satelliteshort}} workloads and that your control plane continues to run, even if one compute host becomes unavailable.

![Default setup for the {{site.data.keyword.satelliteshort}} control plane.](images/sat_ha_default.svg "{{site.data.keyword.satelliteshort}} control plane"){: caption="Figure 2. {{site.data.keyword.satelliteshort}} control plane" caption-side="bottom"}

Review the characteristics of the basic setup.

- **Groups of 3 compute hosts**: In the basic setup, you must assign at least 3 compute hosts as worker nodes to the {{site.data.keyword.satelliteshort}} location control plane, in separate zones. With 3 hosts, you make sure that your control plane continues to run, even if one compute host becomes unavailable. The minimum of 3 hosts for the {{site.data.keyword.satelliteshort}} location control plane is for demonstration purposes only. To continue to use the location for production workloads, [add more hosts to the location control plane](/docs/satellite?topic=satellite-attach-hosts) in multiples of 3, such as 6, 9, or 12 hosts. Note that while you can deploy a cluster to a location with only 3 control plane hosts, upgrading and other management operations may not work with bare minimums setups.
- **Host requirements**: All compute hosts must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs). Hosts can be in your own on-premises data center, in public cloud providers, or in edge computing environments. You can add compute hosts from different physical locations if you ensure that the requirements for the network speed and latency between the hosts are met. For more information about how to configure hosts that you want to add from your public cloud providers like AWS, Azure, or Google, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
- **Separate physical hosts**: Every compute host must have a separate physical host. The host might be a bare metal machine or a virtual machine that does not share the hypervisor with another virtual machine that you plan to add to your control plane. With this setup, you ensure that the outage of one physical machine does not lead to all {{site.data.keyword.satelliteshort}} location control plane nodes becoming unavailable.

To make your control plane nodes more highly available, see the [Highly available control plane worker setup](#satellite-ha-setup).


## Highly available control plane worker setup
{: #satellite-ha-setup}

Make your {{site.data.keyword.satelliteshort}} control plane nodes more highly available to provide more compute capacity and prepare for a local data center or infrastructure provider failure.
{: shortdesc}

Depending on where your hosts are, the options that are available to you to increase availability might vary. Use the following table to determine how you can improve availability for your compute hosts. The options in this table are sorted with increased availability.

|High availability option|Description|
|-----------|------------------------|
|Add more compute hosts.|To increase compute capacity but also increase availability of your {{site.data.keyword.satelliteshort}} control plane, you can add more compute hosts as worker nodes to the control plane. Each host must meet the standards that are defined in the [basic control plane worker setup](#satellite-basic-setup). You can optionally add more hosts to your location without assigning them to your control plane. If the {{site.data.keyword.IBM_notm}} Monitoring component detects a capacity issue in your location, unassigned hosts are automatically assigned as a worker node to your control plane. Make sure to [attach hosts](/docs/satellite?topic=satellite-setup-control-plane) in each of the three zones to balance the compute capacity for increased high availability. Ideally, your location control plane has at least 3 hosts. Also, ensure hosts are added in multiples of three, such as 6, 9, or 12 hosts. |
|Add redundant power, network, and storage.|To account for a power, network, or storage outage on one of your physical compute hosts, add redundant power, network, and storage configurations. This setup ensures that your compute hosts continue to run, even if hardware or software issues occur on the physical machine.|
|Isolate machines within one data center.|Compute hosts that are in the same data center or with the same cloud provider are often connected to the same power, network, and storage server. If one of these components experiences an outage, all compute hosts that are connected to the same component might be affected by the outage. To ensure that your compute hosts continue to run, plan to isolate your compute hosts as much as possible and not to share the same power, network, or storage devices. |
|Spread hosts across physical locations.|To account for a data center or cloud provider outage, you can spread your compute hosts across different physical locations. Keep in mind that compute hosts can only be in different physical locations if they still meet the networking speed and latency requirements that are defined in the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs). |
{: caption="High availability for the location control plane nodes." caption-side="bottom"}



### Example for a high availability setup in an on-premises data center
{: #example-ha-onprem}

The following image shows a high availability setup of your control plane nodes within an on-premises data center. All compute hosts are on a separate rack to ensure that power, network, and storage devices are not shared. Because all compute hosts are located in the same data center, the requirements for networking speed and latency between the hosts are met.
{: shortdesc}

![High availability setup for an on-premises data center.](images/sat_control_plane_ha_onprem.svg "High availability setup in an on-premises data center."){: caption="Figure 3. High availability setup for an on-premises data center." caption-side="bottom"}

### Example for a high availability setup in a public cloud provider
{: #example-ha-cloudprovider}

The following image shows a highly available setup for compute hosts that are in a public cloud provider. All virtual machines are hosted on a separate physical machine that is dedicated to you only. To ensure further isolation and availability, all compute hosts are spread across multiple availability zones within the same metro. Because the availability zones belong to the same metro, the requirements for networking speed and latency between the hosts are met.
{: shortdesc}


![High availability setup with compute hosts that are in a public cloud provider.](/images/sat_control_plane_to_cloud_ha.svg){: caption="Figure 4. High availability setup with compute hosts that are at a public cloud provider" caption-side="bottom"}
    
