---

copyright:
  years: 2022, 2022
lastupdated: "2022-04-19"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Washington, D.C.
{: #reqs-host-network-outbound-wdc}

Before creating region specific firewall rules, make sure to create the general firewall rules for hosts in any region.
{: shortdesc}


To check your host set up, you can use the `satellite-host-check` script. For more information, see [Checking your host set up](/docs/satellite?topic=satellite-host-network-check).
{: tip}



## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-wdc}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs:  169.63.123.154, 169.63.110.114, 169.62.13.2, 169.60.123.162, 169.59.152.58, 52.117.93.26  
* Destination hostnames:  `c107.us-east.satellite.cloud.ibm.com`, `c107-1.us-east.satellite.cloud.ibm.com`, `c107-2.us-east.satellite.cloud.ibm.com`, `c107-3.us-east.satellite.cloud.ibm.com`, `c107-e.us-east.satellite.cloud.ibm.com`, `c117.us-east.satellite.cloud.ibm.com`, `c117-1.us-east.satellite.cloud.ibm.com`, `c117-2.us-east.satellite.cloud.ibm.com`, `c117-3.us-east.satellite.cloud.ibm.com`, `c117-e.us-east.satellite.cloud.ibm.com` 
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-wdc}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: N/A
* Destination hostnames: `s3.us.cloud-object-storage.appdomain.cloud` and `*.s3.us.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS 443

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-wdc}

You can find the hostnames or IPs by running the `dig c-<XX>-ws.us-east.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 52.117.112.242, 169.47.156.154, 169.47.174.178, 169.59.135.26, 169.60.122.226, 169.61.101.226, 169.62.1.34, 169.62.53.58, 169.63.113.122, 169.63.121.178, 169.63.133.10, 169.63.148.250 
* Destination hostnames: `c-01-ws.us-east.link.satellite.cloud.ibm.com`, `c-02-ws.us-eat.link.satellite.cloud.ibm.com`, `c-03-ws.us-east.link.satellite.cloud.ibm.com`, `c-04-ws.us-east.link.satellite.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-wdc}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: 169.62.30.148, 169.63.121.164, 169.63.170.60  
* Destination hostnames: `origin.us-east.containers.cloud.ibm.com` 
* Protocol and ports: TCP 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} the service API, Config API, and Link API
{: #host-out-akamai-wdc}

{{site.data.keyword.satelliteshort}} control plane hosts
 [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
* Destination hostnames: `api.us-east.link.satellite.cloud.ibm.com`, `config.us-east.satellite.cloud.ibm.com`, `us-east.containers.cloud.ibm.com` 
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-wdc}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: N/A
* Destination hostnames: `icr.io`, `us.icr.io`, `registry.bluemix.net`,
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-wdc}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: TCP 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-wdc}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: TCP 443


