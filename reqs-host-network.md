---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-21"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Host network requirements
{: #reqs-host-network}

Review the following requirements that relate to the network setup of host machines.
{: shortdesc}

## Networking configurations
{: #reqs-host-network-config}

In general, do not set any custom networking configurations on your hosts, such as network manager scripts, `dnsmasq` setups, custom IP table rules, or custom MTU settings like jumbo frames.
{: shortdesc}

- All {{site.data.keyword.satelliteshort}} hosts must have the same MTU values.
- The `localhost` value must resolve to a valid local host IP address, typically `127.0.0.1`.
- Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block certain ports that might block communication across hosts.
- You cannot use custom iptables to route traffic to the public or private network, because default {{site.data.keyword.satelliteshort}} and Calico policies override custom iptables.
- The following IP address ranges are reserved, and must not be used in any of the networks that you want to use in {{site.data.keyword.satellitelong_notm}}, including the host networks.
    ```sh
    172.16.0.0/16, 172.18.0.0/16, 172.19.0.0/16, 172.20.0.0/16, and 192.168.255.0/24
    ```
    {: screen}

- Host IP addresses must remain static and cannot change over time, such as due to a reboot or other potential infrastructure updates.
- If you are provisioning your host on-prem, you must configure your host to use a public DNS server, such as `8.8.8.8`.

## Host network bandwidth
{: #reqs-host-network-bandwidth}

- The hosts must have minimum network bandwidth connectivity of 100 Mbps, with 1 Gbps preferred.
- The bandwidth required between hosts varies with the number of clusters in the location, and the workloads that run in the cluster. Insufficient network bandwidth can lead to network performance problems.

## Network gateways and interfaces
{: #reqs-host-network-interface}

- All {{site.data.keyword.satelliteshort}} hosts must use the same default gateway.
- Hosts can have multiple IPv4 network interfaces. However, the `eth0`, `ens0`, or `bond0` network interface must serve as the default route. To find the default network interface for a host, SSH into the host and run the following command:
    ```sh
    ip route | grep default | awk '{print $5}'
    ```
    {: pre}

    In this example output, `eth0` is the default network interface:
    ```sh
    default via 161.202.250.1 dev eth0 onlink
    ```
    {: screen}

- All {{site.data.keyword.satelliteshort}} hosts must have an IPv4 address that can access `containers.cloud.ibm.com` and must have full IPV4 backend connectivity to the other hosts in the location on the network interface that serves as the default route (`eth0`, `ens0`, or `bond0`).


## Inbound connectivity for requirements {{site.data.keyword.satelliteshort}} hosts
{: #reqs-host-network-firewall-inbound}

Hosts must have inbound connectivity on the primary network interface via the default gateway or firewall of the system.
{: shortdesc}

For example, if the primary network interface for a host is `eth0`, you must open the following required IP addresses and ports on the default gateway or firewall on the `eth0` private network interface.

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow hosts that are assigned to services in your location to communicate with each other and with the {{site.data.keyword.satelliteshort}} control plane | All {{site.data.keyword.satelliteshort}} hosts | All {{site.data.keyword.satelliteshort}} hosts | All ports and protocols |
| Access the API to make changes in an {{site.data.keyword.redhat_openshift_notm}} cluster and access the {{site.data.keyword.redhat_openshift_notm}} web console or through the {{site.data.keyword.redhat_openshift_notm}} router | Clients or authorized users | Control plane hosts | TCP 30000 - 32767 |
| Access the web console for an {{site.data.keyword.redhat_openshift_notm}} cluster through the {{site.data.keyword.redhat_openshift_notm}} router | Clients or authorized users | {{site.data.keyword.redhat_openshift_notm}} cluster hosts | TCP 443 |
{: caption="Required inbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required inbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

## Outbound connectivity for hosts in all locations
{: #reqs-host-network-firewall-outbound}

Hosts must have outbound connectivity to all ports and IP addresses on the primary network interface through the default gateway.
{: shortdec}

For example, if the primary network interface is public, you can try these commands to test that your host has outbound network connectivity on the public network.
```sh
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

In addition to the [outbound connectivity](#reqs-host-network-firewall-outbound) for {{site.data.keyword.satelliteshort}}, be sure that you allow the required outbound connectivity for the {{site.data.keyword.satelliteshort}} services that you want to use. Review the service documentation for outbound connectivity requirements. For more information, see [Supported managed services for Satellite](/docs/satellite?topic=satellite-managed-services). 

To secure your outbound connectivity, allow only TCP on the Kubernetes API server NodePorts and UDP on the VPN NodePorts for your location. You can find your active NodePorts by running the **`ibmcloud sat location get --location <location>`** command.
{: tip}


### Required outbound connectivity for hosts in all regions
{: #all-regions-outbound}

In addition to the following outbound connectivity requirements, hosts must also meet the regional outbound connectivity requirements for the region where they are located. For more information, see [regional outbound connectivity requirements for RHEL-based locations](/docs/satellite?topic=satellite-reqs-host-network-outbound).
{: important}




Allow hosts to connect to {{site.data.keyword.IBM_notm}}.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination**: `cloud.ibm.com`, `containers.cloud.ibm.com`
:   **Protocol and ports**: Port 443

Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination**: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
:   **Protocol and ports**: NTP protocol and UDP port 123

Allow hosts to communicate with {{site.data.keyword.iamshort}}. Your firewall must be Layer 7 to allow the IAM domain name. IAM does not have specific IP addresses that you can allow. If your firewall does not support Layer 7, you can allow all HTTPS network traffic on port 443.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination** `https://iam.bluemix.net`, `https://iam.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to connect to the LaunchDarkly service.
:   **Source**: Control plane hosts
:   **Destination**: `app.launchdarkly.com`,`clientstream.launchdarkly.com`
:   **Protocol and ports**: HTTPS 443

Allow hosts to communicate with Red Hat Container Registry.
:   See [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs and hostnames**: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
:   **Protocol and ports** TCP 443 and 6443

Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs and hostnames**: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
:   **Protocol and ports**: TCP 443 and 80


