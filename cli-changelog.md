---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-09"

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
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
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
| 1.0.275 | 26 May 2021 | <ul><li>Adds the following changes to storage commands:<ul><li>Adds the `--cluster` flag to the `ibmcloud sat storage assignment create` command to assign to an individual cluster ID instead of a cluster group.</li><li>Adds `--location` as a required flag to the `ibmcloud sat storage config create` command and an optional flag to the `ibmcloud sat storage config ls` command.</li><li>Changes the `--config` flag to `--config-name` in the `ibmcloud sat storage config sc add` command.</li></ul></li><li>Adds the optional `--output json` flag to the `ibmcloud sat config get`, `ibmcloud sat config ls`, and `ibmcloud sat config version get` commands.</li><li>Increases the maximum size for the file in the `--read-config` flag of the `ibmcloud sat config version create` command from 1MB to 3MB.</li><li>Fixes the `ibmcloud sat group create` command to enable cluster group creation.</li><li>The IAM token that is used for your CLI session is now refreshed 5 minutes before expiration to keep the session active.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.258 | 26 Apr 2021 | <ul><li>Adds the `ibmcloud sat storage config sc add`, `ibmcloud sat storage config sc get`, and `ibmcloud sat storage config sc ls` beta commands to create and view custom storage classes of {{site.data.keyword.satelliteshort}} storage configurations.</li><li>Adds the `ibmcloud sat messages` command to view current messages from {{site.data.keyword.satellitelong_notm}}.</li><li>Fixes a `golang` vulnerability for [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.233 | 01 Mar 2021 | <ul><li>Adds the `ibmcloud sat storage config rm` command to remove a {{site.data.keyword.satelliteshort}} storage configuration.</li><li>Removes `beta` tags from `ibmcloud sat` commands for the generally available release of {{site.data.keyword.satellitelong_notm}}.</li></ul> |
| 1.0.231 | 25 Feb 2021 | <ul><li>Adds the `ibmcloud sat service ls` command to view {{site.data.keyword.satelliteshort}}-enabled service clusters in your {{site.data.keyword.satelliteshort}} location.</li><li>Updates the Go version to 1.15.8.</li><li>Updates the help text in various languages.</li></ul> |
| 1.0.223 | 08 Feb 2021 | <ul><li>Adds the `ibmcloud sat storage` command group to view and manage the storage resources that run in {{site.data.keyword.openshiftshort}} clusters that are registered with {{site.data.keyword.satelliteshort}} config.</li><li>Adds the optional `--ha-zone` flag to the `ibmcloud sat location create` command to specify three arbitrary zone names in your {{site.data.keyword.satelliteshort}} location.</li><li>Adds the optional `--reset-key` flag to the `ibmcloud sat host attach` command to reset the key that the control plane uses to communicate with all of the hosts in the location.</li><li>Moves `ibmcloud sat config configuration` commands to the `ibmcloud sat config` command group.</li><li>Moves `ibmcloud sat config subscription` commands to the `ibmcloud sat subscription` command group.</li><li>Renames `cluster-group` commands and `--cluster-group` flags to `group`.</li><li>Renames `configuration` commands and `--configuration` flags to `config`.</li><li>Renames `--type` flags to `--file-format`.</li><li>Renames the `--label` (short form `-l`) flag in the `ibmcloud sat host assign`, `attach`, and `update` commands to `--host-label` (short form `-hl`).</li><li>The following changes are made to `ibmcloud oc` commands for managing {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}:<ul><li>Adds the [`ibmcloud ks worker-pool create satellite` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_worker_pool_create_sat) to add worker pools to {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}.</li><li>Adds the [`ibmcloud ks zone add satellite` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_zone_add_sat) to add zones to {{site.data.keyword.openshiftshort}} clusters and worker pools in {{site.data.keyword.satelliteshort}}.</li><li>Adds the optional `--host-label`, `--pod-subnet`, `--pull-secret`, `--service-subnet`, `--workers`, and `--zone` flags to the [`ibmcloud ks cluster create satellite` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite).</li><li>Adds the `satellite` value to the `--provider` flag in the [`ibmcloud ks zone ls` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_datacenters) to list zones in your {{site.data.keyword.satelliteshort}} location.</li></ul></li></ul> |
| 1.0.197 | 18 Nov 2020 | Adds the `--endpoint` flag to the [`ibmcloud oc cluster config` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_cluster_config) to use the Link endpoint URL for the cluster context. |
| 1.0.178 | 06 Oct 2020 | <ul><li>Adds `http-tunnel` as a supported source or destination protocol in the `ibmcloud sat endpoint create` and `ibmcloud sat endpoint update` commands.</li><li>Updates the Go version to 1.15.2.</li><li>Updates the help text in various languages.</li></ul> |
{: caption="Overview of version changes for version 1.0 of the CLI plug-in for {{site.data.keyword.satelliteshort}} commands" caption-side="top"}
{: summary="The rows are read from left to right, with the CLI plug-in version in column one, the release date of the version in column two, and a brief description of the changes for the version in column three."}
