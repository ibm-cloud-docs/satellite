---

copyright:
  years: 2020, 2022
lastupdated: "2022-04-14"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Host storage and attached devices
{: #reqs-host-storage}

Review the following requirements that relate to the storage setup of host machines.
{: shortdesc}

- Hosts must have a boot disk with an `ext4` file system sufficient space to boot the host and run the operating system.
- Hosts must have an additional disk that is attached to the host and that provides a minimum of 100 GB of unmounted and unformatted disk space. 
    - If `/tmp` is not part of the 100 GB disk space, then it needs to be at least 1.5 GB.
    - If `/usr` is not part of the 100 GB disk space, then it needs to be at least 1.5 GB.
- For hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane, the attached storage device must support at least 1000 IOPs. For example, a 10 IOPs/GB disk with at least 100 GB capacity. The required IOPS varies with the number of clusters in the location, and the activity of the masters for those clusters.
- Hosts cannot have a device that is mounted to `/var/data`.

