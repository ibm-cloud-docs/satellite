---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-05"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Tokyo
{: #reqs-host-network-outbound-tok}

Before you create region-specific firewall rules, make sure to create the [general firewall rules](/docs/satellite?topic=satellite-reqs-host-network-outbound) for hosts in any region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}



## Allow control plane worker nodes to communicate with the control plane master in locations without CoreOS enabled
{: #host-out-cp-tok}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts in Red Hat CoreOS enabled locations.
* Destination IP addresses: 161.202.151.170, 128.168.96.130, 165.192.108.34
* Destination hostnames: `c117.jp-tok.satellite.cloud.ibm.com`, `c117-1.jp-tok.satellite.cloud.ibm.com`,`c117-2.jp-tok.satellite.cloud.ibm.com`, `c117-3.jp-tok.satellite.cloud.ibm.com`, `c117-e.private.jp-tok.satellite.cloud.ibm.com`
* Protocol and ports: TCP 30000 - 32767

{{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: 161.202.104.226, 128.168.67.106, 165.192.108.10 
* Destination hostnames:  `c114.jp-tok.satellite.cloud.ibm.com`, `c114-1.jp-tok.satellite.cloud.ibm.com`, `c114-2.jp-tok.satellite.cloud.ibm.com`, `c114-3.jp-tok.satellite.cloud.ibm.com`, `c114-e.jp-tok.satellite.cloud.ibm.com` 
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-tok}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: N/A
* Destination hostnames: `s3.ap.cloud-object-storage.appdomain.cloud` and `*.s3.ap.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS 443

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-tok}

You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.jp-tok.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: 161.202.150.66, 128.168.89.146, 165.192.71.226, 169.56.18.98, 128.168.68.42, 165.192.76.2, 161.202.235.106, 128.168.106.18, 165.192.111.170, 161.202.89.122, 128.168.151.170, 165.192.64.2 
* Destination hostnames: `c-01-ws.jp-tok.link.satellite.cloud.ibm.com`, `c-02-ws.jp-tok.link.satellite.cloud.ibm.com`, `c-03-ws.jp-tok.link.satellite.cloud.ibm.com`, `c-04-ws.jp-tok.link.satellite.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-tok}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: 165.192.69.69, 161.202.126.210, 128.168.71.117
* Destination hostnames: `origin.jp-tok.containers.cloud.ibm.com`
* Protocol and ports: HTTPS 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-tok}

Allow the following addresses, hostnames, protocols, and ports for {{site.data.keyword.satelliteshort}} control plane hosts.
* Destination IP addresses: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
* Destination hostnames: `api.jp-tok.link.satellite.cloud.ibm.com`, `config.jp-tok.satellite.cloud.ibm.com`, `jp-tok.containers.cloud.ibm.com` 
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-tok}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses: N/A
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `jp.icr.io`, `registry.au-syd.bluemix.net`, `au.icr.io`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-tok}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts
* Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: HTTPS 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-tok}

Allow the following addresses, hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: HTTPS 443

