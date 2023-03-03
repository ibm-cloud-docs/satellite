---

copyright:
  years: 2023, 2023
lastupdated: "2023-03-03"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Red Hat CoreOS hosts in Sydney (`au-syd`): Requred outbound connectivity
{: #reqs-host-rhcos-outbound-syd}

Review the following network requirements for outbound connectivity for Red Hat CoreOS (RHCOS) hosts in the Sydney (`au-syd`) region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Common outbound connectivity requirements
{: #common-out-reqs-rhcos-syd}

The following network requirements are common for RHCOS hosts in all regions. 

Optional: Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    Allowing access to the NTP servers is optional for RHCOS hosts. You can also define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

## Network requirements for Sydney (`au-syd`)
{: #host-out-rhcos-syd}

The following outbound network requirements are specific for RHCOS hosts in the Sydney (`au-syd`) region.


Allow control plane worker nodes to communicate with the control plane master 
:    * Destination IP addresses:  168.1.27.26,130.198.65.146,135.90.87.90
     * Destination hostnames: `c114.au-syd.satellite.cloud.ibm.com`, `c114-1.au-syd.satellite.cloud.ibm.com`, `c114-2.au-syd.satellite.cloud.ibm.com`, `c114-3.au-syd.satellite.cloud.ibm.com`, `c114-e.au-syd.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767
     
Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 130.198.66.26, 135.90.69.66, 168.1.8.195
     * Destination hostnames: `origin.au-syd.containers.cloud.ibm.com` 
     * Protocol and ports: HTTPS 443     

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `au.icr.io`, `registry.au-syd.bluemix.net`
     * Protocol and ports: HTTPS 443

Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 130.198.75.74, 135.90.67.154, 168.1.201.194
     * Destination hostnames: `c-01-ws.au-syd.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.au-syd.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443
     
:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.



     
