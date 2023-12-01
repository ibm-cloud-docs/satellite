---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Common permissions in other cloud providers
{: #iam-common}

To create and manage the underlying infrastructure in other cloud providers, you must have the appropriate permissions. Review some commonly required permissions. For more information, consult your cloud provider's documentation.
{: shortdesc}



## AWS permissions
{: #permissions-aws}

When you use an [{{site.data.keyword.bplong}} template](/docs/satellite?topic=satellite-loc-aws-create-auto) to create your {{site.data.keyword.satelliteshort}} location, you must be assigned a role that can create virtual instances and networks in AWS. For example, you can be assigned the [**AmazonEC2FullAccess** built-in role](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonEC2FullAccess.html){: external} in AWS. For more information about other built-in roles, see the [AWS documentation](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/policy-list.html){: external}.

## Azure permissions
{: #permissions-azure}

When you use an [{{site.data.keyword.bplong}} template](/docs/satellite?topic=satellite-loc-azure-create-auto) to create your {{site.data.keyword.satelliteshort}} location, you must be assigned a role that can create virtual instances and networks in Microsoft Azure. For example, you can be assigned the [**Contributor** built-in role](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor){: external} in Azure. For more information about other built-in roles, see the [Azure documentation](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles){: external}.


## Google Cloud Platform permissions
{: #permissions-gcp}

When you use an [{{site.data.keyword.bplong}} template](/docs/satellite?topic=satellite-loc-gcp-create-auto) to create your {{site.data.keyword.satelliteshort}} location, you must be assigned a role that can create virtual instances and networks in Google Cloud Platform. For example, you can be assigned the [**Cloud Build Editor**](https://cloud.google.com/iam/docs/understanding-roles#cloudbuild.builds.editor){: external} role in a specific project in GCP IAM. For more information about role permissions in GCP, see the [GCP documentation](https://cloud.google.com/iam/docs/permissions-reference){: external}.




## VMware permissions
{: #permissions-vmware}

When you use an [{{site.data.keyword.bplong}} template](/docs/satellite?topic=satellite-loc-vmware-create-auto) to create your {{site.data.keyword.satelliteshort}} location, you must be assigned a role that can create virtual instances and networks in VMware. For example, you can be assigned the **Administrator** role for the VMware vSphere vCenter servers. For more information about role permissions in VMWare, see the [VMware documentation]https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-93B962A7-93FA-4E96-B68F-AE66D3D6C663.html){: external}.
{: shortdesc}


