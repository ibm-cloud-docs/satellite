---


copyright:
  years: 2022, 2025
lastupdated: "2025-12-04"

keywords: satellite, hybrid, multicloud, tenancy, resellers, satellite reseller, satellite use case

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.satelliteshort}} for resellers
{: #tenancy-model}

As a hybrid or distributed cloud reseller, I want to set up {{site.data.keyword.satelliteshort}} locations and deploy {{site.data.keyword.openshiftshort}} clusters for my customers.
{: shortdesc}

## Account overview
{: #tenancy-account-overview}

In this model, you create an enterprise account and separate client accounts for each client. Within each client account, you create a {{site.data.keyword.satelliteshort}} location and deploy an {{site.data.keyword.openshiftshort}} cluster.

![Concept overview of Satellite tenancy setup](/images/account_overview_graphic.svg){: caption="Enterprise account setup for resellers." caption-side="bottom"}

## Managing client locations and clusters
{: #location-mgmt-tenancy}

Review the best practices for managing client locations and clusters.

Understand the client's requirements and use case.
:   Gather requirements for your client's use case before creating their location and cluster. Understand the types of workloads your clients want to run in their location and the compute, storage, and other requirements that go along with their use case.

Keep extra hosts available and unassigned in the location
:   For faster scale up, plan to keep extra hosts available and ready for assignment to clusters and services in the location.

## Understanding responsibilities for your enterprise and your clients
{: #responsibilities-tenancy}

Create a responsibility matrix for your enterprise and your clients. The following example shows a responsibility matrix that you can adapt for your purposes. 

| Your enterprise | Client cluster administrator | Client cluster operator | Client developer |
| --- | --- | --- | --- |
| - Prepare requirements template & review client requirements.  \n - Prepare quote.  \n - Create client accounts, locations, and clusters, and API keys. | - Perform requirements assessment.  \n - Complete the requirements template.  \n - Review and approve quote.  \n - Review and confirm environment setup.  \n - Assign cluster operator and developer roles. | Verify cluster operator permissions | Verify cluster developer permissions |
{: caption="Example responsibility matrix for resellers." caption-side="bottom"}


## Setting up your account
{: #account-setup-tenancy}

Before you can resell {{site.data.keyword.satelliteshort}}, set up an {{site.data.keyword.cloud_notm}} enterprise and client accounts.


### Creating an enterprise account
{: #create-enterprise-tenancy}

[Create an enterprise account](/docs/enterprise-management?topic=enterprise-management-create-enterprise) for your organization. This account serves as the parent account. After you create the enterprise account, you can [set up client accounts](#create-account-tenancy) for each customer.

### Setting up client accounts
{: #create-account-tenancy}
{: step}

Complete the following steps to set up a client account with a resource group, location, logging and monitoring instances, and a {{site.data.keyword.satelliteshort}} location.

Want to automate this step? After you create an account, you can use the IBM terraform provider plug-in to provision {{site.data.keyword.cloud_notm}} service instances.
{: tip}

1. [Add a client account to your enterprise](/docs/enterprise-management?topic=enterprise-management-enterprise-add).

1. In the client account, provision an {{site.data.keyword.logs_full_notm}} instance.

1. In the client account, [provision an {{site.data.keyword.cos_full_notm}} instance](/docs/monitoring?topic=monitoring-provision#provision).


Repeat the steps for creating a client account for each client that you add to your enterprise. Then, continue the following steps to setting up the location and cluster.
{: tip}



## Setting up the client location
{: #setting-up-locations-tenancy}

After setting up your enterprise account and creating at least one client account, gather {{site.data.keyword.satelliteshort}} location requirements and create a location for your customer.

### Sizing and creating the client location
{: #create-provision-tenancy}
{: step}

1. Gather your client's requirements and decide the location size, host flavors, and storage. Also, decide the number of control plane hosts and the number of hosts that they need for services such as clusters. For help with location planning, see [Understanding Satellite locations](/docs/satellite?topic=satellite-location-sizing) and [Planning your environment for Satellite](/docs/satellite?topic=satellite-infrastructure-plan).

1. Provision virtual machines or decide your on-premises hosts that you want to use in your location. Make sure that your hosts meet the requirements for the following:
    * [System requirements](/docs/satellite?topic=satellite-host-reqs)
    * [Networking and connectivity requirements](/docs/satellite?topic=satellite-reqs-host-network)
    * [Storage requirements](/docs/satellite?topic=satellite-reqs-host-storage)
    
Before you begin:

- [Find out more about locations and hosts](/docs/satellite?topic=satellite-location-host).
- [Plan your infrastructure](/docs/satellite?topic=satellite-infrastructure-plan).
- [Verify your host setup](/docs/satellite?topic=satellite-host-network-check).
- Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
- {{site.data.keyword.satelliteshort}} uses {{site.data.keyword.cos_short}} to store data about your location and backups for your location's clusters. You can choose to have a bucket created automatically when you create your location or specify an existing bucket. If you want to use an existing bucket, it must have cross-regional resiliency.


To create a location, open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} and select **Create location**.

Getting started options
:    The following options determine your installation process.
:    - If your infrastructure is on prem or edge, select the **On prem and Edge** option. With this option, you create a location, choose your zones, and download a host attach script to run on your hosts.
:    - If your infrastructure is a cloud provider and you want to use only hosts that are running RHEL, select the cloud provider that you want to use, enter your credentials, and create your location. You can also [download the terraform template](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external} and edit it before you run it.
:    - If your infrastructure is a cloud provider and you want to use hosts that are running RHCOS or want to have more control over your host creation, then select **Advanced configuration**. With this option, you create a location, choose your zones, include your cloud provider credentials, and then download a host attach script (RHEL) or an ignition script (RHCOS) to run on your hosts.

:    If you selected **On prem and Edge**, you can change your location options by clicking **Edit**.
     {: tip}


{{site.data.keyword.satelliteshort}} location options
:    The following options define your location.
:    - **Name**: The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.
:    - **Resource group**: Resource groups organize your account resources in customizable groupings so that you can quickly assign users access to more than one resource at a time. This value is set to `default` by default.
:    - **Physical address**: For on-premises Locations, add a physical address to help associate your on-premises resources with your {{site.data.keyword.satelliteshort}} Location.
:    - **Description** and **Tags**: Descriptions and tags help you organize your {{site.data.keyword.cloud_notm}} resources.
:    - **Managed from**: The {{site.data.keyword.cloud_notm}} region that you want to use to manage your location. For more information about why you must select an {{site.data.keyword.cloud_notm}} region, see [About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the region that is closest to where your host machines physically reside that you plan to attach to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.
:    - **Zones**: The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone for other cloud providers, consult your infrastructure provider.
         - [Alibaba regions and zones](https://www.alibabacloud.com/help/en/ecs/user-guide/regions-and-zones){: external}, such as `us-east-1a` and `us-east-1b`.
         - [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, and `us-east-1c`.
         - [Azure `topology.kubernetes.io/zone` labels](https://learn.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. **Don't** use only the location name (`eastus`) or the zone number (`1`).
         - [GCP regions and zones](https://docs.cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.
 
:    - **Red Hat CoreOS Support**: {{site.data.keyword.satelliteshort}} supports hosts that are running either **RHEL** or **RHCOS**. For more information, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os). 
:    - **Object Storage**: The exact name of an existing {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in an {{site.data.keyword.cos_short}} instance in your account. **Do not delete** your {{site.data.keyword.cos_short}} instance or this bucket. If the service instance or bucket is deleted, your {{site.data.keyword.satelliteshort}} location control plane data cannot be backed up.

After you click **Create**, you can [attach hosts to your location](/docs/satellite?topic=satellite-attach-hosts) and finish the setup of your [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-setup-control-plane). Note that the token in the attach script is an API key, which must be treated and protected as sensitive information. 

The host attach script for your location expires one year from the creation date. To make sure that hosts in your location don't have authentication issues, download a new copy of the host attach script at least once per year and update any unassigned hosts. For more information, see [Why do my unassigned hosts have an `Unresponsive` status?](/docs/satellite?topic=satellite-ts-host-unassigned-unknown).
{: important}

Want to verify if your location is enabled for Red Hat CoreOS? See [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location).




### Attaching hosts to the location
{: #attach-hosts-tenancy}
{: step}

After creating the location, follow the steps to [attach the hosts to the location](/docs/satellite?topic=satellite-attach-hosts).


### Assigning hosts to the control plane
{: #assign-hosts-tenancy}
{: step}

After you have attached hosts to the location, assign hosts to the control plane.

Be sure to leave some hosts unassigned to use in services such as clusters.
{: note}

Before you begin
- [Attach the required number of hosts](/docs/satellite?topic=satellite-attach-hosts) to your location for your {{site.data.keyword.satelliteshort}} control plane. For more information about sizing requirements, see [sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing). For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
- Verify that your location is in an **Action required** state.

To attach hosts as worker nodes to the control plane,

1. From the {{site.data.keyword.satelliteshort}} [**Locations** dashboard](https://cloud.ibm.com/satellite/locations), select the location where you want to finish the setup of your control plane.
2. From the **Hosts** tab, select the hosts to assign as worker nodes to your control plane. All hosts must be in an **Unassigned** status.
3. From the actions menu of each host, click **Assign host**.
4. Select **Control plane** as your cluster.
5. Assign hosts in groups of 3 evenly to the control plane cluster. For high availability, make sure that your hosts correspond to physically separate zones in your infrastructure provider. For example, if your infrastructure provider has `us-east-1a`, `us-east-1b`, and `us-east-1c`, enter these names for your {{site.data.keyword.satelliteshort}} zones. Then, assign 2 hosts from `us-east-1a` in your infrastructure provider to `us-east-1a` in your {{site.data.keyword.satelliteshort}} control plane, and so on. When you assign the hosts to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
6. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} location control plane. The assignment is successful when an IP address is added to your host and the **Health** status changes to **Normal**.
7. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
    {: note}

8. Verify that the IP addresses of all your hosts were registered and added to the DNS record of your location. Check that the cert status is **created** and that the records are populated with the subdomains.

    ```sh
    ibmcloud sat location dns ls --location <location_ID_or_name>
    ```
    {: pre}

    Example output

    ```sh
    Retrieving location subdomains...
    OK
    Hostname                                                                                              Records                                                                                               Health Monitor   SSL Cert Status   SSL Cert Secret Name                                          Secret Namespace   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud   169.62.196.20,169.62.196.23,169.62.196.30                                                             None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000   default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001.us-east.satellite.appdomain.cloud   169.62.196.30                                                                                         None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001   default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002.us-east.satellite.appdomain.cloud   169.62.196.20                                                                                         None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002   default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003.us-east.satellite.appdomain.cloud   169.62.196.23                                                                                         None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003   default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00.us-east.satellite.appdomain.cloud   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud        None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00   default  
    ```
    {: screen}

9. To continue to use the location for production workloads, repeat these steps to attach more hosts to the location control plane in multiples of 3, such as 6, 9, or 12 hosts. For more information, see [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-location-sizing#control-plane-attach-capacity).


## Creating a cluster, API key, and cluster roles
{: #create-cluster-key-tenancy}

After setting up the location and the control plane, create cluster and an API key.


### Creating a {{site.data.keyword.satelliteshort}} cluster
{: #create-cluster-tenancy}
{: step}

1. From the [{{site.data.keyword.redhat_openshift_notm}} clusters console](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift){: external}, click **Create**.
1. In the **Infrastructure** section, select **{{site.data.keyword.satelliteshort}}**.
1. In the **OCP entitlement** section, specify an existing OCP entitlement for the worker nodes in this cluster by providing your [{{site.data.keyword.redhat_full}} account pull secret](https://console.redhat.com/openshift/install/pull-secret){: external} as a file or in raw JSON format. The cluster also uses this pull secret to download {{site.data.keyword.redhat_openshift_notm}} images from your own {{site.data.keyword.redhat_notm}} account.
1. In the **Location** section, select the {{site.data.keyword.satelliteshort}} location where you want to create the cluster. Make sure that the location that you select is in a **Normal** state.

1. In the **Operating System** section, select the operating system of the hosts that you want to use in the default worker pool of your cluster. In locations that are Red Hat CoreOS enabled, you can use either your RHCOS or RHEL hosts.
    In RHCOS enabled locations, when you want to add worker pools to your cluster, you can use RHCOS or RHEL hosts. Make sure to attach hosts with the operating system that you want to use to your location before assigning them to a worker pool.
    {: tip}
    
1. In the **Worker pools** section, configure the details for your default worker pool.
    1. Select the **Satellite zones** that {{site.data.keyword.satelliteshort}} uses to evenly assign hosts across zones that represent zones in your underlying infrastructure provider. Generally, create your worker pool across 3 zones for high availability.
    1. Request the **vCPU**, **Memory (GB)**, and number of **Worker nodes per zone** that you want to create the worker pool with. {{site.data.keyword.satelliteshort}} can automatically assign available hosts to the worker pool to fulfill your request. Generally, select at least 1 worker node per zone for a total of 3 worker nodes in your cluster.
1. In the **{{site.data.keyword.satelliteshort}} Config** section, decide whether to enable cluster admin access for {{site.data.keyword.satelliteshort}} Config. If you don't grant {{site.data.keyword.satelliteshort}} Config access, you can't later use the {{site.data.keyword.satelliteshort}} Config functionality to view or deploy Kubernetes resources for your clusters.If you want to enable access later, you can [create custom RBAC roles for {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig#manual-setup-clusters-satconfig).
1. For the **Resource details**, enter a **Cluster name** and any [{{site.data.keyword.cloud_notm}} tags](/docs/account?topic=account-tag) that you want to associate with your cloud resource. The cluster name must start with a letter, can contain letters, numbers, and hyphen (-), and must be 35 characters or fewer.
1. Click **Create**. When you create the cluster, the cluster master is automatically created in your {{site.data.keyword.satelliteshort}} location control plane, and your worker pool is automatically assigned available hosts that match your worker node request.
1. Wait for the cluster to reach a **Normal** state.

    If you don't have any available and matching hosts in your {{site.data.keyword.satelliteshort}} location, the cluster is still created but enters a **Warning** state. [Attach hosts](/docs/satellite?topic=satellite-attach-hosts) to your {{site.data.keyword.satelliteshort}} location so that hosts can be assigned as worker nodes to the worker pool. If the hosts are not automatically assigned, you can also manually [assign {{site.data.keyword.satelliteshort}} hosts to your cluster](/docs/satellite?topic=satellite-assigning-hosts). Ensure that hosts are assigned as worker nodes in each zone of your default worker pool.
    {: note}

1. From the [{{site.data.keyword.redhat_openshift_notm}} clusters console](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift){: external}, verify that your cluster reaches a **Normal** state.
1. [Access your cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat) to access the {{site.data.keyword.redhat_openshift_notm}} web console or to run `oc` and `kubectl` commands from the CLI. If you enabled {{site.data.keyword.satelliteshort}} Config access, you must complete this step to synchronize the permissions.

    If your location hosts have private network connectivity only, or if you use Amazon Web Services, Google Cloud Platform, or Microsoft Azure hosts, you must be connected to your hosts' private network, such as through VPN access, to connect to your cluster and access the {{site.data.keyword.redhat_openshift_notm}} web console. Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).
    {: note}

1. **Optional**: Set up an internal container image registry. For more information, see [Setting up the internal container image registry](/docs/openshift?topic=openshift-satellite-clusters-registry) in the {{site.data.keyword.openshiftlong_notm}} docs.

1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).

### Creating an API key
{: #create-api-tenancy}
{: step}

Follow the steps to [create an API that can be used in your client account](/docs/account?topic=account-userapikey&interface=ui).


### Setting up cluster admin and developer roles
{: #create-roles-tenancy}

Follow the steps to set up the [cluster roles your client needs](/docs/openshift?topic=openshift-iam-platform-access-roles#example-iam).
