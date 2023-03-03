---

copyright:
  years: 2023, 2023
lastupdated: "2023-03-03"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Requred outbound connectivity for Red Hat CoreOS hosts in Dallas (`us-south`)
{: #reqs-host-rhcos-outbound-dal}

Review the following network requirements for outbound connectivity for Red Hat CoreOS (RHCOS) hosts in the Dallas (`us-south`) region.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Common outbound connectivity requirements
{: #common-out-reqs-rhcos-dal}

The following network requirements are common for RHCOS hosts in all regions. 

Optional: Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    Allowing access to the NTP servers is optional for RHCOS hosts. You can also define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.

## Network requirements for Dallas (us-south)
{: #host-out-rhcos-dal}

The following outbound network requirements are specific for RHCOS hosts in the Dallas (us-south) region.

Allow control plane worker nodes to communicate with the control plane master
:    * Destination IP addresses: 169.46.43.146,169.48.236.58,169.60.150.218
     * Destination hostnames: `c131.us-south.satellite.cloud.ibm.com`, `c131-1.us-south.satellite.cloud.ibm.com`, `c131-2.us-south.satellite.cloud.ibm.com`, `c131-3.us-south.satellite.cloud.ibm.com`, `c131-e.us-south.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767


Allow hosts to be attached to a location and assigned to services in the location
:    * Destination IP addresses: 169.46.110.218, 169.47.70.10, 169.62.166.98 
     * Destination hostnames: `origin.us-south.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `us.icr.io`, `registry.bluemix.net`, `registry.ng.bluemix.net`
     * Protocol and ports: HTTPS 443

Allow Link connectors to connect to the Link tunnel server endpoint
:    * Destination IP addresses: 169.48.139.210, 169.48.188.146, 169.59.239.66, 169.60.2.74, 169.61.140.18, 169.61.156.226, 169.61.31.178, 169.61.38.178, 169.62.221.10
     * Destination hostnames: `c-01-ws.us-south.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.us-south.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no more DNS results are returned.

Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443








