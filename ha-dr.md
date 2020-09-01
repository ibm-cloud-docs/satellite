---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# High availability for {{site.data.keyword.satellitelong_notm}}
{: #ha}

Review what options you have to make your {{site.data.keyword.satelliteshort}} location highly available.
{: shortdesc}

High availability (HA) is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure. For example, you can prepare for the failure of one system component by adding redundancy and setting up failover mechanisms.

## Understanding high availability in {{site.data.keyword.satellitelong_notm}}
{: #ha-understand}

To understand your high availability options in {{site.data.keyword.satelliteshort}}, it is important to understand the components that make up your {{site.data.keyword.satelliteshort}} location and how you can eliminate points of failures.
{: shortdesc}

The following image shows potential points of failure in the {{site.data.keyword.satelliteshort}} architecture.

![Points of failure in the {{site.data.keyword.satelliteshort}} architecture](/images/sat_architecture_ha.png)

1. [{{site.data.keyword.satelliteshort}} control plane master](#ha-control-plane-master)
2. [{{site.data.keyword.satelliteshort}} control plane worker nodes](#ha-control-plane-worker)
3. [{{site.data.keyword.cloud_notm}} services that run in your {{site.data.keyword.satelliteshort}} location](#ha-cloud-services)

### High availability of the {{site.data.keyword.satelliteshort}} control plane master
{: #ha-control-plane-master}

When you create a {{site.data.keyword.satelliteshort}} location, you must choose an {{site.data.keyword.cloud_notm}} multizone metro that runs and manages the {{site.data.keyword.satelliteshort}} control plane master of your location. The control plane master is in an IBM account and is managed by {{site.data.keyword.cloud_notm}}.
{: shortdesc}

IBM provides high availability for your control plane master in the following ways:

- **Multiple instances:** By default, every {{site.data.keyword.satelliteshort}} control plane master is automatically set up with multiple instances to ensure availability and sufficient compute capacity to manage your location. IBM monitors the availability and compute capacity for your {{site.data.keyword.satelliteshort}} control plane master and automatically scales the master instances if necessary.
- **Spread across zones:** IBM automatically spreads the control plane master instances across multiple zones within the same {{site.data.keyword.cloud_notm}} multizone metro. For example, if you choose to manage your location from the `us-east` metro, your control plane master instances are spread across the `us-east-1`, `us-east-2`, and `us-east-3` zones. This zonal spread ensures that your control plane master is available, even if one zone becomes unavailable.
- **Automatic backups to Object Storage:** All {{site.data.keyword.satelliteshort}} control plane data is backed up to an {{site.data.keyword.cos_full_notm}} service instance so that your location can be restored after a disaster. Access to this instance is protected by {{site.data.keyword.iamshort}} and all data is automatically encrypted during transit and at rest.

Because the {{site.data.keyword.satelliteshort}} control plane master is managed by IBM, you cannot change the number of master instances or how high availability is configured. However, you must to ensure that your control plane worker nodes are configured for high availability. The control plan worker nodes can ensure that the workloads that run in your location have enough compute capacity, even if compute hosts become unavailable.
{: note}

### High availability of the {{site.data.keyword.satelliteshort}} control plane worker nodes
{: #ha-control-plane-worker}

The {{site.data.keyword.satelliteshort}} control plane worker nodes run on the compute infrastructure that you add to your {{site.data.keyword.satelliteshort}} location. Your compute hosts can be in an on-premises data center, in other cloud providers, or in edge computing environments.

Your control plane worker nodes run the {{site.data.keyword.satelliteshort}} Link connector component that establishes a secure connection back to {{site.data.keyword.cloud_notm}}. The link connector component is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. Without this connection, your location workloads continue to run, but you cannot make any configuration changes to your location, roll out updates with {{site.data.keyword.satelliteshort}} Config, or change {{site.data.keyword.cloud_notm}} services that are deployed to the location.

Because you manage the compute infrastructure for your {{site.data.keyword.satelliteshort}} location, you must make sure that your compute hosts are set up highly available. A high availability setup ensures that the {{site.data.keyword.satelliteshort}} control plane continues to run, even if your compute hosts experience a power, networking, or storage outage.

For more information, see the [Basic setup](#satellite-basic-setup) and [High availability setup](#satellite-ha-setup).

### High availability of {{site.data.keyword.cloud_notm}} services
{: #ha-cloud-services}

Every {{site.data.keyword.cloud_notm}} service that you run in your {{site.data.keyword.satelliteshort}} location, such as  {{site.data.keyword.openshiftlong_notm}}, comes with a set of options for how to increase service availability. Make sure to review the documentation of each service to find supported options.

<br />


## Basic control plane worker setup
{: #satellite-basic-setup}

The following image shows a basic {{site.data.keyword.satelliteshort}} control plane worker node setup. This setup ensures that your {{site.data.keyword.satelliteshort}} control plane has sufficient compute capacity to run basic {{site.data.keyword.satelliteshort}} workloads and that your control plane continues to run, even if one compute host becomes unavailable.

<img src="/images/satellite_ha_default.png" alt="Default setup for the {{site.data.keyword.satelliteshort}} control plane" width="350" style="width: 350px; border-style: none"/>

Review the characteristics of the basic setup:

- **3 compute hosts**: In the basic setup, you must assign at least 3 compute hosts as worker nodes to the {{site.data.keyword.satelliteshort}} control plane. With 3 hosts, you make sure that your control plane continues to run, even if one compute host becomes unavailable.
- **Host requirements**: All compute hosts must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host) and any [provider-specific requirements](/docs/satellite?topic=satellite-providers). Hosts can be in your own on-premises data center, in other cloud providers, or in edge computing environments. You can add compute hosts from different physical locations if you ensure that the requirements for the network speed and latency between the hosts are met.
- **Separate physical hosts**: Every compute host must have a separate physical host. The host might be a bare metal machine or a virtual machine that does not share the hypervisor with another virtual machine that you plan to add to your control plane. With this setup, you ensure that the outage of one physical machine does not lead to all control plane worker nodes becoming unavailable.

To make your control plane worker nodes more highly available, see the [Highly available control plane worker setup](#satellite-ha-setup).

<br />


## Highly available control plane worker setup
{: #satellite-ha-setup}

Make your {{site.data.keyword.satelliteshort}} control plane worker nodes more highly available to provide more compute capacity and prepare for a local data center or infrastructure provider failure.
{: shortdesc}

Depending on where your hosts are, the options that are available to you to increase availability might vary. Use the following table to determine how you can improve availability for your compute hosts. The options in this table are sorted with increased availability.

|High availability option|Description|
|-----------|------------------------|
|Add more compute hosts.|To increase compute capacity but also increase availability of your {{site.data.keyword.satelliteshort}} control plane, you can add more compute hosts as worker nodes to the control plane. Each host must meet the standards that are defined in the [basic control plane worker setup](#satellite-basic-setup). You can optionally add more hosts to your location without assigning them to your control plane. If the IBM Monitoring component detects a capacity issue in your location, unassigned hosts are automatically assigned as a worker node to your control plane. |
|Add redundant power, network, and storage.|To account for a power, network, or storage outage on one of your physical compute hosts, add redundant power, network, and storage configurations. This setup ensures that your compute hosts continue to run, even if hardware or software issues occur on the physical machine.|
|Isolate machines within one data center.|Compute hosts that are in the same data center or with the same cloud provider are often connected to the same power, network, and storage server. If one of these components experiences an outage, all compute hosts that are connected to the same component might be affected by the outage. To ensure that your compute hosts continue to run, plan to isolate your compute hosts as much as possible and not to share the same power, network, or storage devices. |
|Spread hosts across physical locations.|To account for a data center or cloud provider outage, you can spread your compute hosts across different physical locations. Keep in mind that compute hosts can only be in different physical locations if they still meet the networking speed and latency requirements that are defined in the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host) and any [provider-specific requirements](/docs/satellite?topic=satellite-providers).|
{: caption="High availability for the the location control plane worker nodes." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the high availability option. The second column is a description of the option."}


### Example for a high availability setup in an on-premises data center
{: #example-ha-onprem}

The following image shows a high availability setup of your control plane worker nodes within an on-premises data center. All compute hosts are on a separate rack to ensure that power, network, and storage devices are not shared. Because all compute hosts are located in the same data center, the requirements for networking speed and latency between the hosts are met.
{: shortdesc}

<img src="/images/satellite_ha_onprem.png" alt="High availability setup in an on-premises data center." width="550" style="width: 550px; border-style: none"/>

### Example for a high availability setup in another cloud provider
{: #example-ha-cloudprovider}

The following image shows a highly available setup for compute hosts that are in another cloud provider. All virtual machines are hosted on a separate physical machine that is dedicated to you only. To ensure further isolation and availability, all compute hosts are spread across multiple availability zones within the same metro. Because the availability zones belong to the same metro, the requirements for networking speed and latency between the hosts are met.
{: shortdesc}

<img src="/images/satellite_ha_aws.png" alt="High availability setup with compute hosts that are at another cloud provider." width="550" style="width: 550px; border-style: none"/>
