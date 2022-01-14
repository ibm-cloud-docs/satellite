---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-14"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}
{: #infrastructure-plan}

Plan how to set up your infrastructure environment to use with {{site.data.keyword.satellitelong}}. Your infrastructure environment can be an on-premises data center, in another cloud provider, or on compatible edge devices anywhere.
{: shortdesc}

Don't have your own infrastructure or want a managed solution? [Check out {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-satis-infra-about).
{: tip}

Your {{site.data.keyword.satelliteshort}} location starts with your infrastructure, such as another cloud provider or on-prem. Your infrastructure provides the basis for the hosts and zones that you use to build out your {{site.data.keyword.satelliteshort}} location. For more details on the different responsibilities for your infrastructure and {{site.data.keyword.satelliteshort}} resources, see [Your responsibilities](/docs/satellite?topic=satellite-responsibilities).
{: shortdesc}

![Concept overview of planning your infrastructure](/images/satellite-infra-plan.png){: caption="Figure 1. Your {{site.data.keyword.satelliteshort}} location is built atop the zones and hosts in your infrastructure provider." caption-side="bottom"}

1. Choose the infrastructure provider that you want to use to create a {{site.data.keyword.satelliteshort}} location.
    On-premises
    :    You can use a data center with existing infrastructure, or order infrastructure from {{site.data.keyword.IBM_notm}} with [{{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-satis-infra-about). You might not even have a data center, but rather an edge location that meets the minimum hardware requirements, such as three racks at one of your company's local sites.
    
    Non-{{site.data.keyword.IBM_notm}} cloud provider
    :    You can use a cloud provider of your choice, such as Amazon Web Services (AWS), Google Cloud Platform (GCP), or Microsoft Azure. For more information, see [Cloud infrastructure like AWS, Azure, and GCP](/docs/satellite?topic=satellite-infrastructure-plan).
    
    {{site.data.keyword.cloud_notm}}
    :    You can use {{site.data.keyword.cloud_notm}}. For more information, see [Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ibm).
    
2. In your infrastructure provider, identify a multizone location that meets the latency requirements.

    Multizone
    :    Your location must have at least three zones that are physically separate so that you can spread out hosts evenly across the zones to increase [high availability](/docs/satellite?topic=satellite-ha). For example, your cloud provider might have three different zones within the same region, or you might use three racks with three separate networking and power supply systems in an on-prem environment.
    
    Latency between {{site.data.keyword.cloud_notm}} and the location
    :    The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane like {{site.data.keyword.openshiftshort}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).
    
    Latency between hosts in your location
    :    Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or {{site.data.keyword.satelliteshort}}-enabled service. For example, in cloud providers such as AWS, this setup typically means that all of the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled service degradation, and in extreme cases, failures in your cluster applications.
        
3. In each of the three zones in your infrastructure provider, plan to create compatible hosts to add to {{site.data.keyword.satelliteshort}}. The host instances in your infrastructure provider become the compute hosts to run the services in your {{site.data.keyword.satelliteshort}} location, similar to the worker nodes in a {{site.data.keyword.openshiftlong_notm}} cluster.
    - Each host must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}, such as RHEL 7 operating system; at least 4 CPU, 16 RAM, and 100 GB storage per host; full network connectivity between hosts in the same location; and more.
    - To calculate how many hosts you need, see [Sizing your {{site.data.keyword.satelliteshort}} location](#location-sizing).
    
4. Use your infrastructure to power your {{site.data.keyword.satelliteshort}} resources.

    1. Create your {{site.data.keyword.satelliteshort}} location based on the zones and hosts in your infrastructure provider. 
    2. [Assign hosts](/docs/satellite?topic=satellite-hosts) to {{site.data.keyword.satelliteshort}}-enabled services like {{site.data.keyword.openshiftlong_notm}} clusters.







