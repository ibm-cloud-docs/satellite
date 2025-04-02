---

copyright:
  years: 2024, 2025

lastupdated: "2025-04-02"


keywords: change log, version history, ibm-vpc-block-csi-driver

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

<!-- The content in this topic is auto-generated except for reuse-snippets indicated with {[ ]}. -->


# `ibm-vpc-block-csi-driver` storage template version change log
{: #cl-storage-templates-ibm-vpc-block-csi-driver}

Review the version history for `ibm-vpc-block-csi-driver`.
{: shortdesc}



## Version 5.1
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1}


### Revision 9, released 13 December 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-9}

- Resolves the following CVEs: [CVE-2024-51744](https://nvd.nist.gov/vuln/detail/CVE-2024-51744){: external}.
- Updates Go to version `1.22.9`.

### Revision 7, released 12 July 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-7}

- Updates Go to version `1.21.12`.

### Revision 6, released 20 June 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-6}

- Resolves the following CVEs: [CVE-2024-2961](https://nvd.nist.gov/vuln/detail/CVE-2024-2961){: external}, [CVE-2024-33599](https://nvd.nist.gov/vuln/detail/CVE-2024-33599){: external}, [CVE-2024-33600](https://nvd.nist.gov/vuln/detail/CVE-2024-33600){: external}, [CVE-2024-33601](https://nvd.nist.gov/vuln/detail/CVE-2024-33601){: external}, and [CVE-2024-33602](https://nvd.nist.gov/vuln/detail/CVE-2024-33602){: external}.
- Updates Go to version `1.21.11`.

### Revision 5, released 14 May 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-5}

- Resolves the following CVEs: [CVE-2023-28322](https://nvd.nist.gov/vuln/detail/CVE-2023-28322){: external}, [CVE-2023-38546](https://nvd.nist.gov/vuln/detail/CVE-2023-38546){: external}, [CVE-2023-46218](https://nvd.nist.gov/vuln/detail/CVE-2023-46218){: external}, and [CVE-2024-24786](https://nvd.nist.gov/vuln/detail/CVE-2024-24786){: external}.
- Updates Go to version `1.21.9`.

### Revision 4, released 07 March 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-4}

- Resolves the following CVEs: [CVE-2023-3446](https://nvd.nist.gov/vuln/detail/CVE-2023-3446){: external}, [CVE-2023-3817](https://nvd.nist.gov/vuln/detail/CVE-2023-3817){: external}, [CVE-2023-5678](https://nvd.nist.gov/vuln/detail/CVE-2023-5678){: external}, [CVE-2023-5981](https://nvd.nist.gov/vuln/detail/CVE-2023-5981){: external}, [CVE-2023-39615](https://nvd.nist.gov/vuln/detail/CVE-2023-39615){: external}, [CVE-2023-7104](https://nvd.nist.gov/vuln/detail/CVE-2023-7104){: external}, [CVE-2023-50387](https://nvd.nist.gov/vuln/detail/CVE-2023-50387){: external}, and [CVE-2023-50868](https://nvd.nist.gov/vuln/detail/CVE-2023-50868){: external}.
- Updates Go to version `1.21.7`.

### Revision 3, released 16 February 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-3}

- Resolves the following CVEs: [CVE-2022-48560](https://nvd.nist.gov/vuln/detail/CVE-2022-48560){: external}, [CVE-2022-48564](https://nvd.nist.gov/vuln/detail/CVE-2022-48564){: external}, [CVE-2023-39615](https://nvd.nist.gov/vuln/detail/CVE-2023-39615){: external}, [CVE-2023-43804](https://nvd.nist.gov/vuln/detail/CVE-2023-43804){: external}, [CVE-2023-45803](https://nvd.nist.gov/vuln/detail/CVE-2023-45803){: external}, and [CVE-2023-5981](https://nvd.nist.gov/vuln/detail/CVE-2023-5981){: external}.
- Updates the UBI to version `8.9-1108`.
- Updates Go to version `1.20.11`.
- Updated Kubernetes dependency to 1.28 
- Updated `csi node driver registrar` image to v2.9.3 
- Updated `livenessprobe` image to v2.11.0 
- Updated `csi-snapshotter` image to v6.3.3 
- Updated `csi-provisioner` image to v3.6.3 
- Updated `csi-attacher` image to v4.4.3 
- Updated `csi-resizer` image to v1.9.3 

### Revision 2, released 25 January 2024
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-2}

- Resolves the following CVEs: [CVE-2023-3446](https://nvd.nist.gov/vuln/detail/CVE-2023-3446){: external}, [CVE-2023-3817](https://nvd.nist.gov/vuln/detail/CVE-2023-3817){: external}, and [CVE-2023-5678](https://nvd.nist.gov/vuln/detail/CVE-2023-5678){: external}.
- Updates the UBI to version `8.9-1029`.
- Updates Go to version `1.20.11`.

### Revision 1, released 27 November 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.1-1}

- Updates the UBI to version `8.9-1029`.
- Updates Go to version `1.20.10`.
- Fix for snapshot size to reflect actual source volume size 
- Retry fetching IAM token, if token exchange url is unreachable 
- Initial release
- Updated Kubernetes dependency to 1.26
- `Volumesnapshotclass` supported by `vpc block csi` driver is made as default `snapshotclass`
- `Priorityclass` added in deployment for controller and node pods
- Removed `preStop` hook for csi-driver-registrar



## Version 5.0
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0}


### Revision 15, released 27 November 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-15}

- Resolves the following CVEs: [CVE-2007-4559](https://nvd.nist.gov/vuln/detail/CVE-2007-4559){: external}, [CVE-2023-4641](https://nvd.nist.gov/vuln/detail/CVE-2023-4641){: external}, and [CVE-2023-22745](https://nvd.nist.gov/vuln/detail/CVE-2023-22745){: external}.
- Updates the UBI to version `8.9-1029`.
- Updates Go to version `1.20.11`.
- security fix to use correct socket path as SElinux policy modules has been changed and CSI also recommending to use /var/lib/kubelet/plugins/.

### Revision 14, released 30 October 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-14}

- Resolves the following CVEs: [CVE-2023-4911](https://nvd.nist.gov/vuln/detail/CVE-2023-4911){: external}, [CVE-2023-4527](https://nvd.nist.gov/vuln/detail/CVE-2023-4527){: external}, [CVE-2023-4806](https://nvd.nist.gov/vuln/detail/CVE-2023-4806){: external}, [CVE-2023-4813](https://nvd.nist.gov/vuln/detail/CVE-2023-4813){: external}, [CVE-2023-39325](https://nvd.nist.gov/vuln/detail/CVE-2023-39325){: external}, and [CVE-2023-44487](https://nvd.nist.gov/vuln/detail/CVE-2023-44487){: external}.
- Updates the UBI to version `8.8-1072.1697626218`.
- Updates Go to version `1.20.10`.

### Revision 13, released 19 September 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-13}

- Resolves the following CVEs: [CVE-2023-34969](https://nvd.nist.gov/vuln/detail/CVE-2023-34969){: external}, [CVE-2023-28321](https://nvd.nist.gov/vuln/detail/CVE-2023-28321){: external}, [CVE-2023-2602](https://nvd.nist.gov/vuln/detail/CVE-2023-2602){: external}, [CVE-2023-2603](https://nvd.nist.gov/vuln/detail/CVE-2023-2603){: external}, [CVE-2023-28484](https://nvd.nist.gov/vuln/detail/CVE-2023-28484){: external}, [CVE-2023-29469](https://nvd.nist.gov/vuln/detail/CVE-2023-29469){: external}, [CVE-2023-27536](https://nvd.nist.gov/vuln/detail/CVE-2023-27536){: external}, [CVE-2023-3899](https://nvd.nist.gov/vuln/detail/CVE-2023-3899){: external}, and [CVE-2023-32681](https://nvd.nist.gov/vuln/detail/CVE-2023-32681){: external}.
- Updates the UBI to version `8.8-1037`.
- Updates Go to version `1.19.12`.

### Revision 12, released 01 August 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-12}

- Resolves the following CVEs: [CVE-2023-24329](https://nvd.nist.gov/vuln/detail/CVE-2023-24329){: external}, [CVE-2023-26604](https://nvd.nist.gov/vuln/detail/CVE-2023-26604){: external}, [CVE-2020-24736](https://nvd.nist.gov/vuln/detail/CVE-2020-24736){: external}, [CVE-2023-1667](https://nvd.nist.gov/vuln/detail/CVE-2023-1667){: external}, [CVE-2023-2283](https://nvd.nist.gov/vuln/detail/CVE-2023-2283){: external}, [CVE-2020-24736](https://nvd.nist.gov/vuln/detail/CVE-2020-24736){: external}, [CVE-2023-1667](https://nvd.nist.gov/vuln/detail/CVE-2023-1667){: external}, and [CVE-2023-2283](https://nvd.nist.gov/vuln/detail/CVE-2023-2283){: external}.
- Updates the UBI to version `8.8-1014`.
- Updates Go to version `1.19.10`.

### Revision 11, released 19 June 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-11}

- Resolves the following CVEs: [CVE-2022-43552](https://nvd.nist.gov/vuln/detail/CVE-2022-43552){: external}, [CVE-2022-3204](https://nvd.nist.gov/vuln/detail/CVE-2022-3204){: external}, [CVE-2023-27535](https://nvd.nist.gov/vuln/detail/CVE-2023-27535){: external}, [CVE-2022-36227](https://nvd.nist.gov/vuln/detail/CVE-2022-36227){: external}, [CVE-2022-35252](https://nvd.nist.gov/vuln/detail/CVE-2022-35252){: external}, [CVE-2023-29403](https://nvd.nist.gov/vuln/detail/CVE-2023-29403){: external}, [CVE-2023-29404](https://nvd.nist.gov/vuln/detail/CVE-2023-29404){: external}, [CVE-2023-29405](https://nvd.nist.gov/vuln/detail/CVE-2023-29405){: external}, [CVE-2023-29402](https://nvd.nist.gov/vuln/detail/CVE-2023-29402){: external}, [CVE-2023-29400](https://nvd.nist.gov/vuln/detail/CVE-2023-29400){: external}, [CVE-2023-24540](https://nvd.nist.gov/vuln/detail/CVE-2023-24540){: external}, and [CVE-2023-24539](https://nvd.nist.gov/vuln/detail/CVE-2023-24539){: external}.
- Updates the UBI to version `8.8-860`.
- Updates Go to version `1.19.10`.
- Token exchange URL is determined based on cluster provider. In case of satellite cluster provider, always use provided token exchange URL, if not provided use public IAM endpoint. 

### Revision 10, released 04 May 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-10}

- Resolves the following CVEs: [CVE-2023-0361](https://nvd.nist.gov/vuln/detail/CVE-2023-0361){: external}, [CVE-2023-24536](https://nvd.nist.gov/vuln/detail/CVE-2023-24536){: external}, [CVE-2023-24537](https://nvd.nist.gov/vuln/detail/CVE-2023-24537){: external}, and [CVE-2023-24538](https://nvd.nist.gov/vuln/detail/CVE-2023-24538){: external}.
- Updates the UBI to version `8.7-1107`.
- Updates Go to version `1.19.8`.

### Revision 9, released 04 April 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-9}

- Resolves the following CVEs: [CVE-2022-4304](https://nvd.nist.gov/vuln/detail/CVE-2022-4304){: external}, [CVE-2022-4450](https://nvd.nist.gov/vuln/detail/CVE-2022-4450){: external}, [CVE-2023-0215](https://nvd.nist.gov/vuln/detail/CVE-2023-0215){: external}, [CVE-2023-0286](https://nvd.nist.gov/vuln/detail/CVE-2023-0286){: external}, and [CVE-2023-24532](https://nvd.nist.gov/vuln/detail/CVE-2023-24532){: external}.
- Updates the UBI to version `8.7-1085.1679482090`.
- Updates Go to version `1.19.7`.

### Revision 8, released 21 March 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-8}

- Resolves the following CVEs: [CVE-2023-23916](https://nvd.nist.gov/vuln/detail/CVE-2023-23916){: external}.
- Updates the UBI to version `8.7-1085`.
- Updates Go to version `1.19.6`.

### Revision 7, released 06 March 2023
{: #cl-storage-templates-ibm-vpc-block-csi-driver-5.0-7}

- Resolves the following CVEs: [CVE-2022-4415](https://nvd.nist.gov/vuln/detail/CVE-2022-4415){: external}, [CVE-2020-10735](https://nvd.nist.gov/vuln/detail/CVE-2020-10735){: external}, [CVE-2021-28861](https://nvd.nist.gov/vuln/detail/CVE-2021-28861){: external}, [CVE-2022-45061](https://nvd.nist.gov/vuln/detail/CVE-2022-45061){: external}, [CVE-2022-40897](https://nvd.nist.gov/vuln/detail/CVE-2022-40897){: external}, [CVE-2022-41723](https://nvd.nist.gov/vuln/detail/CVE-2022-41723){: external}, and [CVE-2022-41724](https://nvd.nist.gov/vuln/detail/CVE-2022-41724){: external}.
- Updates the UBI to version `8.7-1085`.
- Updates Go to version `1.19.6`.
