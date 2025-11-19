---

copyright:
  years: 2020, 2025

lastupdated: "2025-11-19"

keywords: satellite, hybrid, multicloud, assigning hosts, host auto assignment, host auto assignment, host labels

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}<!--<qna:hosts>-->

# Manually assigning hosts to worker pools
{: #assigning-hosts}

After you attach hosts to a {{site.data.keyword.satelliteshort}} location, you assign them to {{site.data.keyword.satelliteshort}} resources to provide compute capacity, such as clusters or {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

You can also use [host auto assignment](/docs/satellite?topic=satellite-host-autoassign-ov) for worker pools in {{site.data.keyword.satelliteshort}} clusters. However, you must manually assign hosts to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-setup-control-plane).
{: tip}

When you assign hosts, you are charged a {{site.data.keyword.satelliteshort}} management fee per host vCPU. [Learn more](/docs/satellite?topic=satellite-faqs#pricing).
{: note}

Before you begin,

1. Make sure that you have the {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
2. [Attach hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-attach-hosts), and check that the hosts are healthy and **unassigned**.

## Assigning hosts from the console
{: #host-assign-ui}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Locations**.
2. Select the location where you attached the host machines that you want to assign to your {{site.data.keyword.satelliteshort}} resource.
3. In the **Hosts** tab, from the actions menu of each host machine that you want to add to your resource, click **Assign host**.
4. Select the cluster that you created, and choose one of the available zones. When you assign the hosts to a cluster, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the Health of your machine changes from **Ready** to **Provisioning**.
5. Verify that your hosts are successfully assigned to the cluster. The assignment is successful when an IP address is added to your host and the **Health** status changes to **Normal**.
6. Repeat these steps to ensure that hosts are assigned as worker nodes in each zone of the default worker pool in your cluster.

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
    {: note}<!--</qna:hosts>-->

## Assigning hosts from the CLI
{: #assigning-hosts-cli}

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
        {: caption="Understanding this command's components" caption-side="bottom"}

        
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
    
