---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-10"

keywords: satellite, hybrid, multicloud, pricing, service, billing, charges

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Pricing 
{: #sat-pricing}

{{site.data.keyword.satellitelong_notm}} provides a convenient way for you to consume {{site.data.keyword.cloud_notm}} services in any location that you want, with visibility across your locations.
{: shortdesc}

Flexible consumption
:    No charges are incurred for hosts that are attached to a location, but not assigned to a resource. You can have as many hosts waiting in your location without being charged for future growth. As soon as you unassign a host from a resource, you are no longer charged for that host. Keep in mind that hosts might be automatically assigned to services, depending on your setup.

Application and networking capabilities at no additional charge
:   You do not have separate charges for {{site.data.keyword.satelliteshort}} management capabilities for the locations, hosts, such as endpoints, configuration versions and subscriptions, or other {{site.data.keyword.satelliteshort}} resources.

## {{site.data.keyword.satelliteshort}} locations
{: #pricing-satloc}

Review the following table for pricing details. For more information, see the detailed [Pricing model](https://www.ibm.com/cloud/satellite/pricing){: external}

| Type of charge | Locations created after 15 November 2022 | Locations created before 15 November 2022 | What the charge covers |
| --- | --- | --- | --- |
| Location management fee | A flat monthly fee for the location, charged hourly. | Per vCPU hour of the hosts that are attached to the location. | The benefits of {{site.data.keyword.satellitelong_notm}}, such as to create the cluster on any compatible infrastructure that you want; tooling to consistently deploy apps, storage drivers, and endpoints across the location; integration with {{site.data.keyword.cloud_notm}} platform tooling such as IAM; continuous monitoring by {{site.data.keyword.IBM_notm}} Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more.  |
| Infrastructure fee | Varies by provider | Varies by provider | The underlying infrastructure that you bring to {{site.data.keyword.satelliteshort}} is your own, and might have its own charges. Consult your infrastructure provider for more details, such as about the storage, compute, and networking of the hosts in a cloud or on-prem environment. |
{: caption="{{site.data.keyword.satelliteshort}} location control plane charges." caption-side="bottom"}

## {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services
{: #pricing-services}

Each {{site.data.keyword.cloud_notm}} service instance that you create in your {{site.data.keyword.satelliteshort}} location incurs charges. For more information, see [Supported {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services).

{{site.data.content.cost-estimate}}

{{site.data.content.usage}}

