---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-08"

keywords: satellite, hybrid, multicloud, os upgrade, operating system, security patch

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Setting up {{site.data.keyword.satelliteshort}} hosts
{: #hosts}

Set up {{site.data.keyword.satellitelong}} hosts to bring your own infrastructure to a {{site.data.keyword.satelliteshort}} location. Then, your hosts can be used as compute capacity to run {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

## Understanding {{site.data.keyword.satelliteshort}} hosts
{: #host-concept}

A {{site.data.keyword.satelliteshort}} host represents a compute machine of your own infrastructure, such as an on-premises data center or another cloud provider. You can attach hosts to a {{site.data.keyword.satelliteshort}} location, and then assign the hosts to run {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

The following diagram presents the initial setup steps for hosts.

![Concept overview of Satellite host setup](/images/host-process.png){: caption="Figure 1. The initial setup process for {{site.data.keyword.satelliteshort}} hosts." caption-side="bottom"}

1. **Attach**: Your machine becomes a {{site.data.keyword.satelliteshort}} host after you successfully [attach the host](#attach-hosts) to a {{site.data.keyword.satelliteshort}} location by running a registration script on the machine. Your machine must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs). For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud). After the host is attached and a heartbeat can be detected, its health is **ready** and its status is **unassigned**. You can still log in to the machine via SSH to troubleshoot any issues.

2. **Assign**: The hosts in your {{site.data.keyword.satelliteshort}} location do not run any workloads until you assign them as compute capacity to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service. For example, a basic setup has 6 hosts that are assigned as worker nodes to the {{site.data.keyword.satelliteshort}} location control plane. Other hosts might be assigned to {{site.data.keyword.openshiftlong_notm}} clusters as worker nodes for your Kubernetes workloads, or to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services. After you assign a host, it enters a **provisioning** status.

3. **Bootstrap**: When you assign a host, the host is bootstrapped to become a worker node in a {{site.data.keyword.satelliteshort}}-enabled service such as a managed {{site.data.keyword.openshiftlong_notm}} cluster or your {{site.data.keyword.satelliteshort}} control plane. This bootstrap process consists of three phases, all of which must successfully complete. First, required images are downloaded to the host from {{site.data.keyword.registrylong_notm}}. Then, the host is rebooted to apply the imaging configuration. Finally, software packages for the {{site.data.keyword.satelliteshort}}-enabled service, such as {{site.data.keyword.openshiftshort}}, are set up on the host. After successfully bootstrapping, the host enters a **normal** health state with an **assigned** status. You can no longer log in to the underlying machine via SSH to troubleshoot any issues. Instead, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).

Now, your hosts serve as worker nodes for your {{site.data.keyword.satelliteshort}} location control plane, {{site.data.keyword.openshiftlong_notm}} cluster, or as compute capacity for other {{site.data.keyword.satelliteshort}}-enabled services. If you run an {{site.data.keyword.openshiftshort}} cluster, you can log in to the clusters and use Kubernetes or {{site.data.keyword.openshiftshort}} APIs to manage your containerized workloads, or use [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config) to manage your workloads across clusters.


## Attaching hosts to your {{site.data.keyword.satelliteshort}} location
{: #attach-hosts}

After you create the location, you must attach compute capacity to your location so that you can run the {{site.data.keyword.satelliteshort}} control plane or set up {{site.data.keyword.openshiftshort}} clusters.
{: shortdesc}

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-infrastructure-plan#location-sizing).   \n
Using AWS hosts? You can use a [launch template](/docs/satellite?topic=satellite-aws) to attach hosts to your {{site.data.keyword.satelliteshort}} location.
{: tip}

When you set up the {{site.data.keyword.satelliteshort}} location control plane, keep in mind the following host considerations.
{: important}

- Hosts must meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs). For more information about how to configure hosts in other cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
- Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or {{site.data.keyword.satelliteshort}}-enabled service. For example, in cloud providers such as AWS, this setup typically means that all of the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled service degradation, and in extreme cases, failures in your cluster applications.
- Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then {{site.data.keyword.IBM_notm}} can automatically assign hosts when clusters or the {{site.data.keyword.satelliteshort}} location control plane request more capacity.

### Attaching hosts from the console
{: #attach-hosts-console}

Use the {{site.data.keyword.satelliteshort}} console to attach hosts to your location.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers. For more information about how to configure hosts in other cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
{: important}

1. From the [**Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to attach hosts.  
2. From the **Hosts** tab, click **Attach host**.
3. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use=satcp` or `use=satcluster` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster. By default, your hosts get a `cpu` label, but you might want to add more to control the autoassignment, such as `env=prod` or `service=database`.
4. Enter a file name for your script or use the name that is generated for you.
5. Click **Download script** to generate the host script and download the script to your local machine.

    Depending on the provider of the host, you might also need to update the [required RHEL 7 packages](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) on your hosts before you can run the script. For example, see the [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), or [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) RHEL package updates.
    {: note}

6. Follow the cloud provider-specific steps to update the script and attach your host.
    - [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
    - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
    - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
    - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
    - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)

    **On-premises data center**: To add host machines that reside in your on-premises data center, you can follow these general steps to run the host registration script on your machine.

    1. Retrieve the public IP address of your host, or if your host has only a private network interface, the private IP address of your host.      
    2. Copy the script from your local machine to your host.
        ```sh
        scp -i <filepath_to_key_file> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
        ```
        {: pre}        

    3. Log in to your host.
        ```sh
        ssh -i <filepath_to_key_file> <username>@<IP_address>
        ```
        {: pre}

    4. Update your host to have the required RHEL 7 packages. For more information about how to install these package, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}.
    5. Run the script.
        ```sh
        sudo nohup bash /tmp/attach.sh &
        ```
        {: pre}

    6. Monitor the progress of the registration script.
        ```sh
        journalctl -f -u ibm-host-attach
        ```
        {: pre}

7. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. This process might take a few minutes to complete. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

8. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

### Attaching hosts from the CLI
{: #attach-hosts-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to attach hosts to your location.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers. For more information about how to configure hosts in other cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
{: important}

1. Generate a script that you can copy and run on your machines to attach them as hosts to your location. You might want to include a label to identify the purpose of the hosts, such as `use:satloc`, because the hosts provide compute capacity for the {{site.data.keyword.satelliteshort}} location. Your hosts automatically are assigned labels for the CPU and memory size if these values can be detected on the machine.
    ```sh
    ibmcloud sat host attach --location <location_name> [-hl "use=satloc"]
    ```
    {: pre}

    Example output
    ```sh
    Creating host registration script...
    OK
    The script to attach hosts to Satellite location 'mylocation' was downloaded to the following location:

    <filepath_to_script>/register-host_mylocation_123456789.sh
    ```
    {: screen}

    Depending on the provider of the host, you might also need to update the [required RHEL 7 packages](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) on your hosts before you can run the script. For example, see the [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), or [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) RHEL package updates.
    {: note}

2. On your local machine, find the script.
3. Follow the cloud provider-specific steps to update the script and attach your host.
    - [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
    - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
    - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
    - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
    - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)

    **On-premises data center**: To add host machines that reside in your on-premises data center, you can follow these general steps to run the host registration script on your machine.

    1. Retrieve the public IP address of your host, or if your host has only a private network interface, the private IP address of your host.      
    2. Copy the script from your local machine to your host.
        ```sh
        scp -i <filepath_to_key_file> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
        ```
        {: pre}        

    3. Log in to your host.
        ```sh
        ssh -i <filepath_to_key_file> <username>@<IP_address>
        ```
        {: pre}

    4. Update your host to have the required RHEL 7 packages. For more information about how to install these package, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}.
    5. Run the script.
        ```sh
        sudo nohup bash /tmp/attach.sh &
        ```
        {: pre}

    6. Monitor the progress of the registration script.
        ```sh
        journalctl -f -u ibm-host-attach
        ```
        {: pre}

4. Verify that your hosts are attached to your location. Your hosts are not yet assigned to the {{site.data.keyword.satelliteshort}} control plane or an {{site.data.keyword.openshiftshort}} cluster which is why all of them show an `unassigned` state without any cluster or worker node information.
    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    Name             ID                     State        Status   Cluster   Worker ID   Worker IP   
    machine-name-1   aaaaa1a11aaaaaa111aa   unassigned   -        -         -           -   
    machine-name-2   bbbbbbb22bb2bbb222b2   unassigned   -        -         -           -   
    machine-name-3   ccccc3c33ccccc3333cc   unassigned   -        -         -           -  
    ```
    {: screen}

5. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).


## Using host autoassignment
{: #host-autoassign-ov}

By default, available hosts are automatically assigned to worker pools in {{site.data.keyword.satelliteshort}} resources, such as a cluster or a {{site.data.keyword.satelliteshort}}-enabled service. The worker pools use host labels to request compute capacity from available {{site.data.keyword.satelliteshort}} hosts with matching labels. You can disable and enable host autoassignment.
{: shortdesc}

When you assign hosts, you are charged a {{site.data.keyword.satelliteshort}} management fee per host vCPU. [Learn more](/docs/satellite?topic=satellite-faqs#pricing).
{: note}

Host autoassignment is not available for the {{site.data.keyword.satelliteshort}} location control plane.
{: note}

### About host labels
{: #host-autoassign-about}

Host autoassignment works by matching requesting labels from worker pools in {{site.data.keyword.satelliteshort}} clusters to the host and zone labels on available {{site.data.keyword.satelliteshort}} hosts.
{: shortdesc}

Keep in mind the following information about the host labels that are used for autoassignment.

Default host labels
:    When you attach a host to a {{site.data.keyword.satelliteshort}} location, the host automatically gets labels for `cpu` and `memory` (in bytes). You cannot remove these labels. You can include additional host labels, or update the host metadata later.

Hosts can have more labels than worker pools
:    For example, your host might have `cpu`, `memory`, and `env` host labels, but the requesting worker pool has only a `cpu` host label. Host autoassignment matches just the `cpu` labels. Note that the reverse does not work. If a worker pool has more labels than a host, the host cannot be autoassigned to the worker pool.

Matching is exact
:    Host labels must equal (`=`) each other exactly. Even if the host label is a number, no less than (`<`), greater than (`>`), or other operators are used for matching.

#### Example scenario
{: #host-autoassign-example-scenario}

Say that you have a {{site.data.keyword.satelliteshort}} cluster with a `default` worker pool in `us-east-1a` and the following host labels.
- `cpu=4`
- `env=prod`

Your {{site.data.keyword.satelliteshort}} location has available (unassigned) hosts with host labels as follows.
- Host A: `cpu=4, memory=32, env=prod, zone=us-east-1b`
- Host B: `cpu=4, memory=32, zone=us-east-1a`
- Host C: `cpu=4, memory=64, env=prod`

If you resize the `default` worker pool to request 3 more worker nodes, only Host C can be automatically assigned, but not Host A or Host B.
- Host A meets the CPU and `env=prod` label requests, but can only be assigned in `us-east-1b`. Because the `default` worker pool is only in `us-east-1a`, Host A is not assigned.
- Host B meets the CPU and zone requests. However, the host does not have the `env=prod` label, and so is not assigned.
- Host C is automatically assigned because it matches the `cpu=4` and `env=prod` host labels, and does not have any zone restrictions. The `memory=64` host label is not considered, because the worker pool does not request a `memory` label.

Hosts must be assigned as worker nodes in each zone of the default worker pool in your cluster. If your default worker pool spans multiple zones, ensure that you have hosts with matching labels that can be assigned in each zone.
{: note}

### Automatically assigning hosts
{: #host-autoassign}

{{site.data.keyword.satellitelong_notm}} can automatically assign hosts to worker pools in {{site.data.keyword.satelliteshort}} clusters that request compute capacity via host labels such as `cpu`.
{: shortdesc}

Before you begin, make sure that you [attach hosts](#attach-hosts) to your {{site.data.keyword.satelliteshort}} location, but do not assign the hosts.

1. Review the host labels that the worker pools use to request compute capacity. You have several options.
    - [Create a worker pool in a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters#sat-pool-create-labels) with the host labels that you want to use for autoassignment.
    - Review existing worker pools for their host labels. Note that you cannot update the host labels that a worker pool has. You can review the **Host labels** by running the `ibmcloud oc worker-pool get -c <cluster> --worker-pool <worker_pool>` command.
    - Review the host labels that a {{site.data.keyword.satelliteshort}}-enabled service cluster uses to request resources from the {{site.data.keyword.satelliteshort}}-enabled service instance console.
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
        ...
        ```
        {: screen}

3. Update the labels of your available host to match all of the labels of the worker pool that you want the host to be available for automatic assignment. You can optionally specify a zone to make sure that the host only gets automatically assigned to that zone.

    The `cpu` and `memory` labels on the host are applied automatically and cannot be edited.
    {: note}

    ```sh
    ibmcloud sat host update --location <location_name_or_ID> --host <host_name_or_ID> -hl <key=value> [-hl <key=value>] [--zone <zone1>]
    ```
    {: pre}

### Disabling host autoassignment
{: #host-autoassign-disable}

The following actions disable host autoassignment for a worker pool. Later, you can [reenable host autoassignment](#host-autoassign-enable).
{: shortdesc}

- [Manually assign hosts to a worker pool](#host-assign).
- [Delete an individual worker node from a worker pool](/docs/satellite?topic=openshift-satellite-clusters#sat-pool-maintenance).

### Re-enabling host autoassignment
{: #host-autoassign-enable}

If you [disabled host autoassignment](#host-autoassign-disable), you can re-enable autoassignment.
{: shortdesc}

1. Make sure that you have [available hosts with labels that match the host labels of the worker pool](#host-autoassign).
2. [Resize the worker pool](/docs/satellite?topic=openshift-satellite-clusters#sat-pool-maintenance) to set the requested size per zone, rebalance the worker pool, and enable autoassignment again.

## Manually assigning hosts to {{site.data.keyword.satelliteshort}} resources
{: #host-assign-manual}

After you attach hosts to a {{site.data.keyword.satelliteshort}} location, you assign them to {{site.data.keyword.satelliteshort}} resources to provide compute capacity, such as clusters or {{site.data.keyword.satelliteshort}}-enabled services.
{: shortdesc}

You can also use [host autoassignment](#host-autoassign-ov) for worker pools in {{site.data.keyword.satelliteshort}} clusters. However, you must manually assign hosts to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
{: tip}

When you assign hosts, you are charged a {{site.data.keyword.satelliteshort}} management fee per host vCPU. [Learn more](/docs/satellite?topic=satellite-faqs#pricing).
{: note}

### Prerequisites
{: #host-assign-prereq}

1. Make sure that you have the {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
2. [Attach hosts to your {{site.data.keyword.satelliteshort}} location](#attach-hosts), and check that the hosts are healthy and **unassigned**.

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

    The following example assigns a host by using the host ID.
    ```sh
    ibmcloud sat host assign --location <location_name_or_ID>  --cluster <cluster_name_or_ID> --host <host_ID> --worker-pool default --zone <zone>
    ```
    {: pre}

   The following example assigns a host by using the `use:satcluster` label.
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


## Updating hosts that are assigned as worker nodes to {{site.data.keyword.satelliteshort}}-enabled services like clusters
{: #host-update-workers}

{{site.data.keyword.IBM_notm}} provides version updates for your hosts that are assigned to {{site.data.keyword.satelliteshort}}-enabled services. The version updates include OpenShift Container Platform, the operating system, and security patches. You choose when to apply the host version updates.
{: shortdesc}

Do not use the `ibmcloud ks worker update` command for hosts that are assigned as worker nodes to {{site.data.keyword.satelliteshort}}-enabled services.
{: important}

### Checking if a version update is available for worker node hosts
{: #host-update-workers-check}

You can check if a version update is available for a host that is assigned as a worker node to a {{site.data.keyword.satelliteshort}}-enabled service by using the {{site.data.keyword.cloud_notm}} CLI or the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

To review the changes that are included in each version update, see the [Version changelog for {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-openshift_changelog).

#### Checking if a version update is available with the {{site.data.keyword.cloud_notm}} CLI
{: #host-update-workers-check-cli}

1. Log in to {{site.data.keyword.cloud_notm}}. Include the `--sso` option if you have a federated account.
    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

2. List the {{site.data.keyword.satelliteshort}} clusters in your account. 
    ```sh
    ibmcloud ks cluster ls --provider satellite
    ```
    {: pre}

3. List the worker nodes in the cluster that you want to update the version for. In the output, check for an asterisk `*` with a message that indicates a version update is available.
    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    ID                Primary IP       Flavor   State    Status   Zone     Version   
    sat-worker-<ID>   <IP_address>     upi      normal   Ready    zone-1   4.5.35_1534_openshift*   

    * To update to 4.5.37_1537_openshift version, run 'ibmcloud ks worker replace'. Review and make any required version changes before you update: 'https://ibm.biz/upworker'
    ```
    {: screen}

#### Checking if a version update is available from the {{site.data.keyword.cloud_notm}} console
{: #host-update-workers-check-console}

1. Log in to the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.
2. Click the location with the hosts that you want to update.
3. Click the **Hosts** tab.
4. From the host list, click the link to the **Cluster** for the host that you want to update. A new tab opens for the {{site.data.keyword.openshiftlong_notm}} cluster details.
5. Click the **Worker nodes** tab.
6. In the **Version** column, check for an information icon that says `Update available` when you click on the icon. If no update is available, no icon is present.
7. [Determine if the version update is a major, minor, or patch update](#host-update-workers-type). 

### Manually updating worker node hosts in the CLI
{: #host-update-cli}

#### Determining if the worker node version update is a major, minor, or patch update
{: #host-update-workers-type}

The process to update a worker node depends on whether the update is a major, minor, or patch update.
{: shortdesc}

To determine the type of update that is available, compare your current worker node versions to the latest `worker node fix pack` version in the [{{site.data.keyword.openshiftshort}} version changelog](/docs/openshift?topic=openshift-openshift_changelog). Major updates are indicated by the first digit in the version label (4.x.x), minor updates are indicated by the second digit (x.7.x) and patch updates are indicated by the trailing digits (x.x.23_1528_openshift). For more information on version updates, see [Version information and update actions](/docs/openshift?topic=openshift-openshift_versions).

#### Applying minor and patch version updates to worker node hosts
{: #host-update-workers-minor}

Hosts that are attached to a location do not update automatically. To apply a minor version update, you must first attach and assign new hosts to your {{site.data.keyword.satelliteshort}}-enabled service and then remove the old hosts.
{: shortdesc}

Before you begin

- List your current hosts and make note of their IDs. This helps determine which hosts to remove after applying the update.

    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

- Review the example output.

    ```sh
    ID                                                        Primary IP      Flavor   State    Status   Zone     Version   
    sat-satliberty-5b4c7f3a7bfc14cf58cbb14ad5c08429475274fe   208.43.36.202   upi      normal   Ready    zone-1   4.7.19_1525_openshift*   
    ```
    {: screen}

To apply a minor or patch update,

1. [Attach new hosts to your {{site.data.keyword.satelliteshort}} location](#attach-hosts). The number of hosts you attach must match the number of hosts that you want to update.   
2. [Assign the newly attached hosts to your {{site.data.keyword.satelliteshort}} resource](#host-assign). These hosts automatically receive the update when you assign them.
3. After the new hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource, [remove and delete the old hosts that you previously noted](#host-remove).

#### Applying major version updates to worker node host
{: #host-update-workers-major}

Hosts that are attached to a location do not update automatically. To apply a major version update to your worker node hosts, you must attach, assign, and remove hosts one at at time from your {{site.data.keyword.satelliteshort}} location's control plane. Then, you attach, assign, and remove hosts from your {{site.data.keyword.satelliteshort}}-enabled service.
{: shortdesc}

Before you begin

1. List your location's current control plane hosts and make note of their IDs. This helps determine which hosts to remove from the control plane after applying the update. The control plane hosts have `infrastructure` listed in the `Cluster` column of the output.

   ```sh
   ibmcloud sat hosts ls --location <location>
   ```
   { :pre}
   
   Review the example output.

   ```sh
   Name                ID                     State        Status   Zone     Cluster          Worker ID                                              Worker IP   

   satdemo-cp1         0bc3b92f55968a230985   assigned     Ready    zone-1   infrastructure   sat-satdemocp1-2bda578e901b4047c6e48d766cd99bc11a45fddd   169.62.42.178   
   satdemo-cp2         999cd38c39ddffe4b672   assigned     Ready    zone-2   infrastructure   sat-satdemocp2-940134e69c2609c5421b2426a7640fa80569668d   169.62.42.183   
   satdemo-cp5         6ca4fd8fcad1fa622bb4   assigned     Ready    zone-3   infrastructure   sat-satdemocp5-d46581b509357ea4b429fddc38a18b155463bf1c   169.62.42.181   
   ```
   {: screen}

2. List your current hosts that are assigned as worker nodes to your {{site.data.keyword.satelliteshort}}-enabled service and make note of their IDs. This helps determine which hosts to remove from your {{site.data.keyword.satelliteshort}}-enabled service after applying the update.

    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

    Review the example output.

    ```sh
    ID                                                        Primary IP      Flavor   State    Status   Zone     Version   
    sat-satliberty-5b4c7f3a7bfc14cf58cbb14ad5c08429475274fe   208.43.36.202   upi      normal   Ready    zone-1   4.7.19_1525_openshift*   
    ```
    {: screen}
    
Choose from one of the following scenarios,

- Applying a major update to the control plane hosts.

    See [Procedure to update control plane hosts](#host-update-cp-procedure).

- Applying a major update to your hosts assigned to a {{site.data.keyword.satelliteshort}}-enabled service.

    1. [Attach new hosts to your {{site.data.keyword.satelliteshort}} location](#attach-hosts). The number of hosts that you attach must match the number of hosts that you want to update.
    2. [Assign the newly attached hosts to your {{site.data.keyword.satelliteshort}} resource](#host-assign). These hosts automatically receive the new update when you assign them.
    3. After the new hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource, [remove and delete the old worker node hosts that you previously noted](#host-remove).

### Updating worker node hosts in the {{site.data.keyword.openshiftlong_notm}} console
{: #host-update-roks-console}

You can update worker node hosts by using the {{site.data.keyword.openshiftlong_notm}} console. 
{: shortdesc}

The option to update worker node hosts in the {{site.data.keyword.openshiftlong_notm}} console is available in beta.
{: beta}

1. Log in to the {{site.data.keyword.cloud_notm}} console and click **OpenShift** > **Clusters**.
2. Click the cluster where the hosts that you want to update are assigned and navigate to the **Worker nodes** page.
3. Select each host that you want to update. After you select the hosts, an **Update** option appears. 
4. Click **Update**. In the dialog box that appears, click **Update** again. A message appears that the update started successfully. 
5. Wait while the hosts update. The update process for each host is complete when the **Status** of the host returns to **Normal** and the new version is listed in the **Version** column. 

## Updating {{site.data.keyword.satelliteshort}} location control plane hosts
{: #host-update-location}

{{site.data.keyword.IBM_notm}} provides version updates for your hosts that are assigned to the {{site.data.keyword.satelliteshort}} location control plane. The version updates include OpenShift Container Platform, the operating system, and security patches. You choose when to apply the host version updates by following a process to detach the hosts from your location, reload the host machine in the infrastructure provider, and reattach and reassign the host to the {{site.data.keyword.satelliteshort}} location control plane.
{: shortdesc}

1. Review the [Considerations before you update](#host-update-considerations) to make sure that you have enough capacity to keep your location healthy during the host update.
2. Follow the [Procedure to update control plane hosts](#host-update-cp-procedure).

### Considerations before you update control plane hosts
{: #host-update-considerations}

Review the following considerations before you update your {{site.data.keyword.satelliteshort}} location control plane hosts.
{: shortdesc}

**How can I tell if a version update is available?**

Version updates for hosts become available as the {{site.data.keyword.openshiftlong_notm}} team packages new versions for worker nodes. Typically, worker node version updates are released every two weeks. 

You might check for a version update to meet your required security cadence, such as updates on a monthly or bi-monthly basis. To review available version updates, see the [Version changelog for {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-openshift_changelog).

**Does updating the hosts impact the cluster masters that run in the {{site.data.keyword.satelliteshort}} location control plane?**

Yes. Because the cluster masters run in your {{site.data.keyword.satelliteshort}} location control plane, make sure that you have enough extra hosts in your control plane before you update any hosts. To attach extra hosts, see [Attaching capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale).

**Do the hosts in my {{site.data.keyword.satelliteshort}}-enabled services have to run the same version as my {{site.data.keyword.satelliteshort}} location control plane?**

No, the hosts that are assigned to the {{site.data.keyword.satelliteshort}} location control plane do not have to run the same version as the hosts that are assigned to {{site.data.keyword.satelliteshort}}-enabled services that run in the location, like clusters. However, all hosts in the location must run a supported version.

To review supported {{site.data.keyword.openshiftshort}} versions that hosts can run, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-openshift_versions#version_types) or run `ibmcloud ks versions` in the command line. 

**Is my {{site.data.keyword.satelliteshort}} location control plane subdomain still reachable when I update the hosts?**

If your location subdomain was created automatically for you, the host IP addresses that are registered for the subdomain are automatically managed for you, such as during an update. 

If you manually registered the host IP addresses for the location subdomain with the `ibmcloud sat location dns register` command when you created the {{site.data.keyword.satelliteshort}} location control plane, make sure that you attach three hosts to the control plane before you begin, and manually register these host IPs for the subdomain. Now, these new hosts process requests for the location. Then, you can update the hosts that were previously used for the subdomain.

### Updating control plane hosts
{: #host-update-cp-procedure}

To apply a version update, you must detach, reload, and reattach your host to the {{site.data.keyword.satelliteshort}} location. Then, you can assign the host back to the control plane or to another resource that runs in the location.
{: shortdesc}

When you update control plane hosts, **do not assign or remove multiple hosts at the same time** as doing so may break the control plane. You must wait for a host assignment or removal to complete before assigning or removing another host.
{: important}

1. Optional: [Attach](#attach-hosts) and [assign](#host-assign) extra hosts to the {{site.data.keyword.satelliteshort}} location control plane to handle the compute capacity while your existing hosts are updating.
2. [Remove the host that you want to update from your {{site.data.keyword.satelliteshort}} location](#host-remove).
3. Follow the guidelines from your infrastructure provider to reload the operating system of your host.
4. [Attach the host](#attach-hosts) back to your {{site.data.keyword.satelliteshort}} location.
5. [Assign the host](#host-assign) back to your {{site.data.keyword.satelliteshort}} location control plane. As part of the bootstrapping process, the latest images and {{site.data.keyword.openshiftshort}} version that matches the cluster master is updated for your host and SSH access to the host is removed.


## Updating host metadata
{: #host-update-metadata}

If you want to update metadata about a host, such as labels or zones, see the [`ibmcloud sat host update` command](/docs/satellite?topic=satellite-satellite-cli-reference#host-update). This metadata is used to help manage your hosts, such as for [autoassignment](#host-autoassign-ov). The metadata update does not apply security patches or operating system updates.
{: shortdesc}


## Resetting the host key
{: #host-key-reset}

Reset the key that the control plane uses to communicate with all of the hosts in the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

When you create a location, a key is generated that the {{site.data.keyword.satelliteshort}} API server uses to attach hosts to the location and assign hosts to the control plane or to {{site.data.keyword.satelliteshort}}-enabled services. As you use your location, you might want to reset the existing host key. For example, in the case of a potential security incident, you can reset the key when you request a host attachment script. All existing hosts that run the previous version of the script can no longer communicate with the API for your {{site.data.keyword.satelliteshort}} location, and you can remove and reattach the existing hosts by using the script with the new key.

When you reset the host key, all existing hosts that are attached to your location can no longer communicate with the {{site.data.keyword.satelliteshort}} API server. Until they are reattached, existing hosts have authentication errors and cannot be managed by the control plane, such as for updates.
{: note}

1. List all hosts that are attached to your location.
    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

2. Reset the host key for the {{site.data.keyword.satelliteshort}} location.
    ```sh
    ibmcloud sat host attach --location <location_name> --reset-key [-hl "<key>=<value>"]
    ```
    {: pre}

3. [Remove one host from your {{site.data.keyword.satelliteshort}} location](#host-remove).
4. Follow the guidelines from your infrastructure provider to reload the operating system of your host.
5. [Attach the host](#attach-hosts) back to your {{site.data.keyword.satelliteshort}} location. The host registration script now uses the new host key.
6. [Assign the host](#host-assign) back to your {{site.data.keyword.satelliteshort}} location control plane or {{site.data.keyword.satelliteshort}}-enabled service.
7. Repeat steps 3 - 6 for each host in your location so that each host uses the new key to communicate with the {{site.data.keyword.satelliteshort}} API server.


## Monitoring host health
{: #host-monitor-health}

When you attach hosts to a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your hosts healthy. For more information, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default). For troubleshooting help, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).
{: shortdesc}

You can review the host health from the **Hosts** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat host ls --location <location_name_or_ID>`.

| Health state | Description |
| --- | --- |
| `assigned` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster. View the status for more information. If the status is `-`, the hosts did not complete the bootstrapping process to the {{site.data.keyword.satelliteshort}} resource. For hosts that you just assigned, wait an hour or so for the process to complete. If you still see the status, [log in to the host to continue debugging](/docs/satellite?topic=satellite-ts-hosts-login).|
| `health-pending` | The host is assigned and bootstrapped into the cluster as worker nodes that are provisioned and deployed. However, the health components that {{site.data.keyword.IBM_notm}} sets up in the host cannot communicate status back to {{site.data.keyword.cloud_notm}}. Make sure that your hosts meet the [minimum host and network connectivity requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network) and that the hosts are not blocked by a firewall in your infrastructure provider. |
| `provisioning` | The host is attached to the {{site.data.keyword.satelliteshort}} location and is in the process of bootstrapping to become part of a {{site.data.keyword.satelliteshort}} resource, such as the worker node of a {{site.data.keyword.openshiftlong_notm}} cluster. While the host reports a `provisioning` state, the worker node goes through the states of provisioning and deploying. |
| `ready` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign-manual).|
| `normal` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster, and ready for usage. |
| `reload-required` | The host is attached to the {{site.data.keyword.satelliteshort}} location, but requires a reload before it can be assigned to a {{site.data.keyword.satelliteshort}} resource. For example, you might have deleted a {{site.data.keyword.satelliteshort}} cluster, and now all of the hosts from the cluster require a reload. To reload a host, you must [remove the host from the location](/docs/satellite?topic=satellite-hosts#host-remove), reload the operating system in the underlying infrastructure provider, and [attach the host](/docs/satellite?topic=satellite-hosts#attach-hosts) back to the location. |
| `unassigned` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign-manual). If you tried to assign the host unsuccessfully, see [Cannot assign hosts to a cluster](/docs/satellite?topic=satellite-assign-fails).|
| `unknown` | The health of the host is unknown. If the host is unassigned, try [assigning the host](/docs/satellite?topic=satellite-hosts#host-assign-manual) to a {{site.data.keyword.satelliteshort}} resource, such as a cluster. If the host is assigned, try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug). |
| `unresponsive` | The host did not check in with the {{site.data.keyword.satelliteshort}} location control plane within the past 5 minutes. The host cannot be assigned when it is unresponsive. Try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug), particularly the network connectivity. |
{: caption="Host health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the host. The second column describes what the health state means."}



## Removing hosts
{: #host-remove}

When you remove a host from your location, the host is unassigned from a {{site.data.keyword.satelliteshort}}-enabled service cluster or the {{site.data.keyword.satelliteshort}} location control plane, detached from the location, and no longer available to run workloads from {{site.data.keyword.satelliteshort}}. If you delete an {{site.data.keyword.openshiftshort}} cluster or resize a worker pool, the hosts are still attached to your location, but you must detach and reattach the hosts to use them with another {{site.data.keyword.satelliteshort}} resource.
{: shortdesc}

After removal, the host machine still exists in your underlying infrastructure provider. Reload the operating system before using the host machine for another purpose.

Removing a host cannot be undone. Before you remove a host, make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep. Note that the underlying host infrastructure is not deleted because you manage the infrastructure yourself.
{: important}

### Removing hosts from the console
{: #host-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your hosts as compute capacity from the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep.
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Locations** and then click your location.
3. From the **Hosts** table, find the host that you want to remove.
4. Depending on the type of host, remove the host from a cluster before you remove the host.
    1. If the host **Cluster** is `Control plane`, continue to the next step.
    2. If the host **Cluster** is a hyperlink to the name of a {{site.data.keyword.openshiftlong_notm}} cluster, note the host **IP address** and click the cluster name hyperlink.
    3. From the cluster **Worker Nodes** tab, find the worker node with an **IP address** that matches the **IP address** of the host that you want to remove.
    4. Select the worker node.
    5. From the table action menu, click **Delete**.
    6. In the confirmation message, clear the option to replace the worker node and click **Delete**.
    7. Return to the {{site.data.keyword.satelliteshort}} **Locations > Hosts** table.
5. From the **Hosts** table, hover over the host that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
6. Click **Remove host**, enter the host name to confirm deletion, and click **Remove**.
7. Follow the instructions from your underlying infrastructure provider to complete one of the following actions:
    - To reuse the host for other purposes, reload the operating system of the host. For example, you might reattach the host to a {{site.data.keyword.satelliteshort}} location later. When you reattach a host, the host name can remain the same as the previous name, but a new host ID is generated.
    - To no longer use the host, delete the host from your infrastructure provider.

### Removing hosts from the CLI
{: #host-remove-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to remove your hosts as compute capacity from the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep.
2. Log in your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` flag, or create an API key to log in.
    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

3. List your locations and note the name of the location for the host that you want to remove.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

4. List your hosts. If the host is assigned to a cluster (and not to **infrastructure**) note the worker **ID** of the host that you want to remove.
    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    Retrieving hosts...
    OK
    Name              ID                     State      Status   Cluster          Worker ID                                                 Worker IP   
    machine-name-1    aaaaa1a11aaaaaa111aa   assigned   Ready    infrastructure   sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.xx.xxx.xxx   
    machine-name-2    bbbbbbb22bb2bbb222b2   assigned   Ready    infrastructure   sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.xx.xxx.xxx   
    machine-name-3    ccccc3c33ccccc3333cc   assigned   Ready    mycluster12345   sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.xx.xxx.xxx   
    ```
    {: screen}

5. If your host is assigned to a cluster, remove the worker node of the host by using the cluster name and worker ID that you previously retrieved.
    ```sh
    ibmcloud ks worker rm --cluster <cluster_name> --worker <worker_ID>
    ```
    {: pre}

6. Remove the host from your {{site.data.keyword.satelliteshort}} location.
    ```sh
    ibmcloud sat host rm --location <location_name_or_ID> --host <host_ID>
    ```
    {: pre}

7. Follow the instructions from your underlying infrastructure provider to complete one of the following actions:
    - To reuse the host for other purposes, reload the operating system of the host. For example, you might reattach the host to a {{site.data.keyword.satelliteshort}} location later. When you reattach a host, the host name can remain the same as the previous name, but a new host ID is generated.
    - To no longer use the host, delete the host from your infrastructure provider.
