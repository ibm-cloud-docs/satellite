---


copyright:
  years: 2020, 2026
lastupdated: "2026-08-28"

keywords: satellite, hybrid, multicloud, platform access for satellite, satellite iam access, platform access roles for satellite

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Identity and Access Management (IAM) platform and service access roles
{: #iam-platform-access}

Platform access roles allow users to perform tasks such as managing instances and assigning access. Review the available actions for each role in {{site.data.keyword.satelliteshort}}.
{: shortdesc}

You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}

{{site.data.keyword.satelliteshort}} Config uses a [custom IAM service access role](/docs/iam?topic=iam-custom-roles), **Deployer**, in addition to the standard **Reader**, **Writer**, and **Manager** roles. You can assign users the **Deployer** role so that they can deploy existing configurations to your clusters, but cannot add or edit the actual configurations for your apps.
{: note}



{{../iam/iam-service-roles.md#satellite-roles}}
