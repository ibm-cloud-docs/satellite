---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-27"

keywords: satellite, hybrid, multicloud

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
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
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
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
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



# Release notes
{: #release-notes}

Use the release notes to learn about the latest changes to the {{site.data.keyword.satelliteshort}} documentation that are grouped by month.
{:shortdesc}

## January 2021
{: #january21}

| Date | Description |
| ---- | ----------- |
| 27 January | **{{site.data.keyword.satelliteshort}} link**: Added information about the [maximum number of endpoints](/docs/satellite?topic=satellite-requirements#reqs-link) that you can create for one location. |
| 25 January | <ul><li>**About {{site.data.keyword.satelliteshort}}**: Updated the [About topic](/docs/satellite?topic=satellite-about#location-concept) with a new conceptual diagram and explanation of how {{site.data.keyword.satelliteshort}} locations relate to your infrastructure provider and {{site.data.keyword.cloud_notm}} environments.</li><li>**{{site.data.keyword.satelliteshort}} link**: Expanded information about endpoint security and access controls, and added example use cases.</li></ul> |
| 15 January | **Host requirements**: Updated the [{{site.data.keyword.redhat_notm}} package repositories](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) that you must enable on hosts. |
| 12 January | <ul><li>**Calico VXLAN encapsulation**: If you use AWS, GCP, or {{site.data.keyword.vpc_short}} hosts to create an {{site.data.keyword.openshiftshort}} cluster, you must [update the cluster's Calico plug-in to use VXLAN encapsulation](/docs/satellite?topic=openshift-satellite-clusters#satcluster-create-cli).</li><li>**Host updates**: Added how to update hosts that are used as worker nodes in [clusters](/docs/satellite?topic=satellite-hosts#host-update-workers) and the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-hosts#host-update-location).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in January 2021}

## December 2020
{: #december20}

| Date | Description |
| ---- | ----------- |
| 09 December | <ul><li>**Private hosts**: Hosts with only private network connectivity are now supported for your {{site.data.keyword.satelliteshort}} location. If hosts have multiple IPv4 network interfaces, the lowest-order, non-loopback network interface with a valid IPv4 address is used as the primary network interface for the hosts. Note that if your hosts have private network connectivity only, authorized users must be in the private host network directly or through a VPN connection to access services that run in your location, such as {{site.data.keyword.openshiftlong_notm}} clusters.</li><li>**Azure host support**: Microsoft Azure hosts are now supported for your {{site.data.keyword.satelliteshort}} location. To prepare your Azure hosts, follow the steps in [Provider requirements](/docs/satellite?topic=satellite-providers#azure-reqs).</li><li>**Physical machine support**: Physical machine hosts are now supported for your {{site.data.keyword.satelliteshort}} location.</li><li>**Host network requirements**: Updated the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound) on hosts' primary networks.</li><li>**AWS and GCP host DNS**: When you use AWS and GCP hosts for your {{site.data.keyword.satelliteshort}} location, the requirement to manually configure DNS for the location control plane and for cluster load balancing is removed. The hosts' private IP addresses are automatically registered in DNS.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in December 2020."}

## November 2020
{: #november20}

| Date | Description |
| ---- | ----------- |
| 18 November | <ul><li>**Internal registry**: Added information on how to [Set up the internal container image registry](/docs/satellite?topic=openshift-satellite-clusters#satcluster-internal-registry) for {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} locations.</li><li>**Service overview**: Added an [About {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-about) topic to help you learn about {{site.data.keyword.satelliteshort}} terminology, service architecture, and components.</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in November 2020."}

## October 2020
{: #october20}

| Date | Description |
| ---- | ----------- |
| 08 October | **Host network requirements**: Added the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound) on hosts' public networks. |
| 06 October | <ul><li>**CLI changelog**: Added a CLI plug-in changelog page for the [release of version 1.0.178](/docs/satellite?topic=satellite-satellite-cli-changelog).</li><li>**Location subdomain troubleshooting**: Added steps for further troubleshooting when [the location subdomain does not route traffic to control plane hosts](/docs/satellite?topic=satellite-ts-location-subdomain).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in October 2020."}

## September 2020
{: #september20}

| Date | Description |
| ---- | ----------- |
| 14 September | <ul><li>**Host requirements**: Clarified that the `localhost` value must resolve to a valid local IP address, typically `127.0.0.1`, on a [host](/docs/satellite?topic=satellite-host-reqs#reqs-host-network).</li><li>**IAM**: Updated [assigning access to {{site.data.keyword.satelliteshort}} Config in {{site.data.keyword.cloud_notm}} IAM](/docs/satellite?topic=satellite-iam#iam-resource-types) to note that you cannot scope access policies to specific configurations, subscriptions, or {{site.data.keyword.cloud_notm}} resource group.</li><li>**Provider documentation**: Updated the [provider requirements](/docs/satellite?topic=satellite-providers), such as examples for installing RHEL packages on GCP hosts before you attach the host to your {{site.data.keyword.satelliteshort}} location.</li><li>**Service requirements**: Added that you can provision up to [40 {{site.data.keyword.cloud_notm}} service instances per {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-requirements#reqs-services).</li></ul> |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in September 2020."}

## August 2020
{: #august20}

| Date | Description |
| ---- | ----------- |
| 21 August | **New! {{site.data.keyword.satellitelong_notm}} is now available as a closed beta**. For more information about the service and how it works, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}. To register for the beta, go [here](https://cloud.ibm.com/satellite/beta){: external}. |
{: summary="The table shows release notes. Rows are to be read from the left to right, with the date in column one, the title of the feature in column two and a description in column three."}
{: caption="Documentation updates in August 2020."} 
