---


copyright:
  years: 2020, 2026
lastupdated: "2026-08-14"

keywords: satellite, hybrid, multicloud, platform access for satellite, satellite iam access, platform access roles for satellite

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# IAM platform and service access roles
{: #iam-platform-access}

Platform access roles enable users to perform tasks on service resources at the platform level.
{: shortdesc}

You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}

{{site.data.keyword.satelliteshort}} Config uses a [custom IAM service access role](/docs/account?topic=account-custom-roles-access), **Deployer**, in addition to the standard **Reader**, **Writer**, and **Manager** roles. You can assign users the **Deployer** role so that they can deploy existing configurations to your clusters, but cannot add or edit the actual configurations for your apps.
{: note}



{{../iam/iam-service-roles.md#satellite-roles}}
