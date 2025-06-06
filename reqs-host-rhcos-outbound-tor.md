---


copyright:
  years: 2023, 2025
lastupdated: "2025-06-05"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# RHCOS enabled locations in Toronto
{: #reqs-host-rhcos-outbound-tor}


The following network requirements are for outbound connectivity for Red Hat Enterprise Linux (RHEL) and Red Hat CoreOS (RHCOS) hosts for use with Red Hat CoreOS enabled locations in the Toronto (`ca-tor`) region. 
{: shortdesc}

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location). For more information about operating system support, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).


You can verify your host setup with the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


You can [download a copy of these requirements](https://cloud.ibm.com/media/docs/downloads/satellite/rhcos-toronto.csv){: external}.
{: tip}




Review the following outbound network requirements for RHEL and RHCOS hosts for use with RHCOS enabled locations in the Toronto (`ca-tor`) region.

Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers.
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    If you don't want to use {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers, you can instead define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry.
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.


Allow control plane nodes to communicate with the management plane.
:    * Destination IP addresses:  169.55.168.210, 163.74.69.194, 163.75.72.2
     * Destination hostnames: `c114.ca-tor.satellite.cloud.ibm.com`, `c114-1.ca-tor.satellite.cloud.ibm.com`, `c114-2.ca-tor.satellite.cloud.ibm.com`, `c114-3.ca-tor.satellite.cloud.ibm.com`, `c114-e.ca-tor.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767

Allow hosts to be attached to a location and assigned to services in the location.
:    * Destination IP addresses: 163.75.64.114, 163.74.65.18, 158.85.65.194, 104.94.220.132, 104.94.221.132, 104.94.222.140, 104.94.223.140, 104.96.176.132, 104.96.177.132, 104.96.178.134, 104.96.179.134, 104.96.180.131, 104.96.181.131
     * Destination hostnames: `origin.ca-tor.containers.cloud.ibm.com` and `bootstrap.ca-tor.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}.
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `ca.icr.io`, `us.icr.io`, `registry.ng.bluemix.net`
     * Protocol and ports: HTTPS 443
     
Allow Link tunnel clients to connect to the Link tunnel server endpoint. {: #link-connector-tor}
:    * Destination IP addresses: 163.74.67.114, 163.75.70.74, 158.85.79.18
     * Destination hostnames: `c-01-ws.ca-tor.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.ca-tor.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. 

Optional: Allow hosts to communicate with {{site.data.keyword.logs_full_notm}}.
:    * Destination IP addresses and hostnames: [{{site.data.keyword.logs_full_notm}} endpoints](/docs/cloud-logs?topic=cloud-logs-vpe-connection&interface=cli)
     * Protocol and ports: HTTPS 443

:    If you plan to use {{site.data.keyword.logs_full_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}.
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443

:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.   
