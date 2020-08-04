---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-04"

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

1.  **Add**: Your machine becomes a {{site.data.keyword.satelliteshort}} after you successfully [add the host](#add-hosts) to a {{site.data.keyword.satelliteshort}} location by running a registration script on the machine. Your machine must meet the [minimum host requirements](/docs/satellite?topic=satellite-limitations#limits-host). After the host is added, its health is **unknown** and its status is **unassigned**. You can still log in to the machine via SSH to troubleshoot any issues.
2.  **Assign**: The hosts in your {{site.data.keyword.satelliteshort}} location do not do anything until you assign them to a {{site.data.keyword.satelliteshort}} resource, to use for compute capacity. For example, each location must have at least 3 hosts that are assigned to run control plane operations. Other hosts might be assigned to {{site.data.keyword.openshiftlong_notm}} clusters as worker nodes for your Kubernetes workloads, or to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services. After you assign a host, it enters a **provisioning** status.
3.  **Bootstrap**: When you assign a host to a {{site.data.keyword.satelliteshort}} cluster, the host is bootstrapped to become part of a managed {{site.data.keyword.openshiftlong_notm}} cluster. This bootstrap process consists of three phases, all of which must successfully complete. First, required images are downloaded to the host, which requires public connectivity to pull the images from {{site.data.keyword.registrylong_notm}}. Then, the host is rebooted to apply the imaging configuration. Finally, Kubernetes and {{site.data.keyword.openshiftshort}} are set up on the host. After successfully bootstrapping, the host enters a **normal** health state with an **assigned** status. You can no longer log in to the underlying machine via SSH to troubleshoot any issues. Instead, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts).

Now, your hosts are worker nodes in the {{site.data.keyword.satelliteshort}} location control plane, {{site.data.keyword.openshiftlong_notm}} cluster, or other {{site.data.keyword.satelliteshort}}-enabled service that you assigned the host to. The master of the cluster runs in your {{site.data.keyword.satelliteshort}} location control plane. You can log in to the clusters and use Kubernetes or {{site.data.keyword.openshiftshort}} APIs to manage your containerized workloads.

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

1.  From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external}, click **Locations**.
2.  Select the location where you added the host machines that you want to assign to your {{site.data.keyword.satelliteshort}} resource.
3. In the **Hosts** tab, from the actions menu of each host machine that you want to add to your resource, click **Assign host**.
4. Select the cluster that you created and choose one of the available zones. When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the Health of your machine changes from **Ready** to **Unknown**.
5. Verify that your hosts are successfully assigned to the cluster. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.

<br />


## Updating hosts
{: #host-update}

Because you own the underlying infrastructure, you are responsible for updating your hosts. To update a host, you must remove the host from {{site.data.keyword.satelliteshort}} so that you can can reload the operating system. Then, you reattach and reassign the host to your cluster.
{: shortdesc}

1. [Remove the host from your {{site.data.keyword.satelliteshort}} location](#host-remove) so that you can update the host.
2. From your infrastructure provider, reload the operating system with the latest updates.
3. [Add the host](#add-hosts) back to your {{site.data.keyword.satelliteshort}} location.
4. [Assign the host](#host-assign) back to your {{site.data.keyword.satelliteshort}} location control plane or to your {{site.data.keyword.satelliteshort}} cluster.

<br />


## Removing hosts
{: #host-remove}

When you remove a host from your location, the host is unassigned from a cluster, detached from the location, and no longer available to run workloads from {{site.data.keyword.satellitelong_notm}}. Hosts that are removed from a cluster, such as by deleting a cluster or resizing a worker pool, are still attached to your location, but you must [reload the hosts](#host-update) to return them to an available state.
{: shortdesc}

Removing a host cannot be undone. Before you remove a host, make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep. Note that the underlying host infrastructure is not deleted because you manage the infrastructure yourself.
{: important}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after your remove the host, or back up any data that you want to keep.
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external}, click **Locations** and then click your location.
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
