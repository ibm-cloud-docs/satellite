---

copyright:
  years: 2020, 2023

lastupdated: "2023-11-29"


keywords: satellite storage, change log, version history, ibm vpc block csi driver

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# `ibm-vpc-block-csi-driver` change log
{: #cl-ibm-vpc-block-csi-driver}

Review the version history for the `ibm-vpc-block-csi-driver` {{site.data.keyword.satelliteshort}} storage template.
{: shortdesc}

## Version 5.1
{: #5.1-change-log}


### Revision 1, released 27 November 2023
{: #ibm-vpc-block-csi-driver-5.1-rev-1-change-log}


- Updates the UBI to version `8.9-1029`.
- Updates Go to version `1.20.10`.
- Fix for snapshot size to reflect actual source volume size 
- Retry fetching IAM token, if token exchange url is unreachable 
- Initial release
- Updated Kubernetes dependency to 1.26
- Volumesnapshotclass supported by vpc block csi driver is made as default snapshotclass
- Priorityclass added in deployment for controller and node pods
- Removed preStop hook for csi-driver-registrar


## Version 5.0
{: #5.0-change-log}


### Revision 15, released 27 November 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-15-change-log}


- Resolves the following CVEs: [CVE-2007-4559](https://nvd.nist.gov/vuln/detail/CVE-2007-4559){: external} [CVE-2023-4641](https://nvd.nist.gov/vuln/detail/CVE-2023-4641){: external} [CVE-2023-22745](https://nvd.nist.gov/vuln/detail/CVE-2023-22745){: external} 
- Updates the UBI to version `8.9-1029`.
- Updates Go to version `1.20.11`.
- security fix to use correct socket path as SElinux policy modules has been changed and CSI also recommending to use /var/lib/kubelet/plugins/.

### Revision 14, released 30 October 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-14-change-log}


- Resolves the following CVEs: [CVE-2023-4911](https://nvd.nist.gov/vuln/detail/CVE-2023-4911){: external} [CVE-2023-4527](https://nvd.nist.gov/vuln/detail/CVE-2023-4527){: external} [CVE-2023-4806](https://nvd.nist.gov/vuln/detail/CVE-2023-4806){: external} [CVE-2023-4813](https://nvd.nist.gov/vuln/detail/CVE-2023-4813){: external} [CVE-2023-39325](https://nvd.nist.gov/vuln/detail/CVE-2023-39325){: external} [CVE-2023-44487](https://nvd.nist.gov/vuln/detail/CVE-2023-44487){: external} 
- Updates the UBI to version `8.8-1072.1697626218`.
- Updates Go to version `1.20.10`.

### Revision 13, released 19 September 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-13-change-log}


- Resolves the following CVEs: [CVE-2023-34969](https://nvd.nist.gov/vuln/detail/CVE-2023-34969){: external} [CVE-2023-28321](https://nvd.nist.gov/vuln/detail/CVE-2023-28321){: external} [CVE-2023-2602](https://nvd.nist.gov/vuln/detail/CVE-2023-2602){: external} [CVE-2023-2603](https://nvd.nist.gov/vuln/detail/CVE-2023-2603){: external} [CVE-2023-28484](https://nvd.nist.gov/vuln/detail/CVE-2023-28484){: external} [CVE-2023-29469](https://nvd.nist.gov/vuln/detail/CVE-2023-29469){: external} [CVE-2023-27536](https://nvd.nist.gov/vuln/detail/CVE-2023-27536){: external} [CVE-2023-3899](https://nvd.nist.gov/vuln/detail/CVE-2023-3899){: external} [CVE-2023-32681](https://nvd.nist.gov/vuln/detail/CVE-2023-32681){: external} 
- Updates the UBI to version `8.8-1037`.
- Updates Go to version `1.19.12`.

### Revision 12, released 01 August 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-12-change-log}


- Resolves the following CVEs: [CVE-2023-24329](https://nvd.nist.gov/vuln/detail/CVE-2023-24329){: external} [CVE-2023-26604](https://nvd.nist.gov/vuln/detail/CVE-2023-26604){: external} [CVE-2020-24736](https://nvd.nist.gov/vuln/detail/CVE-2020-24736){: external} [CVE-2023-1667](https://nvd.nist.gov/vuln/detail/CVE-2023-1667){: external} [CVE-2023-2283](https://nvd.nist.gov/vuln/detail/CVE-2023-2283){: external} [CVE-2020-24736](https://nvd.nist.gov/vuln/detail/CVE-2020-24736){: external} [CVE-2023-1667](https://nvd.nist.gov/vuln/detail/CVE-2023-1667){: external} [CVE-2023-2283](https://nvd.nist.gov/vuln/detail/CVE-2023-2283){: external} 
- Updates the UBI to version `8.8-1014`.
- Updates Go to version `1.19.10`.

### Revision 11, released 19 June 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-11-change-log}


- Resolves the following CVEs: [CVE-2022-43552](https://nvd.nist.gov/vuln/detail/CVE-2022-43552){: external} [CVE-2022-3204](https://nvd.nist.gov/vuln/detail/CVE-2022-3204){: external} [CVE-2023-27535](https://nvd.nist.gov/vuln/detail/CVE-2023-27535){: external} [CVE-2022-36227](https://nvd.nist.gov/vuln/detail/CVE-2022-36227){: external} [CVE-2022-35252](https://nvd.nist.gov/vuln/detail/CVE-2022-35252){: external} [CVE-2023-29403](https://nvd.nist.gov/vuln/detail/CVE-2023-29403){: external} [CVE-2023-29404](https://nvd.nist.gov/vuln/detail/CVE-2023-29404){: external} [CVE-2023-29405](https://nvd.nist.gov/vuln/detail/CVE-2023-29405){: external} [CVE-2023-29402](https://nvd.nist.gov/vuln/detail/CVE-2023-29402){: external} [CVE-2023-29400](https://nvd.nist.gov/vuln/detail/CVE-2023-29400){: external} [CVE-2023-24540](https://nvd.nist.gov/vuln/detail/CVE-2023-24540){: external} [CVE-2023-24539](https://nvd.nist.gov/vuln/detail/CVE-2023-24539){: external} 
- Updates the UBI to version `8.8-860`.
- Updates Go to version `1.19.10`.
- Token exchange URL is determined based on cluster provider. In case of satellite cluster provider, always use provided token exchange URL, if not provided use public IAM endpoint. 

### Revision 10, released 04 May 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-10-change-log}


- Resolves the following CVEs: [CVE-2023-0361](https://nvd.nist.gov/vuln/detail/CVE-2023-0361){: external} [CVE-2023-24536](https://nvd.nist.gov/vuln/detail/CVE-2023-24536){: external} [CVE-2023-24537](https://nvd.nist.gov/vuln/detail/CVE-2023-24537){: external} [CVE-2023-24538](https://nvd.nist.gov/vuln/detail/CVE-2023-24538){: external} 
- Updates the UBI to version `8.7-1107`.
- Updates Go to version `1.19.8`.

### Revision 9, released 04 April 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-9-change-log}


- Resolves the following CVEs: [CVE-2022-4304](https://nvd.nist.gov/vuln/detail/CVE-2022-4304){: external} [CVE-2022-4450](https://nvd.nist.gov/vuln/detail/CVE-2022-4450){: external} [CVE-2023-0215](https://nvd.nist.gov/vuln/detail/CVE-2023-0215){: external} [CVE-2023-0286](https://nvd.nist.gov/vuln/detail/CVE-2023-0286){: external} [CVE-2023-24532](https://nvd.nist.gov/vuln/detail/CVE-2023-24532){: external} 
- Updates the UBI to version `8.7-1085.1679482090`.
- Updates Go to version `1.19.7`.

### Revision 8, released 21 March 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-8-change-log}


- Resolves the following CVEs: [CVE-2023-23916](https://nvd.nist.gov/vuln/detail/CVE-2023-23916){: external} 
- Updates the UBI to version `8.7-1085`.
- Updates Go to version `1.19.6`.

### Revision 7, released 06 March 2023
{: #ibm-vpc-block-csi-driver-5.0-rev-7-change-log}


- Resolves the following CVEs: [CVE-2022-4415](https://nvd.nist.gov/vuln/detail/CVE-2022-4415){: external} [CVE-2020-10735](https://nvd.nist.gov/vuln/detail/CVE-2020-10735){: external} [CVE-2021-28861](https://nvd.nist.gov/vuln/detail/CVE-2021-28861){: external} [CVE-2022-45061](https://nvd.nist.gov/vuln/detail/CVE-2022-45061){: external} [CVE-2022-40897](https://nvd.nist.gov/vuln/detail/CVE-2022-40897){: external} [CVE-2022-41723](https://nvd.nist.gov/vuln/detail/CVE-2022-41723){: external} [CVE-2022-41724](https://nvd.nist.gov/vuln/detail/CVE-2022-41724){: external} 
- Updates the UBI to version `8.7-1085`.
- Updates Go to version `1.19.6`.


