---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: satellite, hybrid, attaching hosts, hosts, attach hosts, attach hosts to location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Attaching hosts to your location
{: #attach-hosts}

After you create the location in {{site.data.keyword.satellitelong}}, add compute capacity to your location so that you can set up {{site.data.keyword.redhat_openshift_notm}} clusters or interact with other [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services).
{: shortdesc}

To attach Red Hat CoreOS (RHCOS) hosts to your location, you must have created a Red Hat CoreOS enabled location. For more information, see [Creating a location](/docs/satellite?topic=satellite-locations). Note that you can still attach Red Hat Enterprise Linux hosts to a location that is enabled for Red Hat CoreOS.
{: note}

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
{: tip}

Before you begin, make sure that you have created host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers. For more information about how to configure hosts in public cloud providers to meet these minimum requirements, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
{: important}

## Downloading the host attachment script for your location
{: #host-attach-download}

To attach hosts to your location, you must download a host attachment script. After you download the script, you can run it on your hosts to attach them to your location. You can get the attachment script from the console or by running the `sat host attach` [command](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach). Note that for CoreOS hosts, the attachment script is an ignition (`.ign`) file. For RHEL hosts, the attachment script is a Shell script.

1. Download the host attachment script. 
    * **To get the host attachment script from the console**:
        1. Navigate to the [**Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to attach hosts.  
        2. From the **Hosts** tab, click **Attach host**.
        3. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use=satcp` or `use=satcluster` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.redhat_openshift_notm}} cluster. By default, your hosts get a `cpu`, an `os`, and a `memory` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`. Note that the default value for `os` is `rhel`.
        4. Enter a file name for your script or use the name that is generated for you.
        5. Select **RHEL 7** or **RHCOS** to download the host script for your host system.
        6. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which should be treated and protected as sensitive information.
        
    * **To get the host attachment script from the CLI**:
        1. Generate a script to run on your machines to attach them to your location by using the `sat host attach` command and specify the host operating system by using the `--operating-system` command option. When you run the command to generate the script, you might also want to include labels to identify the purpose of the hosts, such as `use:satloc`. Your hosts are automatically assigned labels for the CPU and memory size if these values can be detected on the machine.

            Example `host attach` command for a RHCOS host.
            
            ```sh
            ibmcloud sat host attach --location <location_name> [-hl "use=satloc"] --operating-system RHCOS
            ```
            {: pre}

            Example `host attach` command for a RHEL host.
            ```sh
            ibmcloud sat host attach --location <location_name> [-hl "use=satloc"] --operating-system RHEL
            ```
            {: pre}
            
            Example output
            ```sh
            Creating host registration script...
            OK
            The script to attach hosts to Satellite location 'mylocation' was downloaded to the following location:
            <filepath_to_script>/register-host_mylocation_attach_hypershift.ign
            ```
            {: screen}
     

1. **Optional**: If your hosts are RHEL hosts, you need to update the [required packages](/docs/satellite?topic=satellite-host-reqs) on your hosts before you can run the script. If your hosts are running the latest Red Hat CoreOS images, you do not need to update the packages.

1. Attach your hosts to your location by running the attachment script.
    * If your hosts are in another cloud provider, follow the provider-specific steps to run the script and attach your host. 
        - [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
        - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
        - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
        - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
        - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)
    * If your hosts are in an on-premises data center, see [Attaching on-premises RHCOS hosts](#attach-rhcos-hosts) or [Attaching on-premises RHEL hosts](#attach-rhel-hosts).

## Attaching on-premises RHEL hosts to your location
{: #attach-rhel-hosts}

To attach RHEL hosts that reside in your on-premises data center to your location, follow these general steps to run the host attachment script.

1. [Download the host script](#attach-hosts) for your location.
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

5. Update your host to have the required RHEL packages. For more information about how to install these package, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}.
6. Run the script.
    ```sh
    sudo nohup bash /tmp/attach.sh &
    ```
    {: pre}

7. Check the progress of the registration script.
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}

8. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. This process might take a few minutes to complete. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.redhat_openshift_notm}} cluster.

9. After you have attached your hosts, assign them to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or use them to create a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

If your host is not attaching to your location, you can log in to the host to debug it. For more information, see [Logging in to a RHEL host machine to debug](/docs/satellite?topic=satellite-ts-hosts-login).
{: tip}

## Attaching on-premises RHCOS hosts to your location
{: #attach-rhcos-hosts}

To attach RHCOS hosts that reside in your on-premises data center to your location, follow these general steps to run the host attachment script.

1. [Download the host script](#attach-hosts) for your location. Note that for RHCOS hosts, the attachment script is a CoreOS ignition (`.ign`) file.
2. Boot your CoreOS host and include the file path to the ignition script as the `--user-data`. For example: `--user-data @/tmp/attach_hypershift.ign`.
3. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. This process might take a few minutes to complete. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.redhat_openshift_notm}} cluster.
4. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).


If your host is not attaching to your location, you can log in to the host to debug it. For more information, see [Logging in to a RHCOS host machine to debug](/docs/satellite?topic=satellite-ts-hosts-login-rhcos).
{: tip}



