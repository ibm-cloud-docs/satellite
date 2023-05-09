---

copyright:
  years: 2020, 2023
lastupdated: "2023-05-09"


keywords: satellite storage, features, overview


subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Storage template feature overview
{: #storage-template-features}


| Name | Version | Supported providers | Encryption at rest | Encryption in transit | Snapshot support | Availability | Volume expansion |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AWS EBS CSI Driver | 1.1.0 | AWS | True | True | True | Zonal | True |
| AWS EBS CSI Driver | 1.5.1 | AWS | True | True | True | Zonal | True |
| AWS EBS CSI Driver | 1.12.0 | AWS | True | True | True | Zonal | True |
| AWS EFS CSI Driver | 1.3.1 | AWS | True | True | False | Regional | False |
| AWS EFS CSI Driver | 1.3.7 | AWS | True | True | False | Regional | False |
| AWS EFS CSI Driver | 1.4.2 | AWS | True | True | False | Regional | False |
| Azure Disk CSI Driver | 1.4.0 | Azure | True | True | False | Zonal | False |
| Azure Disk CSI Driver | 1.18.0 | Azure | True | True | False | Zonal | False |
| Azure Disk CSI Driver | 1.23.0 | Azure | True | True | False | Zonal | False |
| Azure File CSI Driver | 1.9.0 | Azure | True | True | False | Regional | False |
| Azure File CSI Driver | 1.18.0 | Azure | True | True | False | Regional | False |
| Azure File CSI Driver | 1.22.0 | Azure | True | True | False | Regional | False |
| GCP Compute Persistent Disk CSI Driver | 1.0.4 | Google | True | True | True | Zonal | False |
| GCP Compute Persistent Disk CSI Driver | 1.7.1 | Google | True | True | True | Zonal | False |
| GCP Compute Persistent Disk CSI Driver | 1.8.0 | Google | True | True | True | Zonal | False |
| IBM Object Storage Plugin | 2.2 | IBM | 
| IBM System Storage Block CSI driver | 1.4.0 | IBM | False | False | True | Regional | True |
| IBM System Storage Block CSI driver | 1.5.0 | IBM | False | False | True | Regional | True |
| IBM System Storage Block CSI driver | 1.10.0 | IBM | False | False | True | Regional | True |
| [Beta] IBM VPC Block CSI driver | 5.0 | IBM | 
| Local Storage Operator - Block | 4.8 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - Block | 4.9 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - Block | 4.10 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - Block | 4.11 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - Block | 4.12 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - File | 4.8 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - File | 4.9 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - File | 4.10 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - File | 4.11 | AWS,IBM | False | False | False | Zonal | False |
| Local Storage Operator - File | 4.12 | AWS,IBM | False | False | False | Zonal | False |
| NetApp Ontap-NAS Driver | 21.04 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-NAS Driver | 22.04 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-NAS Driver | 22.10 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-SAN Driver | 21.04 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-SAN Driver | 22.04 | AWS,Azure,IBM | True | True | True | Zonal | True |
| NetApp Ontap-SAN Driver | 22.10 | AWS,Azure,IBM | True | True | True | Zonal | True |
| OpenShift Data Foundation for local devices | 4.9 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.10 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.11 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for local devices | 4.12 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.9 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.10 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.11 | OpenShift Container Platform | True | True | False | Regional | True |
| OpenShift Data Foundation for remote storage | 4.12 | OpenShift Container Platform | True | True | False | Regional | True |
| VMware CSI Driver | 2.5.1 | VMware,IBM | False | False | True | Zonal | False |
{: caption="Storage template feature comparison" caption-side="bottom"}

