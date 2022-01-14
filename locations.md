---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-13"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane
{: #control-plane-scale}

As you attach more resources to your {{site.data.keyword.satelliteshort}} location, such as {{site.data.keyword.openshiftlong_notm}} clusters, your location control plane needs more compute capacity to maintain the location. You can assign hosts to the control plane to increase the compute capacity.
{: shortdesc}

## How do I know when to attach capacity to the {{site.data.keyword.satelliteshort}} location control plane?
{: #control-plane-attach-capacity}

When you list locations, such as with the `ibmcloud sat location ls` command or in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, the location enters an `Action required` health state. You see warning messages similar to the following example.

```sh
Hosts in the location control plane are running out of disk space.

Hosts in the location control plane have critical CPU or memory usage issues.

The location control plane is running at max capacity and cannot support any more workloads.
```
{: screen}

## How many {{site.data.keyword.openshiftlong_notm}} clusters can I run before I need to attach capacity to the location control plane?
{: #control-plane-how-many-clusters}

The number of clusters depends on the size of your clusters and the size of the hosts that you use for the {{site.data.keyword.satelliteshort}} location control plane. You must scale up the control plane hosts in multiples of 3, such as 6, 9, or 12.

The following tables provide examples of the number of hosts that the control plane must have to run the masters for various combinations of clusters and worker nodes, for informational purposes only. 

- The size of the hosts that run the control plane, **4 vCPU and 16GB RAM** or **16 vCPU and 64GB RAM**, affect the numbers of clusters and worker nodes that are possible in the location. Keep in mind that actual performance requirements depend on many factors, such as the underlying CPU performance and control plane usage by the applications that run in the location.
- You can assign hosts to the control plane in groups of 3. The table presents examples up to 12 hosts as common configurations to give you an idea of how you might size the control plane for your host and application environment. Note that you can add more than 12 hosts to your control plane in groups of 3. For example you might create a control plane with 18 or 27 hosts.

| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 3 hosts | 1 cluster, for demonstration purposes | Up to 20 workers | 20 workers per cluster |
| 6 hosts | Up to 5 clusters  | 20 workers across 5 clusters, or 80 workers across 2 clusters | 60 workers per cluster |
| 9 hosts |  Up to 8 clusters | 40 workers across 8 clusters, or 140 workers across 3 clusters | 60 workers per cluster |
| 12 hosts |  Up to 11 clusters | 60 workers across 11 clusters, or 200 workers across 4 clusters | 60 workers per cluster |
{: caption="Sizing guidance for example numbers of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run the master components for various combinations of clusters and worker nodes in the location." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the number of clusters that you want to run in the location. The second column describes the number of hosts that the location control plane must have to run the masters for those clusters."}
{: class="simple-tab-table"}
{: #4cpu-16ram}
{: tab-title="4 vCPU, 16GB RAM"}
{: tab-group="loc-size"}

| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 3 hosts | 1 cluster, for demonstration purposes | Up to 100 workers | 100 workers per cluster |
| 6 hosts | Up to 20 clusters | 200 workers across 20 clusters, or 550 workers across 2 clusters | 300 workers per cluster |
| 9 hosts  | Up to 26 clusters | 400 workers across 26 clusters, or 850 workers across 3 clusters | 300 workers per cluster |
| 12 hosts  | Up to 36 clusters | 520 workers across 26 clusters, or 1150 workers across 4 clusters | 300 workers per cluster |
{: caption="Sizing guidance for example numbers of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run the master components for various combinations of clusters and worker nodes in the location." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the number of clusters that you want to run in the location. The second column describes the number of hosts that the location control plane must have to run the masters for those clusters."}
{: class="simple-tab-table"}
{: #16cpu-64ram}
{: tab-title="16 vCPU, 64GB RAM"}
{: tab-group="loc-size"}

## How do I scale up my {{site.data.keyword.satelliteshort}} location control plane to be highly available?
{: #control-plane-scale-up}

See [Highly available control plane worker setup](/docs/satellite?topic=satellite-ha#satellite-ha-setup). Make sure to attach hosts to the control plane location in each zone, in multiples of three. For example, you might have 6 hosts that are assigned to your control plane location that is managed from the {{site.data.keyword.cloud_notm}} `wdc` region, with 2 hosts each zone (`us-east-1`, `us-east-2`, and `us-east-3`).

To scale up your control plane, you can follow the same steps to [Set up the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
