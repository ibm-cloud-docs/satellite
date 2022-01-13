---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-12"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Host storage and attached devices
{: #reqs-host-storage}

Review the following requirements that relate to the storage setup of host machines.
{: shortdesc}

- Hosts must have a boot disk with sufficient space to boot the host and run the operating system.
- Hosts must have an additional disk that is attached to the host and that provides a minimum of 100 GB of unmounted and unformatted disk space.
- For hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane, the attached storage device must have at least 1000 IOPS. The required IOPS varies with the number of clusters in the location, and the activity of the masters for those clusters.
- Hosts cannot have a device that is mounted to `/var/data`.
- Hosts must have a boot disk with an ext4 file system sufficient space to boot the host and run the operating system.
