---

copyright:
  years: 2020, 2020
lastupdated: "2020-07-24"

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


# Setting up {{site.data.keyword.satelliteshort}} hosts
{: #hosts}

Set up {{site.data.keyword.satellitelong}} hosts to bring your own infrastructure to a {{site.data.keyword.satelliteshort}} location. Then, your hosts can be used as compute machines to run {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

{{site.data.keyword.satellitelong}} is available as a tech preview and subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: preview}

## Understanding {{site.data.keyword.satelliteshort}} hosts
{: #host-concept}

A {{site.data.keyword.satelliteshort}} host represents a compute machine of your own infrastructure, such as an on-premises data center or another cloud provider. You can add hosts to a {{site.data.keyword.satelliteshort}} location, and then assign the hosts to run {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

The following diagram presents the initial setup steps for hosts.

![Concept overview of Satellite host setup](/images/host-process.png){: caption="Figure 1. The initial setup process for {{site.data.keyword.satelliteshort}} hosts." caption-side="bottom"}

1.  **Attach**: Your machine becomes a {{site.data.keyword.satelliteshort}} after you successfully [add the host](#add-hosts) to a {{site.data.keyword.satelliteshort}} location by running a registration script on the machine. Your machine must meet the [minimum host requirements](/docs/satellite?topic=satellite-limitations#limits-host). After the host is attached, its health is **unknown** and its status is **unassigned**. You can still log in to the machine via SSH to troubleshoot any issues.
2.  **Assign**: The hosts in your {{site.data.keyword.satelliteshort}} location do not do anything until you assign them to a {{site.data.keyword.satelliteshort}} resource, to use for compute capacity. For example, each location must have at least 3 hosts that are assigned to run control plane operations. Other hosts might be assigned to {{site.data.keyword.openshiftlong_notm}} clusters as worker nodes for your Kubernetes workloads, or to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
3.  **Bootstrap**: When assign a host to a {{site.data.keyword.satelliteshort}} cluster, the host is bootstrapped to become part of a managed {{site.data.keyword.openshiftlong_notm}} cluster. This bootstrap process consists of three phases, all of which must successfully complete. First, required images are downloaded to the host. Then, the host is rebooted to apply the imaging configuration. Finally, Kubernetes and {{site.data.keyword.openshiftshort}} are set up on the host. After successfully bootstrapping, the host enters a **normal** health state with an **assigned** status. You can no longer log into the underlying machine via SSH to troubleshoot any issues. Instead, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts).

<br />


## Adding hosts to your {{site.data.keyword.satelliteshort}} location
{: #add-hosts}

After you create the location, you must add compute capacity to your location so that you can run the {{site.data.keyword.satelliteshort}} control plane or set up {{site.data.keyword.openshiftshort}} clusters.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-limitations#limits-host) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers.

1. From the [**Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, select that location where you want to add hosts.  
2. From the **Hosts** tab, click **Add host**.
3. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. For example, you can use `use=satloc` or `use=satcluster` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster.
4. Click **Download script** to generate the `addHost.sh` host script and download the script to your local machine.
5. Log in to each host machine that you want to add to your location and run the script. The steps for how to log in to your machine and run the script vary by cloud provider. When you run the script on the machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster. The script also disables the ability to SSH in to the machine for security purposes. If you later remove the host from the {{site.data.keyword.satelliteshort}} location, you must reload the host machine to SSH into the machine again.

   **General steps:**
   1. Retrieve the public IP address of your host.
   2. Copy the script from your local machine to your host.
      ```
      scp <path_to_addHost.sh> root@<public_IP_address>:/tmp/attach.sh
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
      scp <path_to_addHost.sh> root@<public_IP_address>:/tmp/attach.sh
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

6. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster.

7. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

<br />


## Assigning hosts to {{site.data.keyword.satelliteshort}} resources
{: #host-assign}

After you add hosts to a {{site.data.keyword.satelliteshort}} location, you can assign them to {{site.data.keyword.satelliteshort}} resources to provide compute capacity. Each location needs at least 3 hosts assigned to the [location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane). Then, you can assign hosts to other resources, such as {{site.data.keyword.satelliteshort}} clusters.
<br />


### Prerequisites
{: #host-assign-prereq}

1.   Make sure that you have the {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for {{site.data.keyword.satelliteshort}}.
2.   [Add hosts to your {{site.data.keyword.satelliteshort}} location](#add-hosts), and check that the hosts are healthy and **unassigned**.
3.   Create the {{site.data.keyword.satelliteshort}} resource that you want to assign hosts to, such as [a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters) in {{site.data.keyword.openshiftlong_not}}.

1.  From the [{{site.data.keyword.satelliteshort}} console](https://test.cloud.ibm.com/satellite/){: external}, click **Locations**.
2.  Select the location where you added the host machines that you want to assign to your {{site.data.keyword.satelliteshort}} resource.
3. In the **Hosts** tab, from the actions menu of each host machine that you want to add to your resource, click **Assign host**.
4. Select the cluster that you created and choose one of the available zones. When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the Health of your machine changes from **Ready** to **Unknown**.
5. Verify that your hosts are successfully assigned to the cluster. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.

1.  List the hosts that are **unassigned** in your location.
    ```
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}
2. Assign at least 3 compute hosts from your location as worker nodes to your {{site.data.keyword.satelliteshort}} cluster to complete the cluster setup. When you assign the host to the cluster, IBM bootstraps your machine. This process might take a few minutes to complete. You can choose to assign a host by using the host ID, or you can also define the label that the host must have to be assigned to the location.

   **Example for assigning a host by using the host ID:**
   ```
   ibmcloud sat host assign --location <location_name_or_ID>  --cluster <cluster_name_or_ID> --host <host_name_or_ID> --worker-pool default --zone us-south-1
   ```
   {: pre}

   **Example for assigning a host by using the `use=satcluster` label:**
   ```
   ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --label "use=satcluster" --worker-pool default --zone us-south-1
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
      <td>Enter the name or ID of the {{site.data.keyword.openshiftlong_notm}} cluster that you created earlier. To retrieve the cluster name or ID, run <code>ibmcloud ks cluster ls</code>.</td>
      </tr>
       <tr>
      <td><code>--host &lt;host_name_or_ID&gt;</em></code></td>
      <td>Enter the host ID or name to assign as worker nodes to the cluster. To view the host ID or name, run <code>ibmcloud sat host ls --location &lt;location_name&gt;</code>. You can also use the <code>--label</code> option to identify the host that you want to assign to your cluster.</td>
      </tr>
      <tr>
      <td><code>--label &lt;label&gt;</code></td>
      <td>Enter the label that you want to use to identify the host that you want to assign. The label must be a key-value pair, and must exist on the host machine. When you run this command with the `label` option, the first host that is in an `unassigned` state and matches the label is assigned to your control plane. </td>
      </tr>
        <tr>
      <td><code>--worker-pool &lt;worker-pool&gt;</code></td>
      <td>Enter the name of the worker pool where you want to add your compute hosts. To find available worker pools in your cluster, run <code>ibmcloud oc worker-pool ls --cluster &lt;cluster_name_or_ID&gt;</code>. If you do not specify this option, your compute host is automatically added to the default worker pool. </td>
      </tr>
      <tr>
      <td><code>--zone us-south-1</code></td>
      <td>Enter the zone where you want to add your compute hosts. At the moment, <code>us-south-1</code> is supported only. </td>
      </tr>
      </tbody>
    </table>
3. Repeat the previous step for all compute hosts that you want to assign as worker nodes to your cluster.
4. Wait a few minutes until the bootstrapping process for all computes hosts is complete and your hosts are successfully assigned to your cluster. All hosts are assigned a cluster, a worker node ID, and a public IP address.

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

1. [Remove the host from your {{site.data.keyword.satelliteshort}} location](#host-remove-console-location) so that you can update the host.
2. From your infrastructure provider, reload the operating system with the latest updates.
3. [Add the host](#add-hosts-console) back to your {{site.data.keyword.satelliteshort}} location.
4. [Assign the host](#host-assign-ui) back to your {{site.data.keyword.satelliteshort}} location control plane or to your {{site.data.keyword.satelliteshort}} cluster.

<br />


## Removing hosts
{: #host-remove}

When you remove a host from your location, the host is unassigned from a cluster, detached from the location, and no longer available to run workloads from {{site.data.keyword.satellitelong_notm}}. If you want to remove a host just from the worker pool of a {{site.data.keyword.satelliteshort}} cluster and not from the {{site.data.keyword.satelliteshort}} location, you can remove the worker node that uses the host from the cluster.
{: shortdesc}

Removing a host cannot be undone. Before you remove a host, make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep. Note that the underlying host infrastructure is not deleted because you manage the infrastructure yourself.
{: important}

### Removing hosts from the console
{: #host-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your hosts from the [{{site.data.keyword.satelliteshort}} location entirely](#host-remove-console-location), or from only the [{{site.data.keyword.satelliteshort}} cluster](#host-remove-console-cluster).
{: shortdesc}

#### Removing a host from the {{site.data.keyword.satelliteshort}} location
{: #host-remove-console-location}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep.
2. From the [{{site.data.keyword.satelliteshort}} console](https://test.cloud.ibm.com/satellite/){: external}, click **Locations** and then click your location.
3. From the **Hosts** table, find the host that you want to remove.
4. Depending on the type of host, remove the host from a cluster before you remove the host.
   1. If the host **Cluster** is `Control plane`, continue to the next step.
   2. If the host **Cluster** is a hyperlink to the name of a {{site.data.keyword.satelliteshort}} cluster, note the host **IP address** and click the cluster name hyperlink.
   3. From the cluster **Worker Nodes** tab, find the worker node with an **IP address** that matches the **IP address** of the host that you want to remove.
   4. Select the worker node.
   5. From the table action menu, click **Delete**.
   6. In the confirmation message, clear the option to replace the worker node and click **Delete**.
   7. Return to the {{site.data.keyword.satelliteshort}} **Locations > Hosts** table.
5. From the **Hosts** table, hover over the host that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
6. Click **Remove host**, enter the host name to confirm deletion, and click **Remove**.
7. Optional: To delete the host machine, follow the instructions from your underlying infrastructure provider.

#### Removing a host from the {{site.data.keyword.satelliteshort}} cluster but keep the host attached to the location
{: #host-remove-console-cluster}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep.
2. From the [{{site.data.keyword.satelliteshort}} console](https://test.cloud.ibm.com/satellite/){: external}, click **Locations** and then click your location.
3. From the **Hosts** table, find the host that you want to remove.
4. Note the host **IP address**.
5. Click the **Cluster** hyperlink to go to the {{site.data.keyword.satelliteshort}} cluster details page.
6. From the cluster **Worker Nodes** tab, find the worker node with an **IP address** that matches the **IP address** of the host that you want to remove.
7. Select the worker node.
8. From the table action menu, click **Delete**.
9. In the confirmation message, clear the option to replace the worker node and click **Delete**.
10. Return to the {{site.data.keyword.satelliteshort}} **Locations > Hosts** table. Your host **Status** is now **Unassigned** because the host is no longer assigned to your cluster, but is still attached to the location. You can assign the host to another cluster, or leave the host unassigned as an extra host in case the autoresolver needs to add more capacity later.
