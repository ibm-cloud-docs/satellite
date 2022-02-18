---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does host assign fail with a `crio` error? 
{: #host-assign-file-system}

When you try to assign a host to a cluster, you see a message similar to the following error.
{: tsSymptoms}

``` sh
Dec 01 11:45:31 satellite.host crio[12190]: time="2021-12-01 11:45:31.818616215-06:00" level=info msg="Node configuration value for pid cgroup is true"
Dec 01 11:45:31 satellite.host crio[12190]: time="2021-12-01 11:45:31.818820217-06:00" level=info msg="Node configuration value for memoryswap cgroup is true"
Dec 01 11:45:31 satellite.host crio[12190]: time="2021-12-01 11:45:31.830378308-06:00" level=info msg="Node configuration value for systemd CollectMode is true"
Dec 01 11:45:31 satellite.host crio[12190]: time="2021-12-01 11:45:31.834555839-06:00" level=error msg="Node configuration validation for systemd AllowedCPUs failed: check systemd AllowedCPUs: exit status 1"
Dec 01 11:45:31 satellite.host crio[12190]: time="2021-12-01 11:45:31.834638109-06:00" level=info msg="Node configuration value for systemd AllowedCPUs is false"
Dec 01 11:45:31 satellite.host crio[12190]: time="2021-12-01 11:45:31.837060028-06:00" level=fatal msg="Validating root config: failed to get store to set defaults: kernel does not support overlay fs: 'overlay' is not supported over xfs at \"/var/data/criorootstorage/overlay\": backing file system is unsupported for this graph driver
```
{: screen}

The host file system is a type other than ext4.
{: tsCauses}

Convert the host file system to ext4 or provision a new host with an ext4 file system. 
{: tsResolve}

