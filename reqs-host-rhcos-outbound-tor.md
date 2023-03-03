---

copyright:
  years: 2023, 2023
lastupdated: "2023-03-03"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Red Hat CoreOS hosts in Toronto (`ca-tor`): Requred outbound connectivity
{: #reqs-host-rhcos-outbound-tor}

Review the following network requirements for outbound connectivity for Red Hat CoreOS (RHCOS) hosts in the Toronto (`ca-tor`) region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Common outbound connectivity requirements
{: #common-out-reqs-rhcos-tor}

The following network requirements are common for RHCOS hosts in all regions. 

Optional: Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    Allowing access to the NTP servers is optional for RHCOS hosts. You can also define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

## Network requirements for Toronto (`ca-tor`)
{: #host-out-rhcos-tor}

The following outbound network requirements are specific for RHCOS hosts in the Toronto (`ca-tor`) region.


Allow control plane worker nodes to communicate with the control plane master 
:    * Destination IP addresses:  169.55.168.210, 163.74.69.194, 163.75.72.2
     * Destination hostnames: `c114.ca-tor.satellite.cloud.ibm.com`, `c114-1.ca-tor.satellite.cloud.ibm.com`, `c114-2.ca-tor.satellite.cloud.ibm.com`, `c114-3.ca-tor.satellite.cloud.ibm.com`, `c114-e.ca-tor.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767

Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 163.75.64.114, 163.74.65.18, 158.85.65.194
     * Destination hostnames: `origin.ca-tor.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `ca.icr.io`, `us.icr.io`, `registry.ng.bluemix.net`
     * Protocol and ports: HTTPS 443
     
Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 163.74.67.114, 163.75.70.74, 158.85.79.18
     * Destination hostnames: `c-01-ws.ca-tor.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.ca-tor.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443
     
:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.    


