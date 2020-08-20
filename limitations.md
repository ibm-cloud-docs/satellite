---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-20"

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

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}



## Locations
{: #limits-locations}

You can create up to 20 {{site.data.keyword.satelliteshort}} locations per {{site.data.keyword.cloud_notm}} multizone metro that the location is managed from.
{: shortdesc}

## Hosts
{: #limits-host}

Review the following host requirements for {{site.data.keyword.satellitelong_notm}}. For provider-specific requirements, see [Provider requirements](/docs/satellite?topic=satellite-providers).
{: shortdesc}

Can't meet these host requirements? Contact IBM Support and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.
{: note}

### Host system requirements
{: #limits-host-system}

*   Hosts must run Red Hat Enterprise Linux 7 on x86 architecture. Other operating systems, such as Windows, and mainframe systems, such as IBM Z or Power, are not supported.
*   Hosts must have at least 4 CPU, 16 GB memory, and 100 GB attached storage device. 
*   The hosts must not have any additional packages, configuration, or other customizations.
*   Hosts must have the following packages. You might need to refresh your packages on the host machine. For example, on an IBM Cloud infrastructure virtual server instance, you can run `subscription-manager refresh` and then `subscription-manager repos --enable=*`.
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

*   Do not set any custom networking configurations on your hosts, such as network manager scripts, `dnsmasq` setups, custom IP table rules, or custom MTU settings like jumbo frames.
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


## Link and endpoints
{: #limits-link}

The {{site.data.keyword.satelliteshort}} Link connector instances that run in your [{{site.data.keyword.satelliteshort}} location control plane worker nodes](/docs/satellite?topic=satellite-service-architecture) are limited to 3 instances, one per host. Even if you add hosts to the location control plane, network traffic that is routed through the {{site.data.keyword.satelliteshort}} Link connector is sent only over 3 hosts.

## Cluster and infrastructure network
{: #limits-cluster-network}

* No support for Kubernetes load balancers.
* No support for private service endpoints for {{site.data.keyword.satelliteshort}} clusters.


<br />


## Application configuration
{: #limits-config}

Review the following application configuration limitations for {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

**{{site.data.keyword.satelliteshort}} Config access to modify Kubernetes resources within a cluster**<br>
By default, {{site.data.keyword.satelliteshort}} Config is limited to what Kubernetes resources it can read and modify in your clusters. You must grant {{site.data.keyword.satelliteshort}} Config access in each cluster where you want to use {{site.data.keyword.satelliteshort}} Config to manage your Kubernetes resources.

Choose from the following options:
*   **Cluster admin access**: Create a cluster role binding to grant {{site.data.keyword.satelliteshort}} Config access to the appropriate service accounts.
    ```
    kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
    ```
    {: pre}
*   **Custom access, cluster-wide or scoped to a project**: You can create custom RBAC policies to grant {{site.data.keyword.satelliteshort}} Config access only to the projects (namespaces), actions, and resources that you want {{site.data.keyword.satelliteshort}} Config to manage. For more information and examples, see [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access).

**Configuration files in {{site.data.keyword.satelliteshort}} Config**
* You can upload only an individual configuration file of Kubernetes resources per release version. You cannot upload a directory or several different configuration files.
* Configuration files are subject to Kubernetes limitations, such as that the manifest must be expressed in YAML or JSON format.
