---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-13"

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

## Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}
{: #ibm-host-attach}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from {{site.data.keyword.cloud_notm}}.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).

1. Follow the steps to create a [classic public virtual server](/docs/virtual-servers?topic=virtual-servers-ordering-vs-public) or a virtual server instance in a [VPC](/docs/vpc?topic=vpc-creating-virtual-servers). Make sure that you select a supported RHEL 7 operating system, configure the machine with at least 4 CPU and 16 RAM, and add a boot disk with a size of at least 100 GB. 
2. Wait for your virtual server instance to be provisioned.
3. Get the registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
    ```sh
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}

4. Retrieve the IP address and ID of your machine.
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

5. Retrieve the credentials to log in to your virtual machine.
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

6. Copy the script from your local machine to the virtual server instance.
    ```sh
    scp <path_to_attachHost.sh> root@<ip_address>:/tmp/attach.sh
    ```
    {: pre}

    If you use an SSH key to log in, make sure to convert the key to `.key` format and use the following command.
    ```sh
    scp -i <filepath_to_key_file.key> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
    ```
    {: pre}

7. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.
    ```sh
    ssh root@<ip_address>
    ```
    {: pre}

    If you use an SSH key to log in, use the following command.
    ```sh
    ssh -i <filepath_to_key_file.key> <username>@<IP_address>
    ```
    {: pre}

8. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
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

9. Run the registration script on your machine.
    ```sh
    nohup bash /tmp/attach.sh &
    ```
    {: pre}

10. Monitor the progress of the registration script.
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}

11. Exit the SSH session.  	
    ```sh
    exit
    ```
    {: pre}

12. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

13. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign-manual).


