---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

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
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
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
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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


# Logging for {{site.data.keyword.satelliteshort}}
{: #health}

Integrate {{site.data.keyword.satelliteshort}} and other {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.la_full}} to get a comprehensive view and tools to manage all your resources.
{: shortdesc}

Logging for your {{site.data.keyword.satelliteshort}} location and for the {{site.data.keyword.cloud_notm}} services that run in your location must be set up separately. For example, to collect logs for your {{site.data.keyword.satelliteshort}} location setup, you enable a {{site.data.keyword.la_short}} instance to collect platform logs in the same region that your location is managed from. Then, to collect logs for a {{site.data.keyword.openshiftlong_notm}} cluster that runs in your {{site.data.keyword.satelliteshort}} location, you create a logging agent in your cluster to automatically collect and forward pod logs to a {{site.data.keyword.la_short}} instance. Note that you can use the same {{site.data.keyword.la_short}} instance to collect logs for both your {{site.data.keyword.satelliteshort}} location and services that run in your {{site.data.keyword.satelliteshort}} location.

## Setting up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location platform logs
{: #setup-la}

Forward and view logs that are automatically generated for your {{site.data.keyword.satelliteshort}} location setup in an {{site.data.keyword.la_full_notm}} instance that is enabled for platform-level logs.
{: shortdesc}

### Enabling platform logs
{: #enable-la}

If you already have a {{site.data.keyword.la_short}} instance in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from, and the {{site.data.keyword.la_short}} instance is configured to collect platform logs, the logs that are generated for your {{site.data.keyword.satelliteshort}} location are automatically forwarded to this {{site.data.keyword.la_short}} instance. Otherwise, follow these steps to set up {{site.data.keyword.la_short}} for your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. [Provision an {{site.data.keyword.la_full_notm}} instance](https://cloud.ibm.com/catalog/services){: external} in the same {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.
2. [Enable the instance for platform-level log collection](/docs/log-analysis?topic=log-analysis-config_svc_logs). Note that within one region, only one {{site.data.keyword.la_short}} instance can be enabled for platform logs collection.

### Viewing logs for your {{site.data.keyword.satelliteshort}} location
{: #view-la}

Because the {{site.data.keyword.la_full_notm}} instance is enabled for platform-level log collection, logs for all {{site.data.keyword.la_short}}-integrated services are shown in the {{site.data.keyword.la_short}} dashboard. You can apply filters to only view logs for your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. In the [**Logging** dashboard](https://cloud.ibm.com/observe/logging){: external}, click **Open Dashboard** for your {{site.data.keyword.la_short}} instance.
2. In the Filters toolbar, click **Sources**, select `satellite`, and click **Apply**. The logs for all of your {{site.data.keyword.satelliteshort}} locations in the region are shown.
3. To filter for a specific {{site.data.keyword.satelliteshort}} location, click **Apps** in the Filters toolbar, select the CRN for your {{site.data.keyword.satelliteshort}} location, and click **Apply**. To identify the CRN for your location, get your location ID by running `ibmcloud sat location ls`, look for this location's ID at the end of the listed CRNs.

For more tips on identifying logs in the dashboard, review how you can [search and filter logs](/docs/log-analysis?topic=log-analysis-view_logs).
{: tip}

### Analyzing logs for your {{site.data.keyword.satelliteshort}} location
{: #analyze-la}

Use logs that are automatically generated for your {{site.data.keyword.satelliteshort}} location to monitor and maintain its health.
{: shortdesc}

#### How often are logs posted?
{: #analyze-la-time}

Logs are collected for your location and posted every 60 seconds.

#### What kinds of logs are collected?
{: #analyze-la-types}

By default, three types of logs are automatically generated for your {{site.data.keyword.satelliteshort}} location: [`R00XX`-level error messages](#logs-error), [the status of whether resource deployment to the location is enabled](#logs-deploy), and [the status of {{site.data.keyword.satelliteshort}} Link](#logs-link). Review the following sections for example logs for each log type and descriptions of each log field.

#### How can I set up alerts for location error logs?
{: #analyze-la-alert}

You can use the built-in {{site.data.keyword.la_short}} dashboard tools to save log searches and set up alerts for certain types of logs, such as errors.

1. To filter for a specific {{site.data.keyword.satelliteshort}} location, click **Apps** in the Filters toolbar, select the CRN for your {{site.data.keyword.satelliteshort}} location, and click **Apply**. To identify the CRN for your location, look for the location's ID at the end of the CRN.
2. Search for a specific query that you want an alert for. For example, to be alerted for any logs that contain `R00XX`-level location error messages, search for `R00`. To be alerted for {{site.data.keyword.satelliteshort}} Link health check failures, search for `Failed to reach endpoint`.
3. Click **Unsaved view > Save as new view**. Add a name and an optional category.
4. In the **Alert** drop-down list, select **View-specific alert** and follow the steps for the notification channel that you selected to configure a custom alert for this log query.
5. Click **Save view**.

#### Is {{site.data.keyword.IBM_notm}} alerted for any of these logs?
{: #analyze-la-monitor}

The {{site.data.keyword.monitoringfull_notm}} component generates certain alerts for issues with your location setup and host infrastructure. To review the alerts that {{site.data.keyword.IBM_notm}} monitors, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default).

### `R00XX` error logs
{: #logs-error}

`R00XX` error logs report messages and more detailed information about issues with your location setup and host infrastructure. For more information about each `R00XX` error message, including troubleshooting steps, see [Location error messages](/docs/satellite?topic=satellite-ts-locations-debug).
{: shortdesc}

Example log:
```
{"logSourceCRN":"crn:v1:bluemix:public:satellite:us-south:a/f601ad712b0dd981276cf3b995554afc:c1hk4ek107l5au5mq8hg::","saveServiceCopy":true,"Details":{"message":"R0025: The Satellite location has OpenShift clusters in critical health.","errorDetails":"Customer etcd cluster moved down to 1 or less available pods. Quorum broke. Manual recovery of cluster needed.","messageID":"R0025"}}
```
{: screen}

|Log field|Description|
|---------|-----------|
|`logSourceCRN`|The CRN of the {{site.data.keyword.satelliteshort}} location. To identify the CRN for a location, look for the location's ID at the end of the CRN.|
|`saveServiceCopy`|Set to `true` so that a copy of the log record is sent to {{site.data.keyword.IBM_notm}} for monitoring and alerts.|
|`Details`|The detailed information for log.|
|`Details.message`|The current error message for the location, including any troubleshooting steps or documentation links.|
|`Details.errorDetails`|Other details for the current error, such as specific causes or issues with certain components. These details are used by {{site.data.keyword.IBM_notm}} site reliability engineers to manage alerts, but can help provide more details about the issue while you troubleshoot.|
|`Details.messageID`|The error message's `R00XX` identifier.|
{: caption="Pre-defined fields for R00XX error logs" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the log field name. The second column is the description of the log field."}

### Enablement of resource deployment logs
{: #logs-deploy}

Enablement of resource deployment logs report the current status of whether resources such as hosts, clusters, or {{site.data.keyword.satelliteshort}}-enabled service instances can be changed or deployed in your location, and the reason for this status. For example, resource deployment might be set to false due to one or more location errors.
{: shortdesc}

Example log:
```
{"logSourceCRN":"crn:v1:bluemix:public:satellite:us-south:a/f601ad712b0dd981276cf3b995554afc:c1hk4ek107l5au5mq8hg::","saveServiceCopy":true,"message":"Enablement of resource deployment in the location is set false due to R0012: The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane. R0025: The Satellite location has OpenShift clusters in critical health."}
```
{: screen}

|Log field|Description|
|---------|-----------|
|`logSourceCRN`|The CRN of the {{site.data.keyword.satelliteshort}} location. To identify the CRN for a location, look for the location's ID at the end of the CRN.|
|`saveServiceCopy`|Set to `true` so that a copy of the log record is sent to {{site.data.keyword.IBM_notm}} for monitoring and alerts.|
|`message`|The status of whether resource deployment is currently enabled (`true` or `false`). If set to `false`, the current `R00XX`-level error messages for the location are listed.|
{: caption="Pre-defined fields of logs for the status of deployment enablement" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the log field name. The second column is the description of the log field."}

### Endpoint health status logs
{: #logs-link}

Endpoint health status logs report the current health check status of the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint. For more information, see [Why is {{site.data.keyword.cloud_notm}} unable to check my location's health?](/docs/satellite?topic=satellite-ts-location-healthcheck).
{: shortdesc}

* If logs report `Successfully checked endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable and healthy.
* If logs report `Failed to reach endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is unreachable.

Example log:
```
{"logSourceCRN":"crn:v1:bluemix:public:satellite:us-east:a/6ef045fd2b43266cfe8e6388dd2ec098:c0rcidjw0s3rf9v8sms0::","saveServiceCopy":true,"message":"Endpoint health status: Failed to reach endpoint. Get \"http://c-03.us-east.link.satellite.cloud.ibm.com:32900\": read tcp 172.XX.XXX.XXX:58564-\u003e166.9.XX.XXX:32900: read: connection reset by peer. Endpoint: http://c-03.us-east.link.satellite.cloud.ibm.com:32900"}
```
{: screen}

|Log field|Description|
|---------|-----------|
|`logSourceCRN`|The CRN of the {{site.data.keyword.satelliteshort}} location. To identify the CRN for a location, look for the location's ID at the end of the CRN.|
|`saveServiceCopy`|Set to `true` so that a copy of the log record is sent to {{site.data.keyword.IBM_notm}} for monitoring and alerts.|
|`message`|The status of whether your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable, and the endpoint that was health checked.|
{: caption="Pre-defined fields for endpoint health status logs" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the log field name. The second column is the description of the log field."}

<br />

## Setting up {{site.data.keyword.at_short}} for {{site.data.keyword.satelliteshort}} location events
{: #setup-at}

To track how users and applications interact with your {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.satellitelong_notm}} automatically generates user-initiated management events and forwards these event logs to {{site.data.keyword.at_full}}.
{: shortdesc}

To access these logs, [provision an instance of {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started) in the same region that your location is managed from. For more information about the types of {{site.data.keyword.satelliteshort}} events that you can track, see [Auditing events for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-at_events).

<br />

## Setting up logging for clusters
{: #setup-clusters}

To understand and set up logging for {{site.data.keyword.openshiftshort}} clusters that run in your {{site.data.keyword.satelliteshort}} location, see the tutorials in the [{{site.data.keyword.la_full_notm}} documentation](/docs/log-analysis?topic=log-analysis-kube#kube).
{: shortdesc}

You cannot currently use the {{site.data.keyword.openshiftlong_notm}} console or the observability plug-in CLI (`ibmcloud ob`) to enable logging for {{site.data.keyword.satelliteshort}} clusters. You must manually deploy logging agents to your cluster to forward logs to {{site.data.keyword.la_short}}.
{: note}


