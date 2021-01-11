---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-11"

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


# Why can't I access the {{site.data.keyword.openshiftshort}} console?
{: #ts-console-fail}

{: tsSymptoms}
When you create an {{site.data.keyword.openshiftshort}} cluster in your {{site.data.keyword.satelliteshort}} location, you cannot access the {{site.data.keyword.openshiftshort}} web console, or access to the console is intermittent.

{: tsCauses}
If you used Amazon Web Services, Google Cloud Platform, or {{site.data.keyword.vpc_short}} hosts to create your location and cluster, the cluster's Calico network plug-in is created with IP in IP encapsulation. To access the {{site.data.keyword.openshiftshort}} web console for your cluster, the Calico plug-in must use VXLAN encapsulation instead.

{: tsResolve}
Update the Calico network plug-in to use VXLAN encapsulation.

1. [Access your cluster from the CLI](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

2. Set the `DATASTORE_TYPE` environment variable to `kubernetes`.
  ```
  export DATASTORE_TYPE=kubernetes
  ```
  {: pre}

3. Patch the `default-ipv4-ippool` IP pool to use VXLAN encapsulation.
  ```
  oc patch ippools.crd.projectcalico.org default-ipv4-ippool --type='merge' -p '{"spec":{"ipipMode":"Never","vxlanMode": "Always"}}'
  ```
  {: pre}

If you did not use Amazon Web Services, Google Cloud Platform, or {{site.data.keyword.vpc_short}} hosts, or if you are still unable to access the {{site.data.keyword.openshiftshort}} web console after completeing these steps, see [Debugging the OpenShift web console](/docs/openshift?topic=openshift-cs_troubleshoot#oc_console_fails) in the {{site.data.keyword.openshiftlong_notm}} troubleshooting documentation.
{: note}
