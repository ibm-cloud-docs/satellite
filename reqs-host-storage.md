---


copyright:
  years: 2020, 2024
lastupdated: "2024-10-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Host storage and attached devices
{: #reqs-host-storage}

Review the following storage requirements for hosts that are assigned to the location control plane and to services. Note that depending on the services you want to use, the storage requirements for hosts that are assigned to those services vary. For specific service requirements, refer to the service documentation.
{: shortdesc}

Hosts must have a boot device with an `ext4` file system and enough space to boot the host and run the operating system. While a minimum of 10 GiB is required, 25 GiB is recommended. In addition, `/tmp` and `/usr` must each have at least 1.5 GiB available. Hosts can't have a device that is mounted to `/var/data`. The `/boot` partition must be a minimum of 1 GiB.

![Host storage](/images/sat_architecture_host_storage.svg){: caption="Satellite host storage requirements" caption-side="bottom"}

Boot device (Operating system)
:   Hosts must have a boot device with an `ext4` file system and enough space to boot the host and run the operating system. While a minimum of 10 GiB is required, 25 GiB is recommended. 

Bootstrap device (Container image storage)
:   Hosts must have a second device with a minimum of 100 GiB of unmounted and unformatted device space. For hosts assigned to the control plane, the attached storage device must support at least 1000 IOPs. For example, a 10 IOPs/GiB device with at least 100 GiB capacity. The required IOPS varies with the number of clusters in the location, and the activity of the masters for those clusters. For more information, see [How many clusters can I run before I need to add capacity to the control plane](/docs/satellite?topic=satellite-location-sizing#control-plane-how-many-clusters)?

Storage for hosts that are assigned to services (for example, clusters) `*`
:   Depending on the services that you want to use, you might need to add more storage before you attach hosts to your location. For service-specific storage requirements, refer to the service documentation. If you plan to deploy additional services or operators to the clusters in your location, you must account for cluster storage plus any additional deployments that you want to use in your clusters. For example, OpenShift Data Foundation.
    - [{{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-managed-services)
    - [{{site.data.keyword.satelliteshort}} storage overview](/docs/satellite?topic=satellite-storage-template-ov)
