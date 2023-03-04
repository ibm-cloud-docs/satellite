---

copyright:
  years: 2023, 2023
lastupdated: "2023-03-04"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Red Hat CoreOS hosts in Sao Paulo
{: #reqs-host-rhcos-outbound-sao}

Review the following network requirements for outbound connectivity for Red Hat CoreOS (RHCOS) hosts in the Sao Paulo (`br-sao`) region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Common outbound connectivity requirements
{: #common-out-reqs-rhcos-sao}

The following network requirements are common for RHCOS hosts in all regions. 

Optional: Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    Allowing access to the NTP servers is optional for RHCOS hosts. You can also define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

## Network requirements for Sao Paulo (`br-sao`)
{: #host-out-rhcos-sao}

The following outbound network requirements are specific for RHCOS hosts in the Sao Paulo (`br-sao`) region.


Allow control plane worker nodes to communicate with the control plane master 
:    * Destination IP addresses:  169.57.165.138,163.107.73.50,163.109.72.194
     * Destination hostnames: `c113.br-sao.satellite.cloud.ibm.com`, `c113-1.br-sao.satellite.cloud.ibm.com`, `c113-2.br-sao.satellite.cloud.ibm.com`, `c113-3.br-sao.satellite.cloud.ibm.com`, `c113-e.br-sao.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767

Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 169.57.161.130, 163.109.65.146, 163.107.65.74
     * Destination hostnames: `origin.br-sao.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443
     
Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `br.icr.io`, `us.icr.io`, `registry.ng.bluemix.net`
     * Protocol and ports: HTTPS 443
     
Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 163.107.69.114, 163.109.70.234, 169.57.155.74 
     * Destination hostnames: `c-01-ws.br-sao.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.br-sao.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443

:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.


