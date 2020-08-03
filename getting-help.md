---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-03"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}


# Getting help
{: #get-help}

Still having issues? Review different ways to get help and support for {{site.data.keyword.satelliteshort}}. For any questions or feedback, post in Slack.
{: shortdesc}

## General ways to resolve issues
{: #help-general}

1. Keep the clusters and hosts in your {{site.data.keyword.satelliteshort}} location up to date.
2. Make sure that your command line tools are up to date.
   * In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use all available commands and flags.
   * Make sure that your `kubectl` and `oc` CLI client matches the same Kubernetes version as your cluster server. [Kubernetes does not support](https://kubernetes.io/docs/setup/release/version-skew-policy/){: external} `kubectl` client versions that are 2 or more versions apart from the server version (n +/- 2).

## Reviewing issues and status
{: #help-cloud-status}

1. To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page](https://cloud.ibm.com/status?selected=status){: external}.
2. Filter for the **Satellite** component and review any cloud status issue.
3. Review the [limitations and known issues documentation](/docs/satellite?topic=satellite-limitations).
4. For issues in open source projects that are used by {{site.data.keyword.cloud_notm}}, see the [IBM Open Source and Third Party policy](https://www.ibm.com/support/pages/node/737271){: external}.

## Feedback and questions
{: #feedback-qs}

1. Search for your question and post in the {{site.data.keyword.cloud_notm}} Slack.
   * If you are an external user, post in the [#general](https://ibm-cloud-success.slack.com/archives/C4G6362ER){: external} channel.
   * If you are an IBMer, use the [internal Slack channel](/docs/containers?topic=containers-cs_internal#internal_help).<p class="tip">If you do not use an IBMid for your {{site.data.keyword.cloud_notm}} account, [request an invitation](https://cloud.ibm.com/kubernetes/slack){: external} to this Slack.</p>
2. Review forums such as Stack Overflow to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
   * For questions about {{site.data.keyword.satelliteshort}}, use the tags `ibm-cloud` and `satellite`.
   * If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.openshiftshort}}, post your question on [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud+containers){: external} and tag your question with `ibm-cloud`, `openshift`, and `containers`.
   * See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.

## Contacting support
{: #help-support}

1. Before you open a support case, gather relevant information about your {{site.data.keyword.satelliteshort}} environment.
   1. Get the details of the resource that you want help to troubleshoot, such as your {{site.data.keyword.satelliteshort}} location or a host.
      ```
      ibmcloud sat location get --location <location_name_or_ID>
      ```
      {: pre}

      ```
      ibmcloud sat host get --location <location_name_or_ID> --host <host_name_or_ID>
      ```
      {: pre}
   2. For any hosts, include relevant details about the underlying infrastructure provider.
   3. For issues with resources within your cluster such as pods or services, log in to the cluster and use the Kubernetes API to get more information about them. If the resources are managed by {{site.data.keyword.satelliteshort}} configuration, get the details of your configuration and subscription.

   You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to gather and export pertinent information to share with IBM Support.
   {: tip}

2.  Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
3.  In your support case, for **Category**, select **Containers**.
4.  For the **Offering**, select your {{site.data.keyword.satelliteshort}} resource. Include the relevant information that you previously gathered.

