---

copyright:
  years: 2020, 2020
lastupdated: "2020-11-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Release notes
{: #release-notes}

Use the release notes to learn about the latest changes to the {{site.data.keyword.satelliteshort}} documentation that are grouped by month.
{:shortdesc}

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
