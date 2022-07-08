---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating a {{site.data.keyword.satelliteshort}} location
{: #locations}

Set up an {{site.data.keyword.satellitelong}} location to represent a data center that you fill with your own infrastructure resources, and start running {{site.data.keyword.cloud_notm}} services on your own infrastructure.
{: shortdesc}

Not sure if your infrastructure is ready to use for {{site.data.keyword.satelliteshort}} locations? See [Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-infrastructure-plan).

## Automating your location setup with a {{site.data.keyword.bpshort}} template
{: #satloc-template}

Automate your setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in a cloud provider, and set up the {{site.data.keyword.satelliteshort}} location control plane for you.
{: shortdesc}


To enable your location for Red Hat CoreOS, create your location [manually](#location-create-manual).
{: tip}


You can set up {{site.data.keyword.satelliteshort}} locations with a {{site.data.keyword.bpshort}} template for the following cloud providers. 

- [AWS](/docs/satellite?topic=satellite-aws#aws-template)
- [Azure](/docs/satellite?topic=satellite-azure#azure-template)
- [GCP](/docs/satellite?topic=satellite-gcp#gcp-template)

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about-locations).
{: note}

What's next?

The {{site.data.keyword.bpshort}} template helped with the initial creation, but you are in control for subsequent location management actions, such as [attaching more hosts](/docs/satellite?topic=satellite-attach-hosts), [creating {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters), or [scaling the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-location-sizing). If you [remove](/docs/satellite?topic=satellite-host-remove#location-remove-console) your {{site.data.keyword.satelliteshort}} location, make sure to [remove your workspace in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-workspace-setup#del-workspace), too.


## Manually creating {{site.data.keyword.satelliteshort}} locations
{: #location-create-manual}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north and south of the country. A {{site.data.keyword.satelliteshort}} location represents a data center that you fill with your own infrastructure resources to run your own workloads and {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Don't have your own infrastructure or want a managed solution? [Check out {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: tip}


### Manually creating locations from the console
{: #location-create-console}

Use the {{site.data.keyword.satelliteshort}} console to create your location.
{: shortdesc}


Before you begin:

- Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
- {{site.data.keyword.satelliteshort}} uses {{site.data.keyword.cos_short}} to store data about your location and backups for your location's clusters. You can choose to have a bucket created automatically when you create your location or specify an existing bucket. If you want to use an existing bucket, it must have cross-regional resiliency.
    Do not delete your {{site.data.keyword.cos_short}} instance or this bucket. If the service instance or bucket is deleted, your {{site.data.keyword.satelliteshort}} location control plane data cannot be backed up.
    {: important}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.

1. Click **Advanced configuration**.

1. In the **Infrastructure type** section, select **On-premises** or **Public cloud**. If your infrastructure is in a Public cloud, you can select your **Cloud provider**. Or, you can select **Other cloud** for a general setup.

1. In the **Workflow** section, select **Manual**.

1. If you selected a cloud provider, enter your credentials.

1. In the **{{site.data.keyword.satelliteshort}} location** section, review the following details. To change any of the default values, click **Edit**.

    1. For **Name**: The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.
        
    1. The **Resource group** is set to `default` by default.  
    
    1. The **Description** and **Tags** fields are optional, and are metadata to help you organize your {{site.data.keyword.cloud_notm}} resources.

    1. In the **Managed from** menu, select the {{site.data.keyword.cloud_notm}} region that you want to use to manage your location. Red Hat CoreOS is available only in the Dallas (`us-south`), Frankfurt (`eu-de`), Tokyo (`jp-tok`), London (`eu-gb`), and Washington D.C. (`us-east`) regions and for only {{site.data.keyword.redhat_openshift_notm}} version 4.9 and 4.10. For more information about why you must select an {{site.data.keyword.cloud_notm}} region, see [About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the region that is closest to where your host machines physically reside that you plan to attach to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.

    1. For **Zones**: The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider.
        - [Alibaba regions and zones](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/regions-and-zones){: external}, such as `us-east-1a` and `us-east-1b`.
        - [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, and `us-east-1c`.
        - [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. **Don't** use only the location name (`eastus`) or the zone number (`1`).
        - [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.
    1. Select **Enable CoreOS** to create a location that is enabled for Red Hat CoreOS. For more information, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).

1. In the **Object Storage** section, you can click **Edit** to optionally enter the exact name of an existing {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in an {{site.data.keyword.cos_short}} instance in your account.

1. In the **Summary** panel, review your order details, and then click **Create location**. When you create the location, a location control plane master is deployed to one of the zones that are located in the {{site.data.keyword.cloud_notm}} region that you selected.

1. Continue with [attaching hosts to your location](/docs/satellite?topic=satellite-attach-hosts) to finish the setup of your {{site.data.keyword.satelliteshort}} location control plane. Note that the token in the attach script is an API key, which must be treated and protected as sensitive information. 

The host attach script for your location expires one year from the creation date. To make sure that hosts in your location don't have authentication issues, download a new copy of the host attach script at least once per year and update any unassigned hosts. For more information, see [Why do my unassigned hosts have an `Unresponsive` status?](/docs/satellite?topic=satellite-ts-host-unassigned-unknown).
{: important}



### Creating locations from the CLI
{: #locations-create-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to create your location.
{: shortdesc}

Before you begin
- [Install the CLI plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli).
- Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
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

    ibmcloud sat location create --managed-from <region> --name <location_name> --ha-zone zone1_name --ha-zone zone2_name --ha-zone zone3_name [--cos-bucket cos_bucket_name] --coreos-enabled  

    ```
    {: pre}

    `--managed-from <region>`
    :   The {{site.data.keyword.cloud_notm}} region, such as `wdc` or `lon`, that your {{site.data.keyword.satelliteshort}} location is managed from. You can use any region, but to reduce latency between {{site.data.keyword.cloud_notm}} and your location, choose the region that is closest to the compute hosts that you plan to attach to your location later. For a list of supported {{site.data.keyword.cloud_notm}} regions, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).
    
    `--name <location_name>`
    :   Enter a name for your {{site.data.keyword.satelliteshort}} location. The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name..
    
    `--cos-bucket <cos_bucket_name>`
    :   Optional: Enter the name of the {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up the control plane data. Otherwise, a new bucket is automatically created in a {{site.data.keyword.cos_short}} instance in your account.
    
    `--ha-zone <ZONE1_NAME> --ha-zone <ZONE2_NAME> --ha-zone <ZONE3_NAME>`
    :   Optional: If you use this option, zone names must be specified in three repeated flags. If you do not use this option, the zones in your location are assigned names such as `zone-1`. Specify three names for high availability zones in your location. The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider. 
        - [Alibaba regions and zones](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/regions-and-zones){: external}, such as `us-east-1` and `us-west-1`.
        - [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, `us-east-1c`.
        - [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. Do **not** use only the location name (`eastus`) or the zone number (`1`).
        - [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.

    `--provider <provider>`
    :    Optional. The name of the infrastructure provider to create the {{site.data.keyword.satelliteshort}} location in. Accepted values are `aws`, `azure`, or `gcp`. If you include this option, you must also include the `--provider-credential` option.    
    
    `--provider-credential <path_to_credentials>`
    :    Optional. The path to a JSON file on your local machine that has the credentials of the infrastructure provider for the {{site.data.keyword.satelliteshort}} location. The credential format is provider-specific. If you include this option, you must also include the `--provider` option.
  
    `--coreos-enabled`
    :    Optional. Enable Red Hat CoreOS. This action cannot be undone. For more information, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).

        
3. Verify that your location is created and wait for the location **Status** to change to `action required`. When you create the location, a location control plane master is deployed to the region that you selected during location creation. During this process, the **Status** of the location shows `deploying`. While the master deploys, you can now attach compute capacity to your location to complete the setup of the {{site.data.keyword.satelliteshort}} location control plane.


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

4. To finish the setup of your location:
    1. [Attach compute hosts to your location](/docs/satellite?topic=satellite-attach-hosts).
    2. Assign these hosts as worker nodes to the [{{site.data.keyword.satelliteshort}} location control plane](#setup-control-plane).

Want to verify if your location is enabled for Red Hat CoreOS? See [Is my location enabled for Red Hat CoreOS?](#verify-coreos-location).

### Is my location enabled for Red Hat CoreOS?
{: #verify-coreos-location}

You can verify that your location is enabled for Red Hat CoreOS by running the **`location get`** command. Look for the `Ignition Server Port:` field to populate. Wait to check until after your location is provisioned. 

Red Hat CoreOS is available only in the Dallas (`us-south`), Frankfurt (`eu-de`), Tokyo (`jp-tok`), London (`eu-gb`), and Washington D.C. (`us-east`) regions and for only {{site.data.keyword.redhat_openshift_notm}} version 4.9 and 4.10.
{: note}


```sh
ibmcloud sat location get --location LOCATION
```
{: pre}

Example output

```sh
Name:                           my-coreos-test   
ID:                             <ID>   
Created:                        2022-03-26 15:02:00 +0000 (4 days ago)   
Managed From:                   dal   
State:                          action required   
Ready for deployments:          no   
Message:                        R0012: The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane.   
Hosts Available:                0   
Hosts Total:                    0   
Host Zones:                     us-south-1, us-south-2, us-south-3   
Provider:                       -   
Provider Region:                -   
Provider Credentials:           no   
Public Service Endpoint URL:    <ENDPOINT>   
Private Service Endpoint URL:   -   
OpenVPN Server Port:            -   
Ignition Server Port:           30119   
Konnectivity Server Port:       32157
```
{: screen}

To create a Red Hat CoreOS-enabled location, see [Manually creating {{site.data.keyword.satelliteshort}} locations](#location-create-manual).



## Setting up the {{site.data.keyword.satelliteshort}} location control plane
{: #setup-control-plane}

The location control plane runs resources that are managed by {{site.data.keyword.satelliteshort}} to help manage the hosts, clusters, and other resources that you attach to the location.
{: shortdesc}

When you set up the {{site.data.keyword.satelliteshort}} location control plane, keep in mind the following host considerations.

- You must attach compute hosts in groups of 3 to your location that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs) and any provider-specific requirements. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
- The minimum of 3 hosts for the control plane is for demonstration purposes. However, depending on your control plane set up, 3 hosts might be sufficient for production workloads. To continue to use the location for production workloads, ensure your control plane meets the [suggested high-availability configuration](/docs/satellite?topic=satellite-ha#satellite-ha-setup) for network redundancy, distribution across physical locations, and so on. You can also [attach more hosts to the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) in multiples of 3, such as 6, 9, or 12 hosts.
- Make sure that your hosts meet the [latency requirements](/docs/satellite?topic=satellite-service-architecture#architecture-latency).

### Setting up the control plane from the console
{: #control-plane-ui}

Use the {{site.data.keyword.satelliteshort}} console to set up a control plane for your location.
{: shortdesc}

Before you begin
- [Attach at least 6 hosts (or 3 hosts for demonstration purposes only) to your location](/docs/satellite?topic=satellite-attach-hosts) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
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

### Setting up the control plane from the CLI
{: #control-plane-cli}

Use the {{site.data.keyword.satelliteshort}} command line to set up a control plane for your location.
{: shortdesc}

Before you begin
- [Attach at least 6 hosts (or 3 hosts for demonstration purposes only) to your location](/docs/satellite?topic=satellite-attach-hosts) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
- Verify that your location is in an **Action required** state.

To create the control plane,

1. Identify the hosts that you want to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. All hosts must be in an `unassigned` state.

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

2. Optional: If you want to assign hosts to your control plane by using a host label, retrieve the details of your host. Available labels that you can use are listed in the **Labels** section of your CLI output.

    ```sh
    ibmcloud sat host get --location <location_name_or_ID> --host <host_ID>
    ```
    {: pre}

    Example output

    ```sh
    Retrieving host details...

    Name:     mymachine1   
    ID:       brjrgp920bg4u254brr0   
    State:    unassigned  
    Status:   -   

    Labels      
    cpu      4   
    memory   32774980  
    use      satloc

    Assignment        
    Cluster:       -  
    Worker Pool:   -  
    Zone:          -  
    Worker ID:     -
    Worker IP:     -   
    Date:          -   
    OK
    ```
    {: screen}

3. Assign your host machine to the {{site.data.keyword.satelliteshort}} location control plane. When you assign the host to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process takes a few minutes to complete. You can choose to assign a host by using the host ID, or you can also define the label that the host must have to be assigned to the location.

    Example for assigning a host by using the host ID.

    ```sh
    ibmcloud sat host assign --location <location_name_or_ID>  --cluster <location_ID> --host <host_ID>  --zone <zone>
    ```
    {: pre}

    Example for assigning a host by using the `use:satloc` label.

    ```sh
    ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --host-label "use:satloc" --zone <zone>
    ```
    {: pre}
    
    Example for assigning a host that is enabled for Red Hat CoreOS.
    
    ```sh
    ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --host-label "use:satloc" --zone <zone>  --operating-system RHCOS

    ```
    {: pre}

    `--location <location_name_or_ID>`
    :   Enter the name or ID of your {{site.data.keyword.satelliteshort}} location. To retrieve the location name or ID, run `ibmcloud sat location ls`.
    
    `--cluster <location_ID>`
    :   Enter the ID of the {{site.data.keyword.satelliteshort}} location where you want to assign the hosts to run the {{site.data.keyword.satelliteshort}} location control plane. To view your location ID, run `ibmcloud sat location ls`.
    
    `--host <host_ID>`
    :   Enter the host ID to assign to the {{site.data.keyword.satelliteshort}} location control plane. To view the host ID, run `ibmcloud sat host ls --location <location_name>`. You can use the `--host-label` option to identify the host that you want to assign to your control plane.
    
    `--host-label <label>`
    :   Enter the label that you want to use to identify the host that you want to assign. The label must be a key-value pair, and must exist on the host machine. When you run this command with the `label` option, the first host that is in an `unassigned` state and matches the label is assigned to your control plane.
    
    `--zone <zone>`
    :   Enter the zone to assign the host in, which can correspond to a physically separate zone in your infrastructure provider. To see the zone names for your location, run `ibmcloud sat location get --location <location_name_or_ID>` and look for the **Host Zones** field.
    
    `--operating-system <RHEL_or_RHCOS>`
    :    The operating system for the hosts you want to assign to your location. The available options are `RHEL7` and `RHCOS`.


4. Repeat the previous step for the other hosts that you want to attach to your {{site.data.keyword.satelliteshort}} location control plane. For high availability, make sure that you assign hosts evenly across zones that correspond to physically separate zones in your infrastructure provider. For example, if your infrastructure provider has `us-east-1a`, `us-east-1b`, and `us-east-1c`, you can enter these names for your {{site.data.keyword.satelliteshort}} zones. Then, assign 2 hosts from `us-east-1a` in your infrastructure provider to `us-east-1a` in your {{site.data.keyword.satelliteshort}} control plane, 2 hosts from `us-east-1b`, and 2 hosts from `us-east-1c`, for a total of 6 hosts in the control plane.

5. Verify that your hosts are successfully assigned to your location. The assignment is successful when all hosts show an **assigned** state and a **Ready** status, and an IP address is assigned to the host. If the **Status** of your machines shows `-`, the bootstrapping process is not yet completed and the health status could not be retrieved. Wait a few minutes, and then try again.

    ```sh
    ibmcloud sat host ls --location <location_name>
    ```
    {: pre}

    Example output

    ```sh
    Retrieving hosts...
    OK
    Name              ID                     State      Status   Cluster          Worker ID                                                 Worker IP   
    machine-name-1    aaaaa1a11aaaaaa111aa   assigned   Ready    infrastructure   sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.xx.xxx.xxx   
    machine-name-2    bbbbbbb22bb2bbb222b2   assigned   Ready    infrastructure   sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.xx.xxx.xxx   
    machine-name-3    ccccc3c33ccccc3333cc   assigned   Ready    infrastructure   sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.xx.xxx.xxx   
    ```
    {: screen}

6. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show **action required**, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
    {: note}

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

    Example output

    ```sh
    OK
    Name         ID                     Status      Ready   Created      Hosts (used/total)   Managed From   
    mylocation   brhtfum2015a6mgqj16g   normal      yes     4 days ago   3 / 3                Washington DC   
    ```
    {: screen}

7. Verify that the IP addresses of all your hosts were registered and added to the DNS record of your location. Check that the cert status is **created** and that the records are populated with the subdomains.

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

8. To continue to use the location for production workloads, repeat these steps to attach more hosts to the location control plane in multiples of 3, such as 6, 9, or 12 hosts. For more information, see [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-location-sizing#control-plane-attach-capacity).


## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #location-control-plane-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.
