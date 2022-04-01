---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-01"

keywords: satellite, hybrid, attaching hosts, hosts, attach hosts, attach hosts to location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Attaching hosts to your {{site.data.keyword.satelliteshort}} location
{: #attach-hosts}

After you create the location in {{site.data.keyword.satellitelong}}, add compute capacity to your location so that you can set up {{site.data.keyword.redhat_openshift_notm}} clusters or interact with other [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
{: shortdesc}

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-about-locations#location-sizing).
{: tip}

## Attaching hosts with the console
{: #attach-hosts-console}

Use the {{site.data.keyword.satelliteshort}} console to attach hosts to your location.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers. For more information about how to configure hosts in public cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
{: important}

1. From the [**Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to attach hosts.  
2. From the **Hosts** tab, click **Attach host**.
3. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use=satcp` or `use=satcluster` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster. By default, your hosts get a `cpu` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`. 
4. Enter a file name for your script or use the name that is generated for you.
5. Select **RHEL 7** or **RHCOS** to download the host script for your host system.
6. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which should be treated and protected as sensitive information.

    Depending on the provider of the host, you might also need to update the [required RHEL 7 packages](/docs/satellite?topic=satellite-host-reqs) on your hosts before you can run the script. For example, see the [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), or [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) RHEL package updates. 
    {: note}

6. Follow the cloud provider-specific steps to update the script and attach your host.
    - [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
    - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
    - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
    - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
    - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)

### Attaching RHEL hosts to an on-premises data center with the console
{: #attach-rhel-hosts-console}

To add host machines that reside in your on-premises data center, you can follow these general steps to run the host registration script on your machine.

1. [Download the host script](#attach-hosts-console) for your location. 
2. Retrieve the public IP address of your host, or if your host has only a private network interface, the private IP address of your host.      
3. Copy the script from your local machine to your host.
    ```sh
    scp -i <filepath_to_key_file> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
    ```
    {: pre}        

4. Log in to your host.
    ```sh
    ssh -i <filepath_to_key_file> <username>@<IP_address>
    ```
    {: pre}

5. Update your host to have the required RHEL 7 packages. For more information about how to install these package, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}.
6. Run the script.
    ```sh
    sudo nohup bash /tmp/attach.sh &
    ```
    {: pre}

7. Monitor the progress of the registration script.
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}

8. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. This process might take a few minutes to complete. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

9. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

## Attaching hosts from the CLI
{: #attach-hosts-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to attach hosts to your location.
{: shortdesc}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers. For more information about how to configure hosts in public cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
{: important}

1. Generate a script that you can copy and run on your machines to attach them as hosts to your location. You might want to include a label to identify the purpose of the hosts, such as `use:satloc`, because the hosts provide compute capacity for the {{site.data.keyword.satelliteshort}} location. Your hosts automatically are assigned labels for the CPU and memory size if these values can be detected on the machine. 

    Example for RHEL7 host script
    ```sh
    ibmcloud sat host attach --location <location_name> [-hl "use=satloc"]
    ```
    {: pre}
    
    Example output
    ```sh
    Creating host registration script...
    OK
    The script to attach hosts to Satellite location 'mylocation' was downloaded to the following location:

    <filepath_to_script>/register-host_mylocation_123456789.sh
    ```
    {: screen}
    
    Depending on the provider of the host, you might also need to update the [required RHEL 7 packages](/docs/satellite?topic=satellite-host-reqs) on your hosts before you can run the script. For example, see the [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), or [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) RHEL package updates. 
    {: note}

2. On your local machine, find the script.
3. Follow the cloud provider-specific steps to update the script and attach your host.
    - [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
    - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
    - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
    - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
    - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)

### Attaching RHEL hosts to an on-premises data center from the CLI
{: #attach-rhel-hosts-cli}

To add host machines that reside in your on-premises data center, you can follow these general steps to run the host registration script on your machine.

1. [Download the host script](#attach-hosts-cli) for your location. 
2. Retrieve the public IP address of your host, or if your host has only a private network interface, the private IP address of your host.      
3. Copy the script from your local machine to your host.
    ```sh
    scp -i <filepath_to_key_file> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
    ```
    {: pre}        

4. Log in to your host.
    ```sh
    ssh -i <filepath_to_key_file> <username>@<IP_address>
    ```
    {: pre}

5. Update your host to have the required RHEL 7 packages. For more information about how to install these package, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}. 
6. Run the script.
    ```sh
    sudo nohup bash /tmp/attach.sh &
    ```
    {: pre}

7. Monitor the progress of the registration script.
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}

8. Verify that your hosts are attached to your location. Your hosts are not yet assigned to the {{site.data.keyword.satelliteshort}} control plane or an {{site.data.keyword.redhat_openshift_notm}} cluster which is why hosts show an `unassigned` state without any cluster or worker node information.
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

9. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).



