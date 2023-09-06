---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-06"

keywords: satellite, hybrid, multicloud, assigning hosts, host auto assignment, host auto assignment, host labels

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Assigning hosts with host label auto assignment
{: #host-autoassign-ov}

Available hosts can be  automatically assigned to worker pools in {{site.data.keyword.satelliteshort}} resources, such as a cluster or a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) when you use host labels. Host labels are used by worker pools to request compute capacity from available {{site.data.keyword.satelliteshort}} hosts with matching labels. You can disable and enable host auto assignment. Your host must be available before it can be assigned.
{: shortdesc}

Host auto assignment is not available for the {{site.data.keyword.satelliteshort}} location control plane. 
{: note}

## Host labels
{: #host-autoassign-about}

Host auto assignment works by matching labels from worker pools in {{site.data.keyword.satelliteshort}} clusters to the host and zone labels on available {{site.data.keyword.satelliteshort}} hosts.
{: shortdesc}

Keep in mind the following information about the host labels that are used for auto assignment.

Default host labels
:    When you attach a host to a {{site.data.keyword.satelliteshort}} location, the host automatically gets labels for `cpu`, `os`, and `memory` (in bytes). You cannot remove these labels. You can include additional host labels, or update the host metadata later. If the host does not include the `os` label, it is automatically assumed as `RHEL7`.

Hosts can have more labels than worker pools
:    For example, your host might have `cpu`, `memory`, and `env` host labels, but the requesting worker pool has only a `cpu` host label. Host auto assignment matches the `cpu` labels. Note that the reverse does not work. If a worker pool has more labels than a host, the host cannot be auto assigned to the worker pool.

Matching is exact
:    Host labels must equal (`=`) each other exactly. Even if the host label is a number, no less than (`<`), greater than (`>`), or other operators are used for matching.

## Example scenario for host auto assignment
{: #host-autoassign-example-scenario}

Say that you have a {{site.data.keyword.satelliteshort}} cluster with a `default` worker pool in `us-east-1a` and the following host labels.
- `cpu=4`
- `env=prod`
- `os=RHEL7`

Your {{site.data.keyword.satelliteshort}} location has available (unassigned) hosts with host labels as follows.
- Host A: `cpu=4, memory=32, env=prod, zone=us-east-1b` `os=rhel`
- Host B: `cpu=4, memory=32, zone=us-east-1a` `os=RHEL7`
- Host C: `cpu=4, memory=64, env=prod` `os=RHEL7`
- Host D: `cpu=4, memory=64, env=prod` `os=RHCOS`

If you resize the `default` worker pool to request 3 more worker nodes, only Host C can be automatically assigned, but not Host A or Host B.
- Host A meets the CPU and `env=prod` label requests, but can only be assigned in `us-east-1b`. Because the `default` worker pool is only in `us-east-1a`, Host A is not assigned.
- Host B meets the CPU and zone requests. However, the host does not have the `env=prod` label, and so is not assigned.
- Host C is automatically assigned because it matches the `cpu=4` and `env=prod` host labels, and does not have any zone restrictions. The `memory=64` host label is not considered, because the worker pool does not request a `memory` label.
- Host D meets the CPU, zone, and `env=prod` label requests, but does not meet the `os` request of `RHEL7`, and so is not assigned.

Hosts must be assigned as worker nodes in each zone of the default worker pool in your cluster. If your default worker pool spans multiple zones, ensure that you have hosts with matching labels that can be assigned in each zone.
{: note}

## Automatically assigning hosts
{: #host-autoassign}

{{site.data.keyword.satellitelong_notm}} can automatically assign hosts to worker pools in {{site.data.keyword.satelliteshort}} clusters that request compute capacity by using host labels, such as `cpu`.
{: shortdesc}

Before you begin, make sure that you create a {{site.data.keyword.satelliteshort}} cluster. If you create a cluster in the [CLI](/docs/openshift?topic=openshift-kubernetes-service-cli&interface=ui#cli_cluster-create-satellite), you must specify the `--workers` option with the number of hosts you want to automatically assign. Then, make sure that you [attach hosts](/docs/satellite?topic=satellite-attach-hosts) to your {{site.data.keyword.satelliteshort}} location, but do not assign the hosts to any resources. 

1. Review the host labels that the worker pools use to request compute capacity. You have several options.
    - [Create a worker pool in a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters) with the host labels that you want to use for auto assignment.
    - Review existing worker pools for their host labels. Note that you cannot update the host labels that a worker pool has. You can review the **Host labels** by running the `ibmcloud oc worker-pool get -c <cluster> --worker-pool <worker_pool>` command.
    - Review the host labels that a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster uses to request resources from the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service instance console.
2. Review the host labels that your available hosts have. Remember that hosts automatically get `cpu` and `memory` labels when you attach the host to your {{site.data.keyword.satelliteshort}} location.
    1. Get the {{site.data.keyword.satelliteshort}} location name.
        ```sh
        ibmcloud sat location ls
        ```
        {: pre}

    2. List the available (unassigned) hosts in your location, and note the IDs of the hosts that you want to check the labels for.
        ```sh
        ibmcloud sat host ls --location <location_name_or_ID> | grep unassigned
        ```
        {: pre}

    3. For each host that you want to check, get the host details and note the **Labels** in the output.
        ```sh
        ibmcloud sat host get --location <location_name_or_ID> --host <host_name_or_ID>
        ```
        {: pre}

        Example output
        ```sh
        ...
        Labels      
        app      webserver   
        cpu      4   
        memory   3874564
        os       RHCOS
        ...
        ```
        {: screen}

3. Update the labels of your available host to match all the labels of the worker pool that you want the host to be available for automatic assignment. You can optionally specify a zone to make sure that the host only gets automatically assigned to that zone.

    The `cpu`, `os`, and `memory` labels on the host are applied automatically and cannot be edited.
    {: note}

    ```sh
    ibmcloud sat host update --location <location_name_or_ID> --host <host_name_or_ID> -hl <key=value> [-hl <key=value>] [--zone <zone1>]
    ```
    {: pre}

## Disabling host auto assignment
{: #host-autoassign-disable}

The following actions disable host auto assignment for a worker pool. Later, you can [reenable host auto assignment](#host-autoassign-enable).
{: shortdesc}

- [Manually assign hosts to a worker pool](#host-assign-manual).
- [Delete an individual worker node from a worker pool](/docs/openshift?topic=openshift-satellite-clusters#satcluster-rm).

## Re-enabling host auto assignment
{: #host-autoassign-enable}

If you [disabled host auto assignment](#host-autoassign-disable), you can re-enable auto assignment.
{: shortdesc}

1. Make sure that you have [available hosts with labels that match the host labels of the worker pool](#host-autoassign).
2. [Resize the worker pool](/docs/openshift?topic=openshift-satellite-clusters) to set the requested size per zone, rebalance the worker pool, and enable auto assignment again.

