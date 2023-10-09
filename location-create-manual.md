---

copyright:
  years: 2022, 2023
lastupdated: "2023-10-09"

keywords: satellite, hybrid, multicloud, plan infrastructure for satellite, satellite infrastructure, satellite supported os, satellite supported providers, satellite third party hosts

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Manually creating {{site.data.keyword.satelliteshort}} locations
{: #loc-manual-create}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north and south of the country. A {{site.data.keyword.satelliteshort}} location represents a data center that you fill with your own infrastructure resources to run your own workloads and {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Do not reuse the same name for multiple locations, even after the other location is deleted. If you use the same name 5 times or more within 7 days, you might reach the Let's Encrypt [Duplicate Certificate rate limit](/docs/openshift?topic=openshift-cs_rate_limit).
{: note}

## Manually creating locations from the console
{: #location-create-console}

Use the {{site.data.keyword.satelliteshort}} console to create your location.
{: shortdesc}


Before you begin:

- [Find out more about locations and hosts](/docs/satellite?topic=satellite-location-host).
- [Plan your infrastructure](/docs/satellite?topic=satellite-infrastructure-plan).
- [Verify your host setup](/docs/satellite?topic=satellite-host-network-check).
- Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
- {{site.data.keyword.satelliteshort}} uses {{site.data.keyword.cos_short}} to store data about your location and backups for your location's clusters. You can choose to have a bucket created automatically when you create your location or specify an existing bucket. If you want to use an existing bucket, it must have cross-regional resiliency.


To create a location, open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} and select **Create location**.

Getting started
:    If your infrastructure is on prem or edge, select the **On prem and Edge** option. With this option, you create a location, choose your zones, and download a host attach script to run on your hosts.
:    If your infrastructure is a cloud provider and you want to use only hosts that are running RHEL, select the cloud provider that you want to use, enter your credentials, and create your location. You can also [download the terraform template](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external} and edit it before you run it.
:    If your infrastructure is a cloud provider and you want to use hosts that are running RHCOS or want to have more control over your host creation, then select **Advanced configuration**. With this option, you create a location, choose your zones, include your cloud provider credentials, and then download a host attach script (RHEL) or an ignition script (RHCOS) to run on your hosts.

If you selected **On prem and Edge**, you can change your location options by clicking **Edit**.
{: tip}


**{{site.data.keyword.satelliteshort}} location** details
:    **Name**: The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.
:    **Resource group**: Resource groups organize your account resources in customizable groupings so that you can quickly assign users access to more than one resource at a time. This value is set to `default` by default.  
:    **Description** and **Tags**: Descriptions and tags help you organize your {{site.data.keyword.cloud_notm}} resources.
:    **Managed from**: The {{site.data.keyword.cloud_notm}} region that you want to use to manage your location. For more information about why you must select an {{site.data.keyword.cloud_notm}} region, see [About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the region that is closest to where your host machines physically reside that you plan to attach to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.
:    **Zones**: The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone for other cloud providers, consult your infrastructure provider.
         - [Alibaba regions and zones](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/regions-and-zones){: external}, such as `us-east-1a` and `us-east-1b`.
         - [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, and `us-east-1c`.
         - [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. **Don't** use only the location name (`eastus`) or the zone number (`1`).
         - [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.
:    **Red Hat CoreOS Support**: {{site.data.keyword.satelliteshort}} supports hosts that are running either **RHEL** or **RHCOS**. For more information, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os). 
:    **Object Storage**: The exact name of an existing {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in an {{site.data.keyword.cos_short}} instance in your account. Do not delete your {{site.data.keyword.cos_short}} instance or this bucket. If the service instance or bucket is deleted, your {{site.data.keyword.satelliteshort}} location control plane data cannot be backed up.

After you click **Create**, you can [attach hosts to your location](/docs/satellite?topic=satellite-attach-hosts) and finish the setup of your [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-setup-control-plane). Note that the token in the attach script is an API key, which must be treated and protected as sensitive information. 

The host attach script for your location expires one year from the creation date. To make sure that hosts in your location don't have authentication issues, download a new copy of the host attach script at least once per year and update any unassigned hosts. For more information, see [Why do my unassigned hosts have an `Unresponsive` status?](/docs/satellite?topic=satellite-ts-host-unassigned-unknown).
{: important}

Want to verify if your location is enabled for Red Hat CoreOS? See [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location).



## Creating locations from the CLI
{: #locations-create-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to create your location.
{: shortdesc}

Before you begin
- [Install the CLI plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-cli-install).
- Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
- {{site.data.keyword.satelliteshort}} uses {{site.data.keyword.cos_short}} to store data about your location and backups for your location's clusters. You can choose to have a bucket created automatically when you create your location or specify an existing bucket. If you want to use an existing bucket, it must have cross-regional resiliency.
    Don't delete your {{site.data.keyword.cos_short}} instance or this bucket. If you delete your service instance or bucket, you can't back up your {{site.data.keyword.satelliteshort}} location control plane data.
    {: important}

To create a {{site.data.keyword.satelliteshort}} location from the CLI,

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` option, or create an API key to log in.

    ```sh
    ibmcloud login [-sso]
    ```
    {: pre}



2. Create a {{site.data.keyword.satelliteshort}} location.

    ```sh

    ibmcloud sat location create --managed-from REGION --name NAME [--cos-bucket COS_BUCKET_NAME] [--description DESCRIPTION] [--ha-zone ZONE1_NAME --ha-zone ZONE2_NAME --ha-zone ZONE3_NAME] [--coreos-enabled] [--logging-account-id LOGGING_ACCOUNT] [--pod-subnet SUBNET] [--pod-network-interface-selection METHOD] [--provider INFRASTRUCTURE_PROVIDER] [--provider-region PROVIDER_REGION] [--provider-credential PATH_TO_PROVIDER_CREDENTIAL] [-q] [--service-subnet SUBNET]

    ```
    {: pre}

    `--managed-from REGION`
    :    Required. The {{site.data.keyword.cloud_notm}} region, such as `wdc` or `lon`, that your {{site.data.keyword.satelliteshort}} location is managed from. You can use any region, but to reduce latency between {{site.data.keyword.cloud_notm}} and your location, choose the region that is closest to the compute hosts that you plan to attach to your location later. For a list of supported {{site.data.keyword.cloud_notm}} regions, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).

    `--name NAME`
    :    Required. Enter a name for your location. The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.

    `--cos-bucket COS_BUCKET_NAME`
    :    Optional. Enter the name of the {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in your {{site.data.keyword.cos_short}} instance.
    :    Do not delete your {{site.data.keyword.cos_short}} instance or this bucket. If the service instance or bucket is deleted, your {{site.data.keyword.satelliteshort}} location control plane data cannot be backed up.
         {: important}

    `--description DESCRIPTION`
    :    Optional. A description of the Satellite location. 

    `--ha-zone ZONE1_NAME --ha-zone ZONE2_NAME --ha-zone ZONE3_NAME`
    :    Specify three names for high availability zones in your location. The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider.
         1. [Alibaba regions and zones](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/regions-and-zones){: external}, such as `us-east-1` and `us-west-1`.
         2. [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, `us-east-1c`.
         3. [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. Do **not** use only the location name (`eastus`) or the zone number (`1`).
         4. [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.

    :    Optional: If you use this option, zone names must be specified in three repeated options. If you do not use this option, the zones in your location are assigned names, such as `zone-1`.

    `--coreos-enabled`
    :    Optional. Enable [Red Hat CoreOS](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os) features within the {{site.data.keyword.satelliteshort}} location. This action cannot be undone.

    `--logging-account-id LOGGING_ACCOUNT`
    :    Optional. The {{site.data.keyword.cloud_notm}} account ID with the instance of {{site.data.keyword.la_full_notm}} that you want to forward your {{site.data.keyword.satelliteshort}} logs to. This option is available only in select environments.

    `--pod-subnet SUBNET`
    :    Optional. Specify a custom subnet CIDR to provide private IP addresses for pods. This option can be used only if you also enable Red Hat CoreOS with the `--coreos-enabled` option. All pods that are deployed to a worker node are assigned a private IP address in the 172.16.0.0/16 range by default. You can avoid subnet conflicts with the network that you use to connect to your location by specifying a custom subnet CIDR that provides the private IP addresses for your pods.
    :    When you choose a subnet size, consider the size of the cluster that you plan to create and the number of worker nodes that you might add in the future. The subnet must have a CIDR of at least `/23`, which provides enough pod IPs for a maximum of four worker nodes in a cluster. For larger clusters, use `/22` to have enough pod IP addresses for eight worker nodes, `/21` to have enough pod IP addresses for 16 worker nodes, and so on.
    :    The subnet that you choose must be within one of the following ranges.The subnet that you choose must be within one of the following ranges.
         - `172.16.0.0 - 172.17.255.255`
         - `172.21.0.0 - 172.31.255.255`
         - `192.168.0.0 - 192.168.254.255`
         - `198.18.0.0 - 198.19.255.255`

    :    Note that the pod and service subnets can't overlap. The service subnet is in the 172.20.0.0/16 range by default. This value can't be set to the value of the related location's pod-subnet or service-subnet.

    `--service-subnet SUBNET`
    :    Optional. Specify a custom subnet CIDR to provide private IP addresses for services. This option can be used only if you also enable Red Hat CoreOS with the `--coreos-enabled` option. All services that are deployed to the cluster are assigned a private IP address in the 172.20.0.0/16 range by default. You can avoid subnet conflicts with the network that you use to connect to your location by specifying a custom subnet CIDR that provides the private IP addresses for your services.
    :    The subnet must be specified in CIDR format with a size of at least `/24`, which allows a maximum of 255 services in the cluster, or larger. The subnet that you choose must be within one of the following ranges.
         - `172.16.0.0 - 172.17.255.255`
         - `172.20.0.0 - 172.31.255.255`
         - `192.168.0.0 - 192.168.254.255`
         - `198.18.0.0 - 198.19.255.255`
 
    :    Note that the pod and service subnets can't overlap. The pod subnet is in the 172.16.0.0/16 range by default. This value can't be set to the value of the related location's pod-subnet or service-subnet.

    `--pod-network-interface-selection METHOD`
    :    Optional. The method for selecting the node network interface for the internal pod network. The available methods are `can-reach` and `interface`. This option can be used only if you also enable Red Hat CoreOS with the `--operating-system` option. 
         - To provide a direct URL or IP address, specify `can-reach=<url>` or `can-reach=<ip_address>`. If the network interface can reach the provided URL or IP address, this option is used. For example, use `can-reach=www.exampleurl.com` for specifying a URL and `can-reach=172.19.0.0` for specifying an IP address.
         - To choose an interface with a Regex string, specify `interface=<regex_string>`; for example, `interface=eth.*`

    `--provider INFRASTRUCTURE_PROVIDER`
    :    Optional. The name of the infrastructure provider to create the {{site.data.keyword.satelliteshort}} location in. Accepted values are `aws`, `azure`, `gcp`. If you include this option, you must also include the `--provider-credential` option.

    `--provider-region PROVIDER_REGION`
    :    Optional. The name of the region in the infrastructure provider where you plan to create all the hosts for the {{site.data.keyword.satelliteshort}} location, such as `us-east-1` in AWS. Consult your infrastructure provider for the region name. If you include this option, you must also include the `--provider` option.

    `--provider-credential PATH_TO_PROVIDER_CREDENTIAL`
    :    Optional. The path to a JSON file on your local machine that has the credentials of the infrastructure provider for the {{site.data.keyword.satelliteshort}} location. The credential format is provider-specific. For more information, see [Providing {{site.data.keyword.satelliteshort}} with credentials to your infrastructure provider](/docs/satellite?topic=satellite-infrastructure-plan). If you include this option, you must also include the `--provider` option.


    `-q`
    :    Optional. Do not show the message of the day or update reminders.

        
3. Verify that your location is created and wait for the location **Status** to change to `action required`. When you create the location, a location management plane is deployed to the region that you selected during location creation. During this process, the **Status** of the location shows `deploying`. While the master deploys, you can now attach compute capacity to your location to complete the setup of the {{site.data.keyword.satelliteshort}} location control plane.


    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

    Example output

    ```sh
    Name         ID                     Status            Ready   Created        Hosts (used/total)   Managed From   
    mylocation   brhtfum2015a6mgqj16g   action required   no      1 minute ago   0 / 3                Washington DC   
    ```
    {: screen}

4. To finish the setup of your location.
    1. [Attach compute hosts to your location](/docs/satellite?topic=satellite-attach-hosts).
    2. Assign these hosts as worker nodes to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-setup-control-plane).

Want to verify if your location is enabled for Red Hat CoreOS? See [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location).

## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #manual-loc-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.

