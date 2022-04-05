---

copyright:
  years: 2022, 2022
lastupdated: "2022-04-05"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# London
{: #reqs-host-network-outbound-lon}

Before creating region specific firewall rules, make sure to create the general firewall rules for hosts in any region.
{: shortdesc}



## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-lon}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 158.175.120.210, 141.125.97.106, 158.176.139.66  
* Destination hostnames:  `c106.eu-gb.satellite.cloud.ibm.com`, `c106-1.eu-gb.satellite.cloud.ibm.com`, `c106-2.eu-gb.satellite.cloud.ibm.com`, `c106-3.eu-gb.satellite.cloud.ibm.com`, `c106-e.eu-gb.satellite.cloud.ibm.com` 
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-lon}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: N/A
* Destination hostnames: `s3.eu.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-lon}

You can find the hostnames or IPs by running the `dig c-<XX>-ws.eu-gb.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 141.125.137.50, 141.125.137.98, 141.125.66.114, 141.125.87.226, 158.175.125.50, 158.175.130.138, 158.175.131.242, 158.175.140.106, 158.176.104.186, 158.176.135.26, 158.176.142.106, 158.176.74.242
* Destination hostnames: `c-01-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-02-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-03-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-04-ws.eu-gb.link.satellite.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-lon}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: 141.125.74.4, 158.175.157.149,  158.176.131.107  
* Destination hostnames: `origin.eu-gb.containers.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-lon}

{{site.data.keyword.satelliteshort}} control plane hosts
 [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  |  - `api.eu-gb.link.satellite.cloud.ibm.com`, `config.eu-gb.satellite.cloud.ibm.com`, `eu-gb.containers.cloud.ibm.com` 
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-lon}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs:   N/A 
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `uk.icr.io`, `registry.eu-gb.bluemix.net`
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-lon}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: TCP 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-lon}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: TCP 443


