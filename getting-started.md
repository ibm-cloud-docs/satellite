---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-20"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Getting started with {{site.data.keyword.satellitelong_notm}}
{: #getting-started}

With {{site.data.keyword.satellitelong_notm}}, you use your own compute infrastructure that is in your on-premises data center, other cloud providers, or edge networks to create a {{site.data.keyword.satelliteshort}} location. Then, you use the capabilities of {{site.data.keyword.satelliteshort}} to run {{site.data.keyword.cloud_notm}} services on your infrastructure, and consistently deploy, manage, and control your app workloads.
{: shortdesc}

## Setting up an on-prem location
{: #gs-video-demo}

Want to see a preview before trying out the steps yourself? Check out the following video of setting up a {{site.data.keyword.satelliteshort}} location for on-prem edge devices.
{: shortdesc}

![Setting up an On-Premises Satellite Location with Mini Personal Computers](https://www.youtube.com/embed/8WNjwlN5gMk){: video output="iframe" data-script="none" id="youtubeplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

## Start by considering your infrastructure
{: #gs-start-here}

To get started, consider the type of infrastructure that you want to use.
{: shortdesc}

- **I have on-prem, cloud, or edge infrastructure**: Continue with the [getting started steps](#create-location).
- **I use Alibaba Cloud, Amazon Web Services, Google Cloud Platform, or Microsoft Azure**: You can continue with the [getting started steps](#create-location), or try an [automated setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template).
- **I don't have my own infrastructure or want to order some**: For a managed offering where {{site.data.keyword.IBM_notm}} sends you the infrastructure and sets up the location, check out [{{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).

## Step 1: Create your location
{: #create-location}

To use {{site.data.keyword.satelliteshort}}, you must create a location. A location represents a data center that you can later fill with your own infrastructure resources to run {{site.data.keyword.cloud_notm}} services or other workloads on your own infrastructure.
{: shortdesc}

Before you begin

- You must be the {{site.data.keyword.cloud_notm}} account owner, or have the administrator permissions to the required {{site.data.keyword.cloud_notm}} services in {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
- You must have an existing {{site.data.keyword.cos_full_notm}} service instance so that control plane data for your {{site.data.keyword.satelliteshort}} location can be backed up to a bucket in that instance. For example, to set up a dedicated {{site.data.keyword.cos_short}} instance and bucket,
    1. [Set up a {{site.data.keyword.cos_full_notm}} instance](https://cloud.ibm.com/objectstorage/create){: external} that you plan to use for all of your {{site.data.keyword.satelliteshort}} locations in your account.
    2. **Optional**: Create a bucket with **Cross-Region** resiliency in this service instance to back up your {{site.data.keyword.satelliteshort}} location control plane. Use a name that can help you identify it later, such as `bucket-<satloc_name>-<region>`. Pass in the name of this bucket when you create the {{site.data.keyword.satelliteshort}} location.

    If you want to create a cross-regional bucket in the CLI with the `ibmcloud cos create-bucket` command, you must first run `ibmcloud cos config region` and `ibmcloud cos endpoint-url` to set the cross region endpoint. For a list of cross region endpoints available for {{site.data.keyword.cos_full_notm}}, see [Cross Region Endpoints](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo). 
    {: tip}

To create a location,
1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.

2. Click **Manual setup**.

3. In the **{{site.data.keyword.satelliteshort}} location** section, review the following details. To change any of the default values, click **Edit**.

    1. For **Name**: The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.
    
    2. The **Description** and **Tags** fields are optional, and are metadata to help you organize your {{site.data.keyword.cloud_notm}} resources.
    
    3. The **Resource group** is set to `default` by default.
    
    4. For **Managed from**: Select the {{site.data.keyword.cloud_notm}} region that you want to use to manage your location. For more information about why you must select an {{site.data.keyword.cloud_notm}} region, see [About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the region that is closest to where your host machines physically reside that you plan to attach to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.

    5. For **Zones**: The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider.
        - [Alibaba regions and zones](https://www.alibabacloud.com/help/en/doc-detail/89155.htm){: external}, such as `us-east-1a` and `us-east-1b`.
        - [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, and `us-east-1c`.
        - [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. Do **not** use only the location name (`eastus`) or the zone number (`1`).
        - [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.
4. In the **Object Storage** section, you can click **Edit** to optionally enter the exact name of an existing {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in an {{site.data.keyword.cos_short}} instance in your account.

5. In the **Summary** panel, review your order details, and then click **Create location**. When you create the location, a location control plane master is deployed to one of the zones that are located in the {{site.data.keyword.cloud_notm}} region that you selected.

## Step 2: Attach compute hosts to your location
{: #attach-hosts-to-location}

With your location set up, you can now attach host machines to your location. The steps vary depending on the infrastructure provider that you use, [cloud providers](#gs-attach-hosts-cloud) or [on-premises data centers and edge networks](#gs-attach-hosts-onprem).
{: shortdesc}

No matter what infrastructure provider you use, all host machines must meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs).
{: important}

### Attaching hosts from cloud providers
{: #gs-attach-hosts-cloud}

1. Follow the guides for your cloud provider.
    - [Alibaba Cloud]( /docs/satellite?topic=satellite-alibaba)
    - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
    - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
    - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
    - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)
2. Confirm that you have at least three host machines in separate zones that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs), such as 4 vCPUs and 16 GB of memory with RHEL 7 packages and any provider-specific requirement from the guide. Make sure that you use minimal RHEL images. Do not install the LAMP stack.

    A setup of three host machines in separate zones is the minimum configuration for a demonstration location. A demonstration location can run only a few resources, such as one or two small clusters. If you want to continue to use the location after the demonstration, see [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-control-plane-scale).
    {: tip}

3. Continue with [Step 3: Assign your hosts to the {{site.data.keyword.satelliteshort}} location control plane](#assign-hosts-to-cp).

### Attaching hosts from on-premises data centers and edge networks
{: #gs-attach-hosts-onprem}

1. In your on-premises environment, identify or create at least three host machines in physically separate racks, which are called _zones_ in {{site.data.keyword.satelliteshort}}, that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs), such as 4 vCPUs and 16 GB of memory with RHEL 7 packages.

    A setup of three host machines in separate zones is the minimum configuration for a demonstration location. A demonstration location can run only a few resources, such as one or two small clusters. If you want to continue to use the location after the demonstration, see [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-control-plane-scale).
    {: tip}

2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the location that you previously created in Step 1.
3. From the **Hosts** tab, click **Attach host**.
4. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use:satcp` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane.
5. Enter a file name for your script or use the name that is generated for you.
6. Click **Download script** to generate the host script and download the script to your local machine.
7. Add the host machines that reside in your on-premises data center to your {{site.data.keyword.satelliteshort}} location.
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

    4. Update your host to have the required RHEL 7 packages. For more information about how to install these packages, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}.
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

    7. Repeat these steps for each of the hosts that you want to attach to the location. Remember that you must attach at least 3 compute hosts in separate zones to your location.
8. As you run the scripts on each machine, check that your hosts show in the **Hosts** tab of your location dashboard. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane.
9. Continue with [Step 3: Assign your hosts to the {{site.data.keyword.satelliteshort}} location control plane](#assign-hosts-to-cp).

## Step 3: Assign your hosts to the {{site.data.keyword.satelliteshort}} location control plane
{: #assign-hosts-to-cp}

To complete the setup of your {{site.data.keyword.satelliteshort}} location, you must assign the 3 compute hosts that you attached in the previous step to the {{site.data.keyword.satelliteshort}} location control plane. The control plane runs the components to securely connect your location to {{site.data.keyword.cloud_notm}}. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture).
{: shortdesc}

You can't change the zone of a host while it is assigned to the control plane or to a service. If you want to change a host's zone, you must first [unassign the host from the control plane or service](/docs/satellite?topic=satellite-host-remove). Then, reassign the host to a different zone. You don't need to delete the host from the location.
{: important}

1. From the actions menu of each host machine that you attached, click **Assign host**.
2. For the **Cluster**, select `Control plane`.
3. For the **Zone**, select a unique zone such as `zone-1`.
4. Click **Assign host**. When you assign the hosts to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
5. Repeat these steps for each host. Make sure that you assign each host to a different zone so that you spread all three hosts across all three zones, such as `zone-1`, `zone-2`, and `zone-3`.
6. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} control plane. The assignment is successful when an IP address is added to your host and the **Health** status changes to **Normal**.

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
    {: note}

## What's next
{: #whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to fill up the location with {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Repeat steps 2 and 3 to add three more hosts to your {{site.data.keyword.satelliteshort}} location, so that you can use the location for more than just demonstration purposes.
2. [Attach at least 3 more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) to add compute capacity to your location so that you can run {{site.data.keyword.satelliteshort}}-enabled services.
3. Create a {{site.data.keyword.satelliteshort}}-enabled service, such as a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster.
4. [Attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) and start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Configs.
5. [Learn more about the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

