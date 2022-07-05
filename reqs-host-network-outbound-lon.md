---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-05"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# London
{: #reqs-host-network-outbound-lon}

Before you create region-specific firewall rules, make sure to create the [general firewall rules](/docs/satellite?topic=satellite-reqs-host-network-outbound) for hosts in any region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-lon}


Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts in Red Hat CoreOS enabled locations.
* Destination IP addresses: 158.175.113.26,141.125.85.26,158.176.90.58
* Destination hostnames: `c116.eu-gb.satellite.cloud.ibm.com`, `c116-1.eu-gb.satellite.cloud.ibm.com`, `c116-2.eu-gb.satellite.cloud.ibm.com`, `c116-3.eu-gb.satellite.cloud.ibm.com`, `c116-e.eu-gb.satellite.cloud.ibm.com`
* Protocol and ports: TCP 30000 - 32767


Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts in locations without CoreOS enabled.
* Destination IP addresses: 158.175.120.210, 141.125.97.106, 158.176.139.66  
* Destination hostnames:  `c106.eu-gb.satellite.cloud.ibm.com`, `c106-1.eu-gb.satellite.cloud.ibm.com`, `c106-2.eu-gb.satellite.cloud.ibm.com`, `c106-3.eu-gb.satellite.cloud.ibm.com`, `c106-e.eu-gb.satellite.cloud.ibm.com` 
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-lon}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: N/A
* Destination hostnames: `s3.eu.cloud-object-storage.appdomain.cloud` and `*.s3.eu.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS 443

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-lon}

You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.eu-gb.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: 141.125.137.50, 141.125.137.98, 141.125.66.114, 141.125.87.226, 158.175.125.50, 158.175.130.138, 158.175.131.242, 158.175.140.106, 158.176.104.186, 158.176.135.26, 158.176.142.106, 158.176.74.242
* Destination hostnames: `c-01-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-02-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-03-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-04-ws.eu-gb.link.satellite.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-lon}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: 159.122.224.242, 158.175.65.170, 158.176.95.146
* Destination hostnames: `origin.eu-gb.containers.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-lon}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  |  - `api.eu-gb.link.satellite.cloud.ibm.com`, `config.eu-gb.satellite.cloud.ibm.com`, `eu-gb.containers.cloud.ibm.com` 
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-lon}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: N/A 
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `uk.icr.io`, `registry.eu-gb.bluemix.net`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-lon}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: HTTPS 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-lon}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: HTTPS 443


