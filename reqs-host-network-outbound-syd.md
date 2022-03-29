---

copyright:
  years: 2022, 2022
lastupdated: "2022-03-28"

keywords: satellite, hybrid, multicloud, hypershift, core os

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Sydney
{: #reqs-host-network-outbound-syd}

Before creating region specific firewall rules, make sure to create the general firewall rules for hosts in any region.
{: shortdesc}




Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:  130.198.65.82, 135.90.66.194, 168.1.58.90
:   **Destination hostnames**: `c106.au-syd.satellite.cloud.ibm.com`, `c106-1.au-syd.satellite.cloud.ibm.com`, `c106-2.au-syd.satellite.cloud.ibm.com`, `c106-3.au-syd.satellite.cloud.ibm.com`, `c106-e.au-syd.satellite.cloud.ibm.com` 
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:   N/A
:   **Destination hostnames**: `s3.ap.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 130.198.70.242, 130.198.75.74, 130.198.79.130, 130.198.86.50, 135.90.64.226, 135.90.67.154, 135.90.70.74, 135.90.93.74, 168.1.1.250, 168.1.195.90, 168.1.201.194, 168.1.57.122 
:   **Destination hostnames**: `c-01-ws.au-syd.link.satellite.cloud.ibm.com`, `c-02-ws.au-syd.link.satellite.cloud.ibm.com`, `c-03-ws.au-syd.link.satellite.cloud.ibm.com`, `c-04-ws.au-syd.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

You can find the hostnames or IPs by running the `dig c-<XX>-ws.au-syd.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
{: tip}

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 130.198.71.20, 135.90.77.68, 168.1.20.148  
:   **Destination hostnames**: `origin.au-syd.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
:   **Destination hostnames**: `api.au-syd.link.satellite.cloud.ibm.com`, `config.au-syd.satellite.cloud.ibm.com`, `au-syd.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `icr.io`, `registry.bluemix.net`, `au.icr.io`, `registry.au-syd.bluemix.net`
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs and hostnames**: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
:   **Protocol and ports** TCP 443 and 6443

Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs and hostnames**: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
:   **Protocol and ports**: TCP 443