---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-27"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Host storage and attached devices
{: #reqs-host-storage}

Review the following storage requirements for hosts assigned to the location control plane and hosts assigned to services. Note that depending on the services you want to use, the storage requirements for hosts assigned to those services vary. For specific service requirements, refer to the service documentation.
{: shortdesc}

Hosts can't have a device mounted to `/var/data`. `/tmp` and `/usr` must each have at least 1.5 GB available.
{: note}


![Host storage](/images/sat_architecture_host_storage.svg){: caption="Figure 1. Satellite host storage requirements" caption-side="bottom"}


Boot device (Operating system)
:   Hosts must have a boot device with an `ext4` file system and enough space to boot the host and run the operating system. 10GiB is shown as a best practice. 

Bootstrap device (Container image storage)
:   Hosts must have a second device with a minimum of 100 GiB of unmounted and unformatted device space. For hosts assigned to the control plane, the attached storage device must support at least 1000 IOPs. For example, a 10 IOPs/GiB device with at least 100 GiB capacity. The required IOPS varies with the number of clusters in the location, and the activity of the masters for those clusters. For more information, see [How many clusters can I run before I need to add capacity to the control plane?](/docs/satellite?topic=satellite-location-sizing#control-plane-how-many-clusters).

Additional storage for hosts assigned to services (like clusters)`*`
:   Depending on the services that you want to use, you might need to add more storage before attaching hosts to your location. For service-specific storage requirements, refer to the service documentation. Note that if you plan to deploy additional services or operators to the clusters in your location, you must account for cluster storage plus any additional deployments that you want to use in your clusters. For example, OpenShift Data Foundation.
    - [{{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-managed-services)
    - [{{site.data.keyword.satelliteshort}} storage overview](/docs/satellite?topic=satellite-sat-storage-template-ov)




