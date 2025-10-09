---


copyright:
  years: 2020, 2025
lastupdated: "2025-10-09"

keywords: satellite, hybrid, multicloud, endpoint capacity, endpoint limits, location endpoint limits, location endpoints, cloud endpoints

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Host system requirements
{: #host-reqs}

Review the following requirements that relate to the computing and system setup of host machines for {{site.data.keyword.satellitelong}}.
{: shortdesc}

You can add hosts from other cloud providers to your location. For more information, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).

- If you plan on deploying {{site.data.keyword.redhat_openshift_notm}} clusters, make sure the operating system that you want to use for your hosts is supported for your location type and cluster version. For more information, see [{{site.data.keyword.redhat_openshift_notm}} version information](/docs/openshift?topic=openshift-openshift_versions).
- Make sure that you use [official Red Hat certified hardware](https://catalog.redhat.com/hardware){: external}.
- If you cannot meet these host requirements, [contact {{site.data.keyword.IBM_notm}} Support](/docs/account?topic=account-using-avatar) and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.

You can verify your host setup with the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


| Host OS | Satellite Control Plane support | Worker node OS support |
| --- | --- | --- |
| RHEL 8 | Yes | Yes |
| RHEL 9 | Yes | Yes |
| RHCOS | No | No |
{: class="simple-tab-table"}
{: caption="Non-CoreOS enabled locations" caption-side="bottom"}
{: #os-support-1}
{: tab-title="{{site.data.keyword.satelliteshort}}"}
{: tab-group="os-table"}

| Host OS | Satellite Control Plane support | Worker node OS support |
| --- | --- | --- |
| RHEL 8 | Yes | Yes |
| RHEL 9 | No | Yes, for OpenShift 4.17 and later only |
| RHCOS | Yes | Yes |
{: class="simple-tab-table"}
{: caption="CoreOS enabled locations" caption-side="bottom"}
{: #os-support-2}
{: tab-title="{{site.data.keyword.satelliteshort}}"}
{: tab-group="os-table"}

## Computing characteristics
{: #reqs-host-compute}

- Hosts must support x86-64-v2 architecture.
- Hosts must run the latest RHEL 8, RHEL 9, or the latest Red Hat CoreOS with the kernel that is distributed with those versions. Other operating systems, such as Windows; other mainframe systems, such as IBM Z or IBM Power; and other kernel versions are not supported.
    - For the latest RHEL 8 version information, see [Red Hat Enterprise Linux Release Dates](https://access.redhat.com/articles/3078#RHEL8){: external}.
    - For the latest RHEL 9 version information, see [Red Hat Enterprise Linux Release Dates](https://access.redhat.com/articles/3078#RHEL9){: external}.
    - For the latest Red Hat CoreOS version information, see [Red Hat CoreOS mirrors](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external}.
- Make sure that you use minimal RHEL images. Do not install the LAMP stack. Do not install virtualization technologies on the hosts, including `libvirtd` or `docker`.
- Hosts can be physical or virtual machines. However, if your hosts are cloned virtual machines, be sure that each one has a unique network identity. For more information, see [Why aren't my hosts attaching to my location?](/docs/satellite?topic=satellite-host-not-attaching).
- Red Hat CoreOS hosts must meet [minimum sizing requirements](/docs/satellite?topic=satellite-location-sizing#control-plane-how-many-clusters-rhcos) and [sufficient storage capacity](/docs/satellite?topic=satellite-reqs-host-storage). 
- RHEL hosts must meet [minimum sizing requirements](/docs/satellite?topic=satellite-location-sizing#control-plane-how-many-clusters-rhel) and [sufficient storage capacity](/docs/satellite?topic=satellite-reqs-host-storage). 

- RHEL hosts must have the `SELINUX=enforcing` policy set. You can verify that this policy is set by running the `sestatus` command and looking for `SELinux status: enabled` in the output.
- If your host includes GPU compute, make sure that you install the Node Feature Discovery and NVIDIA GPU operators. For more information, see the prerequisite steps in [Deploying an app on a GPU machine](/docs/openshift?topic=openshift-deploy_app#gpu_app).
- Hostnames can contain only lowercase alphanumeric characters, `-`, or `.`.
- Hosts must have an ext4 file system for the boot disk.



## Red Hat CoreOS (RHCOS) packages and other machine configurations
{: #reqs-host-packages-rhcos}

[Find a Red Hat CoreOS image](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external} for your system and make it available to your cloud provider. You can use any supported RHCOS image for your host.

## Red Hat Enterprise Linux (RHEL) packages and other machine configurations
{: #reqs-host-packages}

Hosts must have access to RHEL updates and the following packages. 
{: shortdesc}

For RHEL 8
```sh
Repository 'rhel-8-for-x86_64-appstream-rpms' is enabled for this system.
Repository 'rhel-8-for-x86_64-baseos-rpms' is enabled for this system.
```
{: screen}

For RHEL 9
```sh
Repository 'rhel-9-for-x86_64-appstream-rpms' is enabled for this system.
Repository 'rhel-9-for-x86_64-baseos-rpms' is enabled for this system.
```
{: screen}

After the host is created, do not install any additional packages, configuration, or other customizations to your hosts. For more information, see [Why can't I install extra software like vulnerability scanning tools on my host?](/docs/satellite?topic=satellite-faqs#host-software).
{: important}

You might need to refresh your packages on the host machine. For example, in {{site.data.keyword.IBM_notm}} Cloud infrastructure you can run the following commands to add the required packages.

1. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
    ```sh
    subscription-manager refresh
    ```
    {: pre}

2. Enable the package repositories on your machine.
    
   RHEL 8:
    ```sh
    subscription-manager repos --enable rhel-8-for-x86_64-appstream-rpms
    subscription-manager repos --enable rhel-8-for-x86_64-baseos-rpms
    ```
    {: pre}

    RHEL 9:
    ```sh
    subscription-manager repos --enable rhel-9-for-x86_64-appstream-rpms
    subscription-manager repos --enable rhel-9-for-x86_64-baseos-rpms
    ```
    {: pre}

For more information about how to enable the RHEL packages in hosts that you add from other cloud providers, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
