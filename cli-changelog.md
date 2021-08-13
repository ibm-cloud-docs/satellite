---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

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


# CLI changelog for {{site.data.keyword.satelliteshort}} commands
{: #satellite-cli-changelog}

In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
{: shortdesc}

<br>
Refer to the following tables for a summary of changes for each version of the [plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli), which uses the `ibmcloud sat` alias.

To see the CLI plug-in changes that pertain specifically to {{site.data.keyword.openshiftshort}} (`ibmcloud oc`) commands, see the [{{site.data.keyword.openshiftshort}} CLI changelog](/docs/openshift?topic=openshift-cs_cli_changelog).
{: tip}

## Version 1.0
{: #10}

Review the following changes for 1.0 versions of the CLI plug-in.
{: shortdesc}

|Version|Release date|Changes|
|-------|------------|-------|
| 1.0.312 | 09 Aug 2021 |<ul><li>Includes the subscription status in the output of the <code>ibmcloud sat subscription get</code> and <code>ibmcloud sat subscription ls</code> commands.</li><li>Adds the option to display the output of the <code>ibmcloud sat subscription get</code> and <code>ibmcloud sat subscription ls</code> commands in JSON format.</li><li>Updates the help text in various languages.</li><li>Displays OpenVPN Server Port details in the <code>ibmcloud sat location get</code> command output.</li></ul> | 
| 1.0.300 | 12 Jul 2021 | <ul><li>Updates the help text in various languages.</li></ul>|
| 1.0.295 | 24 Jun 2021 | <ul><li>The <code>ibmcloud sat storage config</code> and <code>ibmcloud sat storage template</code> commands are now generally available.</li><li>Adds the <code>--cluster</code> and <code>--service-cluster-id</code> flag to the <code>ibmcloud sat storage assignment ls</code> command to filter output by the ID of a cluster that you created or the ID of a {{site.data.keyword.satelliteshort}}-enabled service cluster.</li><li>Adds the <code>--service-cluster-id</code> flag to the <code>ibmcloud sat storage assignment create</code> command to deploy storage drivers to a specific {{site.data.keyword.satelliteshort}}-enabled service cluster.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.275 | 26 May 2021 | <ul><li>Adds the following changes to storage commands:<ul><li>Adds the <code>--cluster</code> flag to the <code>ibmcloud sat storage assignment create</code> command to assign to an individual cluster ID instead of a cluster group.</li><li>Adds <code>--location</code> as a required flag to the <code>ibmcloud sat storage config create</code> command and an optional flag to the <code>ibmcloud sat storage config ls</code> command.</li><li>Changes the <code>--config</code> flag to <code>--config-name</code> in the <code>ibmcloud sat storage config sc add</code> command.</li></ul></li><li>Adds the optional <code>--output json</code> flag to the <code>ibmcloud sat config get</code>, <code>ibmcloud sat config ls</code>, and <code>ibmcloud sat config version get</code> commands.</li><li>Increases the maximum size for the file in the <code>--read-config</code> flag of the <code>ibmcloud sat config version create</code> command from 1MB to 3MB.</li><li>Fixes the <code>ibmcloud sat group create</code> command to enable cluster group creation.</li><li>The IAM token that is used for your CLI session is now refreshed 5 minutes before expiration to keep the session active.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.258 | 26 Apr 2021 | <ul><li>Adds the <code>ibmcloud sat storage config sc add</code>, <code>ibmcloud sat storage config sc get</code>, and <code>ibmcloud sat storage config sc ls</code> beta commands to create and view custom storage classes of {{site.data.keyword.satelliteshort}} storage configurations.</li><li>Adds the <code>ibmcloud sat messages</code> command to view current messages from {{site.data.keyword.satellitelong_notm}}.</li><li>Fixes a <code>golang</code> vulnerability for [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.233 | 01 Mar 2021 | <ul><li>Adds the <code>ibmcloud sat storage config rm</code> command to remove a {{site.data.keyword.satelliteshort}} storage configuration.</li><li>Removes <code>beta</code> tags from <code>ibmcloud sat</code> commands for the generally available release of {{site.data.keyword.satellitelong_notm}}.</li></ul> |
| 1.0.231 | 25 Feb 2021 | <ul><li>Adds the <code>ibmcloud sat service ls</code> command to view {{site.data.keyword.satelliteshort}}-enabled service clusters in your {{site.data.keyword.satelliteshort}} location.</li><li>Updates the Go version to 1.15.8.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.223 | 08 Feb 2021 | <ul><li>Adds the <code>ibmcloud sat storage</code> command group to view and manage the storage resources that run in {{site.data.keyword.openshiftshort}} clusters that are registered with {{site.data.keyword.satelliteshort}} Config.</li><li>Adds the optional <code>--ha-zone</code> flag to the <code>ibmcloud sat location create</code> command to specify three arbitrary zone names in your {{site.data.keyword.satelliteshort}} location.</li><li>Adds the optional <code>--reset-key</code> flag to the <code>ibmcloud sat host attach</code> command to reset the key that the control plane uses to communicate with all of the hosts in the location.</li><li>Moves <code>ibmcloud sat config configuration</code> commands to the <code>ibmcloud sat config</code> command group.</li><li>Moves <code>ibmcloud sat config subscription</code> commands to the <code>ibmcloud sat subscription</code> command group.</li><li>Renames <code>cluster-group</code> commands and <code>--cluster-group</code> flags to <code>group</code>.</li><li>Renames <code>configuration</code> commands and <code>--configuration</code> flags to <code>config</code>.</li><li>Renames <code>--type</code> flags to <code>--file-format</code>.</li><li>Renames the <code>--label</code> (short form <code>-l</code>) flag in the <code>ibmcloud sat host assign</code>, <code>attach</code>, and <code>update</code> commands to <code>--host-label</code> (short form <code>-hl</code>).</li><li>The following changes are made to <code>ibmcloud oc</code> commands for managing {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}:<ul><li>Adds the [<code>ibmcloud ks worker-pool create satellite</code> command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_worker_pool_create_sat) to add worker pools to {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}.</li><li>Adds the [<code>ibmcloud ks zone add satellite</code> command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_zone_add_sat) to add zones to {{site.data.keyword.openshiftshort}} clusters and worker pools in {{site.data.keyword.satelliteshort}}.</li><li>Adds the optional <code>--host-label</code>, <code>--pod-subnet</code>, <code>--pull-secret</code>, <code>--service-subnet</code>, <code>--workers</code>, and <code>--zone</code> flags to the [<code>ibmcloud ks cluster create satellite</code> command](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite).</li><li>Adds the <code>satellite</code> value to the <code>--provider</code> flag in the [<code>ibmcloud ks zone ls</code> command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_datacenters) to list zones in your {{site.data.keyword.satelliteshort}} location.</li></ul></li></ul> |
| 1.0.197 | 18 Nov 2020 | Adds the `--endpoint` flag to the [`ibmcloud oc cluster config` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_cluster_config) to use the Link endpoint URL for the cluster context. |
| 1.0.178 | 06 Oct 2020 | <ul><li>Adds <code>http-tunnel</code> as a supported source or destination protocol in the <code>ibmcloud sat endpoint create</code> and <code>ibmcloud sat endpoint update</code> commands.</li><li>Updates the Go version to 1.15.2.</li><li>Updates the help text in various languages.</li></ul> |
{: caption="Overview of version changes for version 1.0 of the CLI plug-in for {{site.data.keyword.satelliteshort}} commands" caption-side="top"}
{: summary="The rows are read from left to right, with the CLI plug-in version in column one, the release date of the version in column two, and a brief description of the changes for the version in column three."}


