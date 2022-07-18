---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-18"

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


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


## Allow hosts to connect to {{site.data.keyword.IBM_notm}}
{: #host-out-ibm}

Allow the following hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination hostnames: `cloud.ibm.com`, `containers.cloud.ibm.com`
* Protocol and ports: HTTPS Port 443

## Allow access to {{site.data.keyword.redhat_notm}} network time protocol (NTP) servers
{: #host-out-ntp}

Allow the following hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.
* Destination hostnames: `0.rhel.pool.ntp.org`, `1.rhel.pool.ntp.org`, `2.rhel.pool.ntp.org`, `3.rhel.pool.ntp.org`
* Protocol and ports: Allow NTP protocol and provide UDP on port 123

Note that allowing access to the NTP servers is optional. You can also define a custom NTP server for your RHCOS hosts. For more information, see the [Specifying a custom Network Time Protocol (NTP) server](/docs/satellite?topic=satellite-config-custom-ntp).

## Allow hosts to communicate with {{site.data.keyword.iamshort}}
{: #host-out-iam}

Allow the following hostnames, protocols, and ports for all {{site.data.keyword.satelliteshort}} hosts.

Your firewall must be Layer 7 to allow the IAM domain name. IAM does not have specific IP addresses that you can allow. If your firewall does not support Layer 7, you can allow all HTTPS network traffic on port 443.
{: note}

* Destination hostnames: `https://iam.bluemix.net`, `https://iam.cloud.ibm.com`
* Protocol and ports: TCP 443

## Allow hosts to connect to the LaunchDarkly service
{: #host-out-ld}

Allow the following hostnames, protocols, and ports for Control plane hosts.
* Destination hostnames: `app.launchdarkly.com`,`clientstream.launchdarkly.com`
* Protocol and ports: HTTPS 443

## Allow hosts to communicate with Red Hat Container Registry
{: #host-out-cr}

See [Configuring your firewall](https://docs.openshift.com/container-platform/4.8/installing/install_config/configuring-firewall.html){: external}.













