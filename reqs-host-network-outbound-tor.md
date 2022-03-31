---

copyright:
  years: 2022, 2022
lastupdated: "2022-03-31"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Toronto
{: #reqs-host-network-outbound-tor}

Before creating region specific firewall rules, make sure to create the general firewall rules for hosts in any region.
{: shortdesc}


## Allow control plane worker nodes to communicate with the control plane master
{: #host-out-cp-tor}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs:  163.74.65.138, 163.75.70.50, 169.53.160.154 
* Destination hostnames:  `c105.ca-tor.satellite.cloud.ibm.com`, `c105-1.ca-tor.satellite.cloud.ibm.com`, `c105-2.ca-tor.satellite.cloud.ibm.com`, `c105-3.ca-tor.satellite.cloud.ibm.com`, `c105-e.ca-tor.satellite.cloud.ibm.com` 
* Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

## Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
{: #host-out-worker-tor}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: N/A
* Destination hostnames: `s3.us.cloud-object-storage.appdomain.cloud`
* Protocol and ports: HTTPS

## Allow Link connectors to connect to the Link tunnel server endpoint
{: #host-out-link-tor}

You can find the hostnames or IPs by running the `dig c-<XX>-ws.ca-tor.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: 158.85.124.194, 158.85.79.18, 158.85.86.234, 163.74.67.114, 163.74.70.82, 163.74.70.90, 163.74.70.98, 163.75.70.74, 163.75.70.82, 163.75.70.90, 163.75.70.98, 169.55.154.154 
* Destination hostnames: `c-01-ws.ca-tor.link.satellite.cloud.ibm.com`, `c-02-ws.ca-tor.link.satellite.cloud.ibm.com`, `c-03-ws.ca-tor.link.satellite.cloud.ibm.com`, `c-04-ws.ca-tor.link.satellite.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to be attached to a location and assigned to services in the location
{: #host-out-services-tor}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: 158.85.95.211, 163.74.65.28, 163.75.64.213  
* Destination hostnames: `origin.ca-tor.containers.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
{: #host-out-akamai-tor}

{{site.data.keyword.satelliteshort}} control plane hosts
* Destination IPs: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
* Destination hostnames: `api.ca-tor.link.satellite.cloud.ibm.com`, `config.ca-tor.satellite.cloud.ibm.com`, `ca-tor.containers.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
{: #host-out-cr-tor}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs: N/A
* Destination hostnames: `icr.io`, `registry.bluemix.net`, `ca.icr.io`
* Protocol and ports: TCP 443

## Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
{: #host-out-mon-tor}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
* Protocol and ports: TCP 443 and 6443

## Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
{: #host-out-la-tor}

All {{site.data.keyword.satelliteshort}} hosts
* Destination IPs and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
* Protocol and ports: TCP 443



