---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

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

## Identifying issues for {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service
{: #help-services}

If you experience an issue with a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service in your location, first check whether your issue is listed in the troubleshooting topics in the {{site.data.keyword.satelliteshort}} documentation. If the issue is not listed in the {{site.data.keyword.satelliteshort}} documentation, check the {{site.data.keyword.cloud_notm}} documentation set for the service.
{: shortdesc}

For example, if you cannot access the {{site.data.keyword.redhat_openshift_notm}} console for a {{site.data.keyword.redhat_openshift_notm}} cluster on {{site.data.keyword.satelliteshort}}, first check whether the issue is [specific to your {{site.data.keyword.satelliteshort}} location setup](/docs/satellite?topic=satellite-ts-console-fail). If your {{site.data.keyword.satelliteshort}} location setup is not the source of the issue, then check the [{{site.data.keyword.openshiftlong_notm}} documentation for troubleshooting console issues](/docs/openshift?topic=openshift-ocp-debug).

## Reviewing logs
{: #review-logs}

You can review logs to troubleshoot issues.
{: shortdesc}

If your host is not assigned to a cluster, or if the assignment fails, you can SSH into the host machine and view logs. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host by using SSH for security purposes. For more information about viewing logs on your host machine, see [Logging in to a host machine to debug](/docs/satellite?topic=satellite-ts-hosts-login).

If you are using {{site.data.keyword.bpshort}}, you can view job log details to find information about your deployment. For more information, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-workspace-setup&interface=ui#job-logs).

You can also view logs that are automatically generated for your {{site.data.keyword.satelliteshort}} location setup and collected in an {{site.data.keyword.la_full_notm}} instance. For more information, see [Logging for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-health).

## Feedback and questions
{: #feedback-qs}

1. Search for your question and post in the {{site.data.keyword.cloud_notm}} [Slack](https://cloud.ibm.com/kubernetes/slack){: external}, in the `#satellite` channel.
2. Review forums such as Stack Overflow to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    - For questions about {{site.data.keyword.satelliteshort}}, use the tags `ibm-cloud` and `satellite`.
    - If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.redhat_openshift_notm}}, post your question on [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud+containers){: external} and tag your question with `ibm-cloud`, `openshift`, and `containers`.
    - See [Getting help](/docs/get-support?topic=get-support-using-avatar#using-avatar) for more details about using the forums.

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
3. For the **Problem type**, search for or select **{{site.data.keyword.satelliteshort}}**.
4. For the **Case details**, provide a descriptive title and include the details that you previously gathered. From the **Resources**, you can also select the cluster that the issue is related to, if any.

## Opting in to receive email notifications
{: #opt-in-email}

You can opt in to receive email notifications about {{site.data.keyword.cloud_notm}} platform-related items across your account and resource-related items for incidents, maintenance, security bulletins, and resource activity updates. Email notifications are not turned on by default. To understand how to set up these email notifications, see [Setting email preferences for notifications](/docs/satellite?topic=account-email-prefs).
