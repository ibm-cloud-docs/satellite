---

copyright: 
  years: 2025, 2025
lastupdated: "2025-07-07"


keywords: high availability, disaster recover, HA, DR, responsibilities

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.satelliteshort}}
{: #sat-ha-dr}

[High availability](x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state. {: shortdesc}

{{site.data.keyword.satelliteshort}} is a highly available regional or zonal service designed for availability during a regional or zonal outage. {{site.data.keyword.satelliteshort}} is designed to meet the Service Level Objectives (SLO) with the Standard plan.

For more information about the available region and data center locations, see Service and infrastructure availability by location.

## High availability architecture
{: #ha-architecture}

The following image shows specific areas to watch in the {{site.data.keyword.satelliteshort}} architecture so you can improve your high availability.

![Highly available {{site.data.keyword.satelliteshort}} architecture](/images/sat_architecture_ha1.svg "Highly available Satellite architecture"){: caption="Highly available Satellite architecture" caption-side="bottom"}

### High availability of the {{site.data.keyword.satelliteshort}} location control plane
{: #ha-control-plane-master}

When you create a {{site.data.keyword.satelliteshort}} location, you must choose an {{site.data.keyword.cloud_notm}} multizone metro that runs and manages the {{site.data.keyword.satelliteshort}} control plane of your location. The control plane is in an {{site.data.keyword.IBM_notm}} account and is managed by {{site.data.keyword.cloud_notm}}.
{: shortdesc}

{{site.data.keyword.IBM_notm}} provides high availability for your {{site.data.keyword.satelliteshort}} location control plane in the following ways.

Multiple instances
:    By default, every {{site.data.keyword.satelliteshort}} control plane is automatically set up with multiple instances to ensure availability and sufficient compute capacity. {{site.data.keyword.IBM_notm}} monitors the availability and compute capacity for your {{site.data.keyword.satelliteshort}} management plane and automatically scales the master instances if necessary.

Spread across zones
:    {{site.data.keyword.IBM_notm}} automatically spreads the management plane instances across multiple zones within the same {{site.data.keyword.cloud_notm}} multizone metro. For example, if you choose to manage your location from the `wdc` metro in US East region, your {{site.data.keyword.satelliteshort}} location management plane instances are spread across the `us-east-1`, `us-east-2`, and `us-east-3` zones. This zonal spread ensures that your management plane is available, even if one zone becomes unavailable.


Because the {{site.data.keyword.satelliteshort}} management plane is managed by {{site.data.keyword.IBM_notm}}, you cannot change the number of master instances or how high availability is configured. However,you must configure your control plane nodes for high availability. The control plan worker nodes can ensure that the workloads that run in your location have enough compute capacity, even if compute hosts become unavailable. The time to recover a location or cluster is dependent on the size of the location or cluster and the network latency between {{site.data.keyword.cloud_notm}} and your host infrastructure. 
{: note}

### High availability of the {{site.data.keyword.satelliteshort}} control plane nodes
{: #ha-control-plane-worker}

The {{site.data.keyword.satelliteshort}} control plane nodes run on the compute infrastructure that you add to your {{site.data.keyword.satelliteshort}} location. Your compute hosts can be in an on-premises data center, in public cloud providers, or in edge computing environments.

Your control plane nodes run the {{site.data.keyword.satelliteshort}} Link tunnel client component that establishes a secure connection back to {{site.data.keyword.cloud_notm}}. The Link tunnel client component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. Without this connection, your location workloads continue to run, but you cannot make any configuration changes to your location, roll out updates with {{site.data.keyword.satelliteshort}} Config, or change {{site.data.keyword.cloud_notm}} services that are deployed to the location.

Because you manage the compute infrastructure for your {{site.data.keyword.satelliteshort}} location, you must make sure that your compute hosts are set up highly available. A high availability setup ensures that the {{site.data.keyword.satelliteshort}} control plane continues to run, even if your compute hosts experience a power, networking, or storage outage.


## Disaster recovery architecture
{: #dr-features}

The general strategy for disaster recovery is to configure storage and backups of your data with solutions such as Object Storage. All {{site.data.keyword.satelliteshort}} control plane data is backed up to an {{site.data.keyword.cos_full_notm}} service instance so that you can create a new location with this data after a disaster. Access to this instance is protected by {{site.data.keyword.iamshort}} and all data is automatically encrypted during transit and at rest. Note that when you create a location, you also provide an {{site.data.keyword.cos_short}} service instance that you control for backup of the location control plane nodes. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own.

There are additional storage options you can implement, such as storage templates that you create, or your own operators, drivers, or plug-ins. For more information, see [What are my options for deploying storage to Satellite?](/docs/satellite?topic=satellite-storage-template-ov#storage-sat-configure).

For information relevant to IBM Cloud, see [How IBM Cloud prepares for disaster recovery](/docs/resiliency?topic=resiliency-disaster-recovery).


## Recovery time objective (RTO) and recovery point objective (RPO)
{: #rto-rpo-features}


| Feature | RTO and RPO | Considerations
| -------------- | -------------- | ----------- |
| Cloud Object Storage | See the [object storage docs](/docs/cloud-object-storage?topic=cloud-object-storage-cos-ha-dr#rto-rpo-features). | 
{: caption="RTO/RPO features" caption-side="bottom"}

## How {{site.data.keyword.IBM}} helps ensure disaster recovery
{: #ibm-disaster-recovery}

{{site.data.keyword.IBM}} takes specific recovery actions if a disaster occurs. 


### How {{site.data.keyword.IBM_notm}} recovers from failures
{: #ibm-zone-failure}

If there is a zone or regional failure, IBM is responsible for the recovery of  components. IBM will attempt to restore the cluster in the same region based on the last state in internal persistent storage. IBM updates and recovers operational components within the cluster, such as the Ingress application load balancer and file storage plug-in.

IBM also provides the ability to integrate with other IBM Cloud services such as storage providers so that data can be backed up and restored. It is your responsibility to implement these integrations. 

## How {{site.data.keyword.IBM_notm}} maintains services
{: #ibm-service-maintenance}

All upgrades follow {{site.data.keyword.IBM_notm}} service best practices, including recovery plans and rollback processes. Regular maintenance might cause short interruptions, mitigated by [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha). Changes are rolled out sequentially, region by region, and zone by zone within a region. {{site.data.keyword.IBM_notm}} reverts updates at the first sign of a defect.

Complex changes are enabled and disabled with feature flags to control exposure.

Changes that impact customer workloads are detailed in {{site.data.keyword.cloud_notm}} notifications. For more information about planned maintenance, announcements, and release notes that impact this service, see [Monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status).

## Your responsibilities for high availability and disaster recovery
{: #feature-responsibilities}



It is your responsibility to continuously test your plan for HA and DR.

Interruptions in network connectivity and short periods of unavailability of a service might occur. It is your responsibility to make sure that application source code includes [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha) to maintain high availability of the application.
{: note}

You are responsible for configuring your cluster to achieve the appropriate level of availability for your apps and services. The level of availability that you set up for your cluster impacts your coverage under the [{{site.data.keyword.cloud_notm}} HA service level agreement terms](/docs/overview?topic=overview-slas). For example, to receive full HA coverage under the SLA terms, you must set up a multizone cluster with a total of at least 6 worker nodes, two worker nodes per zone that are evenly spread across three zones.

You are responsible for the recovery of the workloads that run the cluster and your application data. For more information on your responsibilities for disaster recovery, see [Your responsibilities](/docs/satellite?topic=satellite-responsibilities).

## Change management
{: #change-management-hadr}

Change management includes tasks such as upgrades, configuration changes, and deletion. Keep the following points in mind to reduce downtime or data loss for your workload. 

- It is recommended that you grant users and processes the IAM roles and actions with the least privilege required for their work. For example, limit the ability to delete production resources.

- Use the API, CLI, or console tools to apply the provided worker node updates that include operating system patches, or to request that worker nodes are rebooted, reloaded, or replaced.

- Use the API, CLI, or console tools to apply the provided [updates](/docs/openshift?topic=openshift-openshift_versions#os-satellite-without-coreos) for any Satellite clusters you own. Make sure to review the information and requirements for each version update to prevent issues or downtime. 

- Make sure your cluster components stay [updated](/docs/openshift?topic=openshift-update&interface=ui) and run the latest available versions.

- Make sure any agents or storage templates run the latest version. You can check the version history in the reference section of the documentation to stay up-to-date.
