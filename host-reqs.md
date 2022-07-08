---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

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


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Computing characteristics
{: #reqs-host-compute}

- Hosts must run Red Hat Enterprise Linux 7 or the latest Red Hat CoreOS on x86 architecture with the kernel that is distributed with that version. Other operating systems, such as Windows; other mainframe systems, such as IBM Z or Power; and other kernel versions are not supported. Make sure that you use minimal RHEL images. Do not install the LAMP stack.
- Hosts can be physical or virtual machines. However, if your hosts are cloned virtual machines, be sure that each one has a unique network identity. For more information, see [Why aren't my hosts attaching to my location?](/docs/satellite?topic=satellite-host-not-attaching).

- Red Hat CoreOS hosts must have at least 8 vCPU and 16GB memory and [sufficient storage capacity](/docs/satellite?topic=satellite-reqs-host-storage).
- RHEL hosts must have at least 4 vCPU, 16 GB memory, and [sufficient storage capacity](/docs/satellite?topic=satellite-reqs-host-storage). 

- RHEL hosts must have the `SELINUX=enforcing` policy set. You can verify that this policy is set by running `sestatus` and looking for `SELinux status: enabled` in the output.
- If your host has GPU compute, make sure that you install the node feature discovery and NVIDIA GPU operators. For more information, see the prerequisite in [Deploying an app on a GPU machine](/docs/openshift?topic=openshift-deploy_app#gpu_app).
- Hostnames can contain only lowercase alphanumeric characters, `-`, or `.`.
- Hosts must have an ext4 filesystem for the boot disk.



## Red Hat CoreOS (RHCOS) packages and other machine configurations
{: #reqs-host-packages-rhcos}

[Find a Red Hat CoreOS image](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external} for your system and make it available to your cloud provider. You can use any supported RHCOS image for your host.

## Red Hat Enterprise Linux (RHEL) packages and other machine configurations
{: #reqs-host-packages}

Hosts must have access to RHEL updates and the following packages. 
{: shortdesc}

These updates are required for hosts that are running RHEL. If your hosts are running Red Hat CoreOS images, you do not need to update the packages.
{: note}

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

For more information about how to enable the RHEL packages in hosts that you add from other cloud providers, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).

