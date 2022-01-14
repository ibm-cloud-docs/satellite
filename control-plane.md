---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-14"

keywords: satellite, hybrid, multicloud

subcollection: satellite-working

---

{{site.data.keyword.attribute-definition-list}}


# Understanding Satellite locations
{: #about-locations}

A location is a representation of an environment in your infrastructure provider, such as an on-prem data center or cloud, that you want to bring {{site.data.keyword.cloud_notm}} services to so that you can run workloads in your own environment. You create the location by attaching host machines from across at least 3 zones in your infrastructure. 
{: shortdesc}

To set up a Satellite location, you must first create the location, attach hosts to it, and then create the location control plane.

The location control plane runs resources that are managed by Satellite to help manage the hosts, clusters, and other resources that you attach to the location.

When you set up the {{site.data.keyword.satelliteshort}} location control plane, keep in mind the following host considerations.	
{: important}	

- Hosts must meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs). For more information about how to configure hosts in other cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).	
- Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or {{site.data.keyword.satelliteshort}}-enabled service. For example, in cloud providers such as AWS, this setup typically means that all of the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled service degradation, and in extreme cases, failures in your cluster applications.	
- Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then {{site.data.keyword.IBM_notm}} can automatically assign hosts when clusters or the {{site.data.keyword.satelliteshort}} location control plane request more capacity.

## Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you attach to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

1. Set up a highly available {{site.data.keyword.satelliteshort}} location control plane with enough compute capacity to manage the resources in your {{site.data.keyword.satelliteshort}} location.
    Minimum size
    :    To get started, you must attach and assign hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs). For testing purposes such as a proof of concept, you can have a minimum of 3 hosts assigned to the control plane, but for production purposes, you must have a minimum of 6 hosts. As you continue to use your location, [you might need to scale the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-control-plane-scale) in multiples of 3, such as 6, 9, or 12 hosts.
    
    High availability
    :    When you assign hosts to the {{site.data.keyword.satelliteshort}} location control plane, assign the hosts evenly across each of the 3 available zones of your {{site.data.keyword.cloud_notm}} multizone metro that you selected during location creation. To make the control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might assign 2 hosts each that run in 3 separate availability zones in your cloud provider, or that run in 3 separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable. For more information, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).
    
    Compute capacity
    :    {{site.data.keyword.satelliteshort}} monitors the available compute capacity of your location. When the location reaches 70% capacity, you see a warning status to notify you to attach more hosts to the location. If the location reaches 80% capacity, the status changes to **critical** and you see another warning that tells you to attach more hosts to the location.
    
2. Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then {{site.data.keyword.IBM_notm}} can assign the hosts to your {{site.data.keyword.satelliteshort}} location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.

3. To decide on the size and number of hosts to attach to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations:
    - How many resources does my app require?
    - What else besides my app might use resources in the cluster?
    - What type of availability do I want my workload to have?
    - How many worker nodes (hosts) do I need to handle my workload?
    - How do I monitor resource usage and capacity in my cluster?
    
