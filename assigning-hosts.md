---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-18"

keywords: satellite, hybrid, multicloud, assigning hosts, host auto assignment, host auto assignment, host labels

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Assigning hosts to worker pools
{: #assigning-hosts}

After you have attached your hosts to {{site.data.keyword.satellitelong_notm}}, you can assign them to the control plane or to worker pools.
{: shortdesc}

## Using host auto assignment
{: #host-autoassign-ov}

By default, available hosts are automatically assigned to worker pools in {{site.data.keyword.satelliteshort}} resources, such as a cluster or a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). The worker pools use host labels to request compute capacity from available {{site.data.keyword.satelliteshort}} hosts with matching labels. You can disable and enable host auto assignment.
{: shortdesc}

When you assign hosts, you are charged a {{site.data.keyword.satelliteshort}} management fee per host vCPU. [Learn more](/docs/satellite?topic=satellite-faqs#pricing).
{: note}

Host auto assignment is not available for the {{site.data.keyword.satelliteshort}} location control plane. Your cluster must be available before it can be assigned.
{: note}

### Host labels
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

#### Example scenario for host auto assignment
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

### Automatically assigning hosts
{: #host-autoassign}

{{site.data.keyword.satellitelong_notm}} can automatically assign hosts to worker pools in {{site.data.keyword.satelliteshort}} clusters that request compute capacity by using host labels, such as `cpu`.
{: shortdesc}

Before you begin, make sure that you [attach hosts](/docs/satellite?topic=satellite-attach-hosts) to your {{site.data.keyword.satelliteshort}} location, but do not assign the hosts.

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

### Disabling host auto assignment
{: #host-autoassign-disable}

The following actions disable host auto assignment for a worker pool. Later, you can [reenable host auto assignment](#host-autoassign-enable).
{: shortdesc}

- [Manually assign hosts to a worker pool](#host-assign-manual).
- [Delete an individual worker node from a worker pool](/docs/openshift?topic=openshift-satellite-clusters#satcluster-rm).

### Re-enabling host auto assignment
{: #host-autoassign-enable}

If you [disabled host auto assignment](#host-autoassign-disable), you can re-enable auto assignment.
{: shortdesc}

1. Make sure that you have [available hosts with labels that match the host labels of the worker pool](#host-autoassign).
2. [Resize the worker pool](/docs/openshift?topic=openshift-satellite-clusters) to set the requested size per zone, rebalance the worker pool, and enable auto assignment again.

## Manually assigning hosts to {{site.data.keyword.satelliteshort}} resources
{: #host-assign-manual}

After you attach hosts to a {{site.data.keyword.satelliteshort}} location, you assign them to {{site.data.keyword.satelliteshort}} resources to provide compute capacity, such as clusters or {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

You can also use [host auto assignment](#host-autoassign-ov) for worker pools in {{site.data.keyword.satelliteshort}} clusters. However, you must manually assign hosts to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
{: tip}

When you assign hosts, you are charged a {{site.data.keyword.satelliteshort}} management fee per host vCPU. [Learn more](/docs/satellite?topic=satellite-faqs#pricing).
{: note}

Before you begin,

1. Make sure that you have the {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
2. [Attach hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-attach-hosts), and check that the hosts are healthy and **unassigned**.

### Assigning hosts from the console
{: #host-assign-ui}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Locations**.
2. Select the location where you attached the host machines that you want to assign to your {{site.data.keyword.satelliteshort}} resource.
3. In the **Hosts** tab, from the actions menu of each host machine that you want to add to your resource, click **Assign host**.
4. Select the cluster that you created, and choose one of the available zones. When you assign the hosts to a cluster, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the Health of your machine changes from **Ready** to **Provisioning**.
5. Verify that your hosts are successfully assigned to the cluster. The assignment is successful when an IP address is added to your host and the **Health** status changes to **Normal**.
6. Repeat these steps to ensure that hosts are assigned as worker nodes in each zone of the default worker pool in your cluster.

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
    {: note}

### Assigning hosts from the CLI
{: #host-assign-cli}

1. List the hosts in your location and find the ones that are in an **unassigned** status.
    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

2. Assign at least 3 compute hosts from your location as worker nodes to your {{site.data.keyword.satelliteshort}} control plane or an existing {{site.data.keyword.openshiftlong_notm}} cluster. When you assign the host, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. You can choose to assign a host by using the host ID, or you can also define the label that the host must have to be assigned to the location.

    - The following example assigns a host by using the host ID.
        ```sh
        ibmcloud sat host assign --location <location_name_or_ID>  --cluster <cluster_name_or_ID> --host <host_ID> --worker-pool default --zone <zone>
        ```
        {: pre}

    - The following example assigns a host by using the `use:satcluster` label.
        ```sh
        ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --host-label "use:satcluster" --worker-pool default --zone us-east-1
        ```
        {: pre}
    
        | Component             | Description      | 
        |--------------------|------------------|
        | `--location <location_name_or_ID>` | Enter the name or ID of the location where you created the cluster. To retrieve the location name or ID, run `ibmcloud sat location ls`.  | 
        | `--cluster <cluster_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.openshiftlong_notm}} cluster that you created earlier. To retrieve the cluster name or ID, run `ibmcloud ks cluster ls`. If you want to assign hosts to the {{site.data.keyword.satelliteshort}} control plane, you must enter the location ID as your cluster ID. To retrieve the location ID, run `ibmcloud sat location ls`. |
        | `--host <host_name_or_ID>` | Enter the host ID to assign as worker nodes to the {{site.data.keyword.satelliteshort}} resource. To view the host ID, run `ibmcloud sat host ls --location <location_name>`. You can also use the `--host-label` option to identify the host that you want to assign to your cluster. |
        | `--host-label <label>` | Enter the label that you want to use to identify the host that you want to assign. The label must be a key-value pair, and must exist on the host machine. When you run this command with the `label` option, the first host that is in an `unassigned` state and matches the label is assigned to your {{site.data.keyword.satelliteshort}} resource. |
        | `--worker-pool <worker-pool>` | Enter the name of the worker pool where you want to attach your compute hosts. To find available worker pools in your cluster, run `ibmcloud oc worker-pool ls --cluster <cluster_name_or_ID>`. If you do not specify this option, your compute host is automatically added to the default worker pool. |
        | `--zone <zone>` | The name of the zone where you want to assign the compute host. To see the zone names for your location, run `ibmcloud sat location get --location` and look for the `Host Zones` field. |
        {: caption="Table 1. Understanding this command's components" caption-side="top"}
        {: summary="This table is read from left to right. The first column has the command component. The second column has the description of the command."}
        
    - The following example assigns a host by using the `os=RHCOS` host label.
        ```sh
        ibmcloud sat host assign --location <location_name_or_ID>  --cluster <cluster_name_or_ID> --host-label os=RHCOS --zone <zone>
        ```
        {: pre}

3. Repeat the previous step for all compute hosts that you want to assign as worker nodes to your {{site.data.keyword.satelliteshort}} resource.
4. Wait a few minutes until the bootstrapping process for all computes hosts is complete and your hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource. All hosts are assigned a cluster, a worker node ID, and an IP address.

    You can monitor the bootstrapping process of your compute hosts by running `ibmcloud ks worker get --cluster <cluster_name_or_ID> --worker <worker_ID>`.
    {: tip}

    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    Retrieving hosts...
    OK
    Name              ID                     State      Status   Cluster             Worker ID                                                 Worker IP   
    satcluster        brkijjq20r3nd89b1sog   assigned   Ready    satcluster          sat-satc-dd629ca11947c4aaec1a0208e0a37ca790475ee0   169.62.196.24   
    satcluster2       brkiku2202thnmhb1sp0   assigned   Ready    satcluster          sat-satc-69f2410d3ecfea9127aeec07f01475f241728a16   169.62.196.22   
    satcluster3       brkiltb20m0oqr29mo2g   assigned   Ready    satcluster          sat-satc-9985014f499827ddde3ce3e5bedad26af5a73263   169.62.196.29   
    controlplane01    brjrgp920bg4u254brr0   assigned   Ready    infrastructure      sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.62.196.30   
    controlplane02    brjrdmd20dfjgaai4vc0   assigned   Ready    infrastructure      sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.62.196.23   
    controlplane03    brjs18u20ohqh54svnog   assigned   Ready    infrastructure      sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.62.196.20   
    ```
    {: screen}
    
