---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-12"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

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


# Why is {{site.data.keyword.cloud_notm}} unable to check my location's health?
{: #ts-location-healthcheck}

{: tsSymptoms}
After you assign hosts to your {{site.data.keyword.satelliteshort}} location control plane, you see a message similar to the following.

```
R0047 IBM Cloud is unable to use the health check Link endpoint to check the location's health.
```
{: screen}

{: tsCauses}
The location control plane is not accessible from {{site.data.keyword.cloud_notm}} through {{site.data.keyword.satelliteshort}} Link due to one of the following reasons:
* The automated health check endpoint was disabled.
* The hosts are behind a firewall that blocks traffic to or from {{site.data.keyword.cloud_notm}}.

{: tsResolve}
1. Verify that the automated health check endpoint for your location is enabled.
  1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
  2. From the **Link endpoints** tab, verify that the **Status** for the endpoint in the format `satellite-healthcheck-<location_ID>` is toggled to **Enabled**.

2. Verify that the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable.
  1. [Set up LogDNA for {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-health#setup-logdna).
  2. In the [**Logging** dashboard](https://cloud.ibm.com/observe/logging){:external}, click **View LogDNA** for your {{site.data.keyword.la_short}} instance. The LogDNA dashboard opens.
  3. Check the `Endpoint health status` logs. These logs report the results of health checks for the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint.
      * If logs report `Successfully checked endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable. Continue to the next step.
      * If logs report `Failed to reach endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is unreachable.
3. Resolve the issue that prevents the tunnel from being reached.
     * If you have a firewall in your infrastructure provider, allow traffic from the hosts to the location control plane access through the firewall. For example, see [AWS Security group](/docs/satellite?topic=satellite-aws#aws-reqs-secgroup).
     * If you still see the `R0047` error message, [open a support case](/docs/satellite?topic=satellite-get-help#help-support).
