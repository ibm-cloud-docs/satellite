---

copyright:
  years: 2020, 2023

lastupdated: "2023-04-05"


keywords: satellite storage, change log, version history, ibm object storage plugin

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# `ibm-object-storage-plugin` change log
{: #cl-ibm-object-storage-plugin}

Review the version history for the `ibm-object-storage-plugin` {{site.data.keyword.satelliteshort}} storage template.
{: shortdesc}

## Version 2.2
{: #2.2-change-log}


### Revision 10, released 04 April 2023
{: #ibm-object-storage-plugin-2.2-rev-10-change-log}


- Resolves the following CVEs: [CVE-2022-4304](https://nvd.nist.gov/vuln/detail/CVE-2022-4304){: external} [CVE-2022-4450](https://nvd.nist.gov/vuln/detail/CVE-2022-4450){: external} [CVE-2023-0215](https://nvd.nist.gov/vuln/detail/CVE-2023-0215){: external} [CVE-2023-0286](https://nvd.nist.gov/vuln/detail/CVE-2023-0286){: external} 
- Updates the UBI to version `8.7-1085.1679482090`.
- Updates Golang to version `1.19.7`.
- Fix for helm chart installation failing on regions with no Regional COS endpoint 

### Revision 9, released 21 March 2023
{: #ibm-object-storage-plugin-2.2-rev-9-change-log}


- Resolves the following CVEs: [CVE-2023-23916](https://nvd.nist.gov/vuln/detail/CVE-2023-23916){: external} [CVE-2023-24532](https://nvd.nist.gov/vuln/detail/CVE-2023-24532){: external} 
- Updates the UBI to version `8.7-1085`.
- Updates Golang to version `1.19.7`.
- Adds support to automatically populate the s3 endpoints for IBM, AWS, and Wasabi.

### Revision 8, released 06 March 2023
{: #ibm-object-storage-plugin-2.2-rev-8-change-log}


- Resolves the following CVEs: [CVE-2022-4415](https://nvd.nist.gov/vuln/detail/CVE-2022-4415){: external} [CVE-2020-10735](https://nvd.nist.gov/vuln/detail/CVE-2020-10735){: external} [CVE-2021-28861](https://nvd.nist.gov/vuln/detail/CVE-2021-28861){: external} [CVE-2022-45061](https://nvd.nist.gov/vuln/detail/CVE-2022-45061){: external} [CVE-2022-40897](https://nvd.nist.gov/vuln/detail/CVE-2022-40897){: external} [CVE-2022-41723](https://nvd.nist.gov/vuln/detail/CVE-2022-41723){: external} [CVE-2022-41724](https://nvd.nist.gov/vuln/detail/CVE-2022-41724){: external} 
- Updates the UBI to version `8.7-1085`.
- Updates Golang to version `1.19.6`.

### Revision 7, released 20 February 2023
{: #ibm-object-storage-plugin-2.2-rev-7-change-log}


- Resolves the following CVEs: [CVE-2022-47629](https://nvd.nist.gov/vuln/detail/CVE-2022-47629){: external} 
- Updates the UBI to version `8.7-1049.1675784874`.
- Updates Golang to version `1.18.9`.
- Sets the default values for CPU request and CPU limit to `100m` & `500m` respectively. 
- Sets the default values for Memory request and Memory limit are updated to `128Mi & 500Mi` respectively. 
- Adds an option to disable reading secret from cross namespace for PVC creation.
- Adds support for automatically populating the object store endpoint in the storage classes based on `storageclass` and `s3provider` parameters.
- Adds support for Wasabi & AWS s3 provider on Satellite clusters.


