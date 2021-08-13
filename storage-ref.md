---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

keywords: satellite storage, satellite config, block, file, ocs

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Storage class reference
{: #storage-class-ref}

Review the storage class reference for the storage provider that you want to use in your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}


<br />

## AWS EBS
{: #ebs-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EBS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. Note that data volumes are automatically encrypted by an AWS managed default key. For more information, see [Default KMS key for EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#EBSEncryption_key_mgmt){: external}. For more information on AWS EBS encryption, see [How AWS EBS encryption works](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#how-ebs-encryption-works){: external}.
{: shortdesc}

| Storage class name | EBS volume type | File system type | Provisioner | Default IOPS per GB | Size range | Hard disk | Encrypted? | Volume binding mode | Reclaim policy | More info | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `sat-aws-block-gold` | io2 | ext4 | `ebs.csi.aws.com` | 10 | 10 GiB - 6.25 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external}
| `sat-aws-block-silver` | gp3 | ext4 | `ebs.csi.aws.com` | N/A | 1 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external} |
| `sat-aws-block-bronze` | st1 | ext4 | `ebs.csi.aws.com` | N/A | 125 GiB - 16 TiB | HDD | True |  WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external} |
{: caption="Table 1. AWS EBS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the volume type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the default IOPs per GB. The size column is the supported size range. The seventh column is disk type. The eighth column is encryption support. The ninth column is the volume binding mode. The tenth column is the reclaim policy."}

<br />

## AWS EFS
{: #efs-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EFS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-aws-file-gold` | NFS | Delete |

{: caption="Table 2. AWS EFS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the file system type. The third column is the reclaim policy."}

<br />

## Azure Disk
{: #azure-disk-ref}

| Storage class name | IOPS range per disk | Size range | Disk type | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- |
| `sat-azure-block-platinum` |  1200 - 160000 | 4 GiB - 64 TiB | SSD | Delete | Immediate |
| `sat-azure-block-platinum-metro`  | 1200 - 160000 | 4 GiB - 64 TiB | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-gold` | 120 - 20000 | 32 GiB - 32 TiB | SSD | Delete | Immediate |
| `sat-azure-block-gold-metro` | 120 - 20000 | 32 GiB - 32 TiB | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-silver`  | 120 - 6000 | N/A | SSD | Delete | Immediate |
| `sat-azure-block-silver-metro` | 120 - 6000 | N/A | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-bronze`  | 500 - 2000 | 32 GiB - 32 TiB | HDD | Delete | Immediate |
| `sat-azure-block-bronze-metro` | 500 - 2000 | 32 GiB - 32 TiB | HDD | Delete | WaitForFirstConsumer |
{: caption="Table 2. Azure Disk storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the IOPs range per disk. The third column is the size range . The fourth column is the disk type. The fifth column is the reclaim policy. The sixth column is the volume binding mode."}

## Local block storage
{: #local-block-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `sat-local-block-gold ` | Block | Retain |
{: caption="Table 3. Local block storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}

<br />

## Local file storage
{: #local-file-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local file storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-local-file-gold` | `ext4` or `xfs` | Retain |
{: caption="Table 4. Local file storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}

<br />



## NetApp ONTAP-NAS 20.07
{: #netapp-nas-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-NAS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-netapp-file-gold` | ONTAP-NAS | File | Delete |
| `sat-netapp-file-silver` | ONTAP-NAS | File | Delete |
| `sat-netapp-file-bronze` | ONTAP-NAS | File | Delete |
{: caption="Table 5. NetApp ONTAP-NAS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}

<br />



## NetApp ONTAP-SAN 20.07
{: #netapp-san-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-SAN. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-netapp-block-gold` | ONTAP-SAN | Block | Delete |
| `sat-netapp-block-silver` | ONTAP-SAN | Block | Delete |
| `sat-netapp-block-bronze` | ONTAP-SAN | Block | Delete |
{: caption="Table 6. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}

<br />

## OpenShift Data Foundation for local volumes
{: #ocs-local-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Data Foundation. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
{: caption="Table 7. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}

<br />

## OpenShift Data Foundation for remote volumes
{: #ocs-remote-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Data Foundation. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
{: caption="Table 8. Storage class reference for OpenShift Container storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the volume binding mode. The sixth column is volume expansion support. The seventh column is the reclaim policy."}




