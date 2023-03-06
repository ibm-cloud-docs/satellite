---

copyright:
  years: 2023, 2023
lastupdated: "2023-03-06"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# London RHCOS host requirements
{: #reqs-host-rhcos-outbound-lon}

Review the following network requirements for outbound connectivity for Red Hat CoreOS (RHCOS) hosts for Red Hat CoreOS enabled locations in the London (`eu-gb`) region. 
{: shortdesc}

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location).


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Common outbound connectivity requirements
{: #common-out-reqs-rhcos-lon}

The following network requirements are common for RHCOS hosts in all regions. 

Optional: Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    Allowing access to the NTP servers is optional for RHCOS hosts. You can also define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

## Network requirements for London (`eu-gb`)
{: #host-out-rhcos-lon}

The following outbound network requirements are specific for RHCOS hosts in the London (`eu-gb`) region.


Allow control plane worker nodes to communicate with the control plane master
:    * Destination IP addresses: 158.175.113.26,141.125.85.26,158.176.90.58 
     * Destination hostnames:  `c116.eu-gb.satellite.cloud.ibm.com`, `c116-1.eu-gb.satellite.cloud.ibm.com`, `c116-2.eu-gb.satellite.cloud.ibm.com`, `c116-3.eu-gb.satellite.cloud.ibm.com`, `c116-e.eu-gb.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767
     
Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 159.122.224.242, 158.175.65.170, 158.176.95.146
     * Destination hostnames: `origin.eu-gb.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A 
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `uk.icr.io`, `registry.eu-gb.bluemix.net`
     * Protocol and ports: HTTPS 443
     
Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 158.175.130.138, 141.125.87.226, 158.176.74.242
     * Destination hostnames: `c-01-ws.eu-gb.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443
     
:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.eu-gb.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned.
     
 Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443
     
:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.
     
