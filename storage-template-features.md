---

copyright:
  years: 2020, 2026
lastupdated: "2026-07-20"


keywords: satellite storage, features, overview


subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Storage template feature overview
{: #storage-template-features}


| Name | Version | Supported providers | Encryption at rest | Encryption in transit | Snapshot support | Availability | Volume expansion |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AWS EBS CSI Driver | 1.31.0 | AWS | True | True | True | Zonal | True |
| AWS EBS CSI Driver | 1.55.0 | AWS | True | True | True | Zonal | True |
| AWS EFS CSI Driver | 1.4.2 | AWS | True | True | False | Regional | False |
| AWS EFS CSI Driver | 2.0.3 | AWS | True | True | False | Regional | False |
| AWS EFS CSI Driver | 2.3.0 | AWS | True | True | False | Regional | False |
| Azure Disk CSI Driver | 1.23.0 | Azure | True | True | False | Zonal | False |
| Azure Disk CSI Driver | 1.30.3 | Azure | True | True | False | Zonal | False |
| Azure File CSI Driver | 1.22.0 | Azure | True | True | False | Regional | False |
| Azure File CSI Driver | 1.31.2 | Azure | True | True | True | Regional | True |
| GCP Compute Persistent Disk CSI Driver | 1.8.0 | Google | True | True | True | Zonal | False |
| IBM Object Storage Plugin | 2.2 | IBM | 
| [Beta] IBM VPC Block CSI driver | 5.1 | IBM | 
| [Beta] Local Storage File and/or Block | 1.0.0 | AWS, IBM | False | False | False | Zonal | False |
| NetApp Ontap-NAS Driver | 24.02 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-NAS Driver | 25.06 | AWS, Azure, IBM, GCP | True | True | True | Zonal | True |
| NetApp Ontap-SAN Driver | 24.02 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-SAN Driver | 25.06 | AWS, Azure, IBM, GCP | True | True | True | Zonal | True |
| OpenShift Data Foundation for local devices | 4.15 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.16 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.17 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.18 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.19 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.20 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.15 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.16 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.17 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.18 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.19 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.20 | OpenShift Container Platform | True | True | False | Regional | True |
| VMware CSI Driver | 2.7.0 | VMware,IBM | False | False | True | Zonal | False |
{: caption="Storage template feature comparison" caption-side="bottom"}
