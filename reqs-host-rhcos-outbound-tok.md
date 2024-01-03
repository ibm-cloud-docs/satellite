---


copyright:
  years: 2023, 2024
lastupdated: "2024-01-03"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# RHCOS enabled locations in Tokyo 
{: #reqs-host-rhcos-outbound-tok}

The following network requirements are for outbound connectivity for Red Hat Enterprise Linux (RHEL) and Red Hat CoreOS (RHCOS) hosts for use with Red Hat CoreOS enabled locations in the Tokyo (`jp-tok`) region. 
{: shortdesc}

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location). For more information about operating system support, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).


You can verify your host setup with the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


You can [download a copy of these requirements](https://cloud.ibm.com/media/docs/downloads/satellite/rhcos-tokyo.csv){: external}.
{: tip}




Review the following outbound network requirements for RHEL and RHCOS hosts for use with RHCOS enabled locations in the Tokyo (`jp-tok`) region.
{: #host-out-rhcos-tok}

Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers.
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    If you don't want to use {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers, you can instead define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Allow hosts to communicate with Red Hat Container Registry.
:    Allow your hosts to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.


Allow control plane nodes to communicate with the management plane.
:    * Destination IP addresses:  161.202.151.170, 128.168.96.130, 165.192.108.34
     * Destination hostnames: `c117.jp-tok.satellite.cloud.ibm.com`, `c117-1.jp-tok.satellite.cloud.ibm.com`, `c117-2.jp-tok.satellite.cloud.ibm.com`, `c117-3.jp-tok.satellite.cloud.ibm.com`, `c117-e.private.jp-tok.satellite.cloud.ibm.com`
     * Protocol and ports: TCP 30000 - 32767
     
Allow hosts to be attached to a location and assigned to services in the location.
:    * Destination IP addresses: 165.192.69.69, 161.202.126.210, 128.168.71.117, 104.94.220.124, 104.94.221.124, 104.94.222.132, 104.94.223.132, 104.96.176.124, 104.96.177.124, 104.96.178.126, 104.96.179.126, 104.96.180.123, 104.96.181.123
     * Destination hostnames: `origin.jp-tok.containers.cloud.ibm.com` and `bootstrap.jp-tok.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}.
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `jp.icr.io`, `registry.au-syd.bluemix.net`, `au.icr.io`
     * Protocol and ports: HTTPS 443
     
Allow Link tunnel clients to connect to the Link tunnel server endpoint. {: #link-connector-tok}
:    * Destination IP addresses: 161.202.150.66, 128.168.89.146, 165.192.71.226
     * Destination hostnames: `c-01-ws.jp-tok.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

:    You can find the hostnames or IP addresses by running the `dig c-<XX>-ws.jp-tok.link.satellite.cloud.ibm.com +short` command. Replace `<XX>` with `01`, `02`, and so on, until no DNS results are returned. 


Optional: Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}.
:    * Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
     * Protocol and ports: HTTPS 443

:    If you plan to use {{site.data.keyword.loganalysislong_notm}} in your {{site.data.keyword.openshiftshort}}  {{site.data.keyword.satelliteshort}} clusters, then include these network options.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}.
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443

:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.


