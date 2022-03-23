---

copyright:
  years: 2022, 2022
lastupdated: "2022-03-23"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Required outbound connectivity for hosts in standard locations
{: #reqs-host-network-outbound}

Review the following outbound connectivity requirements for hosts in RHEL-based locations.
{: shortdesc}


## Dallas
{: #dal-outbound}

Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 52.117.39.146, 169.48.134.66, 169.63.36.210
:   **Destination hostnames**: `c119.us-south.satellite.cloud.ibm.com`, `c119-1.us-south.satellite.cloud.ibm.com`, `c119-2.us-south.satellite.cloud.ibm.com`, `c119-3.us-south.satellite.cloud.ibm.com`, `c119-e.us-south.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: N/A
:   **Destination hostnames** `s3.us.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.us-south.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 169.48.139.210, 169.48.188.146, 169.59.239.66, 169.60.2.74, 169.61.140.18, 169.61.156.226, 169.61.31.178, 169.61.38.178, 169.62.221.10
:   **Destination hostnames**: `c-01-ws.us-south.link.satellite.cloud.ibm.com`, `c-02-ws.us-south.link.satellite.cloud.ibm.com`, `c-03-ws.us-south.link.satellite.cloud.ibm.com`, `c-04-ws.us-south.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 52.116.223.52, 169.48.141.13, 169.61.50.28  
:   **Destination hostnames**: `origin.us-south.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:  [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
:   **Destination hostnames**: `api.us-south.link.satellite.cloud.ibm.com`, `config.us-south.satellite.cloud.ibm.com`, `us-south.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**:   N/A
:   **Destination hostnames**: `icr.io`, `us.icr.io`, `registry.bluemix.net`,
:   **Protocol and ports**: TCP 443

## Frankfurt
{: #fra-outbound}

Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 149.81.188.122, 158.177.88.18, 161.156.38.122  
:   **Destination hostnames**:  `c116.eu-de.satellite.cloud.ibm.com`, `c116-1.eu-de.satellite.cloud.ibm.com`, `c116-2.eu-de.satellite.cloud.ibm.com`, `c116-3.eu-de.satellite.cloud.ibm.com`, `c116-e.eu-de.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:   N/A
:   **Destination hostnames**: `s3.eu.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.eu-de.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 149.81.188.130, 149.81.188.138, 149.81.188.146, 149.81.188.154, 158.177.109.210, 158.177.169.162, 158.177.179.154, 158.177.75.210, 161.156.38.10, 161.156.38.18, 161.156.38.2, 161.156.38.26  
:   **Destination hostnames**:  `c-01-ws.eu-de.link.satellite.cloud.ibm.com`, `c-02-ws.eu-de.link.satellite.cloud.ibm.com`, `c-03-ws.eu-de.link.satellite.cloud.ibm.com`, `c-04-ws.eu-de.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 149.81.72.236, 158.177.210.28, 161.156.137.131  
:   **Destination hostnames**: `origin.eu-de.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}
:   **Destination hostnames**: `api.eu-de.link.satellite.cloud.ibm.com`, `config.eu-de.satellite.cloud.ibm.com`, `eu-de.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `icr.io`, `registry.bluemix.net`, `de.icr.io`, `registry.eu-de.bluemix.net`
:   **Protocol and ports**: TCP 443


## London
{: #lon-outbound}

Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 158.175.120.210, 141.125.97.106, 158.176.139.66  
:   **Destination hostnames**:  `c106.eu-gb.satellite.cloud.ibm.com`, `c106-1.eu-gb.satellite.cloud.ibm.com`, `c106-2.eu-gb.satellite.cloud.ibm.com`, `c106-3.eu-gb.satellite.cloud.ibm.com`, `c106-e.eu-gb.satellite.cloud.ibm.com` 
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `s3.eu.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.eu-gb.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 141.125.137.50, 141.125.137.98, 141.125.66.114, 141.125.87.226, 158.175.125.50, 158.175.130.138, 158.175.131.242, 158.175.140.106, 158.176.104.186, 158.176.135.26, 158.176.142.106, 158.176.74.242
:   **Destination hostnames**: `c-01-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-02-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-03-ws.eu-gb.link.satellite.cloud.ibm.com`, `c-04-ws.eu-gb.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 141.125.74.4, 158.175.157.149,  158.176.131.107  
:   **Destination hostnames**: `origin.eu-gb.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
 [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}  |  - `api.eu-gb.link.satellite.cloud.ibm.com`, `config.eu-gb.satellite.cloud.ibm.com`, `eu-gb.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**:   N/A 
:   **Destination hostnames**: `icr.io`, `registry.bluemix.net`, `uk.icr.io`, `registry.eu-gb.bluemix.net`
:   **Protocol and ports**: TCP 443




## Sao Paulo
{: #sao-outbound}

Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:  163.107.67.18, 163.109.71.82, 169.57.144.42
:   **Destination hostnames**: `c105.br-sao.satellite.cloud.ibm.com`, `c105-1.br-sao.satellite.cloud.ibm.com`, `c105-2.br-sao.satellite.cloud.ibm.com`, `c105-3.br-sao.satellite.cloud.ibm.com`, `c105-e.br-sao.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443, 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `s3.us.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.br-sao.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 163.107.67.26, 163.107.69.114, 163.107.70.162, 163.107.70.74, 163.109.67.210, 163.109.70.226, 163.109.70.234, 163.109.71.90, 169.57.155.74, 169.57.162.26, 169.57.163.90, 169.57.215.218 
:   **Destination hostnames**: `c-01-ws.br-sao.link.satellite.cloud.ibm.com`, `c-02-ws.br-sao.link.satellite.cloud.ibm.com`, `c-03-ws.br-sao.link.satellite.cloud.ibm.com`, `c-04-ws.br-sao.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 163.107.65.70, 163.109.64.230, 169.57.163.14  
:   **Destination hostnames**: `origin.br-sao.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
:   **Destination hostnames**: `api.br-sao.link.satellite.cloud.ibm.com`, `config.br-sao.satellite.cloud.ibm.com`, `br-sao.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**:   N/A
:   **Destination hostnames**: `icr.io`, `registry.bluemix.net`, `br.icr.io`
:   **Protocol and ports**: TCP 443


## Sydney
{: #syd-outbound}

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


## Tokyo
{: #tok-outbound}

Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:  161.202.104.226, 128.168.67.106, 165.192.108.10 
:   **Destination hostnames**:  `c114.jp-tok.satellite.cloud.ibm.com`, `c114-1.jp-tok.satellite.cloud.ibm.com`, `c114-2.jp-tok.satellite.cloud.ibm.com`, `c114-3.jp-tok.satellite.cloud.ibm.com`, `c114-e.jp-tok.satellite.cloud.ibm.com` 
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:   N/A
:   **Destination hostnames**: `s3.ap.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.jp-tok.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 161.202.150.66, 128.168.89.146, 165.192.71.226, 169.56.18.98, 128.168.68.42, 165.192.76.2, 161.202.235.106, 128.168.106.18, 165.192.111.170, 161.202.89.122, 128.168.151.170, 165.192.64.2 
:   **Destination hostnames**: `c-01-ws.jp-tok.link.satellite.cloud.ibm.com`, `c-02-ws.jp-tok.link.satellite.cloud.ibm.com`, `c-03-ws.jp-tok.link.satellite.cloud.ibm.com`, `c-04-ws.jp-tok.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 161.202.247.13, 128.168.89.14, 165.192.85.219  
:   **Destination hostnames**: `origin.jp-tok.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
:   **Destination hostnames**: `api.jp-tok.link.satellite.cloud.ibm.com`, `config.jp-tok.satellite.cloud.ibm.com`, `jp-tok.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `icr.io`, `registry.bluemix.net`, `jp.icr.io`
:   **Protocol and ports**: TCP 443

## Toronto
{: #tor-outbound}

Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:  163.74.65.138, 163.75.70.50, 169.53.160.154 
:   **Destination hostnames**:  `c105.ca-tor.satellite.cloud.ibm.com`, `c105-1.ca-tor.satellite.cloud.ibm.com`, `c105-2.ca-tor.satellite.cloud.ibm.com`, `c105-3.ca-tor.satellite.cloud.ibm.com`, `c105-e.ca-tor.satellite.cloud.ibm.com` 
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `s3.us.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.ca-tor.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 158.85.124.194, 158.85.79.18, 158.85.86.234, 163.74.67.114, 163.74.70.82, 163.74.70.90, 163.74.70.98, 163.75.70.74, 163.75.70.82, 163.75.70.90, 163.75.70.98, 169.55.154.154 
:   **Destination hostnames**: `c-01-ws.ca-tor.link.satellite.cloud.ibm.com`, `c-02-ws.ca-tor.link.satellite.cloud.ibm.com`, `c-03-ws.ca-tor.link.satellite.cloud.ibm.com`, `c-04-ws.ca-tor.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 158.85.95.211, 163.74.65.28, 163.75.64.213  
:   **Destination hostnames**: `origin.ca-tor.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
:   **Destination hostnames**: `api.ca-tor.link.satellite.cloud.ibm.com`, `config.ca-tor.satellite.cloud.ibm.com`, `ca-tor.containers.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `icr.io`, `registry.bluemix.net`, `ca.icr.io`
:   **Protocol and ports**: TCP 443



## Washington D.C.
{: #wdc-outbound}


Allow control plane worker nodes to communicate with the control plane master
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:  169.63.123.154, 169.63.110.114, 169.62.13.2, 169.60.123.162, 169.59.152.58, 52.117.93.26  
:   **Destination hostnames**:  `c107.us-east.satellite.cloud.ibm.com`, `c107-1.us-east.satellite.cloud.ibm.com`, `c107-2.us-east.satellite.cloud.ibm.com`, `c107-3.us-east.satellite.cloud.ibm.com`, `c107-e.us-east.satellite.cloud.ibm.com`, `c117.us-east.satellite.cloud.ibm.com`, `c117-1.us-east.satellite.cloud.ibm.com`, `c117-2.us-east.satellite.cloud.ibm.com`, `c117-3.us-east.satellite.cloud.ibm.com`, `c117-e.us-east.satellite.cloud.ibm.com` 
:   **Protocol and ports**: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**:   N/A
:   **Destination hostnames**: `s3.us.cloud-object-storage.appdomain.cloud`
:   **Protocol and ports**: HTTPS

Allow Link connectors to connect to the Link tunnel server endpoint. You can find the hostnames or IPs by running the `dig c-<XX>-ws.us-east.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
:   **Destination IPs**: 52.117.112.242, 169.47.156.154, 169.47.174.178, 169.59.135.26, 169.60.122.226, 169.61.101.226, 169.62.1.34, 169.62.53.58, 169.63.113.122, 169.63.121.178, 169.63.133.10, 169.63.148.250 
:   **Destination hostnames**: `c-01-ws.us-east.link.satellite.cloud.ibm.com`, `c-02-ws.us-eat.link.satellite.cloud.ibm.com`, `c-03-ws.us-east.link.satellite.cloud.ibm.com`, `c-04-ws.us-east.link.satellite.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to be attached to a location and assigned to services in the location
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: 169.62.30.148, 169.63.121.164, 169.63.170.60  
:   **Destination hostnames**: `origin.us-east.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} the service API, Config API, and Link API
:   **Source**: {{site.data.keyword.satelliteshort}} control plane hosts
 [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
:   **Destination hostnames**: `api.us-east.link.satellite.cloud.ibm.com`, `config.us-east.satellite.cloud.ibm.com`, `us-east.containers.cloud.ibm.com` 
:   **Protocol and ports**: TCP 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination IPs**: N/A
:   **Destination hostnames**: `icr.io`, `us.icr.io`, `registry.bluemix.net`,
:   **Protocol and ports**: TCP 443
