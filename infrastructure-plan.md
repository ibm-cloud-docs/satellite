---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Planning your environment for {{site.data.keyword.satelliteshort}}
{: #infrastructure-plan}

Plan how to set up your infrastructure environment to use with {{site.data.keyword.satellitelong}}. Your infrastructure environment can be an on-premises data center, in a public cloud provider, or on compatible edge devices anywhere.
{: shortdesc}

Don't have your own infrastructure or want a managed solution? [Check out {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: tip}

Your {{site.data.keyword.satelliteshort}} location starts with your infrastructure, such as a public cloud provider or on-prem. Your infrastructure provides the basis for the hosts and zones that you use to build out your {{site.data.keyword.satelliteshort}} location. For more details on the different responsibilities for your infrastructure and {{site.data.keyword.satelliteshort}} resources, see [Your responsibilities](/docs/satellite?topic=satellite-responsibilities).
{: shortdesc}

![Concept overview of planning your infrastructure](/images/satellite-infra-plan.png){: caption="Figure 1. Your {{site.data.keyword.satelliteshort}} location is built atop the zones and hosts in your infrastructure provider." caption-side="bottom"}

## Planning your infrastructure
{: #infra-plan-infra}

### Plan your infrastructure provider
{: #infra-plan-provider}

Choose the infrastructure provider that you want to use to create a {{site.data.keyword.satelliteshort}} location.

On-premises
:    You can use a data center with existing infrastructure, or order infrastructure from {{site.data.keyword.IBM_notm}} with [{{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service). You might not even have a data center, but rather an edge location that meets the minimum hardware requirements, such as three racks at one of your company's local sites.
    
Non-{{site.data.keyword.IBM_notm}} cloud provider
:    You can use a cloud provider of your choice, such as [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws), [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp), [Microsoft Azure](/docs/satellite?topic=satellite-azure), or [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba).
    
{{site.data.keyword.cloud_notm}}
:    You can use [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) for testing purposes.

### Plan for a multizone location
{: #infra-plan-multizone}

In your infrastructure provider, identify a multizone location that meets the latency requirements.

Multizone
:    Your location must have at least three zones that are physically separate so that you can spread out hosts evenly across the zones to increase [high availability](/docs/satellite?topic=satellite-ha). For example, your cloud provider might have three different zones within the same region, or you might use three racks with three separate networking and power supply systems in an on-prem environment.
    
Latency between {{site.data.keyword.cloud_notm}} and the location
:    The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).
    
Latency between hosts in your location
:    Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.

### Plan your host systems
{: #infra-plan-compatible}

In each of the three zones in your infrastructure provider, plan to create compatible hosts to add to {{site.data.keyword.satelliteshort}}. The host instances in your infrastructure provider become the compute hosts to create your location control plane or to run the services in your {{site.data.keyword.satelliteshort}} location, similar to the worker nodes in a {{site.data.keyword.redhat_openshift_notm}} cluster.
- Each host must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}.
- To calculate how many hosts you need, see [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).

To check your host set up, you can use the satellite-host-check script. For more information, see Checking your host set up. {: tip}

## Planning your operating system
{: #infras-plan-os}
  
Choose your operating system for your hosts. You can choose Red Hat Enterprise Linux or Red Hat CoreOS. If you want to use Red Hat CoreOS for your managed services such as worker nodes, you must create and enable a new location to use it. See [Creating a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

Red Hat Enterprise Linux 7
:    Select `RHEL7` to choose Red Hat Enterprise Linux 7 (RHEL 7. RHEL 7 is the default operating system supported for {{site.data.keyword.satelliteshort}} hosts on {{site.data.keyword.redhat_openshift_notm}} version 4.9 or earlier. 
    
Red Hat CoreOS
:    Select `RHCOS` to choose Red Hat CoreOS (RHCOS). RHCOS is a minimal operating system for running containerized workloads securely and at scale. It is based on RHEL and includes automated, remote upgrade features. For more information about the key benefits of RHCOS, see [Red Hat Enterprise Linux CoreOS (RHCOS)](https://docs.openshift.com/container-platform/4.10/architecture/architecture-rhcos.html){: external}.

Red Hat CoreOS is available only in the Dallas (`us-south`), Frankfurt (`eu-de`), Tokyo (`jp-tok`), London (`eu-gb`), and Washington D.C. (`us-east`) regions and for only {{site.data.keyword.redhat_openshift_notm}} version 4.9 and 4.10.
{: note}



## Deciding how to create your {{site.data.keyword.satelliteshort}} location
{: #create-options}

Depending on your infrastructure provider, you have different options to create a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### On-premises infrastructure
{: #create-options-onprem}

For on-prem infrastructure, you can manually setup a {{site.data.keyword.satelliteshort}} location. For more information, see [Manually creating Satellite locations](/docs/satellite?topic=satellite-locations#location-create-manual).
{: shortdesc}

### Cloud provider infrastructure
{: #create-options-cloud}

For cloud provider infrastructure, you can follow provider-specific guides. For more information, see [Creating a Satellite location](/docs/satellite?topic=satellite-locations).
{: shortdesc}

### {{site.data.keyword.IBM_notm}}-managed infrastructure
{: #create-options-sat-is}

{{site.data.keyword.IBM_notm}} can send infrastructure and set up a {{site.data.keyword.satelliteshort}} location for you. See [Getting started with {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: shortdesc}

## Providing {{site.data.keyword.satelliteshort}} with credentials to your cloud provider
{: #infra-credentials}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide credentials to the cloud provider. For example, you might [automate your location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template) or use a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service that sets up a {{site.data.keyword.satelliteshort}} location for you.
{: shortdesc}

The credentials that you provide are stored and encrypted in etcd of the {{site.data.keyword.satelliteshort}} location control plane master. For more information, see [Securing your data](/docs/satellite?topic=satellite-data-security).





