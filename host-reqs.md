---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Host requirements
{: #host-reqs}

Review the following host requirements for {{site.data.keyword.satellitelong_notm}}. For provider-specific requirements, see [Provider requirements](/docs/satellite?topic=satellite-providers).
{: shortdesc}

Can't meet these host requirements? [Contact IBM Support](/docs/get-support?topic=get-support-getting-customer-support) and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.
{: note}

## Host system requirements
{: #reqs-host-system}

*   Hosts must run Red Hat Enterprise Linux 7 on x86 architecture. Other operating systems, such as Windows, and mainframe systems, such as IBM Z or Power, are not supported.
*   Hosts must have at least 4 vCPU, 16 GB memory, and 100 GB attached storage device. 
*   The hosts must not have any additional packages, configuration, or other customizations.
*   Hosts must have access to Red Hat updates and the following packages. You might need to refresh your packages on the host machine. For example, on an IBM Cloud infrastructure virtual server instance, you can run `subscription-manager refresh` and then `subscription-manager repos --enable=*`.
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

<br />


## Host storage and attached devices
{: #reqs-host-storage}

* Hosts cannot have a device that is mounted to `/var/data`.
* To set up LUKS encryption, your hosts must have two attached disks: a primary boot disk that is mounted to `/`, and a secondary disk that is unmounted.

<br />


## Host network
{: #reqs-host-network}

*   Do not set any custom networking configurations on your hosts, such as network manager scripts, `dnsmasq` setups, custom IP table rules, or custom MTU settings like jumbo frames.
*   All hosts must have the same MTU values.
*   Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block certain ports that might block communication across hosts.
*   Hosts must have inbound and outbound connectivity on the public network, via the default gateway of the system. You cannot use custom iptables to route traffic to the public network, because default {{site.data.keyword.satelliteshort}} and Calico policies override custom iptables. 
    **Inbound**: The following ports must be open.
    ```
    TCP ports 30000-32767
    Port 80
    Port 443
    ```
    {: screen}

    **Outbound**: Allow connectivity to all ports and IP addresses.
*   If you do not open outbound connectivity, you must [allow all the required ports in your firewall](/docs/containers?topic=containers-firewall#vyatta_firewall), as well as to the following Red Hat network time protocol (NTP) servers.
    ```
    0.rhel.pool.ntp.org
    1.rhel.pool.ntp.org
    2.rhel.pool.ntp.org
    3.rhel.pool.ntp.org
    ```
    {: screen} 
*   The following IP address ranges are reserved, and must not be used in any of the networks that you want to use in {{site.data.keyword.satellitelong_notm}}, including the host networks.
    ```
    172.16.0.0/16, 172.18.0.0/16, 172.19.0.0/16, and 172.20.0.0/16
    ```
    {: screen}
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