---

copyright:
  years: 2020, 2022
lastupdated: "2022-08-09"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Common permissions in other cloud providers
{: #iam-common}

To create and manage the underlying infrastructure in other cloud providers, you must have the appropriate permissions. Review some commonly required permissions. For more information, consult your cloud provider's documentation.
{: shortdesc}

## Alibaba permissions
{: #permissions-alibaba}

To allow users in Alibaba to do various actions for {{site.data.keyword.satelliteshort}}, you can grant the users access to the **AliyunECSFullAccess** security policy or create a custom security policy that allows access to only specific instances. For more information about the permissions of this role, see the [Alibaba documentation](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/ram-overview){: external}.
{: shortdesc}

## AWS permissions
{: #permissions-aws}

Review the following example policies that you might give users in AWS to do various actions for {{site.data.keyword.satelliteshort}}. If you want to further restrict permissions, consult the AWS documentation.
{: shortdesc}

### Manually creating a {{site.data.keyword.satelliteshort}} location in AWS
{: #permissions-aws-manual}

- `AmazonEC2FullAccess`
- `AmazonElasticFileSystemFullAccess`
- `AmazonVPCFullAccess`
- `AWSMarketplaceFullAccess`

### Automatically creating a {{site.data.keyword.satelliteshort}} location from a {{site.data.keyword.bpshort}} template in AWS
{: #permissions-aws-auto}

- `AmazonEC2FullAccess`
- `AmazonElasticFileSystemFullAccess`
- `AmazonSSMFullAccess`
- `AmazonVPCFullAccess`
- `AWSMarketplaceFullAccess`
- `IAMFullAccess`

## Azure permissions
{: #permissions-azure}

To allow users in Microsoft Azure to do various actions for {{site.data.keyword.satelliteshort}}, you can grant the users the general **Contributor** built-in role in Azure role-based access control. For more information about further restricting permissions, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles){: external}.
{: shortdesc}

## Google Cloud Platform permissions
{: #permissions-gcp}

To allow users in Google Cloud Platform to do various actions for {{site.data.keyword.satelliteshort}}, you can grant the users the **Editor** role to the project in GCP IAM. For more information about the permissions of this role, see the [GCP documentation](https://cloud.google.com/iam/docs/permissions-reference){: external}.
{: shortdesc}


