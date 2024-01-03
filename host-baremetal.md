---


copyright:
  years: 2023, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, bare metal host, bare metal, baremetal

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Attaching Bare metal hosts
{: #host-baremetal}

You can use a supported bare metal server as a host attached to your {{site.data.keyword.satelliteshort}} location, including {{site.data.keyword.baremetal_long}} for Classic.
{: shortdesc}

## {{site.data.keyword.baremetal_short_sing}} requirements
{: #bare-metal-reqs}

To attach a bare metal host, your {{site.data.keyword.baremetal_short_sing}} must meet the following requirements. 

- Must support virtualization technology.
    - For Intel CPUs, support for virtualization is referred to as `Intel VT` or `VT-x`.
    - For AMD CPUs, support for virtualization is referred to as `AMD Virtualization` or `AMD-V`.
- Must have a minimum of 8 cores and 32 GB RAM, plus any additional cores that you need for your vCPU overhead. For more information, see [CPU overhead](https://docs.openshift.com/container-platform/4.11/virt/install/preparing-cluster-for-virt.html#CPU-overhead_preparing-cluster-for-virt){: external} in the {{site.data.keyword.redhat_openshift_notm}} docs.
- Must include enough memory for your workload needs. For example: `360 MiB + (1.002 * requested memory) + 146 MiB + 8 MiB * (number of vCPUs) + 16 MiB * (number of graphics devices)`. For more information, see [Memory overhead](https://docs.openshift.com/container-platform/4.11/virt/install/preparing-cluster-for-virt.html#memory-overhead_preparing-cluster-for-virt){: external} in the {{site.data.keyword.redhat_openshift_notm}} docs.
- No operating system installed. The Red Hat CoreOS operating system is installed later in this process.
- If you want to use OpenShift Data Foundation as your storage solution, add 2 storage disks to each of your {{site.data.keyword.baremetal_short}} when you provision them.

If your servers do not meet these requirements, you can [create a {{site.data.keyword.baremetal_short_sing}}](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server). For a list of bare metal options, see [Available options for a bare metal server](/docs/bare-metal?topic=bare-metal-about-bm#options-for-bare-metal-servers).
{: tip}

## Attaching bare metal servers to your location
{: #bare-metal-attach}


Follow these general steps to attach your bare metal servers to your location. These steps might vary, depending on your specific hardware. For a complete tutorial, see [Attaching {{site.data.keyword.cloud_notm}} Bare Metal Servers for Classic hosts](/docs/satellite?topic=satellite-assign-bare-metal). 

1. [Download a Red Hat CoreOS ISO](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external}. Find the corresponding ISO version that matches the {{site.data.keyword.redhat_openshift_notm}} version that you want to use. For example, if you want to use version 4.11, [download a version of RHCOS for 4.11](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/4.11/){: external}, such as `rhcos-4.11.9-x86_64-live.x86_64.iso`.
2. Log in to your bare metal server.
3. In the BIOS settings, ensure that virtualization is enabled.
4. Set up your boot order to boot the Red Hat CoreOS ISO file that you downloaded in step 1.
5. Boot your system to install the ISO.
6. After Red Hat CoreOS is booted into memory, set up network connections so you can provide the location ignition file and attach the server to your location.
7. Download the ignition script for your {{site.data.keyword.satelliteshort}} location.
    
    ```sh
    ibmcloud sat host attach --location <location_name> --operating-system RHCOS
    ```
    {: pre}
    
9. Edit your ignition file to include the bare metal host name and network information. For more information about adding these details, see [Configuring your ignition file](/docs/satellite?topic=satellite-assign-bare-metal#config-ignition). Note that you must edit the ignition file for each bare metal server that you want to attach to your location.
10. Put your ignition file in a location that your bare metal server can reach. For example, you can upload it to an {{site.data.keyword.cos_full_notm}} bucket.
11. Download the ignition script to your bare metal server.

    ```sh
    curl <ignition_file_location > ignition.ign
    ```
    {: pre}
    
12. Run the following `install` command to start the ignition file. Replace `<diskName>` with the full path of the disk location where you want Red Hat  CoreOS installed and replace `<filename>` with the path of the ignition file.

    ```sh
    sudo coreos-installer install <diskName> --ignition-file <filename>
    ```
    {: pre}
    
    The installation process can take an hour or two to complete. 
    {: note}

13. After the installation completes, unplug your RHCOS ISO file and reboot from your hard disk. 
14. Check your {{site.data.keyword.satelliteshort}} location to confirm that your bare metal server is attached.
15. Repeat these steps for each bare metal host that you want to attach to your location.

## I added hosts to my location, what's next?
{: #bare-metal-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.


