---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-30"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Getting help
{: #get-help}

Still having issues? Review different ways to get help and support for {{site.data.keyword.satelliteshort}}. For questions or feedback, post in Slack.
{: shortdesc}

## General ways to resolve issues
{: #help-general}

- Make sure that your command-line tools are up to date.
    - In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use all available commands and flags.
    - Make sure that your `kubectl` and `oc` CLI client matches the same Kubernetes version as your cluster server. [Kubernetes does not support](https://kubernetes.io/releases/version-skew-policy/){: external} `kubectl` client versions that are 2 or more versions apart from the server version (n +/- 2).
- Keep the clusters and hosts in your {{site.data.keyword.satelliteshort}} location up to date.
- Enable and review [logging](#review-logs) and [monitoring](/docs/satellite?topic=satellite-monitor) details to troubleshoot your {{site.data.keyword.satelliteshort}} components.

## Reviewing cloud issues and status
{: #help-cloud-status}

1. To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page](https://cloud.ibm.com/status?selected=status){: external}.
2. Filter for the **Satellite** component and review any cloud status issue.
3. Review the [requirements, limitations, and known issues documentation](/docs/satellite?topic=satellite-requirements).
4. For issues in open source projects that are used by {{site.data.keyword.cloud_notm}}, see the [{{site.data.keyword.IBM_notm}} open source and third-party policy](https://www.ibm.com/support/pages/node/737271){: external}.

## Identifying issues for {{site.data.keyword.satelliteshort}}-enabled services
{: #help-services}

If you experience an issue with a {{site.data.keyword.satelliteshort}}-enabled service in your location, first check whether your issue is listed in the troubleshooting topics in the {{site.data.keyword.satelliteshort}} documentation. If the issue is not listed in the {{site.data.keyword.satelliteshort}} documentation, check the {{site.data.keyword.cloud_notm}} documentation set for the service.
{: shortdesc}

For example, if you cannot access the {{site.data.keyword.openshiftshort}} console for an {{site.data.keyword.openshiftshort}} cluster on {{site.data.keyword.satelliteshort}}, first check whether the issue is [specific to your {{site.data.keyword.satelliteshort}} location setup](/docs/satellite?topic=satellite-ts-console-fail). If your {{site.data.keyword.satelliteshort}} location setup is not the source of the issue, then check the [{{site.data.keyword.openshiftlong_notm}} documentation for troubleshooting console issues](/docs/openshift?topic=openshift-ocp-debug).

## Using {{site.data.keyword.la_short}} to review {{site.data.keyword.satelliteshort}} location logs
{: #review-logs}

Troubleshoot issues by analyzing logs that are automatically generated for your {{site.data.keyword.satelliteshort}} location setup and collected in an {{site.data.keyword.la_full_notm}} instance.
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

Example log
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

Example log
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

- If logs report `Successfully checked endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable and healthy.
- If logs report `Failed to reach endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is unreachable.

Example log
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

## Feedback and questions
{: #feedback-qs}

1. Search for your question and post in the {{site.data.keyword.cloud_notm}} [Slack](https://cloud.ibm.com/kubernetes/slack){: external}, in the `#satellite` channel.
2. Review forums such as Stack Overflow to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    * For questions about {{site.data.keyword.satelliteshort}}, use the tags `ibm-cloud` and `satellite`.
    * If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.openshiftshort}}, post your question on [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud+containers){: external} and tag your question with `ibm-cloud`, `openshift`, and `containers`.
    * See [Getting help](/docs/get-support?topic=get-support-using-avatar#using-avatar) for more details about using the forums.

## Contacting support
{: #help-support}

1. Before you open a support case, gather relevant information about your {{site.data.keyword.satelliteshort}} environment.
    1. Get the details of the resource that you want help with troubleshooting, such as your {{site.data.keyword.satelliteshort}} location or a host.
        ```sh
        ibmcloud sat location get --location <location_name_or_ID>
        ```
        {: pre}

        ```sh
        ibmcloud sat host get --location <location_name_or_ID> --host <host_ID>
        ```
        {: pre}

    2. For any hosts, include relevant details about the underlying infrastructure provider, such as if the host is in an Amazon Web Services, Google Cloud Platform, Microsoft Azure, or other environment.
    3. For issues with resources within your cluster such as pods or services, log in to the cluster and use the Kubernetes API to get more information about them. If the resources are managed by {{site.data.keyword.satelliteshort}} configuration, get the details of your configuration and subscription.

    You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-debug-tool) to gather and export pertinent information to share with {{site.data.keyword.IBM_notm}} Support.
    {: tip}

2. Contact {{site.data.keyword.IBM_notm}} Support by [opening a case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}. To learn about opening an {{site.data.keyword.IBM_notm}} support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-using-avatar).
3. For the **Problem type**, search for or select **{{site.data.keyword.openshiftlong_notm}}**.
4. For the **Case details**, provide a descriptive title and include the details that you previously gathered. From the **Resources**, you can also select the cluster that the issue is related to, if any.


