---


copyright:
  years: 2023, 2024
lastupdated: "2024-01-03"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, rhcos

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

  
# RHCOS enabled locations with reduced firewall in London
{: #req-minimum-outbound-lon}
  
Review the following network requirements for outbound connectivity for hosts in a minimum internet access location in the London (`eu-gb`) region. Because this type of location requires a single network destination instead of multiple destinations, it reduces the number of outbound IP addresses that you must allow from your firewall. For more information, see [Creating Red Hat CoreOS enabled Locations with reduced firewall footprint](/docs/satellite?topic=satellite-coreos-reduced-firewall).
{: shortdesc}


You can verify your host setup with the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}

  
The following outbound network requirements are specific for hosts in the London (`eu-gb`) region.

     
Allow Link tunnel clients to connect to the Link tunnel server endpoint.
:    * Destination IP addresses: 158.175.130.138, 141.125.87.226, 158.176.74.242
     * Destination hostnames: `c-01-ws.eu-gb.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443
  
Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers.
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123
     
:    If you don't want to use {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers, you can instead define a [custom NTP server for your RHCOS hosts](/docs/satellite?topic=satellite-config-custom-ntp).

Optional:  Allow hosts to connect to HPCS for encrypting cluster secrets.
:    * Domain: `api.eu-gb.hs-crypto.cloud.ibm.com`
     * Port: 8000-19999 

:    If you have a preconfigured set of instances, you can find the assigned port to your instance in the overview page and allowlist just that port on the domain.
  
For access to services such as {{site.data.keyword.loganalysislong_notm}} or {{site.data.keyword.monitoringlong_notm}}, you must add the outbound access for them. For more information, see [RHCOS enabled locations in London](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-lon).
  

  


