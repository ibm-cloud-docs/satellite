---

copyright:
  years: 2020, 2020
lastupdated: "2020-12-03"

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
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Getting help
{: #get-help}

Still having issues? Review different ways to get help and support for {{site.data.keyword.satelliteshort}}. For questions or feedback, post in Slack.
{: shortdesc}

## General ways to resolve issues
{: #help-general}

1. Keep the clusters and hosts in your {{site.data.keyword.satelliteshort}} location up to date.
2. Make sure that your command-line tools are up to date.
   * In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use all available commands and flags.
   * Make sure that your `kubectl` and `oc` CLI client matches the same Kubernetes version as your cluster server. [Kubernetes does not support](https://kubernetes.io/docs/setup/release/version-skew-policy/){: external} `kubectl` client versions that are 2 or more versions apart from the server version (n +/- 2).

## Reviewing issues and status
{: #help-cloud-status}

1. To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page](https://cloud.ibm.com/status?selected=status){: external}.
2. Filter for the **Satellite** component and review any cloud status issue.
3. Review the [requirements, limitations, and known issues documentation](/docs/satellite?topic=satellite-requirements).
4. For issues in open source projects that are used by {{site.data.keyword.cloud_notm}}, see the [IBM open source and third-party policy](https://www.ibm.com/support/pages/node/737271){: external}.

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
      ```
      ibmcloud sat location get --location <location_name_or_ID>
      ```
      {: pre}

      ```
      ibmcloud sat host get --location <location_name_or_ID> --host <host_ID>
      ```
      {: pre}
   2. For any hosts, include relevant details about the underlying infrastructure provider, such as if the host is in an Amazon Web Services, Google Cloud Platform, Microsoft Azure, or other environment.
   3. For issues with resources within your cluster such as pods or services, log in to the cluster and use the Kubernetes API to get more information about them. If the resources are managed by {{site.data.keyword.satelliteshort}} configuration, get the details of your configuration and subscription.

   You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to gather and export pertinent information to share with IBM Support.
   {: tip}

2.  Contact IBM Support by [opening a case](https://cloud.ibm.com/unifiedsupport/cases/form){: external}. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-using-avatar).
3.  For the **Problem type**, search for or select **{{site.data.keyword.openshiftlong_notm}}**.
4.  For the **Case details**, provide a descriptive title and include the details that you previously gathered. From the **Resources**, you can also select the cluster that the issue is related to, if any.
