---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-05"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Dallas
{: #reqs-host-network-outbound-dal}

Before you create region-specific firewall rules, make sure to create the [general firewall rules](/docs/satellite?topic=satellite-reqs-host-network-outbound) for hosts in any region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-dal}



Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts in Red Hat CoreOS enabled locations.
* Destination IP addresses: 169.46.43.146,169.48.236.58,169.60.150.218
* Destination hostnames: `c131.us-south.satellite.cloud.ibm.com`, `c131-1.us-south.satellite.cloud.ibm.com`, `c131-2.us-south.satellite.cloud.ibm.com`, `c131-3.us-south.satellite.cloud.ibm.com`, `c131-e.us-south.satellite.cloud.ibm.com`
* Protocol and ports: TCP 30000 - 32767

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts in locations without Red Hat CoreOS enabled.

* Destination IP addresses: 52.117.39.146, 169.48.134.66, 169.63.36.210
* Destination hostnames: `c119.us-south.satellite.cloud.ibm.com`, `c119-1.us-south.satellite.cloud.ibm.com`, `c119-2.us-south.satellite.cloud.ibm.com`, `c119-3.us-south.satellite.cloud.ibm.com`, `c119-e.us-south.satellite.cloud.ibm.com`
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-dal}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: N/A
* Destination hostnames: `s3.us.cloud-object-storage.appdomain.cloud` and `*.s3.us.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS 443

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-dal}

You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.us-south.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: 169.48.139.210, 169.48.188.146, 169.59.239.66, 169.60.2.74, 169.61.140.18, 169.61.156.226, 169.61.31.178, 169.61.38.178, 169.62.221.10
* Destination hostnames: `c-01-ws.us-south.link.satellite.cloud.ibm.com`, `c-02-ws.us-south.link.satellite.cloud.ibm.com`, `c-03-ws.us-south.link.satellite.cloud.ibm.com`, `c-04-ws.us-south.link.satellite.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-dal}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: 169.46.110.218, 169.47.70.10, 169.62.166.98 
* Destination hostnames: `origin.us-south.containers.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-dal}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
* Destination hostnames: `api.us-south.link.satellite.cloud.ibm.com`, `config.us-south.satellite.cloud.ibm.com`, `us-south.containers.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-dal}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: N/A
* Destination hostnames: `icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-dal}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: HTTPS 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-dal}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: HTTPS 443






