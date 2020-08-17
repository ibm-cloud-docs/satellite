---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-17"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# Setting up {{site.data.keyword.satelliteshort}} hosts
{: #hosts}

Set up {{site.data.keyword.satellitelong}} hosts to bring your own infrastructure to a {{site.data.keyword.satelliteshort}} location. Then, your hosts can be used as compute capacity to run {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

## Understanding {{site.data.keyword.satelliteshort}} hosts
{: #host-concept}

A {{site.data.keyword.satelliteshort}} host represents a compute machine of your own infrastructure, such as an on-premises data center or another cloud provider. You can add hosts to a {{site.data.keyword.satelliteshort}} location, and then assign the hosts to run {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

The following diagram presents the initial setup steps for hosts.

![Concept overview of Satellite host setup](/images/host-process.png){: caption="Figure 1. The initial setup process for {{site.data.keyword.satelliteshort}} hosts." caption-side="bottom"}

1.  **Add**: Your machine becomes a {{site.data.keyword.satelliteshort}} host after you successfully [add the host](#add-hosts) to a {{site.data.keyword.satelliteshort}} location by running a registration script on the machine. Your machine must meet the [minimum host requirements](/docs/satellite?topic=satellite-limitations#limits-host). After the host is added and a heartbeat can be detected, its health is **ready** and its status is **unassigned**. You can still log in to the machine via SSH to troubleshoot any issues.
2.  **Assign**: The hosts in your {{site.data.keyword.satelliteshort}} location do not run any workloads until you assign them as compute capacity to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service. For example, each location must have at least 3 hosts that are assigned as worker nodes to the {{site.data.keyword.satelliteshort}} control plane. Other hosts might be assigned to {{site.data.keyword.openshiftlong_notm}} clusters as worker nodes for your Kubernetes workloads, or to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services. After you assign a host, it enters a **provisioning** status.
3.  **Bootstrap**: When you assign a host, the host is bootstrapped to become a worker node in a managed {{site.data.keyword.openshiftlong_notm}} cluster or your {{site.data.keyword.satelliteshort}} control plane. This bootstrap process consists of three phases, all of which must successfully complete. First, required images are downloaded to the host, which requires public connectivity to pull the images from {{site.data.keyword.registrylong_notm}}. Then, the host is rebooted to apply the imaging configuration. Finally, Kubernetes and {{site.data.keyword.openshiftshort}} are set up on the host. After successfully bootstrapping, the host enters a **normal** health state with an **assigned** status. You can no longer log in to the underlying machine via SSH to troubleshoot any issues. Instead, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts).

Now, your hosts serve as worker nodes for your {{site.data.keyword.satelliteshort}} location control plane, {{site.data.keyword.openshiftlong_notm}} cluster, or as compute capacity for other {{site.data.keyword.satelliteshort}}-enabled services. If you run an {{site.data.keyword.openshiftshort}} cluster, you can log in to the clusters and use Kubernetes or {{site.data.keyword.openshiftshort}} APIs to manage your containerized workloads, or use [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config) to manage your workloads across clusters.

<br />


## Adding hosts to your {{site.data.keyword.satelliteshort}} location
{: #add-hosts}

After you create the location, you must add compute capacity to your location so that you can run the {{site.data.keyword.satelliteshort}} control plane or set up {{site.data.keyword.openshiftshort}} clusters.
{: shortdesc}

### Adding hosts from the console
{: #add-hosts-console}

Use the {{site.data.keyword.satelliteshort}} console to add hosts to your location.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-limitations#limits-host) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers.

1. From the [**Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, select that location where you want to add hosts.  
2. From the **Hosts** tab, click **Add host**.
3. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use=satcp` or `use=satcluster` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.
4. Enter a file name for your script or use the name that is generated for you.
5. Click **Download script** to generate the host script and download the script to your local machine.
5. Log in to each host machine that you want to add to your location and run the script. The steps for how to log in to your machine and run the script vary by cloud provider. When you run the script on the machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster. The script also disables the ability to SSH in to the machine for security purposes. If you later remove the host from the {{site.data.keyword.satelliteshort}} location, you must reload the host machine to SSH into the machine again.

   **General steps:**
   1. Retrieve the public IP address of your host.
   2. Copy the script from your local machine to your host.
      ```
      scp <path_to_script> root@<public_IP_address>:/tmp/attach.sh
      ```
      {: pre}

   3. Log in to your host.
      ```
      ssh root@<public_IP_address>
      ```
      {: pre}

   4. Run the script.
      ```
      nohup bash /tmp/attach.sh &
      ```
      {: pre}
      </br>

   **Example for adding {{site.data.keyword.cloud_notm}} classic virtual servers**:
   1. Retrieve the **public_ip** address and **id** of your machine.
      ```
      ibmcloud sl vs list
      ```
      {: pre}

   2. Retrieve the credentials to log in to your virtual machine.
      ```
      ibmcloud sl vs credentials <vm_ID>
      ```
      {: pre}

   3. Copy the script from your local machine to the virtual server instance.
      ```
      scp <path_to_script> root@<public_IP_address>:/tmp/attach.sh
      ```
      {: pre}

   4. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.
      ```
      ssh root@<public_IP_address>
      ```
      {: pre}

   5. Refresh the Red Hat packages on your machine.
      ```
      subscription-manager refresh
      ```
      {: pre}

      ```
      subscription-manager repos --enable=*
      ```
      {: pre}

   6. Run the script on your machine.
      ```
      nohup bash /tmp/attach.sh &
      ```
      {: pre}

   7. Exit the SSH session.  
      ```
      exit
      ```
      {: pre}

6. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

7. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

### Adding hosts from the CLI
{: #add-hosts-cli}

Use the {{site.data.keyword.satelliteshort}} CLI to add hosts to your location.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-limitations#limits-host) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers.

1.  Generate a script that you can copy and run on your machines to add them as hosts to your location. You might want to include a label to identify the purpose of the hosts, such as `use:satloc`, because the hosts provide compute capacity for the {{site.data.keyword.satelliteshort}} location. Your hosts automatically are assigned labels for the CPU and memory size if these values can be detected on the machine.
    ```
    ibmcloud sat host attach --location <location_name> [-l "use=satloc"]
    ```
    {: pre}

    Example output:
    ```
    Creating host registration script...
    OK
    The script to attach hosts to Satellite location 'mylocation' was downloaded to the following location:

    <filepath_to_script>/register-host_mylocation_123456789.sh
    ```
    {: screen}

2.  On your local machine, find the script.
3.  Log in to each host machine that you want to add to your location and run the script. The steps for how to log in to your machine and run the script vary by cloud provider. When you run the script on the machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} control plane or an {{site.data.keyword.openshiftshort}} cluster. The script also disables the ability to SSH in to the machine for security purposes. If you later remove the host from the {{site.data.keyword.satelliteshort}} location, you must reload the host machine to SSH into the machine again.

    **General steps:**
    1. Retrieve the public IP address of your host.
    2. Copy the script from your local machine to your host.
       ```
       scp <path_to_script> root@<public_IP_address>:/tmp/attach.sh
       ```
       {: pre}

    3. Log in to your host.
       ```
       ssh root@<public_IP_address>
       ```
       {: pre}

    4. Run the script.
       ```
       nohup bash /tmp/attach.sh &
       ```
       {: pre}
       </br>

    **Example for adding {{site.data.keyword.cloud_notm}} classic virtual servers**:
    1. Retrieve the **public_ip** address and **id** of your machine.
       ```
       ibmcloud sl vs list
       ```
       {: pre}

    2. Retrieve the credentials to log in to your virtual machine.
       ```
       ibmcloud sl vs credentials <vm_ID>
       ```
       {: pre}

    3. Copy the script from your local machine to the virtual server instance.
       ```
       scp <path_to_script> root@<public_IP_address>:/tmp/attach.sh
       ```
       {: pre}

    4. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.
       ```
       ssh root@<public_IP_address>
       ```
       {: pre}

    5. Refresh the Red Hat packages on your machine.
       ```
       subscription-manager refresh
       ```
       {: pre}

       ```
       subscription-manager repos --enable=*
       ```
       {: pre}

    6. Run the script on your machine.
       ```
       nohup bash /tmp/attach.sh &
       ```
       {: pre}

    7. Exit the SSH session.  
       ```
       exit
       ```
       {: pre}

4.  Verify that your hosts are added to your location. Your hosts are not yet assigned to the {{site.data.keyword.satelliteshort}} control plane or an {{site.data.keyword.openshiftshort}} cluster which is why all of them show an `unassigned` state without any cluster or worker node information.
    ```
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Name             ID                     State        Status   Cluster   Worker ID   Worker IP   
    machine-name-1   aaaaa1a11aaaaaa111aa   unassigned   -        -         -           -   
    machine-name-2   bbbbbbb22bb2bbb222b2   unassigned   -        -         -           -   
    machine-name-3   ccccc3c33ccccc3333cc   unassigned   -        -         -           -  
    ```
    {: screen}

5. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

<br />


## Assigning hosts to {{site.data.keyword.satelliteshort}} resources
{: #host-assign}

After you add hosts to a {{site.data.keyword.satelliteshort}} location, you can assign them to {{site.data.keyword.satelliteshort}} resources to provide compute capacity. For example, each location needs at least 3 hosts to be assigned as worker nodes to the [location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane). Any remaining hosts can be assigned as worker nodes to {{site.data.keyword.openshiftlong_notm}} clusters or as compute capacity to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
<br />


### Prerequisites
{: #host-assign-prereq}

1.   Make sure that you have the {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for {{site.data.keyword.satelliteshort}}.
2.   [Add hosts to your {{site.data.keyword.satelliteshort}} location](#add-hosts), and check that the hosts are healthy and **unassigned**.
3.   If you plan to use the host for a {{site.data.keyword.satelliteshort}}-enabled service, such as a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters), create this service instance in your {{site.data.keyword.cloud_notm}} account.

### Assigning hosts from the console
{: #host-assign-ui}

1.  From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external}, click **Locations**.
2.  Select the location where you added the host machines that you want to assign to your {{site.data.keyword.satelliteshort}} resource.
3. In the **Hosts** tab, from the actions menu of each host machine that you want to add to your resource, click **Assign host**.
4. Select the cluster that you created and choose one of the available zones. When you assign the hosts to a cluster, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the Health of your machine changes from **Ready** to **Provisioning**.
5. Verify that your hosts are successfully assigned to the cluster. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Prometheus is not yet initialized` or `Verify that alb steps have been completed for this cluster`.
   {: note}

### Assigning hosts from the CLI
{: #host-assign-cli}

1.  List the hosts in your location and find the ones that are in an **unassigned** status.
    ```
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}
2. Assign at least 3 compute hosts from your location as worker nodes to your {{site.data.keyword.satelliteshort}} control plane or an existing {{site.data.keyword.openshiftlong_notm}} cluster. When you assign the host, IBM bootstraps your machine. This process might take a few minutes to complete. You can choose to assign a host by using the host ID, or you can also define the label that the host must have to be assigned to the location.

   **Example for assigning a host by using the host ID:**
   ```
   ibmcloud sat host assign --location <location_name_or_ID>  --cluster <cluster_name_or_ID> --host <host_name_or_ID> --worker-pool default --zone <zone>
   ```
   {: pre}

   **Example for assigning a host by using the `use:satcluster` label:**
   ```
   ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --label "use:satcluster" --worker-pool default --zone us-east-1
   ```
   {: pre}

   <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component.">
    <caption>Understanding this command's components</caption>
      <thead>
      <th>Component</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
      <td><code>--location &lt;location_name_or_ID&gt;</code></td>
      <td>Enter the name or ID of the location where you created the cluster. To retrieve the location name or ID, run <code>ibmcloud sat location ls</code>.</td>
      </tr>
      <tr>
      <td><code>--cluster &lt;cluster_name_or_ID&gt;</code></td>
      <td>Enter the name or ID of the {{site.data.keyword.openshiftlong_notm}} cluster that you created earlier. To retrieve the cluster name or ID, run <code>ibmcloud ks cluster ls</code>. If you want to assign hosts to the {{site.data.keyword.satelliteshort}} control plane, you must enter the location ID as your cluster ID. To retrieve the location ID, run <code>ibmcloud sat location ls</code>.<</td>
      </tr>
       <tr>
      <td><code>--host &lt;host_name_or_ID&gt;</em></code></td>
      <td>Enter the host ID or name to assign as worker nodes to the {{site.data.keyword.satelliteshort}} resource. To view the host ID or name, run <code>ibmcloud sat host ls --location &lt;location_name&gt;</code>. You can also use the <code>--label</code> option to identify the host that you want to assign to your cluster.</td>
      </tr>
      <tr>
      <td><code>--label &lt;label&gt;</code></td>
      <td>Enter the label that you want to use to identify the host that you want to assign. The label must be a key-value pair, and must exist on the host machine. When you run this command with the `label` option, the first host that is in an `unassigned` state and matches the label is assigned to your {{site.data.keyword.satelliteshort}} resource. </td>
      </tr>
        <tr>
      <td><code>--worker-pool &lt;worker-pool&gt;</code></td>
      <td>Enter the name of the worker pool where you want to add your compute hosts. To find available worker pools in your cluster, run <code>ibmcloud oc worker-pool ls --cluster &lt;cluster_name_or_ID&gt;</code>. If you do not specify this option, your compute host is automatically added to the default worker pool. </td>
      </tr>
      <tr>
      <td><code>--zone &lt;worker-pool&gt;</code></td>
      <td>Enter the zone where you want to add your compute hosts. The zone must belong to the {{site.data.keyword.cloud_notm}} multizone metro that you selected when you created the location.</td>
      </tr>
      </tbody>
    </table>
3. Repeat the previous step for all compute hosts that you want to assign as worker nodes to your {{site.data.keyword.satelliteshort}} resource.
4. Wait a few minutes until the bootstrapping process for all computes hosts is complete and your hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource. All hosts are assigned a cluster, a worker node ID, and a public IP address.

   You can monitor the bootstrapping process of your compute hosts by running `ibmcloud ks worker get --cluster <cluster_name_or_ID> --worker <worker_ID>`.
   {: tip}

   ```
   ibmcloud sat host ls --location <location_name_or_ID>
   ```
   {: pre}

   Example output:  
   ```
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

<br />


## Updating hosts
{: #host-update}

Because you own the underlying infrastructure, you are responsible for updating your hosts. To update a host, you must remove the host from {{site.data.keyword.satelliteshort}} so that you can can reload the operating system. Then, you reattach and reassign the host to your cluster.
{: shortdesc}

The following diagram describes the process for updating hosts.

![Concept overview of Satellite host update process](/images/host-process-updates.png){: caption="Figure 2. The process for updating {{site.data.keyword.satelliteshort}} hosts." caption-side="bottom"}

1. **Remove**: As you work with {{site.data.keyword.cloud_notm}} services that your hosts are assigned to, you might notice that an update is available. For example, when you list worker nodes in a {{site.data.keyword.openshiftlong_notm}} cluster, you might see an `update available` message. For the {{site.data.keyword.satelliteshort}} location control plane, you see available updates when you list hosts. When an update is available, you have a few options.
   * **Replace the host by assigning extra hosts**: Make sure that your {{site.data.keyword.cloud_notm}} service or the control plane has enough compute capacity to continue running your workloads by [assigning extra hosts to your service or control plane](#host-assign) before you remove the host that needs to be updated. If you do not have extra hosts available, consider [adding hosts](#add-hosts) and then assigning them.
   *  **Reload the host that needs to be updated**: To reload and update your host, you must first [remove the host](/docs/satellite?topic=satellite-hosts#host-remove) from your {{site.data.keyword.satelliteshort}} location. Any workloads that run on the host are automatically scheduled onto remaining hosts if possible.
2. **Reload**: After your host is removed from your {{site.data.keyword.satelliteshort}} location, follow the guidance from your infrastructure provider to update and reload the operating system. For example, you might perform maintenance on the machine in your on-prem data center. After you reload the host machine, you can SSH into the host machine again, which was previously disabled while the host was assigned to a {{site.data.keyword.satelliteshort}} resource.
3. **Add**: To reuse the host, [add the host](/docs/satellite?topic=satellite-hosts#add-hosts) back to your {{site.data.keyword.satelliteshort}} location.
4. **Assign**: [Assign the host](/docs/satellite?topic=satellite-hosts#host-assign) back to your {{site.data.keyword.satelliteshort}} resource, such as a {{site.data.keyword.openshiftlong_notm}} cluster. As part of the bootstrapping process, the latest images and {{site.data.keyword.openshiftshort}} version that matches the cluster master is updated for your host and SSH access to the host is removed.

<br>

**Does updating the {{site.data.keyword.satelliteshort}} location control plane worker nodes impact the cluster masters that run in the location control plane?**<br>
Yes. Because the cluster masters run in your location control plane, make sure that you have enough extra hosts in your control plane before you update any hosts.

The location control plane worker nodes and cluster worker nodes do not have to run the same version of {{site.data.keyword.openshiftshort}}, but your hosts must run a supported version.

**Who provides the update for my hosts?**<br>
IBM provides updates for the IBM-managed components, such as the {{site.data.keyword.openshiftshort}} version that worker nodes run or the {{site.data.keyword.satelliteshort}} control plane. For master components, IBM automatically applies these updates, but for components that run on worker nodes, such as the location control plane or cluster worker nodes, you choose when to apply the updates.

For updates to the operating system, you are responsible to remove your hosts, update and reload the hosts, add the hosts back to your {{site.data.keyword.satelliteshort}} location, and assign the hosts to your {{site.data.keyword.satelliteshort}} resources. You can only install [supported operating system versions](/docs/satellite?topic=satellite-limitations#limits-host).

### Updating hosts from the console
{: #host-update-console}

Use the {{site.data.keyword.satelliteshort}} console to update your hosts.
{: shortdesc}

1. [Remove the host from your {{site.data.keyword.satelliteshort}} location](#host-remove) so that you can update the host.
2. Use the guidelines from your infrastructure provider to reload the operating system of your host and apply the latest updates.
3. [Add the host](#add-hosts) back to your {{site.data.keyword.satelliteshort}} location.
4. [Assign the host](#host-assign) back to your {{site.data.keyword.satelliteshort}} location control plane or to your {{site.data.keyword.openshiftlong_notm}} cluster.

### Updating hosts from the CLI
{: #host-update-cli}

Use the {{site.data.keyword.satelliteshort}} CLI to update your hosts.
{: shortdesc}

1. [Remove the host from your {{site.data.keyword.satelliteshort}} location](#host-remove-cli) so that you can update the host.
2. Use the guidelines from your infrastructure provider to reload the operating system of your host and apply the latest updates.
3. [Add the host](#add-hosts-cli) back to your {{site.data.keyword.satelliteshort}} location.
4. [Assign the host](#host-assign-cli) back to your {{site.data.keyword.satelliteshort}} location control plane or to your {{site.data.keyword.openshiftlong_notm}} cluster.

### Updating host metadata
{: #host-update-metadata}

If you want to update metadata about a host, such as labels, see the [`ibmcloud sat host update` command](/docs/satellite?topic=satellite-satellite-cli-reference#host-update). The update does not apply security patches or operating system updates.
{: shortdesc}

<br />


## Removing hosts
{: #host-remove}

When you remove a host from your location, the host is unassigned from a {{site.data.keyword.openshiftlong_notm}} cluster or {{site.data.keyword.satellitelong_notm}} control plane, detached from the location, and no longer available to run workloads from {{site.data.keyword.satelliteshort}}. If you delete an {{site.data.keyword.openshiftshort}} cluster or resize a worker pool, the hosts are still attached to your location, but you must [reload the hosts](#host-update) to use them with another {{site.data.keyword.satelliteshort}} resource.
{: shortdesc}

Removing a host cannot be undone. Before you remove a host, make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep. Note that the underlying host infrastructure is not deleted because you manage the infrastructure yourself.
{: important}

### Removing hosts from the console
{: #host-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your hosts as compute capacity from the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep.
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external}, click **Locations** and then click your location.
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
7. Optional: To delete the host machine, follow the instructions from your underlying infrastructure provider.

### Removing hosts from the CLI
{: #host-remove-cli}

Use the {{site.data.keyword.satelliteshort}} CLI to remove your hosts as compute capacity from the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep.
2. Log in your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` flag, or create an API key to log in.
   ```
   ibmcloud login [--sso]
   ```
   {: pre}
3. List your locations and note the name of the location for the host that you want to remove.
   ```
   ibmcloud sat location ls
   ```
   {: pre}
4. List your hosts. If the host is assigned to a cluster (and not to **infrastructure**) note the worker **ID** of the host that you want to remove.
   ```
   ibmcloud sat host ls --location <location_name_or_ID>
   ```
   {: pre}

   Example output:
   ```
   Retrieving hosts...
   OK
   Name              ID                     State      Status   Cluster          Worker ID                                                 Worker IP   
   machine-name-1    aaaaa1a11aaaaaa111aa   assigned   Ready    infrastructure   sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.xx.xxx.xxx   
   machine-name-2    bbbbbbb22bb2bbb222b2   assigned   Ready    infrastructure   sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.xx.xxx.xxx   
   machine-name-3    ccccc3c33ccccc3333cc   assigned   Ready    mycluster12345   sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.xx.xxx.xxx   
   ```
   {: screen}
5. If your host is assigned to a cluster, remove the worker node of the host by using the cluster name and worker ID that you previously retrieved.
   ```
   ibmcloud ks worker rm --cluster <cluster_name> --worker <worker_ID>
   ```
   {: pre}
6. Remove the host from your {{site.data.keyword.satelliteshort}} location.
   ```
   ibmcloud sat host rm --location <location_name_or_ID> --host <host_name_or_ID>
   ```
   {: pre}
7. Optional: To delete the host machine, follow the instructions from your underlying infrastructure provider.
