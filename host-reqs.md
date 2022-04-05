---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-05"

keywords: satellite, hybrid, multicloud, endpoint capacity, endpoint limits, location endpoint limits, location endpoints, cloud endpoints

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Host system requirements
{: #host-reqs}

Review the following requirements that relate to the computing and system setup of host machines for {{site.data.keyword.satellitelong}}.
{: shortdesc}

You can add hosts from other cloud providers to your location. For more information, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).

Can't meet these host requirements? [Contact {{site.data.keyword.IBM_notm}} Support](/docs/get-support?topic=get-support-using-avatar) and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.
{: note}



## Computing characteristics
{: #reqs-host-compute}

- Hosts must run Red Hat Enterprise Linux 7 on x86 architecture with the kernel that is distributed with that version. Other operating systems, such as Windows; other mainframe systems, such as IBM Z or Power; and other kernel versions are not supported. Make sure that you use minimal RHEL images. Do not install the LAMP stack.
- Hosts can be physical or virtual machines. However, if your hosts are cloned virtual machines, be sure that each one has a unique network identity. For more information, see [Why aren't my hosts attaching to my location?](/docs/satellite?topic=satellite-host-not-attaching).
- Hosts must have at least 4 vCPU, 16 GB memory, and [sufficient storage capacity](/docs/satellite?topic=satellite-reqs-host-storage). 

- If your host has GPU compute, make sure that you install the node feature discovery and NVIDIA GPU operators. For more information, see the prerequisite in [Deploying an app on a GPU machine](/docs/openshift?topic=openshift-deploy_app#gpu_app).
- Hostnames can contain only lowercase alphanumeric characters, `-`, or `.`.
- Hosts must have an ext4 filesystem for the boot disk.



## Red Hat Enterprise Linux (RHEL) packages and other machine configurations
{: #reqs-host-packages}

Hosts must have access to {{site.data.keyword.redhat_notm}} updates and the following packages. 
{: shortdesc}


```sh
Repository 'rhel-server-rhscl-7-rpms' is enabled for this system.
Repository 'rhel-7-server-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-rh-common-rpms' is enabled for this system.
Repository 'rhel-7-server-supplementary-rpms' is enabled for this system.
Repository 'rhel-7-server-extras-rpms' is enabled for this system.
```
{: screen}

After the host is created, do not change the default packages or operating system settings, such as disabling SELinux or IPv6.
In addition, do not install any additional packages, configuration, or other customizations to your hosts. For more information, see [Why can't I install extra software like vulnerability scanning tools on my host?](/docs/satellite?topic=satellite-faqs#host-software).
{: important}

You might need to refresh your packages on the host machine. For example, in {{site.data.keyword.IBM_notm}} Cloud infrastructure you can run the following commands to add the required packages.

1. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
    ```sh
    subscription-manager refresh
    ```
    {: pre}

2. Enable the package repositories on your machine.
    ```sh
    subscription-manager repos --enable rhel-server-rhscl-7-rpms
    subscription-manager repos --enable rhel-7-server-optional-rpms
    subscription-manager repos --enable rhel-7-server-rh-common-rpms
    subscription-manager repos --enable rhel-7-server-supplementary-rpms
    subscription-manager repos --enable rhel-7-server-extras-rpms
    ```
    {: pre}

For more information about how to enable the {{site.data.keyword.redhat_notm}} packages in hosts that you add from other cloud providers, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).


