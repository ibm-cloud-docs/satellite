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


# Getting started with {{site.data.keyword.satellitelong_notm}}
{: #getting-started}

With {{site.data.keyword.satellitelong_notm}}, you can bring your own compute infrastructure that resides in your on-premises data center, at other cloud providers, or in edge networks as a {{site.data.keyword.satelliteshort}} Location to {{site.data.keyword.cloud_notm}}. Then, you use the capabilities of {{site.data.keyword.satelliteshort}} to run {{site.data.keyword.cloud_notm}} services on this infrastructure, and consistently deploy, manage, and control your app workloads.

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

## Prerequisites
{: #sat-prereqs}

This getting started tutorial requires 3 compute hosts that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-limitations#limits-host) so that you can create the control plane of the {{site.data.keyword.satelliteshort}} location. The compute hosts can reside in an on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers. To use these machines with {{site.data.keyword.satelliteshort}}, they must have public network connectivity and you must have sufficient permissions to log in to the machine and run a script to add them to your location.

## Step 1: Create your location
{: #create-location}

To use {{site.data.keyword.satelliteshort}}, you must create a location. A location represents a data center that you can later fill with your own infrastructure resources to run {{site.data.keyword.cloud_notm}} services or other workloads on your own infrastructure.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click **Create location**.
2. Enter a name and an optional description for your location.
3. Select the {{site.data.keyword.cloud_notm}} multizone metro that you want to use to manage your location. For more information about why you must select a {{site.data.keyword.cloud_notm}} multizone metro, see [Understanding supported {{site.data.keyword.cloud_notm}} multizone metros in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the metro that is closest to where your host machines physically reside that you plan to add to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.
4. Click **Create location**. When you create the location, a location master is deployed to one of the zones that are located in the {{site.data.keyword.cloud_notm}} multizone metro that you selected. That process might take a few minutes to complete.
5. Wait for the master to be fully deployed and the location **State** to change to `Action required`.


## Step 2: Add compute hosts to your location
{: #add-hosts-to-location}

With your location set up, you can now add host machines to your location. All host machines must meet the [minimum hardware requirements](/docs/satellite?topic=satellite-limitations#limits-host) for {{site.data.keyword.satelliteshort}} and can physically reside in your own on-premises data center, in other cloud providers, or in edge networks.
{: shortdesc}

1. From the **Hosts** tab, click **Add host**.
2. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use:satcp` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane.
3. Enter a file name for your script or use the name that is generated for you.
4. Click **Download script** to generate the host script and download the script to your local machine.
5. Log in to each host machine that you want to add to your location and run the script. The steps for how to log in to your machine and run the script vary by cloud provider. When you run the script on the machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} control plane.

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

5. As you run the scripts on each machine, check that your hosts show in the **Hosts** tab of your location dashboard. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane. You must add at least 3 compute hosts to your location to continue with the setup of your {{site.data.keyword.satelliteshort}} control plane.


## Step 3: Assign your hosts to the {{site.data.keyword.satelliteshort}} control plane
{: #assign-hosts-to-cp}

To complete the setup of your {{site.data.keyword.satelliteshort}} location, you must assign the 3 compute hosts that you added in the previous step to the {{site.data.keyword.satelliteshort}} control plane. The control plane runs the components to securely connect your location to {{site.data.keyword.cloud_notm}}. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture).
{: shortdesc}

1. From the actions menu of each host machine that you added, click **Assign host**.
2. Select **Control plane** as your cluster and choose one of the available zones. Make sure that you assign each host to a different zone so that you spread all 3 hosts across all 3 zones in US East (`us-east-1`, `us-east-2`, and `us-east-3`). When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
3. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} control plane. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Prometheus is not yet initialized` or `Verify that alb steps have been completed for this cluster`.
   {: note}

## What's next
{: #whats-next}

Now that your location is set up, you can choose among the following options:
- [Add more compute capacity to your location](/docs/satellite?topic=satellite-hosts#add-hosts).
- [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) on your own infrastructure.
- [Attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters) and start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-cluster-config#create-satconfig) with {{site.data.keyword.satelliteshort}} configurations.
- [Learn more about the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.
