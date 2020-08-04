---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-04"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# Usage requirements
{: #limitations}

{{site.data.keyword.satellitelong}} comes with default service settings and limitations to ensure security, convenience, and basic functionality. Limitations are subject to change, and might differ from beta to generally available releases.
{: shortdesc}

{{site.data.keyword.satellitelong}} is available as a tech preview and subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: preview}

## Locations
{: #limits-locations}

You can create up to 20 {{site.data.keyword.satelliteshort}} locations per {{site.data.keyword.cloud_notm}} region that the location is managed from.
{: shortdesc}

## Hosts
{: #limits-host}

Review the following host limitations for {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

Can't meet these host requirements? Contact IBM Support and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.
{: note}

### Host system requirements
{: #limits-host-system}

*   Hosts must run Red Hat Enterprise Linux 7 on x86 architecture. Other operating systems, such as Windows, and mainframe systems, such as IBM Z or Power, are not supported.
*   Hosts must have at least 4 CPU, 16 GB memory, and 100 GB attached storage device. 
*   The hosts must not have any additional packages, configuration, or other customizations.
*   Hosts must have the following packages. You might need to refresh your packages on the host machine. For example, on a IBM Cloud infrastructure virtual server instance, you can run `subscription-manager refresh` and then `subscription-manager repos --enable=*`.
    ```
Repository 'rhel-ha-for-rhel-7-server-eus-rpms' is enabled for this system.
Repository 'rhel-server-rhscl-7-rpms' is enabled for this system.
Repository 'rhel-7-server-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-eus-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-rh-common-rpms' is enabled for this system.
Repository 'rhel-7-server-eus-rpms' is enabled for this system.
Repository 'rhel-ha-for-rhel-7-server-rpms' is enabled for this system.
Repository 'rhel-rs-for-rhel-7-server-eus-rpms' is enabled for this system.
Repository 'rhel-rs-for-rhel-7-server-rpms' is enabled for this system.
Repository 'rhel-7-server-rpms' is enabled for this system.
Repository 'rhel-7-server-supplementary-rpms' is enabled for this system.
Repository 'rhel-7-server-extras-rpms' is enabled for this system.
Repository 'rhel-7-server-eus-supplementary-rpms' is enabled for this system.
```
{: screen}
*   After the host is successfully assigned to a {{site.data.keyword.satelliteshort}} location control plane or cluster, {{site.data.keyword.satelliteshort}} disables the ability to SSH into the host for security purposes. If you remove a host from your location or remove the entire location, you must reload the machine in your host infrastructure provider to SSH into the host again. Otherwise, you might see an error similar to the following when you try to log in.
    ```
    Permission denied, please try again.
    ```
    {: screen}


### Host storage and attached devices
{: #limits-host-storage}

* Hosts cannot have a device that is mounted to `/var/data`.
* To set up LUKS encryption, your hosts must have two attached disks: a primary boot disk that is mounted to `/`, and a secondary disk that is unmounted.

### Host network
{: #limits-host-network}

*   Do not set any custom networking configurations on your hosts, such as network manager scripts, DNSMASQ setups, custom IP table rules, or custom MTU settings like jumbo frames.
*   All hosts must have the same MTU values.
*   Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block certain ports that might block communication across hosts.
*   Hosts must have outbound connectivity on the public network, via the default gateway of the system. The ports that must be open are TCP ports 30000-32767. You cannot use custom iptables to route traffic to the public network, because default {{site.data.keyword.satelliteshort}} and Calico policies override custom iptables.
*   Hosts must have a single, primary IPv4 network interface, such as the following example. However, you can use hosts from IBM Cloud infrastructure classic virtual service instances, even though these machines have two network interfaces: one each for the public and private networks.
    ```
    root@kube-tok02-credfdd3e356854f39b15fb191224b0a16-w1:~# ip addr
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet 172.20.0.1/32 brd 172.20.0.1 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 06:65:92:a1:c9:48 brd ff:ff:ff:ff:ff:ff
        inet 161.202.250.13/27 brd 161.202.250.31 scope global eth0
        valid_lft forever preferred_lft forever
        inet6 fe80::465:92ff:fea1:c948/64 scope link
        valid_lft forever preferred_lft forever
    root@kube-tok02-credfdd3e356854f39b15fb191224b0a16-w1:~# ip route
    default via 161.202.250.1 dev eth0 onlink
    ```
    {: screen}

To test that your host has outbound network connectivity on the public network, you can try these commands.

```
ping 8.8.8.8
nslookup google.com
nslookup google.com 8.8.8.8
curl  https://google.com
curl https://containers.cloud.ibm.com/v1/healthz
curl https://us.icr.io
curl -k [node 1-n]:22
```
{: codeblock}

<br />


## Storage
{: #limits-storage}

Review the following storage limitations for {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

* No support for persistent volumes. You must bring your own storage driver.
* No location-wide storage solution.

<br />

