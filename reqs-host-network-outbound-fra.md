---

copyright:
  years: 2022, 2022
lastupdated: "2022-04-06"

keywords: satellite, hybrid, multicloud, hypershift, core os, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Frankfurt
{: #reqs-host-network-outbound-fra}

Before creating region specific firewall rules, make sure to create the general firewall rules for hosts in any region.
{: shortdesc}


To check your host set up, you can use the `satellite-host-check` script. For more information, see [Checking your host set up](/docs/satellite?topic=satellite-host-network-check).
{: tip}



## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-fra}



{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 149.81.188.122, 158.177.88.18, 161.156.38.122  
* Destination hostnames:  `c116.eu-de.satellite.cloud.ibm.com`, `c116-1.eu-de.satellite.cloud.ibm.com`, `c116-2.eu-de.satellite.cloud.ibm.com`, `c116-3.eu-de.satellite.cloud.ibm.com`, `c116-e.eu-de.satellite.cloud.ibm.com`
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-fra}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs:   N/A
* Destination hostnames: `s3.eu.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-fra}

You can find these hostnames or IPs by running the `dig c-<XX>-ws.eu-de.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 149.81.188.130, 149.81.188.138, 149.81.188.146, 149.81.188.154, 158.177.109.210, 158.177.169.162, 158.177.179.154, 158.177.75.210, 161.156.38.10, 161.156.38.18, 161.156.38.2, 161.156.38.26  
* Destination hostnames:  `c-01-ws.eu-de.link.satellite.cloud.ibm.com`, `c-02-ws.eu-de.link.satellite.cloud.ibm.com`, `c-03-ws.eu-de.link.satellite.cloud.ibm.com`, `c-04-ws.eu-de.link.satellite.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-fra}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: 149.81.72.236, 158.177.210.28, 161.156.137.131  
* Destination hostnames: `origin.eu-de.containers.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-fra}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs:  [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}
* Destination hostnames: `api.eu-de.link.satellite.cloud.ibm.com`, `config.eu-de.satellite.cloud.ibm.com`, `eu-de.containers.cloud.ibm.com` 
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-fra}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs:   N/A
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `de.icr.io`, `registry.eu-de.bluemix.net`
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-fra}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: TCP 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-fra}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: TCP 443


