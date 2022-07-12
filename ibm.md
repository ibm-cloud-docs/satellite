---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-12"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# {{site.data.keyword.cloud_notm}} for tests
{: #ibm}

Test out an {{site.data.keyword.satellitelong}} location with virtual instances that you created in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

**Testing only**: {{site.data.keyword.satelliteshort}} is an extension of {{site.data.keyword.cloud_notm}} into other infrastructure providers. As such, adding {{site.data.keyword.cloud_notm}} infrastructure hosts to {{site.data.keyword.satelliteshort}} is supported only for testing, demo, or proof of concept purposes. For production workloads in your {{site.data.keyword.satelliteshort}} location, use on-premises, edge, or other cloud provider hosts. You can also create {{site.data.keyword.openshiftlong_notm}} clusters in the public cloud and add them to a {{site.data.keyword.satelliteshort}} Config cluster group to deploy the same app across your {{site.data.keyword.satelliteshort}} and {{site.data.keyword.cloud_notm}} clusters.
{: important}

If your hosts are running Red Hat CoreOS (RHCOS), you must manually attach them to your location.
{: note}

## Automating your {{site.data.keyword.cloud_notm}} location setup with a Schematics template
{: #ibm-template}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from {{site.data.keyword.cloud_notm}} with a Schematics template.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. Or, you can [add {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}} manually](#ibm-host-attach).
{: tip}


1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
2. In the **Setup** section, click **IBM VPC Quick Start**.
3. You can edit the region, VSI, location details, and object storage for your location. When you are finished, click **Done editing**.
4. Click **Create location**.Your location might take about 30 minutes to finish provisioning.

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.

## Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}} manually
{: #ibm-host-attach}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from {{site.data.keyword.cloud_notm}}.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. Follow the steps to create a [classic public virtual server](/docs/virtual-servers?topic=virtual-servers-ordering-vs-public) or a virtual server instance in a [VPC](/docs/vpc?topic=vpc-creating-virtual-servers). Make sure that you select a supported RHEL 7 operating system or a supported Red Hat CoreOS image, configure the machine with at least 4 CPU and 16 RAM, and add a boot disk with a size of at least 100 GB. 
1. Wait for your virtual server instance to be provisioned.
1. Get the registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location. Note that the token in the script is an API key, which should be treated and protected as sensitive information. Make a note of the location of the attach script. Also note that for RHEL-based hosts, the attach script is a Shell script and for CoreOS hosts, the attach script is a CoreOS ignition file.
    ```sh
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}

    * **CoreOS hosts only**: 
        1. [Download the Red Hat CoreOS image](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external} that you want to use.
        1. You can use {{site.data.keyword.cos_full_notm}} to store your custom image. If you don't already have an instance, [create one](/docs/openshift?topic=openshift-storage-cos-understand#create_cos_service) and create at least one bucket.
        
        1. [Upload the Red Hat CoreOS image](/docs/cloud-object-storage?topic=cloud-object-storage-upload) that you downloaded earlier to a bucket in your {{site.data.keyword.cos_short}} instance.
            You can use the Minio command line client to copy your image from a directory on your local machine to your bucket. First, [create a set of HMAC service credentials](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials) and make a note of the `access_key_id` and `secret_access_key`. Then, install the [Minio client](/docs/cloud-object-storage?topic=cloud-object-storage-minio){: external} and configure it to use your credentials.
            {: tip}
            
        1. [Grant access to {{site.data.keyword.cos_short}} to import images](/docs/vpc?topic=vpc-object-storage-prereq&interface=cli).
            
        1. [Import your custom CoreOS image in VPC](/docs/vpc?topic=vpc-managing-images&interface=cli#import-custom-image). You can create custom images in the [VPC console](https://cloud.ibm.com/vpc-ext/compute/images){: external}.
        
        1. Give your image a **Name**, select the **Resource group** where you want to create the image and select **Cloud Object Storage**
        
        1. In the **Cloud Object Storage** instances section, select your instance, location, and the bucket where you uploaded your image.
        
        1. In the **Operating system** section, select **Red Hat Enterprise Linux**, then select **fedora-coreos-stable-amd64**.
        
        1. Select the **Encryption** that you want to use and click **Create custom image**

        1. Create VPC Gen 2 instances by using your custom image and attach them to your CoreOS-enabled location. Note that the `ATTACH-SCRIPT-LOCATION` parameter is the location of the ignition file you retrieved earlier by running the `ibmcloud sat host attach` command. Make sure to include the `@` sign before the path to your ignition file.
            ```sh
            ibmcloud is instance-create INSTANCE-NAME VPC VPC-ZONE-NAME VPC-PROFILE-NAME VPC-SUBNET --image VPC-RHCOS-IMAGE-ID --user-data @ATTACH-SCRIPT-LOCATION --keys SSH-KEY-ID
            ```
            {: pre}
            
            Example command to create VPC Gen 2 instances and attach hosts to a CoreOS-enabled location. For more information, about the `instance create` command, see the VPC Gen 2 [command line reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference&interface=cli#instance-create).
            
            ```sh
            ibmcloud is instance-create instance-1 my-vpc us-south-1 bx2d-4x16 0111-11e11111-1c11-1111-11aa-ba1a1d1cd111 —-keys my-key --image r001-a1f111b1-11bc-1e1e-b11c-1d11c1111111 --user-data @/var/register-host_coreos.ign
            ```
            {: pre}
           
            
        1. Repeat the previous step to create VPC Gen 2 instances for each host that you want to attach. Plan to attach at least 3 hosts to use in the control plane, and attach additional hosts for any services that you want to use.
            
        1. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

        1. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).

    * **RHEL hosts only**: 
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

            ```sh
            subscription-manager repos --enable rhel-server-rhscl-7-rpms
            subscription-manager repos --enable rhel-7-server-optional-rpms
            subscription-manager repos --enable rhel-7-server-rh-common-rpms
            subscription-manager repos --enable rhel-7-server-supplementary-rpms
            subscription-manager repos --enable rhel-7-server-extras-rpms
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

1. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).


## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #ibm-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.




