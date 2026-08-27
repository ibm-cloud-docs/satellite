---


copyright:
  years: 2022, 2026
lastupdated: "2026-08-27"

keywords: satellite, hybrid, multicloud, requirements, outbound, network, allowlist, IBM Cloud Satellite, host network requirements, outbound connectivity

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Outbound connectivity requirements for {{site.data.keyword.satelliteshort}} hosts
{: #reqs-host-network-outbound}

Understand the outbound connectivity requirements for hosts in {{site.data.keyword.satellitelong_notm}}, including RHCOS and non-RHCOS locations, and how to verify host network setup.
{: shortdesc}

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location). For more information about operating system support, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).




You can verify your host setup with the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}

Outbound connectivity host requirements for Non-RHCOS locations
- [Non-RHCOS locations in Dallas](/docs/satellite?topic=satellite-reqs-host-network-outbound-dal).
- [Non-RHCOS locations in Frankfurt](/docs/satellite?topic=satellite-reqs-host-network-outbound-fra).
- [Non-RHCOS locations in London](/docs/satellite?topic=satellite-reqs-host-network-outbound-lon).
- [Non-RHCOS locations in Madrid](/docs/satellite?topic=satellite-reqs-host-network-outbound-mad).
- [Non-RHCOS locations in Osaka](/docs/satellite?topic=satellite-reqs-host-network-outbound-osa).
- [Non-RHCOS locations in Sao Paulo](/docs/satellite?topic=satellite-reqs-host-network-outbound-sao).
- [Non-RHCOS locations in Sydney](/docs/satellite?topic=satellite-reqs-host-network-outbound-syd).
- [Non-RHCOS locations in Tokyo](/docs/satellite?topic=satellite-reqs-host-network-outbound-tok).
- [Non-RHCOS locations in Toronto](/docs/satellite?topic=satellite-reqs-host-network-outbound-tor).
- [Non-RHCOS locations in Washington D.C.](/docs/satellite?topic=satellite-reqs-host-network-outbound-wdc)




Outbound connectivity requirements for Red Hat CoreOS (RHCOS) locations
- [CoreOS enabled locations in Dallas](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-dal).
- [CoreOS enabled locations in Frankfurt](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-fra).
- [CoreOS enabled locations in London](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-lon).
- [CoreOS enabled locations in Madrid](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-mad).
- [CoreOS enabled locations in Osaka](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-osa).
- [CoreOS enabled locations in Sao Paulo](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-sao).
- [CoreOS enabled locations in Sydney](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-syd).
- [CoreOS enabled locations in Tokyo](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-tok).
- [CoreOS enabled locations in Toronto](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-tor).
- [CoreOS enabled locations in Washington D.C.](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-wdc)





Outbound connectivity requirements for Red Hat CoreOS (RHCOS) locations with reduced firewall
- [CoreOS enabled locations with reduced firewall in Dallas](/docs/satellite?topic=satellite-req-minimum-outbound-dal).
- [CoreOS enabled locations with reduced firewall in Frankfurt](/docs/satellite?topic=satellite-req-minimum-outbound-fra).
- [CoreOS enabled locations with reduced firewall in London](/docs/satellite?topic=satellite-req-minimum-outbound-lon).
- [CoreOS enabled locations with reduced firewall in Madrid](/docs/satellite?topic=satellite-req-minimum-outbound-mad).
- [CoreOS enabled locations with reduced firewall in Osaka](/docs/satellite?topic=satellite-req-minimum-outbound-osa).
- [CoreOS enabled locations with reduced firewall in Sao Paulo](/docs/satellite?topic=satellite-req-minimum-outbound-sao).
- [CoreOS enabled locations with reduced firewall in Sydney](/docs/satellite?topic=satellite-req-minimum-outbound-syd).
- [CoreOS enabled locations with reduced firewall in Tokyo](/docs/satellite?topic=satellite-req-minimum-outbound-tok).
- [CoreOS enabled locations with reduced firewall in Toronto](/docs/satellite?topic=satellite-req-minimum-outbound-tor).
- [CoreOS enabled locations with reduced firewall in Washington D.C.](/docs/satellite?topic=satellite-req-minimum-outbound-wdc)
