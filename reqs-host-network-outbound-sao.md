---

copyright:
  years: 2022, 2022
lastupdated: "2022-04-22"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Sao Paulo
{: #reqs-host-network-outbound-sao}

Before creating region specific firewall rules, make sure to create the general firewall rules for hosts in any region.
{: shortdesc}


To check your host set up, you can use the `satellite-host-check` script. For more information, see [Checking your host set up](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-sao}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs:  163.107.67.18, 163.109.71.82, 169.57.144.42
* Destination hostnames: `c105.br-sao.satellite.cloud.ibm.com`, `c105-1.br-sao.satellite.cloud.ibm.com`, `c105-2.br-sao.satellite.cloud.ibm.com`, `c105-3.br-sao.satellite.cloud.ibm.com`, `c105-e.br-sao.satellite.cloud.ibm.com`
* Protocol and ports: TCP 443, 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-sao}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: N/A
* Destination hostnames: `s3.us.cloud-object-storage.appdomain.cloud` and `*.s3.us.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS 443

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-sao}

You can find the hostnames or IPs by running the `dig c-<XX>-ws.br-sao.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 163.107.67.26, 163.107.69.114, 163.107.70.162, 163.107.70.74, 163.109.67.210, 163.109.70.226, 163.109.70.234, 163.109.71.90, 169.57.155.74, 169.57.162.26, 169.57.163.90, 169.57.215.218 
* Destination hostnames: `c-01-ws.br-sao.link.satellite.cloud.ibm.com`, `c-02-ws.br-sao.link.satellite.cloud.ibm.com`, `c-03-ws.br-sao.link.satellite.cloud.ibm.com`, `c-04-ws.br-sao.link.satellite.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-sao}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: 169.57.161.130, 163.109.65.146, 163.107.65.74
* Destination hostnames: `origin.br-sao.containers.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-sao}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
* Destination hostnames: `api.br-sao.link.satellite.cloud.ibm.com`, `config.br-sao.satellite.cloud.ibm.com`, `br-sao.containers.cloud.ibm.com` 
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-sao}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: N/A
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `br.icr.io`, `us.icr.io`, `registry.ng.bluemix.net`
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-sao}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: TCP 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-sao}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: TCP 443


