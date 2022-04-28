---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-28"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Host storage and attached devices
{: #reqs-host-storage}

Review the following storage requirements for hosts assigned to the location control plane and hosts assigned to services. Note that depending on the services you want to use, the storage requirements for hosts assigned to those services vary. For specific service requirements, refer to the service documentation.
{: shortdesc}

Hosts can't have a device mounted to `/var/data`.
{: note}







- Hosts must have a boot device with an `ext4` file system sufficient space to boot the host and run the operating system.
- Hosts must have an additional device that is attached to the host and that provides a minimum of 100 GB of unmounted and unformatted device space. 
    - If `/tmp` is not part of the 100 GB device space, then it needs to be at least 1.5 GB.
    - If `/usr` is not part of the 100 GB device space, then it needs to be at least 1.5 GB.
- For hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane, the attached storage device must support at least 1000 IOPs. For example, a 10 IOPs/GB device with at least 100 GB capacity. The required IOPS varies with the number of clusters in the location, and the activity of the masters for those clusters.
- Hosts cannot have a device that is mounted to `/var/data`.




