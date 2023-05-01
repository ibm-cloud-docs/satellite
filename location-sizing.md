---

copyright:
  years: 2022, 2023
lastupdated: "2023-05-01"

keywords: satellite, hybrid, multicloud, location, locations, control plane, sizing

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you attach to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

Minimum size requirements
    :   To get started, you must attach and assign hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs). For testing purposes such as a proof of concept, you can have a minimum of 3 hosts assigned to the control plane, but for production purposes, you must have a minimum of 6 hosts. As you continue to use your location, [you might need to scale the {{site.data.keyword.satelliteshort}} location control plane](#control-plane-attach-capacity) in multiples of 3, such as 6, 9, or 12 hosts.
    
High availability
:   When you assign hosts to the {{site.data.keyword.satelliteshort}} location control plane, assign the hosts evenly across each of the 3 available zones of your {{site.data.keyword.cloud_notm}} multizone metro that you selected during location creation. To make the control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might assign 2 hosts each that run in 3 separate availability zones in your cloud provider, or that run in 3 separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable. For more information, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).

Compute capacity
:   {{site.data.keyword.satelliteshort}} monitors the available compute capacity of your location. When the location reaches 70% capacity, you see a warning status to notify you to attach more hosts to the location. If the location reaches 80% capacity, the status changes to **critical** and you see another warning that tells you to attach more hosts to the location.
    
Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then {{site.data.keyword.IBM_notm}} can assign the hosts to your {{site.data.keyword.satelliteshort}} location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.
{: note}


## Location size for non Red Hat CoreOS enabled location
{: #control-plane-how-many-clusters-rhel}

The following tables show sizing guidance for the number of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run the master components for various combinations of clusters and worker nodes in a non Red Hat CoreOS enabled location. These sizings are for reference only. Your sizing requirements can increase depending on the amount of workload running in a cluster. For more information, see [What types of changes can increase my location sizing requirements?](#types-changes-sizing-increase).

While you can deploy a cluster to a location with only 3 control plane hosts, use this type of bare minimum setup for only testing and proof of concept purposes.
{: note}



| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 6 hosts | Up to 5 clusters  | 20 workers across 5 clusters, or 80 workers across 2 clusters | 60 workers per cluster |
| 9 hosts |  Up to 8 clusters | 40 workers across 8 clusters, or 140 workers across 3 clusters | 60 workers per cluster |
| 12 hosts |  Up to 11 clusters | 60 workers across 11 clusters, or 200 workers across 4 clusters | 60 workers per cluster |
{: caption="Sizing guidance for the {{site.data.keyword.satelliteshort}} location control plane" caption-side="bottom"}
{: class="simple-tab-table"}
{: #4cpu-16ram}
{: tab-title="4 vCPU, 16 GB RAM"}
{: tab-group="loc-size"}

| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 6 hosts | Up to 20 clusters | 200 workers across 20 clusters, or 550 workers across 2 clusters | 300 workers per cluster |
| 9 hosts  | Up to 26 clusters | 400 workers across 26 clusters, or 850 workers across 3 clusters | 300 workers per cluster |
| 12 hosts  | Up to 36 clusters | 520 workers across 26 clusters, or 1150 workers across 4 clusters | 300 workers per cluster |
{: caption="Sizing guidance for the {{site.data.keyword.satelliteshort}} location control plane" caption-side="bottom"}
{: class="simple-tab-table"}
{: #16cpu-64ram}
{: tab-title="16 vCPU, 64 GB RAM"}
{: tab-group="loc-size"}

## Location size for Red Hat CoreOS (RHCOS) enabled location
{: #control-plane-how-many-clusters-rhcos}

The following tables show sizing guidance for the number of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run the master components for various combinations of clusters and worker nodes in a Red Hat CoreOS enabled location. These sizings are for reference only. Your sizing requirements can increase depending on the amount of workload running in a cluster. For more information, see [What types of changes can increase my location sizing requirements?](#types-changes-sizing-increase).

While you can deploy a cluster to a location that uses 3 hosts with 4 vCPU and 16 GB for Red Hat CoreOS enabled locations, use this type of bare minimum setup for only testing and proof of concept purposes.
{: note}



| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 6 hosts | Up to 3 clusters | 20 workers across 3 clusters, or 80 workers across 2 clusters | 60 workers per cluster |
| 9 hosts | Up to 5 clusters  | 40 workers across 5 clusters, or 140 workers across 3 clusters | 60 workers per cluster |
| 12 hosts | Up to 8 clusters | 60 workers across 8 clusters, or 200 workers across 4 clusters | 60 workers per cluster |
{: caption="Sizing guidance for the {{site.data.keyword.satelliteshort}} location control plane" caption-side="bottom"}
{: class="simple-tab-table"}
{: #4cpu-16ram-coreos}
{: tab-title="4 vCPU, 16 GB RAM"}
{: tab-group="loc-sizerhcos"}

| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 6 hosts | Up to 9 clusters | 200 workers across 9 clusters, or 550 workers across 2 clusters | 300 workers per cluster |
| 9 hosts  | Up to 18 clusters | 400 workers across 18 clusters, or 850 workers across 3 clusters | 300 workers per cluster |
| 12 hosts  | Up to 26 clusters | 520 workers across 26 clusters, or 1150 workers across 4 clusters | 300 workers per cluster |
{: caption="Sizing guidance for the {{site.data.keyword.satelliteshort}} location control plane" caption-side="bottom"}
{: class="simple-tab-table"}
{: #16cpu-64ram-coreos}
{: tab-title="16 vCPU, 64 GB RAM"}
{: tab-group="loc-sizerhcos"}


## Location size for testing
{: #control-plane-how-many-clusters-test}

The following table shows sizing guidance for the number of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run a {{site.data.keyword.satelliteshort}} location demonstration. This configuration is not intended for production use. 


 
For a non-RHCOS enabled location, your hosts must have at least 4 vCPU and 16 GB RAM. For an RHCOS enabled location, your hosts must have at least 8 vCPU and 32 GB RAM. 

| Number of control plane hosts | Max clusters in location |  Max cluster size |
| --- | --- | --- |
| 3 hosts 4x16 | Up to 3 clusters | 20 workers per cluster |
{: class="simple-tab-table"}
{: #demo-non-rhcos}
{: tab-title="Non RHCDOS enabled location"}
{: tab-group="loc-demo"}
{: caption="Sizing guidance for demonstrations" caption-side="bottom"}

| Number of control plane hosts | Max clusters in location |  Max cluster size |
| --- | --- | --- |
| 3 hosts 8x32 | Up to 3 clusters | 20 workers per cluster |
{: class="simple-tab-table"}
{: #demo-rhcos}
{: tab-title="RHCDOS enabled location"}
{: tab-group="loc-demo"}
{: caption="Sizing guidance for demonstrations" caption-side="bottom"}


## FAQs about location sizing
{: #location-sizing-faq}

Review the following frequently asked questions for more information about sizing your location.

### How do I know what size and number of hosts to attach to my cluster?
{: #cluster-size-number}

To decide on the size and number of hosts to attach to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations.
- How many resources does my app require?
- What else besides my app might use resources in the cluster?
- What type of availability do I want my workload to have?
- How many worker nodes (hosts) do I need to handle my workload?
- How do I monitor resource usage and capacity in my cluster?

### How do I know when to attach capacity to the {{site.data.keyword.satelliteshort}} location control plane?
{: #control-plane-attach-capacity}

When you list locations, such as with the `ibmcloud sat location ls` command or in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, the location enters an `Action required` health state. You see warning messages similar to the following example.

```sh
Hosts in the location control plane are running out of disk space.

Hosts in the location control plane have critical CPU or memory usage issues.

The location control plane is running at max capacity and cannot support any more workloads.
```
{: screen}

After you determine the size of your location, [add hosts to the location control plane](/docs/satellite?topic=satellite-attach-hosts).

### How do I scale up my {{site.data.keyword.satelliteshort}} location control plane to be highly available?
{: #control-plane-scale-up}

See [Highly available control plane worker setup](/docs/satellite?topic=satellite-ha#satellite-ha-setup). Make sure to attach hosts to the control plane location in each zone, in multiples of three. For example, you might have 6 hosts that are assigned to your control plane location that is managed from the {{site.data.keyword.cloud_notm}} `wdc` region, with 2 hosts each zone (`us-east-1`, `us-east-2`, and `us-east-3`).

To scale up your control plane, you can follow the same steps to [Set up the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-setup-control-plane).

### How many {{site.data.keyword.openshiftlong_notm}} clusters can I run before I need to attach capacity to the location control plane?
{: #control-plane-how-many-clusters}

The number of clusters depends on the size of your clusters and the size of the hosts that you use for the {{site.data.keyword.satelliteshort}} location control plane. You must scale up the control plane hosts in multiples of 3, such as 6, 9, or 12.

The following tables provide examples of the number of hosts that the control plane must have to run the masters for various combinations of clusters and worker nodes, for informational purposes only. 

- The size of the hosts that run the control plane, **4 vCPU and 16GB RAM** or **16 vCPU and 64GB RAM**, affect the numbers of clusters and worker nodes that are possible in the location. Keep in mind that actual performance requirements depend on many factors, such as the underlying CPU performance and control plane usage by the applications that run in the location.
- You can assign hosts to the control plane in groups of 3. The table presents examples up to 12 hosts as common configurations to give you an idea of how you might size the control plane for your host and application environment. Note that you can add more than 12 hosts to your control plane in groups of 3. For example you might create a control plane with 18 or 27 hosts.

### What types of changes can increase my location sizing requirements? 
{: #types-changes-sizing-increase}

Your sizing requirements can increase depending on the amount of workload that is running in a cluster. The following examples can cause your sizing requirements for your location to increase.

- Large amounts of a dynamic pod workload, such as more storage required to hold all pod, service, or app metadata.
- Large amounts of configuration information, such as ConfigMaps and Secrets, which can lead to increased memory or CPU of control plane that is holding or processing that information.
- Aggregated `kube-apiserver` request workload and response sizes of data gathered. For example, if your cluster contains many ConfigMaps and an application queries for the full list of that data, that request can cause the control plane to require more resources.
