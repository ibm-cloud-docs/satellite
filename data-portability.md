---

copyright:
  years: 2024, 2024
lastupdated: "2024-11-07"

keywords: data, portability

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding data portability for Satellite
{: #data-portability}

[Data Portability](#x2113280){: term} involves a set of tools and procedures that enable customers to export the digital artifacts that are needed to implement similar workload and data processing on different service providers or on-premises software. It includes procedures for copying and storing the service customer content, including the related configuration that is used by the service to store and process the data, on the customer’s own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.Bluemix_notm}} services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer is responsible for the use of the exported data and configuration for data portability to other infrastructures, which includes:

- The planning and execution for setting up alternative infrastructure on different cloud providers or on-premises software that provide similar capabilities to the {{site.data.keyword.IBM_notm}} services.
- The planning and execution for the porting of the required application code on the alternative infrastructure, including the adaptation of customer's application code, deployment automation, and so on.
- The conversion of the exported data and configuration to the format that's required by the alternative infrastructure and adapted applications.

For more information, see [Your responsibilities with Satellite](/docs/satellite?topic=satellite-responsibilities).


## Data export procedures
{: #data-portability-procedures}

Satellite provides mechanisms to export your content that was uploaded, stored, and processed using the service.

When you create a Satellite location, you provide a COS bucket where your data is stored. You can follow the COS documentation to export your data from one COS provider to another.

You can also review the following links for more information about exporting data from Satellite-enabled services. 

For more information, see [Satellite-enabled services](/docs/satellite?topic=satellite-managed-services).


| Title | Description |
| --- | --- |
| Red Hat OpenShift on IBM Cloud | For data portability information, see [Understanding data portability for Red Hat OpenShift on IBM Cloud](/docs/openshift?topic=openshift-data-portability). |
| [Rclone](https://rclone.org/){: external} | When you create a Satellite location, you provide a COS bucket where your data is stored. You can use `rclone` to move this data from COS to another s3 storage provider. For an example scenario, see the [Migrating Cloud Object Storage (COS) apps and data between IBM Cloud accounts](https://cloud.ibm.com/docs/satellite?topic=satellite-storage-cos-app-migration) tutorial for steps on using `rclone` to move data in one COS bucket to another COS bucket in IBM Cloud or in another cloud provider. Also, review [Using `rclone`](/docs/cloud-object-storage?topic=cloud-object-storage-rclone). |
| [OpenShift APIs for Data Protection](https://access.redhat.com/articles/5456281){: external} (OADP) | OADP (OpenShift APIs for Data Protection) is an operator that Red Hat has created to create, backup, and restore APIs for OpenShift clusters. For more information, see [Backup and restore Red Hat OpenShift cluster applications with OADP](https://developer.ibm.com/tutorials/awb-backup-and-restore-redhat-openshift-clusters-with-oadp/){: external} and the [OADP documentation](https://docs.openshift.com/container-platform/4.17/backup_and_restore/application_backup_and_restore/oadp-intro.html){: external} |
| Understanding data portability for {{site.data.keyword.cos_full_notm}} | Many backup and restore tools for Kubernetes or OpenShift clusters use {{site.data.keyword.cos_full_notm}} as the backup and restore location. Review the {{site.data.keyword.cos_full_notm}} documentation for more data portability options. |
| Understanding data portability for VPC | Review the VPC documentation for more data portability options. |
{: caption="Other options for exporting data" caption-side="bottom"}


## Exported data formats
{: #data-portability-data-formats}


Review the product documentation for the services you run on Satellite for information about data export formats.




## Data ownership
{: #data-ownership}

All exported data is classified as customer content and is therefore applied to the full customer ownership and licensing rights, as stated in [IBM Cloud Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS){: external}.




