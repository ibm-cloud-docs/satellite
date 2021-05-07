---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-07"

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
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Host requirements
{: #host-reqs}

Review the following host requirements for {{site.data.keyword.satellitelong}}.
{: shortdesc}

Want to add hosts from other cloud providers to your location? See [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
{: tip}

Can't meet these host requirements? [Contact IBM Support](/docs/get-support?topic=get-support-using-avatar) and include the following information: the host system configuration that you want, why you want the system configuration, and how many hosts you intend to create.
{: note}

## Host system requirements
{: #reqs-host-system}

Review the following requirements that relate to the computing and system setup of host machines.
{: shortdesc}

### Computing characteristics
{: #reqs-host-compute}

*   Hosts must run Red Hat Enterprise Linux 7 on x86 architecture. Other operating systems, such as Windows, and mainframe systems, such as IBM Z or Power, are not supported.
*   Hosts can be physical or virtual machines.
*   Hosts must have at least 4 vCPU, 16 GB memory, and [sufficient storage capacity](#reqs-host-storage). 
*   If your host has GPU compute, make sure that you install the node feature discovery and NVIDIA GPU operators. For more information, see the prerequisite in [Deploying an app on a GPU machine](/docs/openshift?topic=openshift-deploy_app#gpu_app).

### Packages and other machine configurations
{: #reqs-host-packages}

Hosts must have access to {{site.data.keyword.redhat_notm}} updates and the following packages. The hosts must not have any additional packages, configuration, or other customizations.
{: shortdesc}

```
Repository 'rhel-server-rhscl-7-rpms' is enabled for this system.
Repository 'rhel-7-server-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-rh-common-rpms' is enabled for this system.
Repository 'rhel-7-server-supplementary-rpms' is enabled for this system.
Repository 'rhel-7-server-extras-rpms' is enabled for this system.
```
{: screen}

You might need to refresh your packages on the host machine. For example, in IBM Cloud infrastructure you can run the following commands to add the required packages.
  1.  Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
      ```
      subscription-manager refresh
      ```
      {: pre}
  2.  Enable the package repositories on your machine.
      ```
      subscription-manager repos --enable rhel-server-rhscl-7-rpms
      subscription-manager repos --enable rhel-7-server-optional-rpms
      subscription-manager repos --enable rhel-7-server-rh-common-rpms
      subscription-manager repos --enable rhel-7-server-supplementary-rpms
      subscription-manager repos --enable rhel-7-server-extras-rpms
      ```
      {: pre}

For more information about how to enable the {{site.data.keyword.redhat_notm}} packages in hosts that you add from other cloud providers, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
{: tip}

<br />

## Host storage and attached devices
{: #reqs-host-storage}

Review the following requirements that relate to the storage setup of host machines.
{: shortdesc}
* Hosts must have a boot disk with sufficient space to boot the host and run the operating system.
* Hosts must have an additional disk that is attached to the host and that provides a minimum of 100 GB of unmounted and unformatted disk space.
* For hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane, the attached storage device must have at least 1000 IOPS. The required IOPS varies with the number of clusters in the location, and the activity of the masters for those clusters.
* Hosts cannot have a device that is mounted to `/var/data`.


<br />

## Host network
{: #reqs-host-network}

Review the following requirements that relate to the networking setup of host machines.
{: shortdesc}

### Networking configurations
{: #reqs-host-network-config}

In general, do not set any custom networking configurations on your hosts, such as network manager scripts, `dnsmasq` setups, custom IP table rules, or custom MTU settings like jumbo frames.
{: shortdesc}

* All hosts must have the same MTU values.
* The `localhost` value must resolve to a valid local host IP address, typically `127.0.0.1`.
* Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block certain ports that might block communication across hosts.
* You cannot use custom iptables to route traffic to the public or private network, because default {{site.data.keyword.satelliteshort}} and Calico policies override custom iptables.
* The following IP address ranges are reserved, and must not be used in any of the networks that you want to use in {{site.data.keyword.satellitelong_notm}}, including the host networks.
    ```
    172.16.0.0/16, 172.18.0.0/16, 172.19.0.0/16, 172.20.0.0/16, and 192.168.255.0/24
    ```
    {: screen}
* Host IP addresses must remain static and cannot change over time, such as due to a reboot or other potential infrastructure updates.

### Host network bandwidth
{: #reqs-host-network-bandwidth}

* The hosts must have minimum network bandwidth connectivity of 100 Mbps, with 1 Gbps preferred.
* The bandwidth required between hosts varies with the number of clusters in the location, and the workloads that run in the cluster. Insufficient network bandwidth can lead to network performance problems.

### Network gateways and interfaces
{: #reqs-host-network-interface}

* All hosts must use the same default gateway.
* Hosts can have multiple IPv4 network interfaces. However, the `eth0`, `ens0`, or `bond0` network interface must serve as the default route. To find the default network interface for a host, SSH into the host and run the following command:
  ```
  ip route | grep default | awk '{print $5}'
  ```
  {: pre}
  In this example output, `eth0` is the default network interface:
  ```
  default via 161.202.250.1 dev eth0 onlink
  ```
  {: screen}
* All hosts must have an IPv4 address that can access `containers.cloud.ibm.com` and must have full IPV4 backend connectivity to the other hosts in the location on the network interface that serves as the default route (`eth0`, `ens0`, or `bond0`).

### Inbound connectivity
{: #reqs-host-network-firewall-inbound}

Hosts must have inbound connectivity on the primary network interface via the default gateway or firewall of the system.
{: shortdesc}

For example, if the primary network interface for a host is `eth0`, you must open the following required IP addresses and ports on the default gateway or firewall on the `eth0` private network interface.

|Description|Source IP|Destination IP|Protocol and ports|
|-----------|---------|--------------|------------------|
| Allow hosts that are assigned to services in your location to communicate with each other and with the {{site.data.keyword.satelliteshort}} control plane | All hosts | All hosts | All ports and protocols |
| Access the API to make changes in an {{site.data.keyword.openshiftshort}} cluster and access the {{site.data.keyword.openshiftshort}} web console or through the {{site.data.keyword.openshiftshort}} router | Client or authorized user | Control plane hosts | TCP 30000 - 32767 |
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

If you do not open all outbound connectivity, you must allow the following outbound connectivity in your firewall. The required IP addresses vary with the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.

|Description|Source IP|Destination IP|Protocol and ports|
|-----------|---------|--------------|------------------|
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 169.63.123.154</br>169.60.123.162</br>52.117.93.26 | TCP 443, 30000 - 32767</br>UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | [All IP addresses listed in the **US East** row of the table in step 2 of the {{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and client or authorized user | All IP addresses listed for US East (`wdc`) in steps 3 - 5 of the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound) | See documentation |
| Allow Cloudflare proxied load balancers for {{site.data.keyword.satelliteshort}} Config | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 443 |
| Allow Cloudflare proxied load balancers for the {{site.data.keyword.satelliteshort}} Link API | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | 0.rhel.pool.ntp.org</br>1.rhel.pool.ntp.org</br>2.rhel.pool.ntp.org</br>3.rhel.pool.ntp.org | NTP protocol and UDP port 123 |
{: #firewall-outbound-wdc}
{: tab-title="Washington DC (wdc)"}
{: class="comparison-tab-table"}
{: tab-group="firewall-outbound"}
{: caption="Required outbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

|Description|Source IP|Destination IP|Protocol and ports|
|-----------|---------|--------------|------------------|
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 158.175.120.210</br>141.125.97.106</br>158.176.139.66 | TCP 443, 30000 - 32767</br>UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.eu.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | [All IP addresses listed in the **UK South** row of the table in step 2 of the {{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and client or authorized user | All IP addresses listed for UK South (`lon`) in steps 3 - 5 of the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#firewall_outbound) | See documentation |
| Allow Cloudflare proxied load balancers for {{site.data.keyword.satelliteshort}} Config | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 443 |
| Allow Cloudflare proxied load balancers for the {{site.data.keyword.satelliteshort}} Link API | Control plane hosts | [Cloudflare's IPv4 IPs](https://www.cloudflare.com/ips/){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | 0.rhel.pool.ntp.org</br>1.rhel.pool.ntp.org</br>2.rhel.pool.ntp.org</br>3.rhel.pool.ntp.org | - |
{: #firewall-outbound-lon}
{: tab-title="London (lon)"}
{: class="comparison-tab-table"}
{: tab-group="firewall-outbound"}
{: caption="Required outbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

## Host latency
{: #host-latency-test}

Review the following latency requirements for hosts to communicate with each other in the same {{site.data.keyword.satelliteshort}} location, and with the {{site.data.keyword.cloud_notm}} region that manages the location.
{: shortdesc}

### Latency across hosts in the same location
{: #host-latency-location}

Your host infrastructure setup must have a low latency connection of less than 10 milliseconds (`< 10ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane and the hosts that are used for other resources in the location, like clusters or services. For example, in cloud providers such as AWS, this setup typically means that the all of the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`.

### Latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts
{: #host-latency-mzr}

Each {{site.data.keyword.satelliteshort}} location is [managed from an {{site.data.keyword.cloud_notm}} multizone region](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). You can test the latency between your hosts and the region to make sure you use a low latency connection of less than 100 milliseconds (`< 100ms`) round-trip time (RTT).
{: shortdesc}

1.  In your infrastructure provider, log in to a host machine that you want to add to a {{site.data.keyword.satelliteshort}} location. For example, you might SSH into the machine from a command line.
2.  Note the IP addresses for the {{site.data.keyword.cloud_notm}} region that you want to test.
    *   Washington, DC:
        169.63.123.154</br>169.60.123.162</br>52.117.93.26
    *   London:
        158.175.120.210</br>141.125.97.106</br>158.176.139.66
3.  From your host, ping the IP addresses of the {{site.data.keyword.cloud_notm}} region.
    ```
    ping <ip_address>
    ```
    {: pre}
4.  After a few packets complete transmission, close the connection. For example, from the command line, you might enter `ctrl+c`.
5.  In the `ping statistics` output, note the average (`avg`) round-trip distance in milliseconds (ms) between the host and the {{site.data.keyword.cloud_notm}} region, and compare whether the connection meets the latency requirement of less than 100 ms (`<100 ms`).

    Example of a connection that meets the latency requirements:
    ```
    --- 169.63.123.154 ping statistics ---
    25 packets transmitted, 25 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 48.131/77.716/181.397/27.893 ms
    ```
    {: screen}

    Example of a connection that does not meet the latency requirements:
    ```
    --- 158.175.120.210 ping statistics ---
    9 packets transmitted, 9 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 138.453/187.370/389.901/78.211 ms
    ```
    {: screen}
