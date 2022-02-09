---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-09"

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

- All hosts must have the same MTU values.
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

- All hosts must use the same default gateway.
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

- All hosts must have an IPv4 address that can access `containers.cloud.ibm.com` and must have full IPV4 backend connectivity to the other hosts in the location on the network interface that serves as the default route (`eth0`, `ens0`, or `bond0`).

## Inbound connectivity
{: #reqs-host-network-firewall-inbound}

Hosts must have inbound connectivity on the primary network interface via the default gateway or firewall of the system.
{: shortdesc}

For example, if the primary network interface for a host is `eth0`, you must open the following required IP addresses and ports on the default gateway or firewall on the `eth0` private network interface.

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow hosts that are assigned to services in your location to communicate with each other and with the {{site.data.keyword.satelliteshort}} control plane | All hosts | All hosts | All ports and protocols |
| Access the API to make changes in an {{site.data.keyword.openshiftshort}} cluster and access the {{site.data.keyword.openshiftshort}} web console or through the {{site.data.keyword.openshiftshort}} router | Clients or authorized users | Control plane hosts | TCP 30000 - 32767 |
| Access the web console for an {{site.data.keyword.openshiftshort}} cluster through the {{site.data.keyword.openshiftshort}} router | Clients or authorized users | {{site.data.keyword.openshiftshort}} cluster hosts | TCP 443 |
{: caption="Required inbound connectivity for hosts on the primary network interface" caption-side="top"}
{: summary="The table shows the required inbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

## Outbound connectivity
{: #reqs-host-network-firewall-outbound}

Hosts must have outbound connectivity to all ports and IP addresses on the primary network interface through the default gateway of the system.
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

In addition to the [outbound connectivity](#reqs-host-network-firewall-outbound) for {{site.data.keyword.satelliteshort}}, be sure that you allow the required outbound connectivity for the {{site.data.keyword.satelliteshort}} services that you want to use. Review the service documentation for outbound connectivity requirements. For example, if you are creating OpenShift clusters, you must allow your hosts to access the LaunchDarkly service. For more information, see [Supported managed services for Satellite](/docs/satellite?topic=satellite-managed-services). 

To secure your outbound connectivity, allow only TCP on the Kubernetes API server NodePorts and UDP on the VPN NodePorts for your location. You can find your active NodePorts by running the **`ibmcloud sat location get --location <location>`** command.
{: tip}







### Dallas
{: #dal-outbound}

See the following outbound requirements for the Dallas region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 52.117.39.146, 169.48.134.66, 169.63.36.210 | - TCP 30000 - 32767  \n -UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 52.117.55.50  \n - 150.239.137.98  \n - 169.46.88.106  \n - 169.48.139.210  \n - 169.48.188.146  \n - 169.59.239.66  \n - 169.60.2.74  \n - 169.61.140.18  \n - 169.61.156.226  \n - 169.61.31.178  \n - 169.61.38.178  \n - 169.62.221.10 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | All IP addresses listed in the **US South** row of the table of the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and client or authorized user | All IP addresses listed for US South (`dal`) in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips) | See documentation |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts |  - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | NTP protocol and UDP port 123 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Dallas region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

### Frankfurt
{: #fra-outbound}

See the following outbound requirements for the Frankfurt region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 149.81.188.122, 158.177.88.18, 161.156.38.122 | - TCP 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.eu.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 149.81.188.130  \n - 149.81.188.138  \n - 149.81.188.146  \n - 149.81.188.154  \n - 158.177.109.210  \n - 158.177.169.162  \n - 158.177.179.154  \n - 158.177.75.210  \n - 161.156.38.10  \n - 161.156.38.18  \n - 161.156.38.2  \n - 161.156.38.26  \n **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.eu-de.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | All IP addresses listed for **EU Central** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and clients or authorized users | All IP addresses listed for **EU Central** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips) | [See documentation](/docs/openshift?topic=openshift-firewall#master_ips) |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | - |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Frankfurt region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### London
{: #lon-outbound}

See the following outbound requirements for the London region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 158.175.120.210, 141.125.97.106, 158.176.139.66 | - TCP 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.eu.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 141.125.137.50  \n - 141.125.137.98  \n - 141.125.66.114  \n - 141.125.87.226  \n - 158.175.125.50  \n - 158.175.130.138  \n - 158.175.131.242  \n - 158.175.140.106  \n - 158.176.104.186  \n - 158.176.135.26  \n - 158.176.142.106  \n - 158.176.74.242  \n **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.eu-gb.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | All IP addresses listed for **UK South** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and clients or authorized users | All IP addresses listed for **UK South** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips) | [See documentation](/docs/openshift?topic=openshift-firewall#master_ips) |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | - |
{: caption="Required outbound connectivity for hosts on the primary network interface in the London region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}



### Sao Paulo
{: #sao-outbound}

See the following outbound requirements for the Sao Paulo region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 163.107.67.18, 163.109.71.82, 169.57.144.42 | - TCP 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 163.107.67.26  \n - 163.107.69.114  \n - 163.107.70.162  \n - 163.107.70.74  \n - 163.109.67.210  \n - 163.109.70.226  \n - 163.109.70.234  \n - 163.109.71.90  \n - 169.57.155.74  \n - 169.57.162.26  \n - 169.57.163.90  \n - 169.57.215.218  \n **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.br-sao.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | - |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Sao Paulo region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### Sydney
{: #syd-outbound}

See the following outbound requirements for the Sydney region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 130.198.65.82, 135.90.66.194, 168.1.58.90 | - TCP 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.ap.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 130.198.70.242  \n - 130.198.75.74  \n - 130.198.79.130  \n - 130.198.86.50  \n - 135.90.64.226  \n - 135.90.67.154  \n - 135.90.70.74  \n - 135.90.93.74  \n - 168.1.1.250  \n - 168.1.195.90  \n - 168.1.201.194  \n 168.1.57.122  \n -  **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.au-syd.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | - |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Sydney region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### Tokyo
{: #tok-outbound}

See the following outbound requirements for the Tokyo region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 161.202.104.226, 128.168.67.106, 165.192.108.10 | - TCP 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.ap.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 161.202.150.66  \n - 128.168.89.146  \n - 165.192.71.226  \n - 169.56.18.98  \n - 128.168.68.42  \n - 165.192.76.2  \n - 161.202.235.106  \n - 128.168.106.18  \n - 165.192.111.170  \n - 161.202.89.122  \n - 128.168.151.170  \n - 165.192.64.2  \n **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.jp-tok.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | All IP addresses listed for **AP North** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and clients or authorized users | All IP addresses listed for **AP North** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips) | [See documentation](/docs/openshift?topic=openshift-firewall#master_ips) |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | - |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Tokyo region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

### Toronto
{: #tor-outbound}

See the following outbound requirements for the Toronto region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 163.74.65.138, 163.75.70.50, 169.53.160.154 | - TCP 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 158.85.124.194  \n - 158.85.79.18  \n - 158.85.86.234  \n - 163.74.67.114  \n - 163.74.70.82  \n - 163.74.70.90  \n - 163.74.70.98  \n - 163.75.70.74  \n - 163.75.70.82  \n - 163.75.70.90  \n - 163.75.70.98  \n - 169.55.154.154  \n **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.ca-tor.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | All IP addresses listed for **Toronto** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and client or authorized user | All IP addresses listed for Toronto (`tor`) in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips) | See documentation |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts |  - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | NTP protocol and UDP port 123 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Toronto region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### Washington D.C.
{: #wdc-outbound}

See the following outbound requirements for the Washington D.C. region.
{: shortdec}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | 169.63.123.154, 169.63.110.114, 169.62.13.2, 169.60.123.162, 169.59.152.58, 52.117.93.26 | - TCP 30000 - 32767  \n UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 52.117.112.242  \n - 169.47.156.154  \n - 169.47.174.178  \n - 169.59.135.26  \n - 169.60.122.226  \n - 169.61.101.226  \n - 169.62.1.34  \n - 169.62.53.58  \n - 169.63.113.122  \n - 169.63.121.178  \n - 169.63.133.10  \n - 169.63.148.250  \n **Tip**: To programmatically retrieve this list of IP addresses, you can run `dig c-<XX>-ws.us-east.link.satellite.cloud.ibm.com +short` from a host that is attached to your location but unassigned to any resources. Replace `<XX>` with `01`, `02`, and so on, and run this `dig` until no further DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | All IP addresses listed for **US East** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips), or allow all outbound | TCP 443 |
| Allow [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies) to set up and manage your location | All hosts and clients or authorized users | All IP addresses listed for **US East** in the [{{site.data.keyword.openshiftlong_notm}} firewall documentation](/docs/openshift?topic=openshift-firewall#master_ips) | [See documentation](/docs/openshift?topic=openshift-firewall#master_ips) |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} | TCP 80, 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers | All hosts | - 0.rhel.pool.ntp.org  \n - 1.rhel.pool.ntp.org  \n - 2.rhel.pool.ntp.org  \n - 3.rhel.pool.ntp.org | NTP protocol and UDP port 123 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Washington DC region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}







