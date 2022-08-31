---

copyright:
  years: 2020, 2022
lastupdated: "2022-08-31"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Host network requirements
{: #reqs-host-network}

Review the following requirements that relate to the network setup of host machines.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Networking configurations
{: #reqs-host-network-config}

In general, do not set any custom networking configurations on your hosts, such as network manager scripts, `dnsmasq` setups, custom IP table rules, or custom MTU settings like jumbo frames.
{: shortdesc}

All hosts must meet the following network requirements:
- The `localhost` value must resolve to a valid local host IP address, typically `127.0.0.1`.
- You cannot use custom iptables to route traffic to the public or private network, because default {{site.data.keyword.satelliteshort}} and Calico policies override custom iptables.
- The following IP address ranges are reserved, and must not be used in any of the networks that you want to use in {{site.data.keyword.satellitelong_notm}}, including the host networks.

    Non-CoreOS enabled locations:
    ```sh
    172.16.0.0/16, 172.18.0.0/16, 172.19.0.0/16, 172.20.0.0/16, and 192.168.255.0/24
    ```
    {: screen}
    
    CoreOS enabled locations:
    ```sh
    172.20.0.0/16 and 172.16.0.0/16
    ```
    {: screen}

- Host IP addresses must remain static and cannot change over time, such as due to a reboot or other potential infrastructure updates.
- If you are provisioning your host on-prem, you must configure your host to use a public DNS server, such as `8.8.8.8`. You can use a private DNS server, but it must be able to resolve hostnames on the public Internet.

Hosts assigned to a specific {{site.data.keyword.redhat_openshift_notm}} cluster or to the control plane must share some properties, which can be different across clusters.
- All {{site.data.keyword.satelliteshort}} hosts must have the same MTU values.
- Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block certain ports that might block communication across hosts.

## Host network bandwidth
{: #reqs-host-network-bandwidth}

- The hosts must have minimum network bandwidth connectivity of 100 Mbps, with 1 Gbps preferred.
- The bandwidth required between hosts varies with the number of clusters in the location, and the workloads that run in the cluster. Insufficient network bandwidth can lead to network performance problems.

## Network gateways and interfaces
{: #reqs-host-network-interface}

- Host kernels must have IPv6 support enabled. Do not disable IPv6.
- All {{site.data.keyword.satelliteshort}} hosts must have an IPv4 address that can access `containers.cloud.ibm.com` and must have full IPv4 backend connectivity to the other hosts in the same cluster in the location.
- Hosts can use gateways to connect to the location control plane.

## Inbound connectivity for requirements {{site.data.keyword.satelliteshort}} hosts
{: #reqs-host-network-firewall-inbound}

Hosts must have inbound connectivity on the primary network interface via the default gateway or firewall of the system.
{: shortdesc}

For example, if the primary network interface for a host is `eth0`, you must open the following required IP addresses and ports on the default gateway or firewall on the `eth0` private network interface.

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow hosts that are assigned to services in your location to communicate with each other and with the {{site.data.keyword.satelliteshort}} control plane | All {{site.data.keyword.satelliteshort}} hosts | All {{site.data.keyword.satelliteshort}} hosts | All ports and protocols |
| Access the API to make changes in a {{site.data.keyword.redhat_openshift_notm}} cluster and access the {{site.data.keyword.redhat_openshift_notm}} web console or through the {{site.data.keyword.redhat_openshift_notm}} router | Clients or authorized users | Control plane hosts | TCP 30000 - 32767 |
| Access the web console for a {{site.data.keyword.redhat_openshift_notm}} cluster through the {{site.data.keyword.redhat_openshift_notm}} router | Clients or authorized users | {{site.data.keyword.redhat_openshift_notm}} cluster hosts | TCP 443 |
{: caption="Required inbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required inbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}
