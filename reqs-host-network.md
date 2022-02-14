---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-14"

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


### Required outbound connectivity for hosts in all regions
{: #all-regions-outbound}

| Description | Source | Destination |Protocol and ports|
| --- | --- | --- | --- |
| Allow hosts to connect to {{site.data.keyword.IBM_notm}} | All hosts | `cloud.ibm.com`, `containers.cloud.ibm.com` | Port 443 |
| Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers| All hosts | `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`| NTP protocol and UDP port 123 |
| Allow hosts to communicate with {{site.data.keyword.iamshort}} | All hosts | `https://iam.bluemix.net`, `https://iam.cloud.ibm.com` | TCP 443. **Note**: Your firewall must be Layer 7 to allow the IAM domain name. IAM does not have specific IP addresses that you can allow. If your firewall does not support Layer 7, you can allow all HTTPS network traffic on port 443. |
| Allow hosts to connect to the LaunchDarkly service.| Control plane hosts| `app.launchdarkly.com`, `clientstream.launchdarkly.com` |
{: caption="Required outbound connectivity all hosts" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source addresses are in the second column. The destination addresses are in the third column. The protocol and ports are in the fourth column."}






### Dallas
{: #dal-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 52.117.39.146, 169.48.134.66, 169.63.36.210  \n - **Destination hostnames**: `c119.us-south.satellite.cloud.ibm.com`, `c119-1.us-south.satellite.cloud.ibm.com`, `c119-2.us-south.satellite.cloud.ibm.com`, `c119-3.us-south.satellite.cloud.ibm.com`, `c119-e.us-south.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 169.48.139.210, 169.48.188.146, 169.59.239.66, 169.60.2.74, 169.61.140.18, 169.61.156.226, 169.61.31.178, 169.61.38.178, 169.62.221.10  \n - **Destination hostnames**: `c-01-ws.us-south.link.satellite.cloud.ibm.com`.  \n - `c-02-ws.us-south.link.satellite.cloud.ibm.com`  \n - `c-03-ws.us-south.link.satellite.cloud.ibm.com`  \n - `c-04-ws.us-south.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.us-south.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | - **Destination IPs**: 52.116.223.52, 169.48.141.13, 169.61.50.28  \n - **Destination hostname**: `origin.us-south.containers.cloud.ibm.com` | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.us-south.link.satellite.cloud.ibm.com`, Config API: `config.us-south.satellite.cloud.ibm.com`, Service API: `us-south.containers.cloud.ibm.com`  | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Dallas region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

### Frankfurt
{: #fra-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 149.81.188.122, 158.177.88.18, 161.156.38.122  \n - **Destination hostnames**: `c116.eu-de.satellite.cloud.ibm.com`, `c116-1.eu-de.satellite.cloud.ibm.com`, `c116-2.eu-de.satellite.cloud.ibm.com`, `c116-3.eu-de.satellite.cloud.ibm.com`, `c116-e.eu-de.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.eu.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 149.81.188.130, 149.81.188.138, 149.81.188.146, 149.81.188.154, 158.177.109.210, 158.177.169.162, 158.177.179.154, 158.177.75.210, 161.156.38.10, 161.156.38.18, 161.156.38.2, 161.156.38.26  \n - **Destination hostnames**: `c-01-ws.eu-de.link.satellite.cloud.ibm.com`  \n - `c-02-ws.eu-de.link.satellite.cloud.ibm.com`  \n - `c-03-ws.eu-de.link.satellite.cloud.ibm.com`  \n - `c-04-ws.eu-de.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.eu-de.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | - **Destination IPs**: 149.81.72.236, 158.177.210.28, 161.156.137.131  \n - **Destination hostname**: `origin.eu-de.containers.cloud.ibm.com` | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.eu-de.link.satellite.cloud.ibm.com`, Config API: `config.eu-de.satellite.cloud.ibm.com`, Service API: `eu-de.containers.cloud.ibm.com` | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts | `icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`, `de.icr.io`, `registry.eu-de.bluemix.net` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Frankfurt region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### London
{: #lon-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 158.175.120.210, 141.125.97.106, 158.176.139.66  \n - **Destination hostnames**: `c106.eu-gb.satellite.cloud.ibm.com`, `c106-1.eu-gb.satellite.cloud.ibm.com`, `c106-2.eu-gb.satellite.cloud.ibm.com`, `c106-3.eu-gb.satellite.cloud.ibm.com`, `c106-e.eu-gb.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.eu.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 141.125.137.50, 141.125.137.98, 141.125.66.114, 141.125.87.226, 158.175.125.50, 158.175.130.138, 158.175.131.242, 158.175.140.106, 158.176.104.186, 158.176.135.26, 158.176.142.106, 158.176.74.242  \n - **Destination hostnames**:  \n - `c-01-ws.eu-gb.link.satellite.cloud.ibm.com`  \n - `c-02-ws.eu-gb.link.satellite.cloud.ibm.com`  \n - `c-03-ws.eu-gb.link.satellite.cloud.ibm.com`  \n - `c-04-ws.eu-gb.link.satellite.cloud.ibm.com`.  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.eu-gb.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | - **Destination IPs**: 141.125.74.4, 158.175.157.149,  158.176.131.107  \n - **Destination hostname**: `origin.eu-gb.containers.cloud.ibm.com` | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.eu-gb.link.satellite.cloud.ibm.com`, Config API: `config.eu-gb.satellite.cloud.ibm.com`, Service API: `eu-gb.containers.cloud.ibm.com` | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`, `uk.icr.io`, `registry.eu-gb.bluemix.net` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the London region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}



### Sao Paulo
{: #sao-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 163.107.67.18, 163.109.71.82, 169.57.144.42  \n - **Destination hostnames**: `c105.br-sao.satellite.cloud.ibm.com`, `c105-1.br-sao.satellite.cloud.ibm.com`, `c105-2.br-sao.satellite.cloud.ibm.com`, `c105-3.br-sao.satellite.cloud.ibm.com`, `c105-e.br-sao.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - 163.107.67.26, 163.107.69.114, 163.107.70.162, 163.107.70.74, 163.109.67.210, 163.109.70.226, 163.109.70.234, 163.109.71.90, 169.57.155.74, 169.57.162.26, 169.57.163.90, 169.57.215.218  \n - **Destination hostnames**:  - `c-01-ws.br-sao.link.satellite.cloud.ibm.com`  \n - `c-02-ws.br-sao.link.satellite.cloud.ibm.com`  \n - `c-03-ws.br-sao.link.satellite.cloud.ibm.com`.  \n  - `c-04-ws.br-sao.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.br-sao.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | - **Destination IPs**: 163.107.65.70, 163.109.64.230, 169.57.163.14  \n - **Destination hostname**: `origin.br-sao.containers.cloud.ibm.com` | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.br-sao.link.satellite.cloud.ibm.com`, Config API: `config.br-sao.satellite.cloud.ibm.com`, Service API: `br-sao.containers.cloud.ibm.com` | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`, `br.icr.io` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Sao Paulo region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### Sydney
{: #syd-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 130.198.65.82, 135.90.66.194, 168.1.58.90  \n - **Destination hostnames**: `c106.au-syd.satellite.cloud.ibm.com`, `c106-1.au-syd.satellite.cloud.ibm.com`, `c106-2.au-syd.satellite.cloud.ibm.com`, `c106-3.au-syd.satellite.cloud.ibm.com`, `c106-e.au-syd.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.ap.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 130.198.70.242, 130.198.75.74, 130.198.79.130, 130.198.86.50, 135.90.64.226, 135.90.67.154, 135.90.70.74, 135.90.93.74, 168.1.1.250, 168.1.195.90, 168.1.201.194, 168.1.57.122  \n - **Destination hostnames**: - `c-01-ws.au-syd.link.satellite.cloud.ibm.com`  \n - `c-02-ws.au-syd.link.satellite.cloud.ibm.com`  \n - `c-03-ws.au-syd.link.satellite.cloud.ibm.com`.  \n  - `c-04-ws.au-syd.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.au-syd.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | - **Destination IPs**: 130.198.71.20, 135.90.77.68, 168.1.20.148  \n - **Destination hostname**: `origin.au-syd.containers.cloud.ibm.com`  | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.au-syd.link.satellite.cloud.ibm.com`, Config API: `config.au-syd.satellite.cloud.ibm.com`, Service API: `au-syd.containers.cloud.ibm.com`  | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`, `au.icr.io`, `registry.au-syd.bluemix.net` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Sydney region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### Tokyo
{: #tok-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 161.202.104.226, 128.168.67.106, 165.192.108.10 \n - **Destination hostnames**: `c114.jp-tok.satellite.cloud.ibm.com`, `c114-1.jp-tok.satellite.cloud.ibm.com`, `c114-2.jp-tok.satellite.cloud.ibm.com`, `c114-3.jp-tok.satellite.cloud.ibm.com`, `c114-e.jp-tok.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.ap.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 161.202.150.66, 128.168.89.146, 165.192.71.226, 169.56.18.98, 128.168.68.42, 165.192.76.2, 161.202.235.106, 128.168.106.18, 165.192.111.170, 161.202.89.122, 128.168.151.170, 165.192.64.2  \n - **Destination hostnames**:  - `c-01-ws.jp-tok.link.satellite.cloud.ibm.com`  \n - `c-02-ws.jp-tok.link.satellite.cloud.ibm.com`  \n - `c-03-ws.jp-tok.link.satellite.cloud.ibm.com`.  \n  - `c-04-ws.jp-tok.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.jp-tok.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | - **Destination IPs**: 161.202.247.13, 128.168.89.14, 165.192.85.219  \n - **Destination hostname**: `origin.jp-tok.containers.cloud.ibm.com` | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.jp-tok.link.satellite.cloud.ibm.com`, Config API: `config.jp-tok.satellite.cloud.ibm.com`, Service API: `jp-tok.containers.cloud.ibm.com` | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`, `jp.icr.io` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Tokyo region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}

### Toronto
{: #tor-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 163.74.65.138, 163.75.70.50, 169.53.160.154  \n - **Destination hostnames**: `c105.ca-tor.satellite.cloud.ibm.com`, `c105-1.ca-tor.satellite.cloud.ibm.com`, `c105-2.ca-tor.satellite.cloud.ibm.com`, `c105-3.ca-tor.satellite.cloud.ibm.com`, `c105-e.ca-tor.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n - UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 158.85.124.194, 158.85.79.18, 158.85.86.234, 163.74.67.114, 163.74.70.82, 163.74.70.90, 163.74.70.98, 163.75.70.74, 163.75.70.82, 163.75.70.90, 163.75.70.98, 169.55.154.154  \n - **Destination hostnames**:  - `c-01-ws.ca-tor.link.satellite.cloud.ibm.com`  \n - `c-02-ws.ca-tor.link.satellite.cloud.ibm.com`  \n - `c-03-ws.ca-tor.link.satellite.cloud.ibm.com`.  \n  - `c-04-ws.ca-tor.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.ca-tor.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts |  - **Destination IPs**: 158.85.95.211, 163.74.65.28, 163.75.64.213  \n - **Destination hostname**: `origin.ca-tor.containers.cloud.ibm.com` | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.ca-tor.link.satellite.cloud.ibm.com`, Config API: `config.ca-tor.satellite.cloud.ibm.com`, Service API: `ca-tor.containers.cloud.ibm.com`| TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`, `ca.icr.io` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Toronto region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}


### Washington D.C.
{: #wdc-outbound}

|Description|Source IP|Destination IP|Protocol and ports|
| --- | --- | --- | --- |
| Allow control plane worker nodes to communicate with the control plane master | Control plane hosts | - **Destination IPs**: 169.63.123.154, 169.63.110.114, 169.62.13.2, 169.60.123.162, 169.59.152.58, 52.117.93.26  \n - **Destination hostnames**: `c107.us-east.satellite.cloud.ibm.com`, `c107-1.us-east.satellite.cloud.ibm.com`, `c107-2.us-east.satellite.cloud.ibm.com`, `c107-3.us-east.satellite.cloud.ibm.com`, `c107-e.us-east.satellite.cloud.ibm.com`, `c117.us-east.satellite.cloud.ibm.com`, `c117-1.us-east.satellite.cloud.ibm.com`, `c117-2.us-east.satellite.cloud.ibm.com`, `c117-3.us-east.satellite.cloud.ibm.com`, `c117-e.us-east.satellite.cloud.ibm.com` | - TCP 443, 30000 - 32767  \n UDP 30000 - 32767 |
| Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}} | Control plane hosts | `s3.us.cloud-object-storage.appdomain.cloud` | HTTPS |
| Allow Link connectors to connect to the Link tunnel server endpoint | Control plane hosts | - **Destination IPs**: 52.117.112.242, 169.47.156.154, 169.47.174.178, 169.59.135.26, 169.60.122.226, 169.61.101.226, 169.62.1.34, 169.62.53.58, 169.63.113.122, 169.63.121.178, 169.63.133.10, 169.63.148.250  \n - **Destination hostnames**:  - `c-01-ws.us-east.link.satellite.cloud.ibm.com`  \n - `c-02-ws.us-eat.link.satellite.cloud.ibm.com`  \n - `c-03-ws.us-east.link.satellite.cloud.ibm.com`.  \n  - `c-04-ws.us-east.link.satellite.cloud.ibm.com`  \n - **Tip**: You can find the hostnames or IPs by running the `dig c-<XX>-ws.us-east.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. | TCP 443 |
| Allow hosts to be attached to a location and assigned to services in the location | All hosts | **Destination IPs**: 169.62.30.148, 169.63.121.164, 169.63.170.60  \n - **Destination hostnames**: `origin.us-east.containers.cloud.ibm.com`  | TCP 443 |
| Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} the service API, Config API, and Link API | Control plane hosts | - **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  \n - **Destination hostnames**: Link API: `api.us-east.link.satellite.cloud.ibm.com`, Config API: `config.us-east.satellite.cloud.ibm.com`, Service API: `us-east.containers.cloud.ibm.com` | TCP 80, 443 |
| Allow hosts to communicate with {{site.data.keyword.registrylong_notm}} | All hosts |`icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net` | TCP 443 |
| Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}} | All hosts | [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints). | TCP 443 and 6443 |
| Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}} | All hosts | [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public). | TCP 443 and 80 |
{: caption="Required outbound connectivity for hosts on the primary network interface in the Washington DC region" caption-side="top"}
{: summary="The table shows the required outbound connectivity for hosts on the primary network interface. Rows are to be read from the left to right. The description is in the first column. The source IP addresses are in the second column. The destination IP addresses are in the third column. The protocol and ports are in the fourth column."}





