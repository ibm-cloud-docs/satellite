---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-04"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Non Red Hat CoreOS hosts in Sydney
{: #reqs-host-network-outbound-syd}

Review the following network requirements for outbound connectivity for non Red Hat CoreOS (RHCOS) hosts in the Sydney (au-syd) region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}

## Common outbound connectivity requirements
{: #common-out-reqs-syd}

The following network requirements are common for non-RHCOS hosts in all regions. 

Allow hosts to connect to {{site.data.keyword.IBM_notm}}
:    * Destination hostnames: `cloud.ibm.com`, `containers.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS Port 443

Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123

Allow hosts to communicate with {{site.data.keyword.iamshort}}
:    Your firewall must be Layer 7 to allow the IAM domain name. IAM does not have specific IP addresses that you can allow. If your firewall does not support Layer 7, you can allow all HTTPS network traffic on port 443.
:    * Destination hostnames: `https://iam.bluemix.net`, `https://iam.cloud.ibm.com`
     * Protocol and ports: TCP 443

Allow hosts to connect to the LaunchDarkly service
:    * Destination hostnames: `app.launchdarkly.com`,`clientstream.launchdarkly.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.



## Network requirements for Sydney (au-syd)
{: #host-out-non-syd}

The following outbound network requirements are specific for non-RHCOS hosts in the Sydney (au-syd) region.

Allow control plane worker nodes to communicate with the control plane master
:    * Destination IP addresses:  130.198.65.82, 135.90.66.194, 168.1.58.90
     * Destination hostnames: `c106.au-syd.satellite.cloud.ibm.com`, `c106-1.au-syd.satellite.cloud.ibm.com`, `c106-2.au-syd.satellite.cloud.ibm.com`, `c106-3.au-syd.satellite.cloud.ibm.com`, `c106-e.au-syd.satellite.cloud.ibm.com` 
     * Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane worker nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `s3.ap.cloud-object-storage.appdomain.cloud` and `*.s3.ap.cloud-object-storage.appdomain.cloud`
     * Protocol and ports: HTTPS 443

Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 130.198.75.74, 135.90.67.154, 168.1.201.194
     * Destination hostnames: `c-01-ws.au-syd.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.au-syd.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.


Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 130.198.66.26, 135.90.69.66, 168.1.8.195
     * Destination hostnames: `origin.au-syd.containers.cloud.ibm.com` 
     * Protocol and ports: HTTPS 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API
:    * Destination IP addresses: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} 
     * Destination hostnames: `api.au-syd.link.satellite.cloud.ibm.com`, `config.au-syd.satellite.cloud.ibm.com`, `au-syd.containers.cloud.ibm.com`, `config.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `au.icr.io`, `registry.au-syd.bluemix.net`
     * Protocol and ports: HTTPS 443

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443
     
:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.

Optional: Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
     * Protocol and ports: HTTPS 443
     
:    If you plan to use {{site.data.keyword.loganalysislong_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.

