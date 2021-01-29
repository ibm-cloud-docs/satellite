---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-28"

keywords: satellite cli changelog, satellite commands, satellite cli, satellite reference

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
{:swift-ios: .ph data-hd-programlang='iOS Swift'}
{:swift-server: .ph data-hd-programlang='server-side Swift'}
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


# {{site.data.keyword.satelliteshort}} CLI changelog
{: #satellite-cli-changelog}

In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
{: shortdesc}

<br>
Refer to the following tables for a summary of changes for each version of the [{{site.data.keyword.satelliteshort}} CLI](/docs/satellite?topic=satellite-setup-cli), which uses the `ibmcloud sat` alias.

To see the CLI plug-in changes that pertain specifically to {{site.data.keyword.openshiftshort}} (`ibmcloud oc`) commands, see the [{{site.data.keyword.openshiftshort}} CLI changelog](/docs/openshift?topic=openshift-cs_cli_changelog).
{: tip}

## Version 1.0
{: #10}

Review the following changes for 1.0 versions of the CLI plug-in.
{: shortdesc}

|Version|Release date|Changes|
|-------|------------|-------|
| 1.0.197 | 18 Nov 2020 | Adds the `--endpoint` flag to the [`ibmcloud oc cluster config` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_cluster_config) to use the Link endpoint URL for the cluster context. |
| 1.0.178 | 06 Oct 2020 | <ul><li>Adds `http-tunnel` as a supported source or destination protocol in the `ibmcloud sat endpoint create` and `ibmcloud sat endpoint update` commands.</li><li>Updates the Go version to 1.15.2.</li><li>Updates the help text in various languages.</li></ul> |
{: caption="Overview of version changes for version 1.0 of the {{site.data.keyword.satelliteshort}} CLI plug-in" caption-side="top"}
{: summary="The rows are read from left to right, with the CLI plug-in version in column one, the release date of the version in column two, and a brief description of the changes for the version in column three."}
