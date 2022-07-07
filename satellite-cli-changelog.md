---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite cli changelog, satellite commands, satellite cli, satellite reference, change log, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# CLI change log for {{site.data.keyword.satelliteshort}} commands
{: #satellite-cli-changelog}

In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available, which includes updates for {{site.data.keyword.satellitelong}}. Be sure to keep your CLI up-to-date so that you can use all available commands and options.
{: shortdesc}

Refer to the following tables for a summary of changes for each version of the [plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli), which uses the `ibmcloud sat` alias.

To see the CLI plug-in changes that pertain specifically to {{site.data.keyword.redhat_openshift_notm}} (`ibmcloud oc`) commands, see the [{{site.data.keyword.redhat_openshift_notm}} CLI change log](/docs/openshift?topic=openshift-cs_cli_changelog).
{: tip}

## Version 1.0
{: #10}

Review the following changes for 1.0 versions of the CLI plug-in.
{: shortdesc}

|Version|Release date|Changes|
|-------|------------|-------|
| 1.0.426 | 6 Jul 2022  | 1. Adds the `ibmcloud sat key` commands, allowing you to view and manage your {{site.data.keyword.satelliteshort}} Config keys.  /n 2. Adds the `ibmcloud sat subscription identity set` command, which updates a Satellite subscription to use your identity to manage resources.  |
| 1.0.422 |  20 Jun 2022  | 1. Makes the `template-version` flag optional for the `ibmcloud sat storage config create` command.  /n 2. Updates the output of the `ibmcloud sat storage template ls` and `ibmcloud sat storage template get` commands to specify if a version is deprecated or the current default template version.  |
| 1.0.420 | 15 Jun 2022 | 1. Adds the new `ibmcloud sat storage config param set` command.  /n 2. Updates the `ibmcloud sat storage template get` command output to indicate whether a storage config is mutable. |
| 1.0.415 | 26 May 2022 | Updates the help text in various languages. |
| 1.0.404 | 28 Apr 2022 | Adds features visible to select users only. |
| 1.0.403 | 26 Apr 2022 | 1. Adds the `ibmcloud sat storage assignment upgrade` and `ibmcloud sat storage config upgrade` commands.  \n 2. Updates the CLI help text in various languages. | 
| 1.0.394 | 7 Apr 2022 | Adds the `ibmcloud sat location dns get` command. |
| 1.0.374 | 24 Feb 2022 | 1. Removes the default zone in the `ibmcloud ks cluster create satellite` command. \n 2. Fixes a routing issue for the `ibmcloud sat storage assignment create --cluster` and the `ibmcloud sat storage assignment ls --cluster` commands. |
| 1.0.372 | 18 Feb 2022 | 1. Updates the `ibmcloud ks location get` command output to indicate whether the location has IaaS provider credentials stored.  \n 2. Updates the `ibmcloud ks storage assignment ls` command to include the `--config` flag option, allowing you to list only storage assignments created with the specified configuration.  |
| 1.0.331 | 03 Dec 2021 | 1. Updates the `ibmcloud sat location get` command output to include `Ignition Server Port` and `Konnectivity Server Port`.  \n 2. Updates the help text in various languages. |
| 1.0.331 | 12 Oct 2021 | Adds the `--output` option for the `ibmcloud sat storage get` command. |
| 1.0.327 | 11 Oct 2021 | Adds the `--data-location` option for the `ibmcloud sat config create` command and the `--location` option for the `ibmcloud sat storage assingment ls` command. Updates the help text in various languages. |
| 1.0.312 | 09 Aug 2021 | 1. Includes the subscription status in the output of the `ibmcloud sat subscription get` and `ibmcloud sat subscription ls` commands.  \n 1. Adds the option to display the output of the `ibmcloud sat subscription get` and `ibmcloud sat subscription ls` commands in JSON format.  \n 1. Updates the help text in various languages.  \n 1. Displays OpenVPN Server Port details in the `ibmcloud sat location get` command output. | 
| 1.0.300 | 12 Jul 2021 | Adds the following parameters to the `ibmcloud sat location create` command so that you can optionally specify an infrastructure provider along with the region and credentials for the provider: `--provider`, `--provider-region`, and `--provider-credential`. Updates the help text in various languages.|
| 1.0.295 | 24 Jun 2021 |  1. The `ibmcloud sat storage config` and `ibmcloud sat storage template` commands are now generally available.  \n 1. Adds the `--cluster` and `--service-cluster-id` option to the `ibmcloud sat storage assignment ls` command to filter output by the ID of a cluster that you created or the ID of a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster.  \n 1. Adds the `--service-cluster-id` option to the `ibmcloud sat storage assignment create` command to deploy storage drivers to a specific {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster.  \n 1. Updates the help text in various languages. |
| 1.0.275 | 26 May 2021 |  1. Adds the following changes to storage commands: 1. Adds the `--cluster` option to the `ibmcloud sat storage assignment create` command to assign to an individual cluster ID instead of a cluster group.  \n 1. Adds `--location` as a required option to the `ibmcloud sat storage config create` command and an optional option to the `ibmcloud sat storage config ls` command.  \n 1. Changes the `--config` option to `--config-name` in the `ibmcloud sat storage config sc add` command.  \n 1. Adds the optional `--output json` option to the `ibmcloud sat config get`, `ibmcloud sat config ls`, and `ibmcloud sat config version get` commands.  \n 1. Increases the maximum size for the file in the `--read-config` option of the `ibmcloud sat config version create` command from 1MB to 3MB.  \n 1. Fixes the `ibmcloud sat group create` command to enable cluster group creation.  \n 1. The IAM token that is used for your CLI session is now refreshed 5 minutes before expiration to keep the session active.  \n 1. Updates the help text in various languages. |
| 1.0.258 | 26 Apr 2021 |  1. Adds the `ibmcloud sat storage config sc add`, `ibmcloud sat storage config sc get`, and `ibmcloud sat storage config sc ls` beta commands to create and view custom storage classes of {{site.data.keyword.satelliteshort}} storage configurations.  \n 1. Adds the `ibmcloud sat messages` command to view current messages from {{site.data.keyword.satellitelong_notm}}.  \n 1. Fixes a `golang` vulnerability for [CVE-2020-28852](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-28852){: external}.  \n 1. Updates the help text in various languages. |
| 1.0.233 | 01 Mar 2021 |  1. Adds the `ibmcloud sat storage config rm` command to remove a {{site.data.keyword.satelliteshort}} storage configuration.  \n 1. Removes `beta` tags from `ibmcloud sat` commands for the generally available release of {{site.data.keyword.satellitelong_notm}}. |
| 1.0.231 | 25 Feb 2021 |  1. Adds the `ibmcloud sat service ls` command to view {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters in your {{site.data.keyword.satelliteshort}} location.  \n 1. Updates the Go version to 1.15.8.  \n 1. Updates the help text in various languages. |
| 1.0.223 | 08 Feb 2021 |  1. Adds the `ibmcloud sat storage` command group to view and manage the storage resources that run in {{site.data.keyword.redhat_openshift_notm}} clusters that are registered with {{site.data.keyword.satelliteshort}} Config.  \n 1. Adds the optional `--ha-zone` option to the `ibmcloud sat location create` command to specify three arbitrary zone names in your {{site.data.keyword.satelliteshort}} location.  \n 1. Adds the optional `--reset-key` option to the `ibmcloud sat host attach` command to reset the key that the control plane uses to communicate with all the hosts in the location.  \n 1. Moves `ibmcloud sat config configuration` commands to the `ibmcloud sat config` command group.  \n 1. Moves `ibmcloud sat config subscription` commands to the `ibmcloud sat subscription` command group.  \n 1. Renames `cluster-group` commands and `--cluster-group` flags to `group`.  \n 1. Renames `configuration` commands and `--configuration` flags to `config`.  \n 1. Renames `--type` flags to `--file-format`.  \n 1. Renames the `--label` (short form `-l`) option in the `ibmcloud sat host assign`, `attach`, and `update` commands to `--host-label` (short form `-hl`).  \n 1. The following changes are made to `ibmcloud oc` commands for managing {{site.data.keyword.redhat_openshift_notm}} clusters in {{site.data.keyword.satelliteshort}}: 1. Adds the [`ibmcloud ks worker-pool create satellite` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_worker_pool_create_sat) to add worker pools to {{site.data.keyword.redhat_openshift_notm}} clusters in {{site.data.keyword.satelliteshort}}.  \n 1. Adds the [`ibmcloud ks zone add satellite` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_zone_add_sat) to add zones to {{site.data.keyword.redhat_openshift_notm}} clusters and worker pools in {{site.data.keyword.satelliteshort}}.  \n 1. Adds the optional `--host-label`, `--pod-subnet`, `--pull-secret`, `--service-subnet`, `--workers`, and `--zone` flags to the [`ibmcloud ks cluster create satellite` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite).  \n 1. Adds the `satellite` value to the `--provider` option in the [`ibmcloud ks zone ls` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_datacenters) to list zones in your {{site.data.keyword.satelliteshort}} location. |
| 1.0.197 | 18 Nov 2020 | Adds the `--endpoint` option to the [`ibmcloud oc cluster config` command](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_cluster_config) to use the Link endpoint URL for the cluster context. |
| 1.0.178 | 06 Oct 2020 |  1. Adds `http-tunnel` as a supported source or destination protocol in the `ibmcloud sat endpoint create` and `ibmcloud sat endpoint update` commands.  \n 1. Updates the Go version to 1.15.2.  \n 1. Updates the help text in various languages. |
{: caption="Overview of version changes for version 1.0 of the CLI plug-in for Satellite commands" caption-side="top"}
{: summary="The rows are read from left to right, with the CLI plug-in version in column one, the release date of the version in column two, and a brief description of the changes for the version in column three."}
