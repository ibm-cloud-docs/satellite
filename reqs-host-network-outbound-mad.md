---

copyright:
  years: 2023, 2023
lastupdated: "2023-10-27"

keywords: satellite, requirements, outbound, network, allowlist, connectivity, firewall, madrid

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Non-RHCOS enabled locations in Madrid
{: #reqs-host-network-outbound-mad}

The following network requirements are for outbound connectivity for Red Hat Enterprise Linux (RHEL) hosts for use with non Red Hat CoreOS enabled locations in the Madrid (`eu-es`) region. 
{: shortdesc}

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location). For more information about operating system support, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).


You can verify your host setup with the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


You can [download a copy of these requirements](https://cloud.ibm.com/media/docs/downloads/satellite/non-rhcos-madrid.csv){: external}.
{: tip}




Review the following outbound network requirements for RHEL hosts for use with non-RHCOS enabled locations in the Madrid (`eu-es`) region.

Allow hosts to connect to {{site.data.keyword.IBM_notm}}.
:    * Destination hostnames: `cloud.ibm.com`, `containers.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS Port 443

Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers.
:    * Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
     * Protocol and ports: Allow NTP protocol and provide UDP on port 123.

Allow hosts to communicate with {{site.data.keyword.iamshort}}.
:    * Destination hostnames: `https://iam.bluemix.net`, `https://iam.cloud.ibm.com`
     * Protocol and ports: TCP 443
     
:    Your firewall must be Layer 7 to allow the IAM domain name. IAM does not have specific IP addresses that you can allow. If your firewall does not support Layer 7, you can allow all HTTPS network traffic on port 443.

Allow hosts to connect to the LaunchDarkly service.
:    * Destination hostnames: `app.launchdarkly.com`,`clientstream.launchdarkly.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with Red Hat Container Registry.
:    Allow your host machines to access the required sites for OpenShift Container Platform. For more information, see [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.


Allow control plane nodes to communicate with the management plane.
:    * Destination IP addresses: 13.120.67.114 13.121.67.98 13.122.67.106
     * Destination hostnames: `c103.eu-es.satellite.cloud.ibm.com`, `c103-1.eu-es.satellite.cloud.ibm.com`, `c103-2.eu-es.satellite.cloud.ibm.com`, `c103-3.eu-es.satellite.cloud.ibm.com`, `c103-e.eu-es.satellite.cloud.ibm.com`
     * Protocol and ports: Protocol and ports: TCP 30000 - 32767 and UDP 30000 - 32767

Allow control plane nodes to back up control plane etcd data to {{site.data.keyword.cos_full_notm}}.
:    * Destination IP addresses: N/A
     * Destination hostnames: `s3.eu-es.cloud-object-storage.appdomain.cloud` and `*.s3.eu-es.cloud-object-storage.appdomain.cloud`
     * Protocol and ports: HTTPS 443



Allow continuous delivery of updates to platform components.
:    * Destination IP addresses: N/A
     * Destination hostnames: `s3.us.cloud-object-storage.appdomain.cloud` and `*.s3.us.cloud-object-storage.appdomain.cloud`
     * Protocol and ports: HTTPS 443

Allow Link tunnel clients to connect to the Link tunnel server endpoint.
:    * Destination IP addresses: 13.120.67.106, 13.121.67.82, 13.122.67.186
     * Destination hostnames: `c-01-ws.eu-es.link.satellite.cloud.ibm.com`, `api.link.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443
     
:    You can find the hostnames or IP addresses by running the dig c-XX-ws.eu-es.link.satellite.cloud.ibm.com +short command. Replace with 01, 02, and so on, until no DNS results are returned.

Allow hosts to be attached to a location and assigned to services in the location.
:    * Destination IP addresses: 13.120.127.26, 13.121.64.26, 13.122.64.138, 2.18.48.89, 2.18.49.89, 2.18.50.89, 2.18.51.89, 2.18.52.89, 2.18.53.89, 2.18.54.89, 2.18.55.89, 23.40.100.89, 23.7.244.89
     * Destination hostnames: `origin.eu-es.containers.cloud.ibm.com` and `bootstrap.eu-es.containers.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow Akamai proxied load balancers for {{site.data.keyword.satelliteshort}} Config and Link API.
:    * Destination IP addresses: [Akamai's source IP addresses](https://github.com/IBM-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external}.
     * Destination hostnames: `api.eu-es.link.satellite.cloud.ibm.com`, `config.eu-es.satellite.cloud.ibm.com`, `eu-es.containers.cloud.ibm.com`, `config.satellite.cloud.ibm.com`
     * Protocol and ports: HTTPS 443

Allow hosts to communicate with {{site.data.keyword.registrylong_notm}}.
:    * Destination IP addresses: N/A
     * Destination hostnames: `icr.io`, `registry.bluemix.net`, `de.icr.io`, `registry.eu-de.bluemix.net`
     * Protocol and ports: HTTPS 443

Optional: Allow hosts to communicate with {{site.data.keyword.loganalysislong_notm}}.
:    * Destination IP addresses and hostnames: [{{site.data.keyword.loganalysislong_notm}} endpoints](/docs/log-analysis?topic=log-analysis-endpoints#endpoints_api_public)
     * Protocol and ports: HTTPS 443

:    If you plan to use {{site.data.keyword.loganalysislong_notm}} in your {{site.data.keyword.openshiftshort}}  {{site.data.keyword.satelliteshort}} clusters, then include these network options.

Optional: Allow hosts to communicate with {{site.data.keyword.monitoringlong_notm}}.
:    * Destination IP addresses and hostnames: [{{site.data.keyword.monitoringshort_notm}} endpoints](/docs/monitoring?topic=monitoring-endpoints)
     * Protocol and ports: HTTPS 443 and 6443

:    If you plan to use {{site.data.keyword.monitoringshort_notm}} in your {{site.data.keyword.openshiftshort}} {{site.data.keyword.satelliteshort}} clusters, then include these network options.




