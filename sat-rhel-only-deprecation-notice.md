---
copyright:
  years: 2025
lastupdated: "2026-10-06"

keywords: satellite, rhel, coreos, deprecation, location, important notice

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Deprecation of RHEL-only location creation
{: #satellite-rhel-only-deprecation-notice}

Creation of new RHEL hosts only (non-Red Hat CoreOS enabled) {{site.data.keyword.satellitelong_notm}} locations is deprecated and will become unsupported 30 days after this announcement.
{: shortdesc}

## What is changing?
{: #rhel-only-deprecation-changes}

- After the deprecation deadline, it will no longer be possible to create a new RHEL hosts only Satellite location.
- All newly created Satellite locations will be **Red Hat CoreOS enabled** by default.
- **Existing RHEL hosts only locations are not affected** — they will continue to operate and receive support as normal.

Red Hat CoreOS enabled locations accept both RHEL and Red Hat CoreOS hosts. While you can continue attaching RHEL hosts to a Red Hat CoreOS enabled location, note that RHEL as a supported host operating system for Satellite locations and OpenShift clusters running on them is planned for deprecation in the future. Consider Red Hat CoreOS hosts when setting up new Red Hat CoreOS enabled locations to ensure long-term supportability and alignment with the platform direction.

## What actions do I need to take?
{: #rhel-only-deprecation-actions}

1. Review the [planning your environment for Satellite locations](/docs/satellite?topic=satellite-infrastructure-plan) documentation to understand Red Hat CoreOS enabled locations and how to set them up.
1. Review the [host system requirements](/docs/satellite?topic=satellite-host-reqs) to understand what operating systems are supported in each location type.

## Which regions are impacted?
{: #rhel-only-deprecation-regions}

This deprecation applies to all {{site.data.keyword.satellitelong_notm}} regions: Dallas, Frankfurt, London, Madrid, Osaka, São Paulo, Sydney, Tokyo, Toronto, and Washington D.C.

## Getting help
{: #rhel-only-deprecation-help}

For assistance, open a support case in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){: external}.
