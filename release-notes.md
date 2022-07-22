---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-22"

keywords: satellite, hybrid, multicloud, release notes, changes

subcollection: satellite

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}


# Release notes
{: #satellite-relnotes}

Use the release notes to learn about the latest changes to the {{site.data.keyword.satellitelong}} documentation that are grouped by month.
{: shortdesc}

## July 2022
{: #satellite-july22}

### 22 July 2022
{: #satellite-july2222}
{: release-note}

New! Beta support for the VMware CSI driver template
:   You can use the VMware CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [VMware CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-vmware-csi).


### 15 July 2022
{: #satellite-july1522}
{: release-note}

New information about using a custom Network Time Protocol (NTP) server
:   You can now define a custom NTP server for your RHCOS hosts. For more information, see the [Specifying a custom Network Time Protocol (NTP) server](/docs/satellite?topic=satellite-config-custom-ntp).

## June 2022
{: #satellite-june22}

### 21 June 2022
{: #satellite-june2122}
{: release-note}

New and updated template parameter for the Azure File CSI driver template.
:   The `subnetName` parameter is now required when you create a configuration. Additionally, you must provide the **one** of the subnet names under the `vnet`. If the nodes are distributed across multiple subnets, you can manually add the subnet names after you create the configuration. For more information, see the [Azure File CSI Driver documentation](/docs/satellite?topic=satellite-config-storage-azurefile-csi).


### 16 June 2022
{: #satellite-june1622}
{: release-note}

New! You can now create CoreOS-enabled {{site.data.keyword.satelliteshort}} locations in the **Washington D.C.** (`wdc`, `us-east`) region.
:   To create a CoreOS-enabled location in the Washington D.C. region, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about-locations).

### 15 June 2022
{: #satellite-june1522}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.420](/docs/satellite?topic=satellite-satellite-cli-changelog).

New! You can now create CoreOS-enabled {{site.data.keyword.satelliteshort}} locations in the **London** (`lon`, `eu-gb`) region.
:   To create a CoreOS-enabled location in the London region, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about-locations).

### 14 June 2022
{: #satellite-june1422}
{: release-note}
 
Satellite for resellers
:   Learn how to set up an enterprise account and resell {{site.data.keyword.satelliteshort}} infrastructure and services to your clients. For more information, see the [{{site.data.keyword.satelliteshort}} [reseller use case](/docs/satellite?topic=satellite-tenancy-model).

### 9 June 2022
{: #satellite-june922}
{: release-note}

Satellite use cases
:   See [Satellite use cases](/docs/satellite?topic=satellite-use-case) for the benefits of using Satellite and some common use cases.

### 1 June 2022
{: #satellite-june122}
{: release-note}

Getting started page redesign
:   Updated the [Getting started with {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-getting-started) page with terms, basic location information, and what you can do next.

New! Think you know {{site.data.keyword.satelliteshort}}? Take a quiz!
:   After you read about {{site.data.keyword.satelliteshort}} terms on the [Getting started with {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-getting-started#sat-key-terms) page, [take a quiz!](https://ibmcloud-quizzes.mybluemix.net/satellite/terms_quiz/quiz.php){: external} and find out how much you learned.

## May 2022
{: #satellite-may22}

### 26 May 2022
{: #satellite-may2622}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.415](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 16 May 2022
{: #satellite-may1622}
{: release-note}

New! You can now create CoreOS-enabled {{site.data.keyword.satelliteshort}} locations in the **Tokyo** (`tok`, `jp-tok`) region.
:   To create a CoreOS-enabled location in the Tokyo region, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about-locations).

### 13 May 2022
{: #satellite-may1322}
{: release-note}

New! Added {{site.data.keyword.messagehub_full}} as a {{site.data.keyword.satelliteshort}}-enabled service
:   See [Supported {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services) and [About {{site.data.keyword.satellitelong_notm}} for {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-satellite_about).


New location error message is added. 
:   For more information see [R0058: DNS registration is failing](/docs/satellite?topic=satellite-ts-locations-debug#R0058).



### 12 May 2022
{: #satellite-may1222}
{: release-note}

New! {{site.data.keyword.satelliteshort}} configuration and assignment update commands
:   You can now update and upgrade your {{site.data.keyword.satelliteshort}} storage configurations and assignments from the CLI. For more information, see the [{{site.data.keyword.satelliteshort}} storage commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-storage-commands).

## April 2022
{: #satellite-apr22}

### 28 April 2022
{: #satellite-apr2822}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.404](/docs/satellite?topic=satellite-satellite-cli-changelog).


### 26 April 2022
{: #satellite-apr2622}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.403](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 20 April 2022
{: #satellite-apr2022}
{: release-note}

New! Osaka location
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Osaka** (`osa`, `jp-osa`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions).

### 18 April 2022
{: #satellite-apr1822}
{: release-note}

Set up an HTTP proxy for your hosts
:   In allowlisted accounts that have locations enabled for Red Hat CoreOS, you can configure an HTTP proxy. For more information, see [Configuring an HTTP proxy for your Satellite hosts](/docs/satellite?topic=satellite-config-http-proxy).

New and updated template parameters for the local block and local file storage templates.
:   Automatic disk discovery is now available for the `local-volume-block` and `local-volume-file` template version 4.9. Enable this feature by setting the `auto-disk-discovery=true` parameter. For more information, see [Local block storage](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-local-block-cli) and [Local file storage](/docs/satellite?topic=satellite-config-storage-local-file).



### 12 April 2022
{: #satellite-apr1222}
{: release-note}

New! Support for Red Hat CoreOS
:   You can add [Red Hat CoreOS](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os) hosts to your Satellite location. Note that Red Hat CoreOS enabled locations are currently available only in new locations that are managed from Dallas (`us-south`) or Frankfurt (`eu-de`). You can't migrate existing locations. For more information, see [Creating a location](/docs/satellite?topic=satellite-locations).

### 6 April 2022
{: #satellite-apr0622}
{: release-note}

New! Check your host setup before attaching hosts to your location.
:   You can now use the `sat-host-check` to verify your hosts meets the requirements for {{site.data.keyword.satelliteshort}}. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).


### 4 April 2022
{: #satellite-apr0422}
{: release-note}

{{site.data.keyword.IBM_notm}} Spectrum Scale template
:   The [{{site.data.keyword.IBM_notm}} Spectrum Scale driver](/docs/satellite?topic=satellite-config-storage-spectrum-scale) is now deprecated. Please reach out to scale@us.ibm.com for further compatibility details and options.

## March 2022
{: #satellite-mar22}

### 21 March 2022
{: #satellite-mar2122}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.384](/docs/satellite?topic=satellite-satellite-cli-changelog).


### 10 March 2022
{: #satellite-mar1022}
{: release-note}

New! Beta support for the {{site.data.keyword.block_storage_is_short}} CSI driver template
:   You can use the {{site.data.keyword.block_storage_is_short}} CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.block_storage_is_short}} CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-vpc-csi).

### 3 March 2022
{: #satellite-mar322}
{: release-note}


New! Beta support for the Google Compute Engine CSI driver template
:   You can use the Google Compute Engine CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [Google Compute Engine CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-gcp-csi).


## February 2022
{: #satellite-feb22}

### 28 February 2022
{: #satellite-feb2822}
{: release-note}

Host outbound connectivity
:   Updated the [host outbound network requirements](/docs/satellite?topic=satellite-reqs-host-network) page to include Red Hat Container Registry.


### 24 February 2022
{: #satellite-feb2422}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.374](/docs/satellite?topic=satellite-satellite-cli-changelog).


{{site.data.keyword.keymanagementservicefull_notm}} 
:   {{site.data.keyword.keymanagementservicefull_notm}} is added as a supported managed service for {{site.data.keyword.satelliteshort}}. For more information, see [Supported {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services) and [About {{site.data.keyword.keymanagementserviceshort}} for {{site.data.keyword.satelliteshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cos-satellite).

New troubleshooting issue for errors when you run the `oc debug` command 
:   See [Why can't I log in to my worker nodes or debug them with `oc debug` command?](/docs/satellite?topic=satellite-ts-cluster-ocdebug).

### 18 February 2022
{: #satellite-feb1822}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.372](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 16 February 2022
{: #satellite-feb1622}
{: release-note}

Host connectivity
:   Updated the [outbound connectivity for hosts](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) page to include hostnames as well as IP addresses.


### 10 February 2022
{: #satellite-feb1022}
{: release-note}

New! Beta support for the Azure File CSI driver template
:   You can use the Azure File CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [Azure File CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-azurefile-csi).

## January 2022
{: #satellite-jan22}

Review the release notes for January 2022.
{: shortdesc}

### 27 January 2022
{: #satellite-jan2722}
{: release-note}

{{site.data.keyword.cos_full_notm}} 
:   {{site.data.keyword.cos_full_notm}} is added as a supported managed service for {{site.data.keyword.satelliteshort}}. For more information, see [Supported {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services) and [About {{site.data.keyword.cos_short}} for {{site.data.keyword.satelliteshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cos-satellite).

### 25 January 2022
{: #satellite-jan2522}
{: release-note}

OpenShift Data Foundation
:   New and updated template parameters for the OpenShift Data Foundation {{site.data.keyword.satelliteshort}} templates.
    - Automatic disk discovery is now available for the `odf-local` template version 4.8. Enable this feature by setting the `auto-disk-discovery=true` parameter. For more information, see [ODF using local disks](/docs/satellite?topic=satellite-config-storage-odf-local).
    - The `monDevicePaths` and `monSize` parameters are no longer required for the `odf-local` template version 4.8.
    - The `monStorageClassName` and `monSize` parameters are no longer required for the `odf-remote` template version 4.8.


### 18 January 2022
{: #satellite-jan1822}
{: release-note}

New! {{site.data.keyword.satellitelong_notm}} CLI Map
:   The [{{site.data.keyword.satellitelong_notm}} CLI Map](/docs/satellite?topic=satellite-icsat_map) lists all `ibmcloud sat` commands as they are structured in the CLI. Use this page as a visual reference for how ibmcloud sat commands are organized, or to quickly find a specific command. 


New! Site map
:   A site map is available for the {{site.data.keyword.satelliteshort}} [documentation](/docs/satellite?topic=satellite-sitemap).

## December 2021
{: #satellite-dec21}

Review the release notes for December 2021.
{: shortdesc}


### 15 December 2021
{: #satellite-dec1521}
{: release-note}

New! Sydney location
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Sydney** (`syd`, `au-syd`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 


### 3 December 2021
{: #satellite-dec321}
{: release-note}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.353](/docs/satellite?topic=satellite-satellite-cli-changelog).


## November 2021
{: #satellite-nov21}

Review the release notes for November 2021.
{: shortdesc}

### 29 November 2021
{: #satellite-nov2921}
{: release-note}


{{site.data.keyword.redhat_openshift_notm}} Data Foundation
:   Updated deployment parameters for {{site.data.keyword.redhat_openshift_notm}} Data Foundation. The `container-private-endpoint` parameter is no longer required.

{{site.data.keyword.at_short}}
:   New {{site.data.keyword.at_short}} events are available for {{site.data.keyword.satelliteshort}} storage. For more information, see [Auditing events](/docs/satellite?topic=satellite-at_events).

### 16 November 2021
{: #satellite-nov1621}
{: release-note}

**New! Sao Paulo location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Sao Paulo** (`sao`, `br-sao`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 

### 11 November 2021
{: #satellite-nov1121}
{: release-note}

New location error message is added. 
:   For more information see [R0052: Ingress certificate generation issues](/docs/satellite?topic=satellite-ts-locations-debug#R0052).

### 10 November 2021
{: #satellite-nov1021}
{: release-note}

Troubleshooting a `certificate expired` error for the {{site.data.keyword.redhat_openshift_notm}} console
:   For more information see [Why can't I access the {{site.data.keyword.redhat_openshift_notm}} web console?](/docs/satellite?topic=satellite-ts-sat-ocp-console).

### 9 November 2021
{: #satellite-nov921}
{: release-note}

Azure Disk CSI driver template
:   Previously, when you created a storage configuration by using the Azure Disk storage template, you passed your Azure configuration parameters as one base64 encoded string. Now, each of the Azure configuration parameters is exposed as a command-line option. For more information, see [Azure Disk CSI driver](/docs/satellite?topic=satellite-config-storage-azure-csi).

### 8 November 2021
{: #satellite-nov821}
{: release-note}

Troubleshooting a degraded Ingress operator
:   For more information, see [Why is my Ingress operator in a warning state?](/docs/satellite?topic=satellite-ts-degraded-ingress).


## October 2021
{: #satellite-oct21}

Review the release notes for October 2021.
{: shortdesc}

### 12 October 2021
{: #satellite-oct1221}
{: release-note}

Review the release notes for 12 October 2021.
{: shortdesc}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.327](/docs/satellite?topic=satellite-satellite-cli-changelog).


## September 2021
{: #satellite-sep21}

Review the release notes for September 2021.
{: shortdesc}

### 29 September 2021
{: #satellite-sep2921}
{: release-note}

Review the release notes for 29 September 2021.
{: shortdesc}

**New! Alibaba host support**
:   Alibaba Cloud hosts are now supported for your {{site.data.keyword.satelliteshort}} location.

**New! Toronto location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Toronto** (`tor`, `ca-tor`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 

### 23 September 2021
{: #satellite-sep2321}
{: release-note}

Review the release notes for 23 September 2021.
{: shortdesc}

**New! Tokyo location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Tokyo** (`tok`, `jp-tok`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 


### 15 September 2021
{: #satellite-sep1521}
{: release-note}

Review the release notes for 15 September 2021.
{: shortdesc}

OpenShift Data Foundation
:   OpenShift Data Foundation is generally available for {{site.data.keyword.satelliteshort}} cluster. For more information see, the following links.
    - [Understanding OpenShift Data Foundation](/docs/openshift?topic=openshift-ocs-storage-prep).
    - [Deploying OpenShift Data Foundation using local disks](/docs/satellite?topic=satellite-config-storage-odf-local).
    - [Deploying OpenShift Data Foundation using remote, dynamically provisioned disks](/docs/satellite?topic=satellite-config-storage-odf-remote).


## August 2021
{: #satellite-aug21}

Review the release notes for August 2021.
{: shortdesc}

### 9 August 2021
{: #satellite-aug921}
{: release-note}

Review the release notes for 9 August 2021.
{: shortdesc}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.312](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 3 August 2021
{: #satellite-aug321}
{: release-note}

Review the release notes for 3 August 2021.
{: shortdesc}

New! Dallas location
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Dallas** (`dal`, `us-south`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions).

## July 2021
{: #satellite-jul21}

Review the release notes for July 2021.
{: shortdesc}

### 19 July 2021
{: #satellite-jul1921}
{: release-note}

Review the release notes for 19 July 2021.
{: shortdesc}

End of UDP endpoint support
:   The UDP protocol is no longer supported for `cloud` and `location` endpoints that you create in {{site.data.keyword.satelliteshort}} Link. For more information, see [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).

### 16 July 2021
{: #satellite-jul1621}
{: release-note}

Review the release notes for 16 July 2021.
{: shortdesc}

New! Beta support for the Azure Disk CSI driver template
:   You can use the Azure Disk CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [Azure Disk CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-azure-csi).

### 8 July 2021
{: #satellite-jul821}
{: release-note}

Review the release notes for 8 July 2021.
{: shortdesc}

DNS provider change from Cloudflare to Akamai
:   On 08 July, the DNS provider is changed from Cloudflare to Akamai for all `containers.cloud.ibm.com` domains. Previously, to use {{site.data.keyword.satelliteshort}} Config and to access the {{site.data.keyword.satelliteshort}} Link API, you were required to allow outbound connectivity from your control plane hosts to the IP addresses for Cloudflare's proxied load balancers. Now, you must instead allow access to [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} on TCP ports 80 and 443. After the migration completes, you can remove the Cloudflare IP address rules.

## June 2021
{: #satellite-jun21}

Review the release notes for June 2021.
{: shortdesc}

### 30 June 2021
{: #satellite-jun3021}
{: release-note}

Review the release notes for 30 June 2021.
{: shortdesc}

New! General availability of the {{site.data.keyword.satelliteshort}} storage templates. 
:   For more information about storage templates, see [Understanding storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).

New! {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services</strong>: Many {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} are now available to deploy to your {{site.data.keyword.satelliteshort}} location. 
:   For more information, see the [{{site.data.keyword.cloud_notm}} blog](https://www.ibm.com/cloud/blog/announcements/deploy-managed-cloud-native-databases-anywhere-with-ibm-cloud-satellite){: external}.

### 24 June 2021
{: #satellite-jun2421}
{: release-note}

Review the release notes for 24 June 2021.
{: shortdesc}

Azure template
:   Automate the setup of a {{site.data.keyword.satelliteshort}} location in Microsoft Azure cloud infrastructure by using a [{{site.data.keyword.bpshort}}](/docs/satellite?topic=satellite-azure#azure-template).

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.295](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 18 June 2021
{: #satellite-jun1821}
{: release-note}

Review the release notes for 18 June 2021.
{: shortdesc}

IP addresses for Link tunnel server endpoints
:   Updates the [required outbound connectivity for hosts](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) to include IP addresses for Link tunnel server endpoints.

### 9 June 2021
{: #satellite-jun921}
{: release-note}

Review the release notes for 9 June 2021.
{: shortdesc}

New! Frankfurt location
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the Frankfurt (`fra`, `eu-de`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions).

Digital certificates
:   Added information about [certificates for various {{site.data.keyword.satellitelong_notm}} domains and hosts](/docs/satellite?topic=satellite-compliance#certs-hosts-domains).

## May 2021
{: #satellite-may21}

Review the release notes for May 2021.
{: shortdesc}

### 26 May 2021
{: #satellite-may2621}
{: release-note}

Review the release notes for 26 May 2021.
{: shortdesc}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.258](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 19 May 2021
{: #satellite-may1921}
{: release-note}

Review the release notes for 19 May 2021.
{: shortdesc}

New! NetApp ONTAP-NAS storage driver template
:   You can use the NetApp ONTAP-NAS storage driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [NetApp ONTAP-NAS storage driver documentation](/docs/satellite?topic=satellite-config-storage-netapp-nas).

### 12 May 2021
{: #satellite-may1221}
{: release-note}

Review the release notes for 12 May 2021.
{: shortdesc}

New! {{site.data.keyword.IBM_notm}} Systems block storage CSI driver template
:   You can use the {{site.data.keyword.IBM_notm}} Systems block storage CSI driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.IBM_notm}} Systems block storage CSI driver documentation](/docs/satellite?topic=satellite-config-storage-block-csi).

### 7 May 2021
{: #satellite-may721}
{: release-note}

Review the release notes for 7 May 2021.
{: shortdesc}

New! Spectrum Scale CSI driver template
:   You can use the {{site.data.keyword.IBM_notm}} Spectrum Scale Container Storage Interface (CSI) driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.IBM_notm}} Spectrum Scale driver documentation](/docs/satellite?topic=satellite-config-storage-spectrum-scale).

## April 2021
{: #satellite-apr21}

Review the release notes for April 2021.
{: shortdesc}

### 26 April 2021
{: #satellite-apr2621}
{: release-note}

Review the release notes for 26 April 2021.
{: shortdesc}

CLI change log
Updated the CLI plug-in change log page for the [release of version 1.0.258](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 15 April 2021
{: #satellite-apr1521}
{: release-note}

Review the release notes for 15 April 2021.
{: shortdesc}

Cluster add-ons
:   Added the [list](/docs/satellite?topic=satellite-addon-errors) of supported add-ons for clusters in {{site.data.keyword.satelliteshort}} locations.

Use case
:   Added an example use case for using [{{site.data.keyword.satelliteshort}} edge environments for AI, IoT, and machine learning](/docs/satellite?topic=satellite-edge-usecase).

:   {{site.data.keyword.cos_short}} buckets
When you create a location from the console, you can now [enter the name of an existing {{site.data.keyword.cos_full_notm}} bucket](/docs/satellite?topic=satellite-locations#location-create-console) that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data.

### 9 April 2021
{: #satellite-apr921}
{: release-note}

Review the release notes for 9 April 2021.
{: shortdesc}

Logging
:   Expanded information for collecting and analyzing logs for your {{site.data.keyword.satelliteshort}} location, including the types of available logs, how to set up log alerts, and how to use logs to troubleshoot location and host errors.

New! Storage template for remote OpenShift Data Foundation
:   The {{site.data.keyword.satelliteshort}} storage template for [using OpenShift Container Storage with remote devices](/docs/satellite?topic=satellite-config-storage-odf-remote) is now available.

## March 2021
{: #satellite-mar21}

Review the release notes for March 2021.
{: shortdesc}

### 25 March 2021
{: #satellite-mar2521}
{: release-note}

Review the release notes for 25 March 2021.
{: shortdesc}

Security and compliance
:   Added an [overview of security and compliance](/docs/satellite?topic=satellite-compliance) information for {{site.data.keyword.satelliteshort}}.

### 23 March 2021
{: #satellite-mar2321}
{: release-note}

Review the release notes for 23 March 2021.
{: shortdesc}

OpenShift Data Foundation using local disks
:   Added steps for removing an ODF configuration and updated the [configuration parameter reference](/docs/satellite?topic=satellite-config-storage-odf-local#sat-storage-odf-local-params-cli).

### 12 March 2021
{: #satellite-mar1221}
{: release-note}

Review the release notes for 12 March 2021.
{: shortdesc}

Resetting the host key
:   In the event of a potential security violation, you can [reset the key](/docs/satellite?topic=satellite-host-update-location#host-key-reset) that the control plane uses to communicate with all the hosts in the {{site.data.keyword.satelliteshort}} location.

Troubleshooting location health checks
:   Added steps for troubleshooting when [{{site.data.keyword.cloud_notm}} is unable to check a location's health](/docs/satellite?topic=satellite-ts-location-healthcheck).

### 10 March 2021
{: #satellite-mar1021}
{: release-note}

Review the release notes for 10 March 2021.
{: shortdesc}

New `health-pending` host state
:   To review the state of your hosts, see [Viewing host health](/docs/satellite?topic=satellite-monitor#host-health).

### 1 March 2021
{: #satellite-mar0121}
{: release-note}

Review the release notes for 1 March 2021.
{: shortdesc}

New! General availability of {{site.data.keyword.satellitelong}}
:   {{site.data.keyword.satellitelong_notm}} is now generally available. If you created locations during the closed beta, you must [re-create your locations](/docs/satellite?topic=satellite-location-copy) for the general availability release. For more information about what you are charged for when you use {{site.data.keyword.satellitelong_notm}}, see the [FAQs](/docs/satellite?topic=satellite-faqs#pricing).

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.233](/docs/satellite?topic=satellite-satellite-cli-changelog).

Cloud provider guides
:   Added instructions to set up {{site.data.keyword.satelliteshort}} locations that use infrastructure from [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), and [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm).

Exposing apps in {{site.data.keyword.satelliteshort}} clusters
:   Added an overview of the [options for exposing apps in Satellite clusters](/docs/openshift?topic=openshift-sat-expose-apps) and steps for setting up each option.

{{site.data.keyword.satelliteshort}} Infrastructure Service
:   [Order managed infrastructure from {{site.data.keyword.IBM_notm}}](/docs/satellite?topic=satellite-infrastructure-service) to create a {{site.data.keyword.satelliteshort}} location for you in your on-premises data center.
  
New! Template for fast provisioning on AWS
:   [Automate your {{site.data.keyword.satelliteshort}} location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template), available for AWS infrastructure.

## February 2021
{: #satellite-feb21}

Review the release notes for February 2021.
{: shortdesc}

### 25 February 2021
{: #satellite-feb2521}
{: release-note}

Review the release notes for 25 February 2021.
{: shortdesc}

Auditing events
:   Added a topic for [auditing {{site.data.keyword.satelliteshort}} events with {{site.data.keyword.cloudaccesstrailshort}}](/docs/satellite?topic=satellite-at_events).

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.223](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 23 February 2021
{: #satellite-feb2321}
{: release-note}

Review the release notes for 23 February 2021.
{: shortdesc}

Accessing clusters
:   Added steps for testing [access to a {{site.data.keyword.satelliteshort}} cluster by using the public network](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

Host requirements
:   Added the minimum required disk storage, storage device IOPS, and network bandwidth to the [hosts requirements](/docs/satellite?topic=satellite-host-reqs).
  
Link endpoints
:   Added FAQs about [{{site.data.keyword.satelliteshort}} Link network security](/docs/satellite?topic=satellite-link-location-cloud#link-security).

Monitoring
:   Added information about how to [set up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} location platform metrics](/docs/satellite?topic=satellite-monitor).

Reviewing resources
:   Added a topic for [reviewing resources that are managed by {{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-satcon-manage#satconfig-resources).

### 18 February 2021
{: #satellite-feb1821}
{: release-note}

Review the release notes for 18 February 2021.
{: shortdesc}

Securing service connections
:   Added a [{{site.data.keyword.satelliteshort}} connection overview](/docs/satellite?topic=satellite-service-connection) to describe user access to resources that run in your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location.

### 12 February 2021
{: #satellite-feb1221}
{: release-note}

Review the release notes for 12 February 2021.
{: shortdesc}

Link endpoints
:   Added information about [default Link endpoints](/docs/satellite?topic=satellite-default-link-endpoints) that are automatically created for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location.

Securing service connections
:   Added a topic to explain all points of access to your {{site.data.keyword.satelliteshort}} location. For more information, see [Securing your connection to Satellite](/docs/satellite?topic=satellite-service-connection).

### 8 February 2021
{: #satellite-feb0821}
{: release-note}

Review the release notes for 8 February 2021.
{: shortdesc}

CLI change log
:   Updated the CLI plug-in change log page for the [release of version 1.0.223](/docs/satellite?topic=satellite-satellite-cli-changelog).

Host auto assignment
:   Added information about how [{{site.data.keyword.satelliteshort}} can automatically assign hosts](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov) to worker pools in clusters or {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that use host labels to request compute capacity.

Logging and monitoring
:   Added information about how to set up [logging and monitoring for {{site.data.keyword.satelliteshort}} health](/docs/satellite?topic=satellite-health).

Worker pool management
:   Described how to manage [worker pools in {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters), such as host labels for host auto assignment.

## January 2021
{: #satellite-jan21}

Review the release notes for January 2021.
{: shortdesc}

### 29 January 2021
{: #satellite-jan2921}
{: release-note}

Review the release notes for 29 January 2021.
{: shortdesc}

AWS hosts
:   Added the list of supported AWS EC2 instance types that can be used as hosts.

{{site.data.keyword.satelliteshort}} Link
:   Added information about the [maximum number of endpoints](/docs/satellite?topic=satellite-requirements#reqs-link) that you can create for one location.

### 25 January 2021
{: #satellite-jan2521}
{: release-note}

Review the release notes for 25 January 2021.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Link
:   Expanded information about endpoint security and access controls, and added example use cases.

### 15 January 2021
{: #satellite-jan1521}
{: release-note}

Review the release notes for 15 January 2021.
{: shortdesc}

Host requirements
:   Updated the [{{site.data.keyword.redhat_notm}} package repositories](/docs/satellite?topic=satellite-host-reqs) that you must enable on hosts.

### 12 January 2021
{: #satellite-jan1221}
{: release-note}

Review the release notes for 12 January 2021.
{: shortdesc}



Host updates
:   Added how to update hosts that are used as worker nodes in [clusters](/docs/satellite?topic=satellite-host-update-workers) and the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-host-update-location).

## December 2020
{: #satellite-dec20}

Review the release notes for December 2020.
{: shortdesc}

### 9 December 2020
{: #satellite-dec0920}
{: release-note}

Review the release notes for 9 December 2020.
{: shortdesc}

Private hosts
:   Hosts with only private network connectivity are now supported for your {{site.data.keyword.satelliteshort}} location. If hosts have multiple IPv4 network interfaces, the lowest-order, non-loopback network interface with a valid IPv4 address is used as the primary network interface for the hosts. Note that if your hosts have private network connectivity only, authorized users must be in the private host network directly or through a VPN connection to access services that run in your location, such as {{site.data.keyword.redhat_openshift_notm}} clusters.

Azure host support
:   Microsoft Azure hosts are now supported for your {{site.data.keyword.satelliteshort}} location.

Physical machine support
:   Physical machine hosts are now supported for your {{site.data.keyword.satelliteshort}} location.

Host network requirements
:   Updated the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) on hosts' primary networks.

AWS and GCP host DNS
:   When you use AWS and GCP hosts for your {{site.data.keyword.satelliteshort}} location, the requirement to manually configure DNS for the location control plane and for cluster load balancing is removed. The hosts' private IP addresses are automatically registered in DNS.

## November 2020
{: #satellite-nov20}

Review the release notes for November 2020.
{: shortdesc}

### 18 November 2020
{: #satellite-nov1820}
{: release-note}

Review the release notes for 18 November 2020.
{: shortdesc}

Internal registry
:   Added information on how to [Set up the internal container image registry](/docs/openshift?topic=openshift-satellite-clusters#satcluster-internal-registry) for {{site.data.keyword.redhat_openshift_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} locations.

Service overview
:   Added an [About {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-faqs) topic to help you learn about {{site.data.keyword.satelliteshort}} terminology, service architecture, and components.

## October 2020
{: #satellite-oct20}

Review the release notes for October 2020.
{: shortdesc}

### 8 October 2020
{: #satellite-oct0820}
{: release-note}

Review the release notes for 8 October 2020.
{: shortdesc}

Host network requirements
:   Added the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) on hosts' public networks.

### 6 October 2020
{: #satellite-oct0620}
{: release-note}

Review the release notes for 6 October 2020.
{: shortdesc}

CLI change log
:   Added a CLI plug-in change log page for the [release of version 1.0.178](/docs/satellite?topic=satellite-satellite-cli-changelog).

Location subdomain troubleshooting
:   Added steps for further troubleshooting when [the location subdomain does not route traffic to control plane hosts](/docs/satellite?topic=satellite-ts-location-subdomain).

## September 2020
{: #satellite-sep20}

Review the release notes for September 2020.
{: shortdesc}

### 14 September 2020
{: #satellite-sep1420}
{: release-note}



Review the release notes for 14 September 2020.
{: shortdesc}

Host requirements
:   Clarified that the `localhost` value must resolve to a valid local IP address, typically `127.0.0.1`, on a [host](/docs/satellite?topic=satellite-reqs-host-network).

IAM
:   Updated [assigning access to {{site.data.keyword.satelliteshort}} Config in {{site.data.keyword.cloud_notm}} IAM](/docs/satellite?topic=satellite-iam#iam-resource-types) to note that you cannot scope access policies to specific configurations, subscriptions, or {{site.data.keyword.cloud_notm}} resource group.

Provider documentation
:   Updated the provider requirements, such as examples for installing RHEL packages on GCP hosts before you attach the host to your {{site.data.keyword.satelliteshort}} location.
  
Service requirements
:   Added that you can provision up to [40 {{site.data.keyword.cloud_notm}} service instances per {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-requirements#reqs-services).

## August 2020
{: #satellite-aug20}

Review the release notes for August 2020.
{: shortdesc}

### 21 August 2020
{: #satellite-aug2120}
{: release-note}

Review the release notes for 21 August 2020.
{: shortdesc}

New! {{site.data.keyword.satellitelong_notm}} is now available as a closed beta.
:   For more information about the service and how it works, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}. To register for the beta, go [here](https://cloud.ibm.com/satellite/overview){: external}.


