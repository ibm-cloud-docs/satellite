---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-17"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Release notes
{: #release-notes}

Use the release notes to learn about the latest changes to the {{site.data.keyword.satelliteshort}} documentation that are grouped by month.
{: shortdesc}



## September 2021
{: #release-sep-2021}

Review the release notes for September 2021.
{: shortdesc}



### 15 September 2021
{: #15sep2021}
{: release-note}

Review the release notes for 15 September 2021.
{: shortdesc}

OpenShift Data Foundation
:   OpenShift Data Foundation is generally available for {{site.data.keyword.satelliteshort}} clusters. For more information see, the following links.
    - [Understanding OpenShift Data Foundation](/docs/openshift?topic=openshift-ocs-storage-prep).
    - [Deploying OpenShift Data Foundation using local disks](/docs/satellite?topic=satellite-config-storage-ocs-local).
    - [Deploying OpenShift Data Foundation using remote, dynamically provisioned disks](/docs/satellite?topic=satellite-config-storage-ocs-remote).



## August 2021
{: #aug21}

| Date | Description |
| --- | --- |
| 09 August 2021 | **CLI changelog**: Updated the CLI plug-in changelog page for the [release of version 1.0.312](/docs/satellite?topic=satellite-satellite-cli-reference). |
| 03 August 2021 | **New! Dallas location**: You can now manage {{site.data.keyword.satelliteshort}} locations from the **Dallas** (`dal`, `us-south`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in August 2021."}

## July 2021
{: #july21}

| Date | Description |
| ---- | ----------- |
| 19 July 2021 | **End of UDP endpoint support**: The UDP protocol is no longer supported for `cloud` and `location` endpoints that you create in {{site.data.keyword.satelliteshort}} Link. For more information, see [Encyrption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).  |
| 16 July 2021 | **New! Beta support for the Azure Disk CSI driver template**: You can use the Azure Disk CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [Azure Disk CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-azure-csi). |
| 08 July 2021 | On 08 July, the DNS provider is changed from Cloudflare to Akamai for all `containers.cloud.ibm.com` domains. Previously, to use {{site.data.keyword.satelliteshort}} Config and to access the {{site.data.keyword.satelliteshort}} Link API, you were required to allow outbound connectivity from your control plane hosts to the IP addresses for Cloudflare's proxied load balancers. Now, you must instead allow access to [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} on TCP ports 80 and 443. After the migration completes, you can remove the Cloudflare IP address rules. |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in July 2021."}

## June 2021
{: #june21}

| Date | Description |
| ---- | ----------- |
| 30 June 2021 | <ul><li><strong>New!</strong> General availability of the {{site.data.keyword.satelliteshort}} storage templates. For more information about storage templates, see [Understanding storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).</li><li><strong>New! {{site.data.keyword.satelliteshort}}-enabled services</strong>: Many {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} are now available to deploy to your {{site.data.keyword.satelliteshort}} location. For more information, see the [{{site.data.keyword.cloud_notm}} blog](https://www.ibm.com/cloud/blog/announcements/deploy-managed-cloud-native-databases-anywhere-with-ibm-cloud-satellite){: external}.</li></ul> |
| 24 June 2021 | <ul><li><strong>Azure template</strong>: Automate the setup of a {{site.data.keyword.satelliteshort}} location in Microsoft Azure cloud infrastructure by using a [{{site.data.keyword.bpshort}}](/docs/satellite?topic=satellite-azure#azure-template).</li><li><strong>CLI changelog</strong>: Updated the CLI plug-in changelog page for the [release of version 1.0.295](/docs/satellite?topic=satellite-satellite-cli-changelog).</li></ul> |
| 18 June 2021 | Updates the [required outbound connectivity for hosts](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound) to include IP addresses for Link tunnel server endpoints. |
| 09 June 2021 | <ul><li><strong>New! Frankfurt location</strong>: You can now manage {{site.data.keyword.satelliteshort}} locations from the <strong>Frankfurt</strong> (<code>fra</code>, <code>eu-de</code>) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions).</li><li><strong>Digital certificates</strong>: Added information about [certificates for various {{site.data.keyword.satellitelong_notm}} domains and hosts](/docs/satellite?topic=satellite-compliance#certs-hosts-domains).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in June 2021."}

## May 2021
{: #may21}

| Date | Description |
| ---- | ----------- |
| 26 May 2021 | **CLI changelog**: Updated the CLI plug-in changelog page for the [release of version 1.0.258](/docs/satellite?topic=satellite-satellite-cli-changelog). |
| 19 May 2021 | **New! NetApp ONTAP-NAS storage driver template**: You can use the NetApp ONTAP-NAS storage driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [NetApp ONTAP-NAS storage driver documentation](/docs/satellite?topic=satellite-config-storage-netapp-nas). |
| 12 May 2021 | **New! {{site.data.keyword.IBM_notm}} Systems block storage CSI driver template**: You can use the {{site.data.keyword.IBM_notm}} Systems block storage CSI driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.IBM_notm}} Systems block storage CSI driver documentation](/docs/satellite?topic=satellite-config-storage-block-csi). |
| 07 May 2021 | **New! Spectrum Scale CSI driver template**: You can use the {{site.data.keyword.IBM_notm}} Spectrum Scale Container Storage Interface (CSI) driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.IBM_notm}} Spectrum Scale driver documentation](/docs/satellite?topic=satellite-config-storage-spectrum-scale). |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in May 2021."}

## April 2021
{: #apr21}

| Date | Description |
| ---- | ----------- |
| 26 April | **CLI changelog**: Updated the CLI plug-in changelog page for the [release of version 1.0.258](/docs/satellite?topic=satellite-satellite-cli-changelog). |
| 15 April | <ul><li><strong>Cluster add-ons</strong>: Added the [list](/docs/satellite?topic=satellite-addon-errors) of supported add-ons for clusters in {{site.data.keyword.satelliteshort}} locations.</li><li><strong>Use case</strong>: Added an example use case for using [{{site.data.keyword.satelliteshort}} edge environments for AI, IoT, and machine learning](/docs/satellite?topic=satellite-edge-usecase).</li><li><strong>{{site.data.keyword.cos_short}} buckets</strong>: When you create a location from the console, you can now [enter the name of an existing {{site.data.keyword.cos_full_notm}} bucket](/docs/satellite?topic=satellite-locations#location-create-console) that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data.</li></ul> |
| 09 April | <ul><li><strong>Logging</strong>: Expanded information for collecting and analyzing logs for your {{site.data.keyword.satelliteshort}} location, including the types of available logs, how to set up log alerts, and how to use logs to troubleshoot location and host errors.</li><li><strong>New! Storage template for remote OpenShift Data Foundation</strong>: The {{site.data.keyword.satelliteshort}} storage template for [using OpenShift Container Storage with remote devices](/docs/satellite?topic=satellite-config-storage-ocs-remote) is now available.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in April 2021."}

## March 2021
{: #mar21}

| Date | Description |
| ---- | ----------- |
| 25 March | **Security and compliance**: Added an [overview of security and compliance](/docs/satellite?topic=satellite-compliance) information for {{site.data.keyword.satelliteshort}}. |
| 23 March | **OpenShift Data Foundation using local disks**: Added steps for removing an ODF configuration and updated the [configuration parameter reference](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-params-cli). |
| 12 March | <ul><li><strong>Resetting the host key</strong>: In the event of a potential security violation, you can [reset the key](/docs/satellite?topic=satellite-hosts#host-key-reset) that the control plane uses to communicate with all of the hosts in the {{site.data.keyword.satelliteshort}} location.</li><li><strong>Troubleshooting location health checks</strong>: Added steps for troubleshooting when [{{site.data.keyword.cloud_notm}} is unable to check a location's health](/docs/satellite?topic=satellite-ts-location-healthcheck).</li></ul>|
| 10 March | **New `health-pending` host state**: To review the state of your hosts, see [Viewing host health](/docs/satellite?topic=satellite-monitor#host-health).|
| 01 March | <ul><li><strong>New! General availability of {{site.data.keyword.satellitelong}}</strong>: {{site.data.keyword.satellitelong_notm}} is now generally available. If you created locations during the closed beta, you must [re-create your locations](/docs/satellite?topic=satellite-locations#location-copy) for the general availability release. For more information about what you are charged for when you use {{site.data.keyword.satellitelong_notm}}, see the [FAQs](/docs/satellite?topic=satellite-faqs#pricing).</li><li><strong>CLI changelog</strong>: Updated the CLI plug-in changelog page for the [release of version 1.0.233](/docs/satellite?topic=satellite-satellite-cli-changelog).</li><li><strong>Cloud provider guides</strong>: Added instructions to set up {{site.data.keyword.satelliteshort}} locations that use infrastructure from [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), and [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm).</li><li><strong>Exposing apps in {{site.data.keyword.satelliteshort}} clusters</strong>: Added an overview of the [options for exposing apps in Satellite clusters](/docs/openshift?topic=openshift-sat-expose-apps) and steps for setting up each option.</li><li><strong>{{site.data.keyword.satelliteshort}} Infrastructure Service</strong>: [Order managed infrastructure from {{site.data.keyword.IBM_notm}} ](/docs/satellite?topic=satellite-infrastructure-service) to create a {{site.data.keyword.satelliteshort}} location for you in your on-premises data center.</li><li><strong>New! Template for fast provisioning on AWS</strong>: [Automate your {{site.data.keyword.satelliteshort}} location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template), available for AWS infrastructure.</li></ul>|
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in March 2021."}

## February 2021
{: #february21}

| Date | Description |
| ---- | ----------- |
| 25 February | <ul><li><strong>Auditing events</strong>: Added a topic for [auditing {{site.data.keyword.satelliteshort}} events with {{site.data.keyword.cloudaccesstrailshort}}](/docs/satellite?topic=satellite-at_events).</li><li><strong>CLI changelog</strong>: Updated the CLI plug-in changelog page for the [release of version 1.0.223](/docs/satellite?topic=satellite-satellite-cli-changelog).</li></ul> |
| 23 February | <ul><li><strong>Accessing clusters</strong>: Added steps for testing [access to a {{site.data.keyword.satelliteshort}} cluster by using the public network](/docs/openshift?topic=openshift-access_cluster#sat_public_access).</li><li><strong>Host requirements</strong>: Added the minimum required disk storage, storage device IOPS, and network bandwidth to the [hosts requirements](/docs/satellite?topic=satellite-host-reqs).</li><li><strong>Link endpoints</strong>: Added FAQs about [{{site.data.keyword.satelliteshort}} Link network security](/docs/satellite?topic=satellite-link-location-cloud#link-security).</li><li><strong>Monitoring</strong>: Added information about how to [set up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} location platform metrics](/docs/satellite?topic=satellite-monitor).</li><li><strong>Reviewing resources</strong>: Added a topic for [reviewing resources that are managed by {{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-satcon-manage#satconfig-resources).</li></ul>
| 18 February | **Securing service connections**: Added a [{{site.data.keyword.satelliteshort}} connection overview](/docs/satellite?topic=satellite-service-connection) to describe user access to resources that run in your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location. |
| 12 February | <ul><li><strong>Link endpoints</strong>: Added information about [default Link endpoints](/docs/satellite?topic=satellite-link-location-cloud#default-link-endpoints) that are automatically created for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location.</li><li><strong>Securing service connections</strong>: Added a topic to explain all points of access to your {{site.data.keyword.satelliteshort}} location. For more information, see [Securing your connection to Satellite](/docs/satellite?topic=satellite-service-connection).</li></ul> |
| 08 February | <ul><li><strong>CLI changelog</strong>: Updated the CLI plug-in changelog page for the [release of version 1.0.223](/docs/satellite?topic=satellite-satellite-cli-changelog).</li><li><strong>Host autoassignment</strong>: Added information about how [{{site.data.keyword.satelliteshort}} can automatically assign hosts](/docs/satellite?topic=satellite-hosts#host-autoassign-ov) to worker pools in clusters or {{site.data.keyword.satelliteshort}}-enabled services that use host labels to request compute capacity.</li><li><strong>Logging and monitoring</strong>: Added information about how to set up [logging and monitoring for {{site.data.keyword.satelliteshort}} health](/docs/satellite?topic=satellite-health).</li><li><strong>Worker pool management</strong>: Described how to manage [worker pools in {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters#satcluster-worker-pools), such as host labels for host autoassignment.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in February 2021."}

## January 2021
{: #january21}

| Date | Description |
| ---- | ----------- |
| 29 January | <ul><li><strong>AWS hosts</strong>: Added the list of supported AWS EC2 instance types that can be used as hosts.</li><li><strong>{{site.data.keyword.satelliteshort}} Link</strong>: Added information about the [maximum number of endpoints](/docs/satellite?topic=satellite-requirements#reqs-link) that you can create for one location.</li></ul> |
| 25 January | <ul><li><strong>{{site.data.keyword.satelliteshort}} Link</strong>: Expanded information about endpoint security and access controls, and added example use cases.</li></ul> |
| 15 January | **Host requirements**: Updated the [{{site.data.keyword.redhat_notm}} package repositories](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) that you must enable on hosts. |
| 12 January | <ul><li><strong>Host updates</strong>: Added how to update hosts that are used as worker nodes in [clusters](/docs/satellite?topic=satellite-hosts#host-update-workers) and the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-hosts#host-update-location).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in January 2021."}

## December 2020
{: #december20}

| Date | Description |
| ---- | ----------- |
| 09 December | <ul><li><strong>Private hosts</strong>: Hosts with only private network connectivity are now supported for your {{site.data.keyword.satelliteshort}} location. If hosts have multiple IPv4 network interfaces, the lowest-order, non-loopback network interface with a valid IPv4 address is used as the primary network interface for the hosts. Note that if your hosts have private network connectivity only, authorized users must be in the private host network directly or through a VPN connection to access services that run in your location, such as {{site.data.keyword.openshiftlong_notm}} clusters.</li><li><strong>Azure host support</strong>: Microsoft Azure hosts are now supported for your {{site.data.keyword.satelliteshort}} location.</li><li><strong>Physical machine support</strong>: Physical machine hosts are now supported for your {{site.data.keyword.satelliteshort}} location.</li><li><strong>Host network requirements</strong>: Updated the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound) on hosts' primary networks.</li><li><strong>AWS and GCP host DNS</strong>: When you use AWS and GCP hosts for your {{site.data.keyword.satelliteshort}} location, the requirement to manually configure DNS for the location control plane and for cluster load balancing is removed. The hosts' private IP addresses are automatically registered in DNS.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in December 2020."}

## November 2020
{: #november20}

| Date | Description |
| ---- | ----------- |
| 18 November | <ul><li><strong>Internal registry</strong>: Added information on how to [Set up the internal container image registry](/docs/satellite?topic=openshift-satellite-clusters#satcluster-internal-registry) for {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} locations.</li><li><strong>Service overview</strong>: Added an [About {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-about) topic to help you learn about {{site.data.keyword.satelliteshort}} terminology, service architecture, and components.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in November 2020."}

## October 2020
{: #october20}

| Date | Description |
| ---- | ----------- |
| 08 October | **Host network requirements**: Added the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound) on hosts' public networks. |
| 06 October | <ul><li><strong>CLI changelog</strong>: Added a CLI plug-in changelog page for the [release of version 1.0.178](/docs/satellite?topic=satellite-satellite-cli-changelog).</li><li><strong>Location subdomain troubleshooting</strong>: Added steps for further troubleshooting when [the location subdomain does not route traffic to control plane hosts](/docs/satellite?topic=satellite-ts-location-subdomain).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in October 2020."}

## September 2020
{: #september20}

| Date | Description |
| ---- | ----------- |
| 14 September | <ul><li><strong>Host requirements</strong>: Clarified that the <code>localhost</code> value must resolve to a valid local IP address, typically <code>127.0.0.1</code>, on a [host](/docs/satellite?topic=satellite-host-reqs#reqs-host-network).</li><li><strong>IAM</strong>: Updated [assigning access to {{site.data.keyword.satelliteshort}} Config in {{site.data.keyword.cloud_notm}} IAM](/docs/satellite?topic=satellite-iam#iam-resource-types) to note that you cannot scope access policies to specific configurations, subscriptions, or {{site.data.keyword.cloud_notm}} resource group.</li><li><strong>Provider documentation</strong>: Updated the provider requirements, such as examples for installing RHEL packages on GCP hosts before you attach the host to your {{site.data.keyword.satelliteshort}} location.</li><li><strong>Service requirements</strong>: Added that you can provision up to [40 {{site.data.keyword.cloud_notm}} service instances per {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-requirements#reqs-services).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in September 2020."}

## August 2020
{: #august20}

| Date | Description |
| ---- | ----------- |
| 21 August | **New! {{site.data.keyword.satellitelong_notm}} is now available as a closed beta**. For more information about the service and how it works, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}. To register for the beta, go [here](https://cloud.ibm.com/satellite/overview){: external}. |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in August 2020."} 


