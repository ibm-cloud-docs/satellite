---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-29"

keywords: satellite at events, satellite activity tracker, satconfig events, satlink events, satellite config events, satellite link events, satellite location events, satellite host events

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



# Auditing events
{: #at_events}

As a security officer, auditor, or manager, you can use {{site.data.keyword.at_full}} to track how users and applications interact with {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started#getting-started).



## Events for {{site.data.keyword.satelliteshort}} clusters
{: #at_actions_clusters}

See the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-at_events).
{: shortdesc}

## Events for the {{site.data.keyword.satelliteshort}} Link component
{: #at_actions_link}

| Action             | Description      |
|--------------------|------------------|
| `satellite.link.create` | A {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.satelliteshort}} Link connector are created. |
| `satellite.link.delete` | A {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.satelliteshort}} Link connector are removed. |
| `satellite.link.get` | Details for a {{site.data.keyword.satelliteshort}} location are retrieved. |
| `satellite.link-endpoint-certs.delete` | A TLS certificate for a {{site.data.keyword.satelliteshort}} endpoint is removed.|
| `satellite.link-endpoint-certs.get` | A list of TLS certificates that are used for a {{site.data.keyword.satelliteshort}} endpoint is retrieved. |
| `satellite.link-endpoint-certs.upload` | A TLS certificate is uploaded for a {{site.data.keyword.satelliteshort}} endpoint.|
| `satellite.link-endpoints.create` | A {{site.data.keyword.satelliteshort}} endpoint is created.  |
| `satellite.link-endpoints.delete` | A {{site.data.keyword.satelliteshort}} endpoint is removed. |
| `satellite.link-endpoints.export`	| The configuration for a {{site.data.keyword.satelliteshort}} endpoint is exported to a file. |
| `satellite.link-endpoints.get` | Details for a {{site.data.keyword.satelliteshort}} endpoint are retrieved. |
| `satellite.link-endpoints.import` |	The configuration for a {{site.data.keyword.satelliteshort}} endpoint is imported from a file. |
| `satellite.link-endpoints.list` | A list of {{site.data.keyword.satelliteshort}} endpoints is retrieved. |
| `satellite.link-endpoints.update` | A {{site.data.keyword.satelliteshort}} endpoint is updated or data for exported. |
| `satellite.link-source-endpoints.list` | A list of {{site.data.keyword.satelliteshort}} endpoints that a client (source) is configured for and the enabled status of the client (source) for each endpoint is retrieved. |
| `satellite.link-source-endpoints.update` | A client (source) is enabled or disabled for one or more {{site.data.keyword.satelliteshort}} endpoints.	|
| `satellite.link-sources.create` | A client (source) is configured for a {{site.data.keyword.satelliteshort}} endpoint. |
| `satellite.link-sources.delete` | A client (source) is removed from a {{site.data.keyword.satelliteshort}} endpoint. |
| `satellite.link-sources.list` | A list of clients (sources) that are configured for a {{site.data.keyword.satelliteshort}} endpoint is retrieved. |
| `satellite.link-sources.update` | A client (source) configuration is updated for a {{site.data.keyword.satelliteshort}} endpoint. |
{: caption="{{site.data.keyword.satelliteshort}} Link actions that generate events." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the action event that is sent to Activity Tracker. The second column is a description of the event."}

## Events for the {{site.data.keyword.satelliteshort}} Config component
{: #at_actions_config}

| Action             | Description      |
|--------------------|------------------|
| `satellite.config-cluster.register` | A {{site.data.keyword.satelliteshort}} or {{site.data.keyword.openshiftlong_notm}} cluster is registered with {{site.data.keyword.satelliteshort}} Config.|
| `satellite.config-clustergroup.manage` | A cluster group is created, or a cluster is added to or removed from a cluster group. |
| `satellite.config-configuration.create` | A {{site.data.keyword.satelliteshort}} configuration is created.|
| `satellite.config-configuration.delete` | A {{site.data.keyword.satelliteshort}} configuration is removed. |
| `satellite.config-configuration.manageversion` | A version is added to a {{site.data.keyword.satelliteshort}} configuration. |
| `satellite.config-configuration.update` | A {{site.data.keyword.satelliteshort}} configuration is updated. |
| `satellite.config-subscription.create` | A {{site.data.keyword.satelliteshort}} subscription is created.|
| `satellite.config-subscription.delete` | A {{site.data.keyword.satelliteshort}} subscription is removed.|
| `satellite.config-subscription.setversion` | A {{site.data.keyword.satelliteshort}} subscription is set to a specific configuration version. |
| `satellite.config-subscription.update` | A {{site.data.keyword.satelliteshort}} subscription is updated.|
{: caption="{{site.data.keyword.satelliteshort}} Config actions that generate events." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the action event that is sent to Activity Tracker. The second column is a description of the event."}

## Viewing events
{: #at_ui}

Events that are generated by {{site.data.keyword.satellitelong_notm}} are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance as follows.
{: shortdesc}

* {{site.data.keyword.satelliteshort}} location events are available in the {{site.data.keyword.at_full_notm}} instance that is in the same region as the location is managed from.
* Global events, such as identity and access (IAM) or account management events, are available in the **Frankfurt** location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the following locations. For more information, see [Navigating to the UI](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-launch).


