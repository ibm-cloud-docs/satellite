---

copyright:
  years: 2023, 2023
lastupdated: "2023-02-14"

keywords: satellite, control plane, location control plane, location, satellite location, create location, create control plane

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the {{site.data.keyword.satelliteshort}} location control plane
{: #setup-control-plane}

The location control plane runs resources that are managed by {{site.data.keyword.satelliteshort}} to help manage the hosts, clusters, and other resources that you attach to the location.
{: shortdesc}

When you set up the {{site.data.keyword.satelliteshort}} location control plane, keep in mind the following host considerations.

- Attach compute hosts in multiples of 3, such as 6, 9, or 12.
- Determine the the correct [size of your location](/docs/satellite?topic=satellite-location-sizing).
- Ensure your control plane meets the [suggested high-availability configuration](/docs/satellite?topic=satellite-ha#satellite-ha-setup) for network redundancy, distribution across physical locations, and so on.
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
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.
