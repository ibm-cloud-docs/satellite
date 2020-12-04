---

copyright:
  years: 2020, 2020
lastupdated: "2020-12-04"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
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
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Host requirements
{: #host-reqs}

Review the following host requirements for {{site.data.keyword.satellitelong}}. For provider-specific requirements, see [Provider requirements](/docs/satellite?topic=satellite-providers).
{: shortdesc}

Can't meet these host requirements? [Contact IBM Support](/docs/get-support?topic=get-support-using-avatar) and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.
{: note}

## Host system requirements
{: #reqs-host-system}

*   Hosts must run Red Hat Enterprise Linux 7 on x86 architecture. Other operating systems, such as Windows, and mainframe systems, such as IBM Z or Power, are not supported.
*   Hosts can be physical or virtual machines.
*   Hosts must have at least 4 vCPU, 16 GB memory, and 100 GB attached storage device. 
*   The hosts must not have any additional packages, configuration, or other customizations.
*   If your host has GPU compute, make sure that you install the node feature discovery and NVIDIA GPU operators. For more information, see the prerequisite in [Deploying an app on a GPU machine](/docs/openshift?topic=openshift-deploy_app#gpu_app).
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

* Do not set any custom networking configurations on your hosts, such as network manager scripts, `dnsmasq` setups, custom IP table rules, or custom MTU settings like jumbo frames.
* All hosts must have the same MTU values.
* The `localhost` value must resolve to a valid local host IP address, typically `127.0.0.1`.
* Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block certain ports that might block communication across hosts.
* You cannot use custom iptables to route traffic to the public or private network, because default {{site.data.keyword.satelliteshort}} and Calico policies override custom iptables.
* The following IP address ranges are reserved, and must not be used in any of the networks that you want to use in {{site.data.keyword.satellitelong_notm}}, including the host networks.
    ```
    172.16.0.0/16, 172.18.0.0/16, 172.19.0.0/16, and 172.20.0.0/16
    ```
    {: screen}
* Hosts can have multiple IPv4 network interfaces. However, the lowest-order, non-loopback network interface with a valid IPv4 address is used as the primary network interface for the hosts, such as `eth0` in the following example. **Note**: If your hosts have private network connectivity only, authorized users must be in the private host network directly or through a VPN connection to access services that run in your location, such as {{site.data.keyword.openshiftlong_notm}} clusters.
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

### Inbound connectivity
{: #reqs-host-network-firewall-inbound}

Hosts must have inbound connectivity on the primary network interface via the default gateway or firewall of the system.
{: shortdesc}

For example, if the lowest-order, non-loopback network interface with a valid IPv4 address is an `eth0` private network interface, you must open the following required IP addresses and ports on the default gateway or firewall on the `eth0` private network interface.

|Description|Source IP|Destination IP|Protocol and ports|
|-----------|---------|--------------|------------------|
| Allow hosts that are assigned to services in your location to communicate with the {{site.data.keyword.satelliteshort}} control plane | All non-control plane hosts | Control plane hosts | TCP, UDP 30000 - 32767 |
| Access the API to make changes in an {{site.data.keyword.openshiftshort}} cluster and access the {{site.data.keyword.openshiftshort}} web console for through the {{site.data.keyword.openshiftshort}} router | Client or authorized user | Control plane hosts | TCP 30000 - 32767 |
| Access the web console for an {{site.data.keyword.openshiftshort}} cluster through the {{site.data.keyword.openshiftshort}} router | Client or authorized user | {{site.data.keyword.openshiftshort}} cluster hosts | TCP 443 |
{: caption="Required inbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required inbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

### Outbound connectivity
{: #reqs-host-network-firewall-outbound}

Hosts must have outbound connectivity to all ports and IP addresses on the primary network interface via the default gateway of the system.
{: shortdec}

For example, if the primary network interface is public, you can try these commands to test that your host has outbound network connectivity on the public network.
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

If you do not open all outbound connectivity, you must allow the following outbound connectivity in your firewall. The required IP addresses vary with the {{site.data.keyword.cloud_notm}} multizone region that your {{site.data.keyword.satelliteshort}} location is managed from.

|Description|Source IP|Destination IP|Protocol and ports|
|-----------|---------|--------------|------------------|
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 169.63.123.154</br>169.60.123.162</br>52.117.93.26 | TCP 443, 30000 - 32767</br>UDP 30000 - 32767 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | [All IP addresses listed in the **US East** row of the table in step 2 of the {{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and client or authorized user | All IP addresses listed for US East (`wdc`) in steps 3 - 5 of the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound) | See documentation |
| Allow Cloudflare proxied load balancers for {{site.data.keyword.satelliteshort}} Config | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 443 |
| Allow Cloudflare proxied load balancers for the {{site.data.keyword.satelliteshort}} Link API | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 80, 443 |
| Allow access to Red Hat network time protocol (NTP) servers | All hosts | 0.rhel.pool.ntp.org</br>1.rhel.pool.ntp.org</br>2.rhel.pool.ntp.org</br>3.rhel.pool.ntp.org | - |
{: #firewall-outbound-wdc}
{: tab-title="Washington DC (wdc)"}
{: class="comparison-tab-table"}
{: tab-group="firewall-outbound"}
{: caption="Required outbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

|Description|Source IP|Destination IP|Protocol and ports|
|-----------|---------|--------------|------------------|
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 158.175.120.210</br>141.125.97.106</br>158.176.139.66 | TCP 443, 30000 - 32767</br>UDP 30000 - 32767 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | [All IP addresses listed in the **UK South** row of the table in step 2 of the {{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and client or authorized user | All IP addresses listed for UK South (`lon`) in steps 3 - 5 of the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound) | See documentation |
| Allow Cloudflare proxied load balancers for {{site.data.keyword.satelliteshort}} Config | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 443 |
| Allow Cloudflare proxied load balancers for the {{site.data.keyword.satelliteshort}} Link API | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 80, 443 |
| Allow access to Red Hat network time protocol (NTP) servers | All hosts | 0.rhel.pool.ntp.org</br>1.rhel.pool.ntp.org</br>2.rhel.pool.ntp.org</br>3.rhel.pool.ntp.org | - |
{: #firewall-outbound-lon}
{: tab-title="London (lon)"}
{: class="comparison-tab-table"}
{: tab-group="firewall-outbound"}
{: caption="Required outbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}
