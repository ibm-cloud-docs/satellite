---


copyright:
  years: 2020, 2024
lastupdated: "2024-02-08"

keywords: satellite, hybrid, multicloud, platform access for satellite, satellite iam access, platform access roles for satellite

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# IAM platform and service access roles
{: #iam-platform-access}

Platform access roles enable users to perform tasks on service resources at the platform level. For example, you can assign user access for the service, create or delete instances, and bind instances to applications. Review the following table for the actions available to platform access roles for {{site.data.keyword.satelliteshort}}. 
{: shortdesc}

You cannot scope access policies to a particular {{site.data.keyword.satelliteshort}} Config **resource**. Instead, scope the policy to the {{site.data.keyword.satellitelong_notm}} service so that users can list {{site.data.keyword.satelliteshort}} Config resources.
{: note}

{{site.data.keyword.satelliteshort}} Config uses a [custom IAM service access role](/docs/account?topic=account-custom-roles), **Deployer**, in addition to the standard **Reader**, **Writer**, and **Manager** roles. You can assign users the **Deployer** role so that they can deploy existing configurations to your clusters, but cannot add or edit the actual configurations for your apps.
{: note}

{{../account/iam-service-roles.md#satellite-roles}}


