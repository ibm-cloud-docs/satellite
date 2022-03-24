---

copyright:
  years: 2022, 2022
lastupdated: "2022-03-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Required outbound connectivity for hosts in all regions
{: #reqs-host-network-outbound}

Review the following outbound connectivity requirements for hosts in all regions.
{: shortdesc}

In addition to the following general outbound connectivity requirements, hosts must also meet the regional outbound connectivity requirements for the region your location is in.
{: important}




Allow hosts to connect to {{site.data.keyword.IBM_notm}}.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination**: `cloud.ibm.com`, `containers.cloud.ibm.com`
:   **Protocol and ports**: Port 443

Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination**: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
:   **Protocol and ports**: NTP protocol and UDP port 123

Allow hosts to communicate with {{site.data.keyword.iamshort}}. Your firewall must be Layer 7 to allow the IAM domain name. IAM does not have specific IP addresses that you can allow. If your firewall does not support Layer 7, you can allow all HTTPS network traffic on port 443.
:   **Source**: All {{site.data.keyword.satelliteshort}} hosts
:   **Destination** `https://iam.bluemix.net`, `https://iam.cloud.ibm.com`
:   **Protocol and ports**: TCP 443

Allow hosts to connect to the LaunchDarkly service.
:   **Source**: Control plane hosts
:   **Destination**: `app.launchdarkly.com`,`clientstream.launchdarkly.com`
:   **Protocol and ports**: HTTPS 443

Allow hosts to communicate with Red Hat Container Registry.
:   See [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.















