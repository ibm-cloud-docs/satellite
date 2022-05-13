---

copyright:
  years: 2020, 2022
lastupdated: "2022-05-13"

keywords: satellite storage, satellite config, block, file, ocs

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Storage class reference
{: #storage-class-ref}

Review the storage class reference for the storage provider that you want to use in your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}


## AWS EBS
{: #ebs-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EBS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. Note that data volumes are automatically encrypted by an AWS managed default key. For more information, see [Default KMS key for EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#EBSEncryption_key_mgmt){: external}. For more information on AWS EBS encryption, see [How AWS EBS encryption works](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#how-ebs-encryption-works){: external}.
{: shortdesc}

| Storage class name | EBS volume type | File system type | Provisioner | Default IOPS per GB | Size range | Hard disk | Encrypted? | Volume binding mode | Reclaim policy | More info | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `sat-aws-block-gold` **Default** | io2 | ext4 | `ebs.csi.aws.com` | 10 | 10 GiB - 6.25 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external}
| `sat-aws-block-silver` | gp3 | ext4 | `ebs.csi.aws.com` | N/A | 1 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external} |
| `sat-aws-block-bronze` | st1 | ext4 | `ebs.csi.aws.com` | N/A | 125 GiB - 16 TiB | HDD | True |  WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external} |
{: caption="Table 1. AWS EBS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the volume type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the default IOPs per GB. The size column is the supported size range. The seventh column is disk type. The eighth column is encryption support. The ninth column is the volume binding mode. The tenth column is the reclaim policy."}


## AWS EFS
{: #efs-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EFS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-aws-file-gold` **Default** | NFS | Delete |
{: caption="Table 2. AWS EFS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the file system type. The third column is the reclaim policy."}


## Azure Disk
{: #azure-disk-ref}

| Storage class name | IOPS range per disk | Size range | Disk type | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- |
| `sat-azure-block-platinum` |  1200 - 160000 | 4 GiB - 64 TiB | SSD | Delete | Immediate |
| `sat-azure-block-platinum-metro`  | 1200 - 160000 | 4 GiB - 64 TiB | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-gold` | 120 - 20000 | 32 GiB - 32 TiB | SSD | Delete | Immediate |
| `sat-azure-block-gold-metro` **Default** | 120 - 20000 | 32 GiB - 32 TiB | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-silver`  | 120 - 6000 | N/A | SSD | Delete | Immediate |
| `sat-azure-block-silver-metro` | 120 - 6000 | N/A | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-bronze`  | 500 - 2000 | 32 GiB - 32 TiB | HDD | Delete | Immediate |
| `sat-azure-block-bronze-metro` | 500 - 2000 | 32 GiB - 32 TiB | HDD | Delete | WaitForFirstConsumer |
{: caption="Table 2. Azure Disk storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the IOPs range per disk. The third column is the size range . The fourth column is the disk type. The fifth column is the reclaim policy. The sixth column is the volume binding mode."}


## Azure File
{: #azure-file-ref}

| Storage class name | Reclaim policy | Volume Binding Mode |
| --- | --- | --- |
| `sat-azure-file-platinum`  | Delete | Immediate |
| `sat-azure-file-platinum-metro` | Delete | WaitForFirstConsumer |
| `sat-azure-file-gold` | Delete | Immediate |
| `sat-azure-file-gold-metro` **Default** | Delete | WaitForFirstConsumer |
| `sat-azure-file-silver` | Delete | Immediate |
| `sat-azure-file-silver-metro` | Delete | WaitForFirstConsumer |
| `sat-azure-file-bronze` | Delete | Immediate |
| `sat-azure-file-bronze-metro` | Delete | WaitForFirstConsumer |
{: caption="Table 2. Azure file storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the IOPs range per disk. The third column is the size range."}

## Google Compute Engine 
{: #google-csi-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for Google compute engine persistent disk storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

 Storage class name | Default Read IOPS per GB | Default Write IOPS per GB | Size range (per disk) | Hard disk | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-gce-block-platinum` | NA | NA | 500 GB - 64 TB | SSD | Delete | Immediate |
| `sat-gce-block-platinum-metro`  | NA | NA | 500 GB - 64 TB | SSD | Delete | WaitForFirstConsumer |
| `sat-gce-block-gold` | 30 | 30 | 10 GB - 64 TB | SSD | Delete | Immediate |
| `sat-gce-block-gold-metro` **Default** | 30 | 30 | 10 GB - 64 TB | SSD | Delete | WaitForFirstConsumer |
| `sat-gce-block-silver`  | 6 | 30 | 10 GB - 64 GB | SSD | Delete | Immediate |
| `sat-gce-block-silver-metro` | 6 | 6 | 10 GB - 64 GB | SSD | Delete | WaitForFirstConsumer |
| `sat-gce-block-bronze`  | 0.75 | 1.5 | 10 GiB - 64 TiB | HDD | Delete | Immediate |
| `sat-gce-block-bronze-metro` | 0.75 | 1.5 | 10 GiB - 64 TiB | HDD | Delete | WaitForFirstConsumer |
{: caption="Table 4. Google Compute Engine storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}

## Local block storage
{: #local-block-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `sat-local-block-gold` | Block | Retain |
{: caption="Table 5. Local block storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}


## Local file storage
{: #local-file-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local file storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-local-file-gold` | `ext4` or `xfs` | Retain |
{: caption="Table 6. Local file storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}




## NetApp ONTAP-NAS 21.04
{: #netapp-nas-ref-2104}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-NAS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.

| Storage class name | Type | File system | IOPs | Encryption | Reclaim policy |
| --- | --- | --- | --- | --- | --- |
| `sat-netapp-file-gold` **Default** | ONTAP-NAS | NFS | no QoS limits | Encryption disabled. | Delete |
| `sat-netapp-file-gold-encrypted` | ONTAP-NAS | NFS | no QoS limits | Encryption enabled. | Delete |
| `sat-netapp-file-silver` | ONTAP-NAS | NFS | User defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-file-silver-encrypted` | ONTAP-NAS | NFS | User defined QoS limit. | Encryption enabled. | Delete |
| `sat-netapp-file-bronze` | ONTAP-NAS | NFS | User-defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-file-bronze-encrypted` | ONTAP-NAS | NFS | User-defined QoS limit.| Encryption enabled. | Delete |
{: caption="Table 7. NetApp ONTAP-NAS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}



## NetApp ONTAP-NAS 20.07
{: #netapp-nas-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-NAS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Reclaim policy |
| --- | --- | --- | --- | 
| `sat-netapp-file-gold` **Default** | ONTAP-NAS | File | Delete |
| `sat-netapp-file-silver` | ONTAP-NAS | File | Delete |
| `sat-netapp-file-bronze` | ONTAP-NAS | File | Delete |
{: caption="Table 8. NetApp ONTAP-NAS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}




## NetApp ONTAP-SAN 21.04
{: #netapp-san-ref-2104}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-SAN. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.

| Storage class name | Type | File system | IOPs | Encryption |Reclaim policy |
| --- | --- | --- | --- | --- | --- |
| `sat-netapp-block-gold` **Default** | ONTAP-SAN | ext4 | no QoS limits. | Encryption disabled. | Delete |
| `sat-netapp-block-gold-encrypted` | ONTAP-SAN | ext4 | no QoS limits. | Encryption enabled. | Delete |
| `sat-netapp-block-silver` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-block-silver-encrypted` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption enabled. | Delete |
| `sat-netapp-block-bronze` | ONTAP-SAN | ext4 | User defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-block-bronze-encrypted` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption enabled. | Delete |
{: caption="Table 9. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}


## NetApp ONTAP-SAN 20.07
{: #netapp-san-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-SAN. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Reclaim policy |
| --- | --- | --- | --- | 
| `sat-netapp-block-gold` **Default** | ONTAP-SAN | Block | Delete |
| `sat-netapp-block-silver` | ONTAP-SAN | Block | Delete |
| `sat-netapp-block-bronze` | ONTAP-SAN | Block | Delete |
{: caption="Table 10. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}

## OpenShift Data Foundation for local volumes
{: #ocs-local-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Data Foundation. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` **Default** | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
| `sat-ocs-cephrbd-gold-metro` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | WaitForFirstConsumer | True | Delete |
| `sat-ocs-cephfs-gold-metro` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | WaitForFirstConsumer | True | Delete |
{: caption="Table 11. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}

## OpenShift Data Foundation for remote volumes
{: #ocs-remote-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Data Foundation. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` **Default** | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
| `sat-ocs-cephrbd-gold-metro` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | WaitForFirstConsumer | True | Delete |
| `sat-ocs-cephfs-gold-metro` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | WaitForFirstConsumer | True | Delete |
{: caption="Table 12. Storage class reference for OpenShift Container storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the volume binding mode. The sixth column is volume expansion support. The seventh column is the reclaim policy."}



## {{site.data.keyword.IBM_notm}} Spectrum Scale
{: #spec-scale-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for {{site.data.keyword.IBM_notm}} Spectrum Scale. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}


| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `ibm-spectrum-scale-csi-lt` | Light weight volumes | Delete  |
{: caption="Table 13. IBM Spectrum Scale storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates if the parameters is required. The third column is a brief description of the parameter."}






## {{site.data.keyword.IBM_notm}} VPC block storage
{: #sat-storage-ibm-vpc-csi-sc-ref}


Review the {{site.data.keyword.satelliteshort}} storage classes for IBM VPC block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

 Storage class name | Default Read IOPS per GB | Default Write IOPS per GB | Size range (per disk) | Hard disk | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-vpc-block-gold-metro` **Default** | 10 | 10 | 10 GB - 4 TB | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-5iops-tier`  | 5 | 5 | 10 GB - 9600 GB | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-custom` | Custom | Custom | Based on IOPS | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-general-purpose` | 3 | 3 | 10 GB - 16 TB | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-10iops-tier`  | 10 | 10 | 10 GB - 4 TB | SSD | Retain | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-5iops-tier` | 5 | 5 | 10 GB - 9600 GB | SSD | Retain | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-custom`  | Custom | Custom | Based on IOPS | SSD | Retain | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-general-purpose` | 3 | 3 | 10 GiB - 16 TB | SSD | Retain | WaitForFirstConsumer |
{: caption="Table 14. Storage class reference for IBM VPC block storage" caption-side="top"}



