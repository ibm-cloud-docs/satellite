---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-05"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Osaka
{: #reqs-host-network-outbound-osa}

Before you create region-specific firewall rules, make sure to create the [general firewall rules](/docs/satellite?topic=satellite-reqs-host-network-outbound) for hosts in any region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-osa}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses:  163.68.73.50, 163.69.65.242, 163.73.67.10  
* Destination hostnames: `c103.jp-osa.satellite.cloud.ibm.com`, `c103-1.jp-osa.satellite.cloud.ibm.com`, `c103-2.jp-osa.satellite.cloud.ibm.com`, `c103-3.jp-osa.satellite.cloud.ibm.com`, `c103-e.jp-osa.satellite.cloud.ibm.com`  
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-osa}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: N/A
* Destination hostnames: `s3.ap.cloud-object-storage.appdomain.cloud` and `*.s3.ap.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS 443

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-osa}

You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.jp-osa.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: 163.68.78.34, 163.68.78.42, 163.68.78.234, 163.68.79.194, 163.69.70.106, 163.69.70.146, 163.69.70.194, 163.69.70.202, 163.73.69.82, 163.73.69.90, 163.73.70.50, 163.73.70.58
* Destination hostnames: `c-01-ws.jp-osa.link.satellite.cloud.ibm.com`, `c-02-ws.jp-osa.link.satellite.cloud.ibm.com`, `c-03-ws.jp-osa.link.satellite.cloud.ibm.com`, `c-04-ws.jp-osa.link.satellite.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-osa}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: 165.192.69.69, 161.202.126.210, 128.168.71.117
* Destination hostnames: `origin.jp-osa.containers.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-osa}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
* Destination hostnames: `api.jp-osa.link.satellite.cloud.ibm.com`, `config.jp-osa.satellite.cloud.ibm.com`, `jp-osa.containers.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-osa}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: N/A
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `jp2.icr.io`, `au.icr.io`, `registy.au-syd.bluemix.net`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-osa}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: HTTPS 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-osa}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: HTTPS 443

