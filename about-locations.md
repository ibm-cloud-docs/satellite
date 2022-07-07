---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud, location, locations, control plane

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Understanding Satellite locations
{: #about-locations}

A location in {{site.data.keyword.satellitelong}} is a representation of an environment in your infrastructure provider, such as an on-prem data center or public cloud, that you want to bring {{site.data.keyword.cloud_notm}} services to so that you can run workloads in your own environment. You create the location by attaching host machines from across at least 3 zones in your infrastructure. 
{: shortdesc}

To set up a Satellite location, you must first create the location, attach hosts to it, and then create the location control plane. 


To use [Red Hat CoreOS (RHCOS)](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os) for a managed service such as worker nodes, you must enable it in your location. You can only enable new locations for RHCOS; you cannot migrate an existing location. For more information, see [Creating a location](/docs/satellite?topic=satellite-locations). Red Hat CoreOS is available only in the Dallas (`us-south`), Frankfurt (`eu-de`), Tokyo (`jp-tok`), London (`eu-gb`), and Washington D.C. (`us-east`) regions and for only {{site.data.keyword.redhat_openshift_notm}} version 4.9 and 4.10. Want to verify if your location is enabled for Red Hat CoreOS? See [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location).
{: note}

## Satellite location control plane
{: #loc-control-plane}

The location control plane runs resources that are managed by Satellite to help manage the hosts, clusters, and other resources that you attach to the location.

When you set up the {{site.data.keyword.satelliteshort}} location control plane, keep in mind the following host considerations.	

- Hosts must meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs). For more information about how to configure hosts in other cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-locations).	
- Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.	
- Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then {{site.data.keyword.IBM_notm}} can automatically assign hosts when clusters or the {{site.data.keyword.satelliteshort}} location control plane request more capacity.

{{site.data.keyword.satelliteshort}} uses {{site.data.keyword.cos_short}} to store data about your location and backups for your location's clusters. You can choose to have a bucket created automatically when you create your location or specify an existing bucket. If you want to use an existing bucket, it must have cross-regional resiliency.


## I created a {{site.data.keyword.satelliteshort}} location, what comes next?
{: #loc-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.

