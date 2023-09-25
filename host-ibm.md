---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-25"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Attaching {{site.data.keyword.cloud_notm}} hosts for tests
{: #ibm}

Test out an {{site.data.keyword.satellitelong}} location with virtual instances that you created in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

**Testing only**: {{site.data.keyword.satelliteshort}} is an extension of {{site.data.keyword.cloud_notm}} into other infrastructure providers. As such, adding {{site.data.keyword.cloud_notm}} infrastructure hosts to {{site.data.keyword.satelliteshort}} is supported only for testing, demo, or proof of concept purposes. For production workloads in your {{site.data.keyword.satelliteshort}} location, use on-premises, edge, or other cloud provider hosts. You can also create {{site.data.keyword.openshiftlong_notm}} clusters in the public cloud and add them to a {{site.data.keyword.satelliteshort}} Config cluster group to deploy the same app across your {{site.data.keyword.satelliteshort}} and {{site.data.keyword.cloud_notm}} clusters.
{: important}

To attach Red Hat CoreOS (RHCOS) hosts, your location must be enabled for Red Hat CoreOS. For more information, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). Note that you can still attach Red Hat Enterprise Linux hosts to a location that is enabled for Red Hat CoreOS.

Before you begin, make sure that you create host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers.

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
{: tip}

## Manually adding {{site.data.keyword.cloud_notm}} RHEL hosts to {{site.data.keyword.satelliteshort}}
{: #ibm-host-attach}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from {{site.data.keyword.cloud_notm}}.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 or 8 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. Follow the steps to create a [classic public virtual server](/docs/virtual-servers?topic=virtual-servers-ordering-vs-public) or a virtual server instance in a [VPC](/docs/vpc?topic=vpc-creating-virtual-servers). Make sure that you select a supported RHEL 7 or 8 operating system or a supported Red Hat CoreOS (RHCOS) image, configure the machine with at least 4 CPU and 16 RAM, and add a boot disk with a size of at least 100 GB. 
1. Wait for your virtual server instance to be provisioned.
1. Get the registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location. Note that the token in the script is an API key, which should be treated and protected as sensitive information. Make a note of the location of the attach script. Also note that for RHEL-based hosts, the attach script is a Shell script. 
    ```sh
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}
    
1. Retrieve the IP address and ID of your machine.
    * Classic
        ```sh
        ibmcloud sl vs list
        ```
       {: pre}

    * VPC
        ```sh
        ibmcloud is instances
        ```
        {: pre}

1. Retrieve the credentials to log in to your virtual machine.
    * Classic
        ```sh
        ibmcloud sl vs credentials <vm_ID>
        ```
        {: pre}

    * VPC
        ```sh
        ibmcloud is instance-initialization-values <instance_ID>
        ```
        {: pre}

1. Copy the script from your local machine to the virtual server instance.
    ```sh
    scp <path_to_attachHost.sh> root@<ip_address>:/tmp/attach.sh
    ```
    {: pre}

    If you use an SSH key to log in, make sure to convert the key to `.key` format and use the following command.
    ```sh
    scp -i <filepath_to_key_file.key> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
    ```
    {: pre}

1. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.
    ```sh
    ssh root@<ip_address>
    ```
    {: pre}

    If you use an SSH key to log in, use the following command.
    ```sh
    ssh -i <filepath_to_key_file.key> <username>@<IP_address>
    ```
    {: pre}

1. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
    ```sh
    subscription-manager refresh
    ```
    {: pre}
            
1. Enable the package repositories on your machine.    

    - RHEL 7:
        ```sh
        subscription-manager repos --enable rhel-server-rhscl-7-rpms
        subscription-manager repos --enable rhel-7-server-optional-rpms
        subscription-manager repos --enable rhel-7-server-rh-common-rpms
        subscription-manager repos --enable rhel-7-server-supplementary-rpms
        subscription-manager repos --enable rhel-7-server-extras-rpms
        ```
        {: pre}
    
    - RHEL 8 classic:
        ```sh
        subscription-manager repos --enable rhel-8-for-x86_64-appstream-rpms
        subscription-manager repos --enable rhel-8-for-x86_64-baseos-rpms
        ```
        {: pre}
            
    - RHEL 8 VPC:
        ```sh
        subscription-manager release --set=8
        subscription-manager repos --enable rhel-8-for-x86_64-baseos-rpms 
        subscription-manager repos --enable rhel-8-for-x86_64-appstream-rpms
        subscription-manager repos --disable='*eus*'
        yum install container-selinux -y
        ```
        {: pre}

1. Run the registration script on your machine.
    ```sh
    nohup bash /tmp/attach.sh &
    ```
    {: pre}
            
1. Monitor the progress of the registration script.
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}

1. Exit the SSH session.  	
    ```sh
    exit
    ```
    {: pre}

1. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

1. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts).

## Manually adding {{site.data.keyword.cloud_notm}} RHCOS hosts to {{site.data.keyword.satelliteshort}}
{: #ibm-host-attach-rhcos}

You can create your {{site.data.keyword.satelliteshort}} location with hosts that you added from {{site.data.keyword.cloud_notm}}.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 or 8 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).


1. Get the registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location. Note that the token in the script is an API key, which should be treated and protected as sensitive information. Make a note of the location of the attach script. Also note that for RHCOS hosts, the attach script is a RHCOS ignition file. 
    ```sh
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}

 
1. [Download the Red Hat CoreOS image](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external} that you want to use.
   Use {{site.data.keyword.cos_full_notm}} to store your custom image. If you don't already have an instance, [create one](/docs/openshift?topic=openshift-storage-cos-understand#create_cos_service) and create at least one bucket.
        
1. [Upload the RHCOS image](/docs/cloud-object-storage?topic=cloud-object-storage-upload) that you downloaded earlier to a bucket in your {{site.data.keyword.cos_short}} instance.
    You can use the Minio command-line client to copy your image from a directory on your local machine to your bucket. First, [create a set of HMAC service credentials](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials) and make a note of the `access_key_id` and `secret_access_key`. Then, install the [Minio client](/docs/cloud-object-storage?topic=cloud-object-storage-minio){: external} and configure it to use your credentials.
    {: tip}
            
1. [Grant access to {{site.data.keyword.cos_short}}](/docs/vpc?topic=vpc-object-storage-prereq&interface=cli).
            
1. [Import your custom RHCOS image in VPC](/docs/account?topic=account-catalog-vsivpc-tutorial). You can create custom images in the [VPC console](https://cloud.ibm.com/vpc-ext/compute/images){: external}.
        
1. Give your image a **Name**, select the **Resource group** where you want to create the image and select **Cloud Object Storage**
        
1. In the **Cloud Object Storage** instances section, select your instance, location, and the bucket where you uploaded your image.
        
1. In the **Operating system** section, select **Red Hat Enterprise Linux**, then select **fedora-coreos-stable-amd64**.
        
1. Select the **Encryption** that you want to use and click **Create custom image**.

1. Using your custom image, create VPC Gen 2 instances and attach them to your RHCOS enabled location. Note that the `ATTACH-SCRIPT-LOCATION` parameter is the location of the ignition file you retrieved earlier by running the `ibmcloud sat host attach` command. Make sure to include the `@` sign before the path to your ignition file.
    ```sh
    ibmcloud is instance-create INSTANCE-NAME VPC VPC-ZONE-NAME VPC-PROFILE-NAME VPC-SUBNET --image VPC-RHCOS-IMAGE-ID --user-data @ATTACH-SCRIPT-LOCATION --keys SSH-KEY-ID
    ```
    {: pre}
            
    Example command to create VPC Gen 2 instances and attach hosts to Red Hat CoreOS enabled location. For more information, about the `instance create` command, see the VPC Gen 2 [command line reference](/docs/vpc?topic=vpc-vpc-reference#instance-create).
            
    ```sh
    ibmcloud is instance-create instance-1 my-vpc us-south-1 bx2d-4x16 0111-11e11111-1c11-1111-11aa-ba1a1d1cd111 —-keys my-key --image r001-a1f111b1-11bc-1e1e-b11c-1d11c1111111 --user-data @/var/register-host_coreos.ign
    ```
    {: pre}
           
            
1. Repeat the previous step to create VPC Gen 2 instances for each host that you want to attach. Plan to attach at least 3 hosts to use in the control plane, and attach additional hosts for any services that you want to use.
            
1. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

1. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts).


## I added hosts to my location, what's next?
{: #ibm-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.




