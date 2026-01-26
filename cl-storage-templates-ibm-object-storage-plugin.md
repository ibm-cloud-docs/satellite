---

copyright:
  years: 2024, 2026

lastupdated: "2026-01-26"


keywords: change log, version history, ibm-object-storage-plug-in

subcollection: "satellite"

---

{{site.data.keyword.attribute-definition-list}}




# `ibm-object-storage-plugin` storage template version change log
{: #cl-storage-templates-ibm-object-storage-plugin}

Review the version history for `ibm-object-storage-plugin`.
{: shortdesc}


## Version 2.2
{: #cl-storage-templates-ibm-object-storage-plugin-2.2}


### Revision 33, released 23 January 2026
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-33}

- Resolves the following CVEs: [CVE-2025-61727](https://nvd.nist.gov/vuln/detail/CVE-2025-61727){: external}, [CVE-2025-61729](https://nvd.nist.gov/vuln/detail/CVE-2025-61729){: external}, [CVE-2025-45582](https://nvd.nist.gov/vuln/detail/CVE-2025-45582){: external}, and [CVE-2025-4598](https://nvd.nist.gov/vuln/detail/CVE-2025-4598){: external}.
- Updates Go to version `1.25.5`.
- cos plugin support on in-che region


### Revision 32, released 04 December 2025
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-32}

- Updates Go to version `1.25.4`.
- Fix the glibc error on rhel8 clusters for charts 2.2.42, 2.2.43, 2.2.44 


### Revision 31, released 13 November 2025
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-31}

- Resolves the following CVEs: [CVE-2025-58185](https://nvd.nist.gov/vuln/detail/CVE-2025-58185){: external}, [CVE-2025-58189](https://nvd.nist.gov/vuln/detail/CVE-2025-58189){: external}, [CVE-2025-61723](https://nvd.nist.gov/vuln/detail/CVE-2025-61723){: external}, [CVE-2025-61725](https://nvd.nist.gov/vuln/detail/CVE-2025-61725){: external}, [CVE-2025-22874](https://nvd.nist.gov/vuln/detail/CVE-2025-22874){: external}, [CVE-2025-4673](https://nvd.nist.gov/vuln/detail/CVE-2025-4673){: external}, [CVE-2025-22871](https://nvd.nist.gov/vuln/detail/CVE-2025-22871){: external}, and [CVE-2025-47906](https://nvd.nist.gov/vuln/detail/CVE-2025-47906){: external}.
- Updates Go to version `1.25.4`.


### Revision 30, released 18 August 2025
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-30}

- Resolves the following CVEs: [CVE-2019-17543](https://nvd.nist.gov/vuln/detail/CVE-2019-17543){: external}, [CVE-2025-22871](https://nvd.nist.gov/vuln/detail/CVE-2025-22871){: external}, [CVE-2025-22874](https://nvd.nist.gov/vuln/detail/CVE-2025-22874){: external}, [CVE-2025-4673](https://nvd.nist.gov/vuln/detail/CVE-2025-4673){: external}, and [CVE-2025-8058](https://nvd.nist.gov/vuln/detail/CVE-2025-8058){: external}.
- Updates Go to version `1.23.12`.
- Plugin support on heterogeneous cluster (cluster with both rhel and rhcos worker pools)


### Revision 29, released 24 June 2025
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-29}

- Resolves the following CVEs: [CVE-2025-22872](https://nvd.nist.gov/vuln/detail/CVE-2025-22872){: external}, [CVE-2025-30204](https://nvd.nist.gov/vuln/detail/CVE-2025-30204){: external}, and [CVE-2025-4802](https://nvd.nist.gov/vuln/detail/CVE-2025-4802){: external}.
- Updates Go to version `1.23.10`.


### Revision 28, released 25 April 2025
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-28}

- Resolves the following CVEs: [CVE-2025-0395](https://nvd.nist.gov/vuln/detail/CVE-2025-0395){: external}.
- Updates Go to version `1.23.8`.
- Enable bucket versioning support


### Revision 25, released 13 December 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-25}

- Resolves the following CVEs: [CVE-2024-3596](https://nvd.nist.gov/vuln/detail/CVE-2024-3596){: external}.
- Updates Go to version `1.22.9`.
- ROKS v4.17 cluster support


### Revision 24, released 01 October 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-24}

- Resolves the following CVEs: [CVE-2024-34158](https://nvd.nist.gov/vuln/detail/CVE-2024-34158){: external}, [CVE-2024-34155](https://nvd.nist.gov/vuln/detail/CVE-2024-34155){: external}, and [CVE-2024-34156](https://nvd.nist.gov/vuln/detail/CVE-2024-34156){: external}.
- Updates Go to version `1.22.7`.
- Fixed mount issue with coreos9 roks 4.16 cluster 
- s3fs-fuse version updated to v1.94
- Support for ubuntu24 SecureByDefault clusters


### Revision 23, released 04 September 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-23}

- Resolves the following CVEs: [CVE-2024-6104](https://nvd.nist.gov/vuln/detail/CVE-2024-6104){: external}, [CVE-2024-24791](https://nvd.nist.gov/vuln/detail/CVE-2024-24791){: external}, [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288){: external}, [CVE-2024-2398](https://nvd.nist.gov/vuln/detail/CVE-2024-2398){: external}, [CVE-2024-37370](https://nvd.nist.gov/vuln/detail/CVE-2024-37370){: external}, and [CVE-2024-37371](https://nvd.nist.gov/vuln/detail/CVE-2024-37371){: external}.
- Updates Go to version `1.21.13`.


### Revision 22, released 17 July 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-22}

- Resolves the following CVEs: [CVE-2023-2953](https://nvd.nist.gov/vuln/detail/CVE-2023-2953){: external}, and [CVE-2024-28182](https://nvd.nist.gov/vuln/detail/CVE-2024-28182){: external}.
- Updates Go to version `1.21.12`.


### Revision 21, released 05 June 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-21}

- Resolves the following CVEs: [CVE-2023-7008](https://nvd.nist.gov/vuln/detail/CVE-2023-7008){: external}, [CVE-2024-2961](https://nvd.nist.gov/vuln/detail/CVE-2024-2961){: external}, [CVE-2024-33599](https://nvd.nist.gov/vuln/detail/CVE-2024-33599){: external}, [CVE-2024-33600](https://nvd.nist.gov/vuln/detail/CVE-2024-33600){: external}, [CVE-2024-33601](https://nvd.nist.gov/vuln/detail/CVE-2024-33601){: external}, and [CVE-2024-33602](https://nvd.nist.gov/vuln/detail/CVE-2024-33602){: external}.
- Updates Go to version `1.21.10`.
- HPCS support for cos s3fs plug-in 


### Revision 20, released 29 April 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-20}

- Resolves the following CVEs: [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288){: external}, [CVE-2024-24785](https://nvd.nist.gov/vuln/detail/CVE-2024-24785){: external}, [CVE-2023-44487](https://nvd.nist.gov/vuln/detail/CVE-2023-44487){: external}, and [CVE-2023-45288](https://nvd.nist.gov/vuln/detail/CVE-2023-45288){: external}.
- Updates Go to version `1.21.9`.
- Fix plug-in installation issue on CoreOS8 cluster 


### Revision 19, released 29 February 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-19}

- Updates Go to version `1.21.7`.
- Fix plug-in installation issue on CoreOS cluster 


### Revision 18, released 02 February 2024
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-18}

- Resolves the following CVEs: [CVE-2023-3446](https://nvd.nist.gov/vuln/detail/CVE-2023-3446){: external}, [CVE-2023-3817](https://nvd.nist.gov/vuln/detail/CVE-2023-3817){: external}, and [CVE-2023-5678](https://nvd.nist.gov/vuln/detail/CVE-2023-5678){: external}.
- Updates Go to version `1.20.13`.
- Replaced UBI image with scratch image 


### Revision 17, released 27 November 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-17}

- Resolves the following CVEs: [CVE-2007-4559](https://nvd.nist.gov/vuln/detail/CVE-2007-4559){: external}, [CVE-2023-4016](https://nvd.nist.gov/vuln/detail/CVE-2023-4016){: external}, [CVE-2023-4641](https://nvd.nist.gov/vuln/detail/CVE-2023-4641){: external}, [CVE-2023-22745](https://nvd.nist.gov/vuln/detail/CVE-2023-22745){: external}, and [CVE-2023-40217](https://nvd.nist.gov/vuln/detail/CVE-2023-40217){: external}.
- Updates the UBI to version `8.9-1029`.
- Updates Go to version `1.21.4`.
- s3fs fuse version updated to v1.93


### Revision 16, released 30 October 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-16}

- Resolves the following CVEs: [CVE-2023-44487](https://nvd.nist.gov/vuln/detail/CVE-2023-44487){: external}, and [CVE-2023-39325](https://nvd.nist.gov/vuln/detail/CVE-2023-39325){: external}.
- Updates the UBI to version `8.8-1072.1697626218`.
- Updates Go to version `1.19.12`.


### Revision 15, released 18 October 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-15}

- Resolves the following CVEs: [CVE-2023-29491](https://nvd.nist.gov/vuln/detail/CVE-2023-29491){: external}, [CVE-2023-4911](https://nvd.nist.gov/vuln/detail/CVE-2023-4911){: external}, [CVE-2023-4527](https://nvd.nist.gov/vuln/detail/CVE-2023-4527){: external}, [CVE-2023-4806](https://nvd.nist.gov/vuln/detail/CVE-2023-4806){: external}, and [CVE-2023-4813](https://nvd.nist.gov/vuln/detail/CVE-2023-4813){: external}.
- Updates the UBI to version `8.8-1072.1696517598`.
- Updates Go to version `1.19.12`.


### Revision 14, released 19 September 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-14}

- Resolves the following CVEs: [CVE-2023-27536](https://nvd.nist.gov/vuln/detail/CVE-2023-27536){: external}, [CVE-2023-2602](https://nvd.nist.gov/vuln/detail/CVE-2023-2602){: external}, [CVE-2023-2603](https://nvd.nist.gov/vuln/detail/CVE-2023-2603){: external}, [CVE-2023-34969](https://nvd.nist.gov/vuln/detail/CVE-2023-34969){: external}, [CVE-2023-28321](https://nvd.nist.gov/vuln/detail/CVE-2023-28321){: external}, [CVE-2023-28484](https://nvd.nist.gov/vuln/detail/CVE-2023-28484){: external}, [CVE-2023-29469](https://nvd.nist.gov/vuln/detail/CVE-2023-29469){: external}, and [CVE-2023-29409](https://nvd.nist.gov/vuln/detail/CVE-2023-29409){: external}.
- Updates the UBI to version `8.8-1037`.
- Updates Go to version `1.19.12`.
- RHCOS 4.13 support


### Revision 11, released 04 May 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-11}

- Resolves the following CVEs: [CVE-2023-24536](https://nvd.nist.gov/vuln/detail/CVE-2023-24536){: external}, [CVE-2023-24537](https://nvd.nist.gov/vuln/detail/CVE-2023-24537){: external}, [CVE-2023-24538](https://nvd.nist.gov/vuln/detail/CVE-2023-24538){: external}, and [CVE-2023-0361](https://nvd.nist.gov/vuln/detail/CVE-2023-0361){: external}.
- Updates the UBI to version `8.7-1107`.
- Updates Go to version `1.19.8`.
- Enable PVC metrics export to node stats


### Revision 10, released 04 April 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-10}

- Resolves the following CVEs: [CVE-2022-4304](https://nvd.nist.gov/vuln/detail/CVE-2022-4304){: external}, [CVE-2022-4450](https://nvd.nist.gov/vuln/detail/CVE-2022-4450){: external}, [CVE-2023-0215](https://nvd.nist.gov/vuln/detail/CVE-2023-0215){: external}, and [CVE-2023-0286](https://nvd.nist.gov/vuln/detail/CVE-2023-0286){: external}.
- Updates the UBI to version `8.7-1085.1679482090`.
- Updates Go to version `1.19.7`.
- Fix for helm chart installation failing on regions with no Regional COS endpoint 


### Revision 9, released 21 March 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-9}

- Resolves the following CVEs: [CVE-2023-23916](https://nvd.nist.gov/vuln/detail/CVE-2023-23916){: external}, and [CVE-2023-24532](https://nvd.nist.gov/vuln/detail/CVE-2023-24532){: external}.
- Updates the UBI to version `8.7-1085`.
- Updates Go to version `1.19.7`.
- Adds support to automatically populate the s3 endpoints for IBM, AWS, and Wasabi.


### Revision 8, released 06 March 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-8}

- Resolves the following CVEs: [CVE-2022-4415](https://nvd.nist.gov/vuln/detail/CVE-2022-4415){: external}, [CVE-2020-10735](https://nvd.nist.gov/vuln/detail/CVE-2020-10735){: external}, [CVE-2021-28861](https://nvd.nist.gov/vuln/detail/CVE-2021-28861){: external}, [CVE-2022-45061](https://nvd.nist.gov/vuln/detail/CVE-2022-45061){: external}, [CVE-2022-40897](https://nvd.nist.gov/vuln/detail/CVE-2022-40897){: external}, [CVE-2022-41723](https://nvd.nist.gov/vuln/detail/CVE-2022-41723){: external}, and [CVE-2022-41724](https://nvd.nist.gov/vuln/detail/CVE-2022-41724){: external}.
- Updates the UBI to version `8.7-1085`.
- Updates Go to version `1.19.6`.


### Revision 7, released 20 February 2023
{: #cl-storage-templates-ibm-object-storage-plugin-2.2-7}

- Resolves the following CVEs: [CVE-2022-47629](https://nvd.nist.gov/vuln/detail/CVE-2022-47629){: external}.
- Updates the UBI to version `8.7-1049.1675784874`.
- Updates Go to version `1.18.9`.
- Sets the default values for CPU request and CPU limit to `100m` and CPU limit to `500m`. 
- Sets the default values for memory request to `128Mi` and the memory limit to `500Mi`. 
- Adds an option to disable reading secret from cross namespace for PVC creation.
- Adds support for automatically populating the object store endpoint in the storage classes based on `storageclass` and `s3provider` parameters.
- Adds support for Wasabi and AWS s3 provider on Satellite clusters.
