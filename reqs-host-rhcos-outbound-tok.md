---

copyright:
  years: 2023, 2023
lastupdated: "2023-03-04"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Red Hat CoreOS hosts in Tokyo
{: #reqs-host-rhcos-outbound-tok}

Review the following network requirements for outbound connectivity for Red Hat CoreOS (RHCOS) hosts in the Tokyo (`jp-tok`) region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Common outbound connectivity requirements
{: #common-out-reqs-rhcos-tok}

The following network requirements are common for RHCOS hosts in all regions. 

Optional: Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    Allowing access to the NTP servers is optional for RHCOS hosts. You can also define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

## Network requirements for Tokyo (`jp-tok`)
{: #host-out-rhcos-tok}

The following outbound network requirements are specific for RHCOS hosts in the Tokyo (`jp-tok`) region.


Allow control plane worker nodes to communicate with the control plane master 
:    * Destination IP addresses:  161.202.151.170, 128.168.96.130, 165.192.108.34
     * Destination hostnames: `c117.jp-tok.satellite.cloud.ibm.com`, `c117-1.jp-tok.satellite.cloud.ibm.com`, `c117-2.jp-tok.satellite.cloud.ibm.com`, `c117-3.jp-tok.satellite.cloud.ibm.com`, `c117-e.private.jp-tok.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767
     
Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 165.192.69.69, 161.202.126.210, 128.168.71.117
     * Destination hostnames: `origin.jp-tok.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `jp.icr.io`, `registry.au-syd.bluemix.net`, `au.icr.io`
     * Protocol and ports: HTTPS 443
     
Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 161.202.150.66, 128.168.89.146, 165.192.71.226
     * Destination hostnames: `c-01-ws.jp-tok.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.jp-tok.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443
     
:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.

