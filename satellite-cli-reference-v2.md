---


copyright:
  years: 2019, 2026
lastupdated: "2026-07-30"

keywords: satellite cli reference, satellite commands, satellite cli, satellite reference

subcollection: satellite

content-type: cli-docs

---



{{site.data.keyword.attribute-definition-list}}



# CLI reference for {{site.data.keyword.satelliteshort}} commands
{: #satellite-cli-reference}

Refer to these commands when you want to automate the creation and management of your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

To install the CLI, see [Installing the the CLI](/docs/satellite?topic=satellite-cli-install). To view a high-level map of all the {{site.data.keyword.satellitelong_notm}} commands, see the [CLI map](/docs/satellite?topic=satellite-icsat_map).
{: tip}

## Before you begin
{: #satellite-cli-prereq}

Install the required CLI plug-ins.

```sh
ibmcloud plugin install oc
```
{: pre}


## Prerequisites
{: #sat-cli-prereq}

* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/satellite?topic=satellite-cli-install).
* Install the `ks` plug-in by running the following command:

   ```sh
   ibmcloud plugin install ks
   ```
   {: pre}

You're notified on the command line when updates to the  CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running **`ibmcloud plugin list`**.
{: tip}

## ibmcloud sat commands
{: #cli_commands}

The following tables list the `ibmcloud sat` command groups. For a complete list of all `ibmcloud sat` commands as they are structured in the CLI, see the [CLI map](/docs/satellite?topic=satellite-icsat_map).
{: shortdesc}

| Command group | Description |
| --- | --- |
| [`ibmcloud sat connector`](#connector-cli) | Create, view, and modify Satellite connectors. |
| [`ibmcloud sat location`](#location-cli) | Create, view, and modify Satellite locations. |
| [`ibmcloud sat endpoint`](#endpoint-cli) | View and manage Satellite endpoints. |
| [`ibmcloud sat acl`](#acl-cli) | View and manage Satellite access control lists (ACLs). |
| [`ibmcloud sat agent`](#agent-cli) | Attach or view Satellite Connector Agents. |
| [`ibmcloud sat host`](#host-cli) | View and modify Satellite hosts. |
| [`ibmcloud sat cluster`](#cluster-cli) | Register and manage clusters for use with Satellite configurations. |
| [`ibmcloud sat messages`](#messages-cli) | View the current user messages. |
| [`ibmcloud sat group`](#group-cli) | View and manage Satellite cluster groups. Cluster groups are used to subscribe clusters to Satellite configurations of Kubernetes resources. |
| [`ibmcloud sat key`](#key-cli) | View and manage Satellite Config keys. |
| [`ibmcloud sat config`](#config-cli) | View and manage Satellite Configuration. |
| [`ibmcloud sat resource`](#resource-cli) | Search and view Kubernetes resources that are managed by a Satellite configuration. |
| [`ibmcloud sat service`](#service-cli) | View Satellite service clusters. |
| [`ibmcloud sat storage`](#storage-cli) | View and manage Satellite storage resources. |
| [`ibmcloud sat subscription`](#subscription-cli) | View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters. |
{: caption="ibmcloud sat CLI command groups" caption-side="bottom"}


## `ibmcloud sat acl` commands
{: #acl-cli}

View and manage Satellite access control lists (ACLs).

### `ibmcloud sat acl create`
{: #acl-create-cli}



Create an ACL.
{: shortdesc}

```sh
ibmcloud sat acl create --name NAME --subnet SUBNET [--subnet SUBNET ...] [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-create-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    A name or ID of an endpoint to enable for this ACL.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    The name for the ACL.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet SUBNET`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #acl-create-examples}

Create an ACL.

```sh
ibmcloud sat acl create --location LOCATION --connector-id CONNECTOR_ID --name NAME
```
{: pre}


### `ibmcloud sat acl endpoint add`
{: #acl-endpoint-add-cli}



Add one or more enabled endpoints to an ACL.
{: shortdesc}

```sh
ibmcloud sat acl endpoint add --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-endpoint-add-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    A name or ID of an endpoint to enable for this ACL.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-endpoint-add-examples}

Add one or more enabled endpoints to an ACL.

```sh
ibmcloud sat acl endpoint add \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl endpoint ls`
{: #acl-endpoint-ls-cli}



List all enabled endpoints for an ACL.
{: shortdesc}

```sh
ibmcloud sat acl endpoint ls --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-endpoint-ls-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-endpoint-ls-examples}

List all enabled endpoints for an ACL.

```sh
ibmcloud sat acl endpoint ls \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl endpoint rm`
{: #acl-endpoint-rm-cli}



Remove one or more enabled endpoints from an ACL.
{: shortdesc}

```sh
ibmcloud sat acl endpoint rm --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-endpoint-rm-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    A name or ID of an endpoint to disable for this ACL.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-endpoint-rm-examples}

Remove one or more enabled endpoints from an ACL.

```sh
ibmcloud sat acl endpoint rm \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl get`
{: #acl-get-cli}



View the details of an ACL.
{: shortdesc}

```sh
ibmcloud sat acl get --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-get-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-get-examples}

View the details of an ACL.

```sh
ibmcloud sat acl get --location LOCATION --connector-id CONNECTOR_ID --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl ls`
{: #acl-ls-cli}



List all ACLs for a Satellite connector or location.
{: shortdesc}

```sh
ibmcloud sat acl ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-ls-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-ls-examples}

List all ACLs for a Satellite connector or location.

```sh
ibmcloud sat acl ls --location LOCATION --connector-id CONNECTOR_ID --output json
```
{: pre}


### `ibmcloud sat acl rm`
{: #acl-rm-cli}



Delete an ACL.
{: shortdesc}

```sh
ibmcloud sat acl rm --acl-id ID [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-rm-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-rm-examples}

Delete an ACL.

```sh
ibmcloud sat acl rm --location LOCATION --connector-id CONNECTOR_ID --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl subnet add`
{: #acl-subnet-add-cli}



Add one or more subnets to an ACL.
{: shortdesc}

```sh
ibmcloud sat acl subnet add --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-subnet-add-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet SUBNET`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #acl-subnet-add-examples}

Add one or more subnets to an ACL.

```sh
ibmcloud sat acl subnet add \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl subnet rm`
{: #acl-subnet-rm-cli}



Remove one or more subnets from an ACL.
{: shortdesc}

```sh
ibmcloud sat acl subnet rm --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-subnet-rm-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet SUBNET`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #acl-subnet-rm-examples}

Remove one or more subnets from an ACL.

```sh
ibmcloud sat acl subnet rm \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat acl update`
{: #acl-update-cli}



Update the name of an ACL.
{: shortdesc}

```sh
ibmcloud sat acl update --acl-id ID --name NAME [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #acl-update-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    The new name for the ACL.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-update-examples}

Update the name of an ACL.

```sh
ibmcloud sat acl update \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}



## `ibmcloud sat agent` commands
{: #agent-cli}

Attach or view Satellite Connector Agents.

### `ibmcloud sat agent attach`
{: #agent-attach-cli}



Get a Satellite Connector Agent for a specific platform. Download the Agent `.zip` for Windows or get a link to the documentation for Docker environments.
{: shortdesc}

```sh
ibmcloud sat agent attach --platform PLATFORM [-q]
```
{: pre}

#### Command options
{: #agent-attach-options}


`--platform PLATFORM`
:    The platform for the Satellite Connector Agent. For more information about Docker, see the documentation at https://ibm.biz/satconagent Available options: windows, docker

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #agent-attach-examples}

Get a Satellite Connector Agent for a specific platform.

```sh
ibmcloud sat agent attach --platform PLATFORM -q
```
{: pre}


### `ibmcloud sat agent ls`
{: #agent-ls-cli}



List all Agents for a Satellite Connector.
{: shortdesc}

```sh
ibmcloud sat agent ls --connector-id ID [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #agent-ls-options}


`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #agent-ls-examples}

List all Agents for a Satellite Connector.

```sh
ibmcloud sat agent ls --connector-id CONNECTOR_ID --output json -q
```
{: pre}



## `ibmcloud sat cluster` commands
{: #cluster-cli}

Register and manage clusters for use with Satellite configurations.

### `ibmcloud sat cluster get`
{: #cluster-get-cli}

[Virtual Private Cloud]{: tag-vpc} [Classic infrastructure]{: tag-classic-inf} [Satellite]{: tag-satellite} 

Get the details of a registered cluster.
{: shortdesc}

```sh
ibmcloud sat cluster get --cluster CLUSTER [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #cluster-get-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or the ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #cluster-get-examples}

Get the details of a registered cluster.

```sh
ibmcloud sat cluster get --cluster CLUSTER_NAME_OR_ID --output json -q
```
{: pre}


### `ibmcloud sat cluster ls`
{: #cluster-ls-cli}

[Virtual Private Cloud]{: tag-vpc} [Classic infrastructure]{: tag-classic-inf} [Satellite]{: tag-satellite} 

List all registered clusters in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat cluster ls [--filter FILTER] [--limit LIMIT] [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #cluster-ls-options}


`--filter FILTER`
:    Filter registered clusters by cluster ID.

`--limit LIMIT`
:    Limit the number of clusters that are returned.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #cluster-ls-examples}

List all registered clusters in your IBM Cloud account.

```sh
ibmcloud sat cluster ls --filter FILTER --limit LIMIT --output json
```
{: pre}


### `ibmcloud sat cluster register`
{: #cluster-register-cli}



Get a `kubectl` command to register your cluster in a Satellite configuration. Log in to your cluster and run this command to install a Satellite Config agent. Clusters that you run in your Satellite location automatically install this agent.
{: shortdesc}

```sh
ibmcloud sat cluster register --name NAME [-q] [--silent]
```
{: pre}

#### Command options
{: #cluster-register-options}


`--name NAME`
:    Specify the name of the cluster that you want to register

`-q`
:    Do not show the message of the day or update reminders.

`--silent`
:    Silent. Return only the registration command in the output.


#### Examples
{: #cluster-register-examples}

Get a `kubectl` command to register your cluster in a Satellite configuration.

```sh
ibmcloud sat cluster register --silent SILENT --name NAME -q
```
{: pre}


### `ibmcloud sat cluster unregister`
{: #cluster-unregister-cli}



Remove a cluster registration. The cluster is no longer subscribed to a Satellite configuration, but the cluster and its existing resources still run.
{: shortdesc}

```sh
ibmcloud sat cluster unregister --cluster CLUSTER [-f] [-q]
```
{: pre}

#### Command options
{: #cluster-unregister-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or the ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #cluster-unregister-examples}

Remove a cluster registration.

```sh
ibmcloud sat cluster unregister --cluster CLUSTER_NAME_OR_ID -f -q
```
{: pre}



## `ibmcloud sat config` commands
{: #config-cli}

View and manage Satellite Configuration.

### `ibmcloud sat config create`
{: #config-create-cli}



Create a configuration to specify what Kubernetes resources you want to deploy to your clusters in your Satellite workloads.
{: shortdesc}

```sh
ibmcloud sat config create --name NAME [-q] (--data-location LOCATION | --provider PROVIDER)
```
{: pre}

#### Command options
{: #config-create-options}


`--data-location LOCATION`
:    Specify the IBM region to store the Satellite configuration data. Strategy: Direct Upload.

`--name NAME`
:    Provide a name for the Satellite configuration.

`--provider PROVIDER`
:    Indicate the remote GitOps provider for the Satellite configuration. This provider stores the Kubernetes resource definitions. Strategy: GitOps. Allowed values: github, gitlab

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-create-examples}

Create a configuration to specify what Kubernetes resources you want to deploy to your clusters in your Satellite worklo.

```sh
ibmcloud sat config create \
  --name NAME \
  --data-location LOCATION \
  --provider PROVIDER_ID
```
{: pre}


### `ibmcloud sat config get`
{: #config-get-cli}



Get details of a Satellite configuration, such as the versions or subscriptions that are associated with the configuration.
{: shortdesc}

```sh
ibmcloud sat config get --config CONFIG [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #config-get-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-get-examples}

Get details of a Satellite configuration, such as the versions or subscriptions that are associated with the configurati.

```sh
ibmcloud sat config get --config CONFIG --output json -q
```
{: pre}


### `ibmcloud sat config ls`
{: #config-ls-cli}



List all Satellite configurations in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat config ls [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #config-ls-options}


`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-ls-examples}

List all Satellite configurations in your IBM Cloud account.

```sh
ibmcloud sat config ls --output json -q
```
{: pre}


### `ibmcloud sat config rename`
{: #config-rename-cli}



Rename a Satellite configuration.
{: shortdesc}

```sh
ibmcloud sat config rename --config CONFIG --name NAME [-q]
```
{: pre}

#### Command options
{: #config-rename-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--name NAME`
:    Provide a new name for the Satellite configuration.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-rename-examples}

Rename a Satellite configuration.

```sh
ibmcloud sat config rename --config CONFIG --name NAME -q
```
{: pre}


### `ibmcloud sat config rm`
{: #config-rm-cli}



Remove a Satellite configuration. All associated subscriptions must be removed first. All versions are deleted. Back up any resource definitions that you want to keep.
{: shortdesc}

```sh
ibmcloud sat config rm --config CONFIG [-f] [-q]
```
{: pre}

#### Command options
{: #config-rm-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-rm-examples}

Remove a Satellite configuration.

```sh
ibmcloud sat config rm --config CONFIG -f -q
```
{: pre}


### `ibmcloud sat config version create`
{: #config-version-create-cli}



Create a configuration version to update existing Kubernetes resources for your Satellite workloads.
{: shortdesc}

```sh
ibmcloud sat config version create --config CONFIG --file-format FORMAT --name NAME --read-config CONFIG [--description DESCRIPTION] [-q]
```
{: pre}

#### Command options
{: #config-version-create-options}


`--config CONFIG`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--description DESCRIPTION`
:    Add a description for the Satellite configuration version.

`--file-format FORMAT`
:    Indicate the file format of the configuration version. Available options: yaml

`--name NAME`
:    Provide a name for the Satellite configuration version.

`-q`
:    Do not show the message of the day or update reminders.

`--read-config CONFIG`
:    Specify the file path for the configuration version file.


#### Examples
{: #config-version-create-examples}

Create a configuration version to update existing Kubernetes resources for your Satellite workloads.

```sh
ibmcloud sat config version create \
  --name NAME \
  --config CONFIG \
  --file-format FILE_PATH
```
{: pre}


### `ibmcloud sat config version get`
{: #config-version-get-cli}



Get details for a Satellite configuration version.
{: shortdesc}

```sh
ibmcloud sat config version get --config CONFIG --version VERSION [--output OUTPUT] [-q] [--save-config]
```
{: pre}

#### Command options
{: #config-version-get-options}


`--config CONFIG`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--save-config`
:    Download and save the configuration version to a temporary file.

`--version VERSION`
:    Specify the name or ID of the Satellite configuration version. To list versions in your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`.


#### Examples
{: #config-version-get-examples}

Get details for a Satellite configuration version.

```sh
ibmcloud sat config version get \
  --config CONFIG \
  --version VERSION \
  --save-config SAVE-CONFIG
```
{: pre}


### `ibmcloud sat config version rm`
{: #config-version-rm-cli}



Remove a Satellite configuration version.
{: shortdesc}

```sh
ibmcloud sat config version rm --config CONFIG --version VERSION [-f] [-q]
```
{: pre}

#### Command options
{: #config-version-rm-options}


`--config CONFIG`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--version VERSION`
:    Indicate the name or ID of the Satellite configuration version. To list versions, run `ibmcloud sat config get --config <configuration_name_or_ID>`.


#### Examples
{: #config-version-rm-examples}

Remove a Satellite configuration version.

```sh
ibmcloud sat config version rm --config CONFIG --version VERSION -f
```
{: pre}



## `ibmcloud sat connector` commands
{: #connector-cli}

Create, view, and modify Satellite connectors.

### `ibmcloud sat connector create`
{: #connector-create-cli}



Create a Satellite connector.
{: shortdesc}

```sh
ibmcloud sat connector create --name NAME --region REGION [-q]
```
{: pre}

#### Command options
{: #connector-create-options}


`--name NAME`
:    The name for the Satellite connector.

`-q`
:    Do not show the message of the day or update reminders.

`--region REGION`
:    The IBM Cloud region to manage your Satellite connector.


#### Examples
{: #connector-create-examples}

Create a Satellite connector.

```sh
ibmcloud sat connector create --name NAME --region REGION -q
```
{: pre}


### `ibmcloud sat connector get`
{: #connector-get-cli}



View the details of a Satellite Connector.
{: shortdesc}

```sh
ibmcloud sat connector get --connector-id ID [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #connector-get-options}


`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #connector-get-examples}

View the details of a Satellite Connector.

```sh
ibmcloud sat connector get --connector-id CONNECTOR_ID --output json -q
```
{: pre}


### `ibmcloud sat connector ls`
{: #connector-ls-cli}



View the Satellite Connectors in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat connector ls [--after AFTER] [--first FIRST] [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #connector-ls-options}


`--after AFTER`
:    Show Satellite Connectors after the given cursor.

`--first FIRST`
:    View the next Satellite Connectors, up to the first number of Connectors.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #connector-ls-examples}

View the Satellite Connectors in your IBM Cloud account.

```sh
ibmcloud sat connector ls --after AFTER --first FIRST --output json
```
{: pre}


### `ibmcloud sat connector rm`
{: #connector-rm-cli}



Delete a Satellite connector.
{: shortdesc}

```sh
ibmcloud sat connector rm --connector-id ID [-f] [-q]
```
{: pre}

#### Command options
{: #connector-rm-options}


`--connector-id ID`
:    The ID of a Satellite connector.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #connector-rm-examples}

Delete a Satellite connector.

```sh
ibmcloud sat connector rm --connector-id CONNECTOR_ID -f -q
```
{: pre}



## `ibmcloud sat endpoint` commands
{: #endpoint-cli}

View and manage Satellite endpoints.

### `ibmcloud sat endpoint authn get`
{: #endpoint-authn-get-cli}



Get the authentication settings for an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint authn get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-authn-get-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-authn-get-examples}

Get the authentication settings for an endpoint.

```sh
ibmcloud sat endpoint authn get \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint authn rotate`
{: #endpoint-authn-rotate-cli}



Replace existing authentication certificates with new ones. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Only the certificates that you specify are replaced.
{: shortdesc}

```sh
ibmcloud sat endpoint authn rotate --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-authn-rotate-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the destination server's certificate. For example `myCA.pem`.

`--dest-cert-file FILE`
:    The client certificate used to authenticate with the destination server. For example `myCert.pem`.

`--dest-key-file FILE`
:    The client private key used to encrypt the client certificate. For example `myKey.pem`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--source-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the source client's certificate when source-tls-mode is mutual. For example `myCA.pem`.

`--source-cert-file FILE`
:    The server certificate to present to the source client. For example `myCert.pem`.

`--source-key-file FILE`
:    The server private key used to encrypt the server certificate. For example `myKey.pem`.


#### Examples
{: #endpoint-authn-rotate-examples}

Replace existing authentication certificates with new ones.

```sh
ibmcloud sat endpoint authn rotate \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint authn set`
{: #endpoint-authn-set-cli}



Set authentication settings for an endpoint. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.
{: shortdesc}

```sh
ibmcloud sat endpoint authn set --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [--dest-tls-mode MODE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] [--source-tls-mode MODE] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-authn-set-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the destination server's certificate. For example `myCA.pem`.

`--dest-cert-file FILE`
:    The client certificate used to authenticate with the destination server. For example `myCert.pem`.

`--dest-key-file FILE`
:    The client private key used to encrypt the client certificate. For example `myKey.pem`.

`--dest-tls-mode MODE`
:    The destination TLS mode. Accepted values: `simple`, `mutual`, `none`

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--source-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the source client's certificate when source-tls-mode is mutual. For example `myCA.pem`.

`--source-cert-file FILE`
:    The server certificate to present to the source client. For example `myCert.pem`.

`--source-key-file FILE`
:    The server private key used to encrypt the server certificate. For example `myKey.pem`.

`--source-tls-mode MODE`
:    The source TLS mode. Accepted values: `simple`, `mutual`


#### Examples
{: #endpoint-authn-set-examples}

Set authentication settings for an endpoint.

```sh
ibmcloud sat endpoint authn set \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint create`
{: #endpoint-create-cli}



Create an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint create --dest-hostname HOSTNAME --dest-port PORT --dest-type TYPE --name NAME --source-protocol PROTOCOL [--dest-protocol PROTOCOL] [--idle-timeout-seconds SECONDS] [--output OUTPUT] [-q] [--sni SNI] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-create-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-hostname HOSTNAME`
:    Indicate the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `cloud` endpoints, this value must resolve to a public IP address or to a private IP address that is accessible within IBM Cloud such as a private cloud service endpoint. For `location` endpoints, this value must resolve from and be reachable from the control plane hosts for Satellite locations or where the agent runs for Satellite Connector.

`--dest-port PORT`
:    Provide the port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol PROTOCOL`
:    Specify the destination's protocol. If you do not specify this option, the destination protocol is inherited from the source protocol. Accepted values: `TCP`, `TLS`

`--dest-type TYPE`
:    Specify where the destination resource runs, either in IBM Cloud (`cloud`) or your Satellite location (`location`). Available options: location, cloud

`--idle-timeout-seconds SECONDS`
:    Specify the timeout interval in seconds for active connections to the destination. Make sure your timeout is compatible with the destination service and protocol `keep-alive` settings.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    Provide a name for the endpoint.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--sni SNI`
:    Specify the server name indicator, if you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake.

`--source-protocol PROTOCOL`
:    Provide the protocol that the source uses to connect the destination resource. See [http://ibm.biz/endpoint-protocols](http://ibm.biz/endpoint-protocols). Available options: TCP, TLS, HTTP, HTTPS, HTTP-tunnel


#### Examples
{: #endpoint-create-examples}

Create an endpoint.

```sh
ibmcloud sat endpoint create \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --name NAME
```
{: pre}


### `ibmcloud sat endpoint disable`
{: #endpoint-disable-cli}



Disable an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint disable --endpoint ENDPOINT [-f] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-disable-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`-f`
:    Force the command to run without user prompts.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-disable-examples}

Disable an endpoint.

```sh
ibmcloud sat endpoint disable \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint enable`
{: #endpoint-enable-cli}



Enable an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint enable --endpoint ENDPOINT [-f] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-enable-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`-f`
:    Force the command to run without user prompts.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-enable-examples}

Enable an endpoint.

```sh
ibmcloud sat endpoint enable \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint get`
{: #endpoint-get-cli}



View the details of an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-get-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-get-examples}

View the details of an endpoint.

```sh
ibmcloud sat endpoint get \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint ls`
{: #endpoint-ls-cli}



List all endpoints in a Satellite location.
{: shortdesc}

```sh
ibmcloud sat endpoint ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-ls-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-ls-examples}

List all endpoints in a Satellite location.

```sh
ibmcloud sat endpoint ls --location LOCATION --connector-id CONNECTOR_ID --output json
```
{: pre}


### `ibmcloud sat endpoint rm`
{: #endpoint-rm-cli}



Delete an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint rm --endpoint ENDPOINT [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-rm-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-rm-examples}

Delete an endpoint.

```sh
ibmcloud sat endpoint rm \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat endpoint update`
{: #endpoint-update-cli}



Update an endpoint. Only the options that you specify are updated.
{: shortdesc}

```sh
ibmcloud sat endpoint update --endpoint ENDPOINT [--dest-hostname HOSTNAME] [--dest-port PORT] [--dest-protocol PROTOCOL] [--idle-timeout-seconds SECONDS] [--name NAME] [-q] [--sni SNI] [--source-protocol PROTOCOL] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #endpoint-update-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-hostname HOSTNAME`
:    Indicate the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `cloud` endpoints, this value must resolve to a public IP address or to a private IP address that is accessible within IBM Cloud such as a private cloud service endpoint. For `location` endpoints, this value must resolve from and be reachable from the control plane hosts for Satellite locations or where the agent runs for Satellite Connector.

`--dest-port PORT`
:    Provide the port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol PROTOCOL`
:    Specify the destination's protocol. If you do not specify this option, the destination protocol is inherited from the source protocol. Accepted values: `TCP`, `TLS`

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--idle-timeout-seconds SECONDS`
:    Specify the timeout interval in seconds for active connections to the destination. Make sure your timeout is compatible with the destination service and protocol `keep-alive` settings.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    Provide a new name for the endpoint.

`-q`
:    Do not show the message of the day or update reminders.

`--sni SNI`
:    Specify the server name indicator, if you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake.

`--source-protocol PROTOCOL`
:    Provide the protocol that the source uses to connect the destination resource. See [http://ibm.biz/endpoint-protocols](http://ibm.biz/endpoint-protocols). Accepted values: `TCP`, `TLS`, `HTTP`, `HTTPS`, `HTTP-tunnel`


#### Examples
{: #endpoint-update-examples}

Update an endpoint.

```sh
ibmcloud sat endpoint update \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}



## `ibmcloud sat experimental` commands
{: #experimental-cli}

[Expires on 2024-11-25] Experiment with new commands. IMPORTANT: Commands here will retire after the [date] in their description.

### `ibmcloud sat experimental acl create`
{: #experimental-acl-create-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl create` instead] Create an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl create --name NAME --subnet SUBNET [--subnet SUBNET ...] [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-create-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    A name or ID of an endpoint to enable for this ACL.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    The name for the ACL.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet SUBNET`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #experimental-acl-create-examples}

Create an ACL.

```sh
ibmcloud sat experimental acl create \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --name NAME
```
{: pre}


### `ibmcloud sat experimental acl endpoint add`
{: #experimental-acl-endpoint-add-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl endpoint add` instead] Add one or more enabled endpoints to an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl endpoint add --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-endpoint-add-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    A name or ID of an endpoint to enable for this ACL.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-endpoint-add-examples}

Add one or more enabled endpoints to an ACL.

```sh
ibmcloud sat experimental acl endpoint add \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl endpoint ls`
{: #experimental-acl-endpoint-ls-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl endpoint ls` instead] List all enabled endpoints for an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl endpoint ls --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-endpoint-ls-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-endpoint-ls-examples}

List all enabled endpoints for an ACL.

```sh
ibmcloud sat experimental acl endpoint ls \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl endpoint rm`
{: #experimental-acl-endpoint-rm-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl endpoint rm` instead] Remove one or more enabled endpoints from an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl endpoint rm --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-endpoint-rm-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    A name or ID of an endpoint to disable for this ACL.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-endpoint-rm-examples}

Remove one or more enabled endpoints from an ACL.

```sh
ibmcloud sat experimental acl endpoint rm \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl get`
{: #experimental-acl-get-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl get` instead] View the details of an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl get --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-get-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-get-examples}

View the details of an ACL.

```sh
ibmcloud sat experimental acl get \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl ls`
{: #experimental-acl-ls-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl ls` instead] List all ACLs for a Satellite connector or location.
{: shortdesc}

```sh
ibmcloud sat experimental acl ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-ls-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-ls-examples}

List all ACLs for a Satellite connector or location.

```sh
ibmcloud sat experimental acl ls \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --output json
```
{: pre}


### `ibmcloud sat experimental acl rm`
{: #experimental-acl-rm-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl rm` instead] Delete an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl rm --acl-id ID [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-rm-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-rm-examples}

Delete an ACL.

```sh
ibmcloud sat experimental acl rm \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl subnet add`
{: #experimental-acl-subnet-add-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl subnet add` instead] Add one or more subnets to an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl subnet add --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-subnet-add-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet SUBNET`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #experimental-acl-subnet-add-examples}

Add one or more subnets to an ACL.

```sh
ibmcloud sat experimental acl subnet add \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl subnet rm`
{: #experimental-acl-subnet-rm-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl subnet rm` instead] Remove one or more subnets from an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl subnet rm --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-subnet-rm-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet SUBNET`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #experimental-acl-subnet-rm-examples}

Remove one or more subnets from an ACL.

```sh
ibmcloud sat experimental acl subnet rm \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental acl update`
{: #experimental-acl-update-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat acl update` instead] Update the name of an ACL.
{: shortdesc}

```sh
ibmcloud sat experimental acl update --acl-id ID --name NAME [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-acl-update-options}


`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    The new name for the ACL.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-acl-update-examples}

Update the name of an ACL.

```sh
ibmcloud sat experimental acl update \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --acl-id ACL_ID
```
{: pre}


### `ibmcloud sat experimental agent attach`
{: #experimental-agent-attach-cli}



[Deactivated on 2024-09-01! Use `ibmcloud sat agent attach` instead] Get a Satellite Connector Agent for a specific platform. Download the Agent `.zip` for Windows or get a link to the documentation for Docker environments.
{: shortdesc}

```sh
ibmcloud sat experimental agent attach --platform PLATFORM [-q]
```
{: pre}

#### Command options
{: #experimental-agent-attach-options}


`--platform PLATFORM`
:    The platform for the Satellite Connector Agent. For more information about Docker, see the documentation at https://ibm.biz/satconagent Available options: windows, docker

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-agent-attach-examples}

Get a Satellite Connector Agent for a specific platform.

```sh
ibmcloud sat experimental agent attach --platform PLATFORM -q
```
{: pre}


### `ibmcloud sat experimental agent ls`
{: #experimental-agent-ls-cli}



[Deactivated on 2024-09-01! Use `ibmcloud sat agent ls` instead] List all Agents for a Satellite Connector.
{: shortdesc}

```sh
ibmcloud sat experimental agent ls --connector-id ID [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #experimental-agent-ls-options}


`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-agent-ls-examples}

List all Agents for a Satellite Connector.

```sh
ibmcloud sat experimental agent ls --connector-id CONNECTOR_ID --output json -q
```
{: pre}


### `ibmcloud sat experimental connector create`
{: #experimental-connector-create-cli}



[Deactivated on 2024-11-18! Use `ibmcloud sat connector create` instead] Create a Satellite connector.
{: shortdesc}

```sh
ibmcloud sat experimental connector create --name NAME --region REGION [-q]
```
{: pre}

#### Command options
{: #experimental-connector-create-options}


`--name NAME`
:    The name for the Satellite connector.

`-q`
:    Do not show the message of the day or update reminders.

`--region REGION`
:    The IBM Cloud region to manage your Satellite connector.


#### Examples
{: #experimental-connector-create-examples}

Create a Satellite connector.

```sh
ibmcloud sat experimental connector create --name NAME --region REGION -q
```
{: pre}


### `ibmcloud sat experimental connector get`
{: #experimental-connector-get-cli}



[Deactivated on 2024-11-18! Use `ibmcloud sat connector get` instead] View the details of a Satellite Connector.
{: shortdesc}

```sh
ibmcloud sat experimental connector get --connector-id ID [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #experimental-connector-get-options}


`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-connector-get-examples}

View the details of a Satellite Connector.

```sh
ibmcloud sat experimental connector get --connector-id CONNECTOR_ID --output json -q
```
{: pre}


### `ibmcloud sat experimental connector ls`
{: #experimental-connector-ls-cli}



[Deactivated on 2024-11-18! Use `ibmcloud sat connector ls` instead] View the Satellite Connectors in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat experimental connector ls [--after AFTER] [--first FIRST] [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #experimental-connector-ls-options}


`--after AFTER`
:    Show Satellite Connectors after the given cursor.

`--first FIRST`
:    View the next Satellite Connectors, up to the first number of Connectors.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-connector-ls-examples}

View the Satellite Connectors in your IBM Cloud account.

```sh
ibmcloud sat experimental connector ls --after AFTER --first FIRST --output json
```
{: pre}


### `ibmcloud sat experimental connector rm`
{: #experimental-connector-rm-cli}



[Deactivated on 2024-11-18! Use `ibmcloud sat connector rm` instead] Delete a Satellite connector.
{: shortdesc}

```sh
ibmcloud sat experimental connector rm --connector-id ID [-f] [-q]
```
{: pre}

#### Command options
{: #experimental-connector-rm-options}


`--connector-id ID`
:    The ID of a Satellite connector.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-connector-rm-examples}

Delete a Satellite connector.

```sh
ibmcloud sat experimental connector rm --connector-id CONNECTOR_ID -f -q
```
{: pre}


### `ibmcloud sat experimental endpoint authn get`
{: #experimental-endpoint-authn-get-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat endpoint authn get` instead] Get the authentication settings for an endpoint.
{: shortdesc}

```sh
ibmcloud sat experimental endpoint authn get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-endpoint-authn-get-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-endpoint-authn-get-examples}

Get the authentication settings for an endpoint.

```sh
ibmcloud sat experimental endpoint authn get \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat experimental endpoint authn rotate`
{: #experimental-endpoint-authn-rotate-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat endpoint authn rotate` instead] Replace existing authentication certificates with new ones. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Only the certificates that you specify are replaced.
{: shortdesc}

```sh
ibmcloud sat experimental endpoint authn rotate --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-endpoint-authn-rotate-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the destination server's certificate. For example `myCA.pem`.

`--dest-cert-file FILE`
:    The client certificate used to authenticate with the destination server. For example `myCert.pem`.

`--dest-key-file FILE`
:    The client private key used to encrypt the client certificate. For example `myKey.pem`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--source-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the source client's certificate when source-tls-mode is mutual. For example `myCA.pem`.

`--source-cert-file FILE`
:    The server certificate to present to the source client. For example `myCert.pem`.

`--source-key-file FILE`
:    The server private key used to encrypt the server certificate. For example `myKey.pem`.


#### Examples
{: #experimental-endpoint-authn-rotate-examples}

Replace existing authentication certificates with new ones.

```sh
ibmcloud sat experimental endpoint authn rotate \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat experimental endpoint authn set`
{: #experimental-endpoint-authn-set-cli}



[Deactivated on 2024-10-01! Use `ibmcloud sat endpoint authn set` instead] Set authentication settings for an endpoint. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.
{: shortdesc}

```sh
ibmcloud sat experimental endpoint authn set --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [--dest-tls-mode MODE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] [--source-tls-mode MODE] (--connector-id ID | --location LOCATION)
```
{: pre}

#### Command options
{: #experimental-endpoint-authn-set-options}


`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the destination server's certificate. For example `myCA.pem`.

`--dest-cert-file FILE`
:    The client certificate used to authenticate with the destination server. For example `myCert.pem`.

`--dest-key-file FILE`
:    The client private key used to encrypt the client certificate. For example `myKey.pem`.

`--dest-tls-mode MODE`
:    The destination TLS mode. Accepted values: `simple`, `mutual`, `none`

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--source-ca-cert-file FILE`
:    Trusted CA certificate or chain used to validate the source client's certificate when source-tls-mode is mutual. For example `myCA.pem`.

`--source-cert-file FILE`
:    The server certificate to present to the source client. For example `myCert.pem`.

`--source-key-file FILE`
:    The server private key used to encrypt the server certificate. For example `myKey.pem`.

`--source-tls-mode MODE`
:    The source TLS mode. Accepted values: `simple`, `mutual`


#### Examples
{: #experimental-endpoint-authn-set-examples}

Set authentication settings for an endpoint.

```sh
ibmcloud sat experimental endpoint authn set \
  --location LOCATION \
  --connector-id CONNECTOR_ID \
  --endpoint ENDPOINT
```
{: pre}


### `ibmcloud sat experimental location update`
{: #experimental-location-update-cli}



[Deactivated on 2024-11-25! Use `ibmcloud sat location update` instead] Update the name or description of a Satellite location.
{: shortdesc}

```sh
ibmcloud sat experimental location update --location-id ID [--description DESCRIPTION] [--name NAME] [-q]
```
{: pre}

#### Command options
{: #experimental-location-update-options}


`--description DESCRIPTION`
:    Enter a new description for the Satellite location. The length of the description is limited to 400 bytes.

`--location-id ID`
:    The ID of the Satellite location. To find the location ID, run `ibmcloud sat location ls`.

`--name NAME`
:    Specify a new name for the Satellite location. Location names must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be fewer than 36 characters. Do not reuse names, including names of deleted locations.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #experimental-location-update-examples}

Update the name or description of a Satellite location.

```sh
ibmcloud sat experimental location update \
  --location-id LOCATION \
  --name NAME \
  --description IP_ADDRESS
```
{: pre}



## `ibmcloud sat group` commands
{: #group-cli}

View and manage Satellite cluster groups. Cluster groups are used to subscribe clusters to Satellite configurations of Kubernetes resources.

### `ibmcloud sat group attach`
{: #group-attach-cli}



Add a cluster to your cluster group. The cluster can run in your Satellite location or in IBM Cloud. To add a cluster that runs in IBM Cloud, you must first register the cluster with Satellite Config.
{: shortdesc}

```sh
ibmcloud sat group attach --cluster CLUSTER [--cluster CLUSTER ...] --group GROUP [-q]
```
{: pre}

#### Command options
{: #group-attach-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-attach-examples}

Add a cluster to your cluster group.

```sh
ibmcloud sat group attach --group GROUP --cluster CLUSTER_NAME_OR_ID -q
```
{: pre}


### `ibmcloud sat group create`
{: #group-create-cli}



Create a cluster group. Then, you can subscribe the cluster group to a Satellite configuration.
{: shortdesc}

```sh
ibmcloud sat group create --name NAME [--cluster CLUSTER ...] [-q]
```
{: pre}

#### Command options
{: #group-create-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or ID to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--name NAME`
:    Provide a name of the Satellite cluster group.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-create-examples}

Create a cluster group.

```sh
ibmcloud sat group create --name NAME --cluster CLUSTER_NAME_OR_ID -q
```
{: pre}


### `ibmcloud sat group detach`
{: #group-detach-cli}



Removes one or more clusters from your Satellite cluster group and deletes the Kubernetes resources that were managed by the group's subscriptions.
{: shortdesc}

```sh
ibmcloud sat group detach --cluster CLUSTER [--cluster CLUSTER ...] --group GROUP [-f] [-q]
```
{: pre}

#### Command options
{: #group-detach-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or ID. To list the clusters in your cluster group, run `ibmcloud sat group get --group <cluster_group_name_or_ID>`.

`-f`
:    Force the command to run without user prompts.

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-detach-examples}

Removes one or more clusters from your Satellite cluster group and deletes the Kubernetes resources that were managed by.

```sh
ibmcloud sat group detach --group GROUP --cluster CLUSTER_NAME_OR_ID -f
```
{: pre}


### `ibmcloud sat group get`
{: #group-get-cli}



Get detailed information for a Satellite cluster group.
{: shortdesc}

```sh
ibmcloud sat group get --group GROUP [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #group-get-options}


`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-get-examples}

Get detailed information for a Satellite cluster group.

```sh
ibmcloud sat group get --group GROUP --output json -q
```
{: pre}


### `ibmcloud sat group ls`
{: #group-ls-cli}



List all Satellite cluster groups in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat group ls [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #group-ls-options}


`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-ls-examples}

List all Satellite cluster groups in your IBM Cloud account.

```sh
ibmcloud sat group ls --output json -q
```
{: pre}


### `ibmcloud sat group rm`
{: #group-rm-cli}



Remove a Satellite cluster group, which unsubscribes clusters and deletes the Kubernetes resources that were managed by the group's subscriptions.
{: shortdesc}

```sh
ibmcloud sat group rm --group GROUP [-f] [-q]
```
{: pre}

#### Command options
{: #group-rm-options}


`-f`
:    Force the command to run without user prompts.

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-rm-examples}

Remove a Satellite cluster group, which unsubscribes clusters and deletes the Kubernetes resources that were managed by .

```sh
ibmcloud sat group rm --group GROUP -f -q
```
{: pre}



## `ibmcloud sat host` commands
{: #host-cli}

View and modify Satellite hosts.

### `ibmcloud sat host assign`
{: #host-assign-cli}



Assign a host to a Satellite location control plane or cluster.
{: shortdesc}

```sh
ibmcloud sat host assign --location LOCATION [--cluster CLUSTER] [--host HOST] [--host-label LABEL ...] [-q] [--worker-pool POOL] [--zone ZONE]
```
{: pre}

#### Command options
{: #host-assign-options}


`--cluster CLUSTER`
:    The name or ID of the cluster to assign the host to. To list available clusters, run `ibmcloud sat cluster ls`. If no cluster is provided, the host is automatically assigned to the Satellite control plane.

`--host HOST`
:    The name or ID of the host to assign. To automatically assign hosts based on labels, do not include this option. To retrieve the host ID, run `ibmcloud sat host ls --location <location_ID_or_name>`.

`--host-label LABEL`, `--hl LABEL`
:    Enter any labels as key-value pairs to identify the host to assign to your Satellite control plane or Red Hat OpenShift cluster. The first host that has this label and is unassigned is automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--worker-pool POOL`, `-p POOL`
:    The name or ID of the worker pool within the cluster to assign the host. If no worker pool is specified, the host is assigned to the default worker pool.

`--zone ZONE`
:    The name or ID of the zone to assign the host. To find available zones, run `ibmcloud sat location get --location <location_name_or_ID>` and look for the `Host Zones` field.


#### Examples
{: #host-assign-examples}

Assign a host to a Satellite location control plane or cluster.

```sh
ibmcloud sat host assign \
  --location LOCATION \
  --cluster CLUSTER_NAME_OR_ID \
  --worker-pool POOL_NAME
```
{: pre}


### `ibmcloud sat host attach`
{: #host-attach-cli}



Create and download a script that you can run on your hosts to attach them to your location. For RHCOS enabled locations, the script is an ignition file.
{: shortdesc}

```sh
ibmcloud sat host attach --location LOCATION [--host-label LABEL ...] [--host-link-agent-endpoint ENDPOINT] [--operating-system SYSTEM] [-q] [--reset-key]
```
{: pre}

#### Command options
{: #host-attach-options}


`--host-label LABEL`, `--hl LABEL`
:    Enter any labels as key-value pairs to identify the host to assign to your Satellite control plane or Red Hat OpenShift cluster. The first host that has this label and is unassigned is automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.

`--host-link-agent-endpoint ENDPOINT`
:    The endpoint that the link agent uses to connect to the link tunnel server.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--operating-system SYSTEM`
:    The operating system of the hosts you want to attach to your location. To attach RHCOS hosts, your location must be RHCOS enabled. Accepted values: `RHEL`, `RHCOS`

`-q`
:    Do not show the message of the day or update reminders.

`--reset-key`
:    Reset the key that the control plane uses to attach and assign hosts in the location. See https://ibm.biz/reset-key.


#### Examples
{: #host-attach-examples}

Create and download a script that you can run on your hosts to attach them to your location.

```sh
ibmcloud sat host attach \
  --location LOCATION \
  --host-label HOSTNAME \
  --reset-key RESET-KEY
```
{: pre}


### `ibmcloud sat host get`
{: #host-get-cli}



View the details of a Satellite host.
{: shortdesc}

```sh
ibmcloud sat host get --host HOST --location LOCATION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #host-get-options}


`--host HOST`
:    The Satellite host ID. To find the host ID, run `ibmcloud sat host ls <location_ID_or_name>`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #host-get-examples}

View the details of a Satellite host.

```sh
ibmcloud sat host get --location LOCATION --host HOSTNAME --output json
```
{: pre}


### `ibmcloud sat host ls`
{: #host-ls-cli}



List all hosts that are attached to a Satellite location, including hosts that are assigned to clusters or the control plane.
{: shortdesc}

```sh
ibmcloud sat host ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #host-ls-options}


`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #host-ls-examples}

List all hosts that are attached to a Satellite location, including hosts that are assigned to clusters or the control p.

```sh
ibmcloud sat host ls --location LOCATION --output json -q
```
{: pre}


### `ibmcloud sat host rm`
{: #host-rm-cli}



Remove a host from a Satellite location.
{: shortdesc}

```sh
ibmcloud sat host rm --host HOST --location LOCATION [-f] [-q]
```
{: pre}

#### Command options
{: #host-rm-options}


`-f`
:    Force the command to run without user prompts.

`--host HOST`
:    The name or ID of the host to remove.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #host-rm-examples}

Remove a host from a Satellite location.

```sh
ibmcloud sat host rm --location LOCATION --host HOSTNAME -f
```
{: pre}


### `ibmcloud sat host update`
{: #host-update-cli}



Update host information, such as zones and labels.
{: shortdesc}

```sh
ibmcloud sat host update --host HOST --location LOCATION [--host-label LABEL ...] [-q] [--zone ZONE]
```
{: pre}

#### Command options
{: #host-update-options}


`--host HOST`
:    The name or ID of the host to assign. To automatically assign hosts based on labels, do not include this option.

`--host-label LABEL`, `--hl LABEL`
:    Enter any labels as key-value pairs to identify the host to assign to your Satellite control plane or Red Hat OpenShift cluster. The first host that has this label and is unassigned is automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--zone ZONE`
:    The name or ID of the zone to associate the host. You cannot change the zone of hosts that are assigned to a resource, such as a cluster. You must unassign them first. To list available zones, run `ibmcloud sat location get --location <ID>`.


#### Examples
{: #host-update-examples}

Update host information, such as zones and labels.

```sh
ibmcloud sat host update --location LOCATION --host-label HOSTNAME --zone ZONE
```
{: pre}



## `ibmcloud sat key` commands
{: #key-cli}

View and manage Satellite Config keys.

### `ibmcloud sat key ls`
{: #key-ls-cli}



List all Satellite Config keys in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat key ls [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #key-ls-options}


`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #key-ls-examples}

List all Satellite Config keys in your IBM Cloud account.

```sh
ibmcloud sat key ls --output json -q
```
{: pre}


### `ibmcloud sat key rm`
{: #key-rm-cli}



Remove a Satellite Config key. Any cluster that still uses this key cannot connect to Satellite Config.
{: shortdesc}

```sh
ibmcloud sat key rm --key KEY [-f] [-q]
```
{: pre}

#### Command options
{: #key-rm-options}


`-f`
:    Force the command to run without user prompts.

`--key KEY`
:    The name or ID of a Satellite Config key.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #key-rm-examples}

Remove a Satellite Config key.

```sh
ibmcloud sat key rm --key KEY -f -q
```
{: pre}


### `ibmcloud sat key rotate`
{: #key-rotate-cli}



Generate a new key for use by managed clusters to connect to Satellite Config.
{: shortdesc}

```sh
ibmcloud sat key rotate --name NAME [-f] [-q]
```
{: pre}

#### Command options
{: #key-rotate-options}


`-f`
:    Force the command to run without user prompts.

`--name NAME`
:    The name of the new Satellite Config key.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #key-rotate-examples}

Generate a new key for use by managed clusters to connect to Satellite Config.

```sh
ibmcloud sat key rotate --name NAME -f -q
```
{: pre}



## `ibmcloud sat location` commands
{: #location-cli}

Create, view, and modify Satellite locations.

### `ibmcloud sat location create`
{: #location-create-cli}



Create a Satellite location. A Satellite location is a representation of an environment in your infrastructure provider. After you create a location, attach hosts from separate zones of your backing infrastructure environment with the `ibmcloud sat host attach` command.
{: shortdesc}

```sh
ibmcloud sat location create --managed-from REGION --name NAME [--capability CAPABILITY ...] [--coreos-enabled] [--cos-bucket BUCKET] [--description DESCRIPTION] [--ha-zone ZONE ...] [--physical-address ADDRESS] [--pod-network-interface-selection SELECTION] [--pod-subnet SUBNET] [--provider PROVIDER] [--provider-credential CREDENTIAL] [--provider-region REGION] [-q] [--service-subnet SUBNET]
```
{: pre}

#### Command options
{: #location-create-options}


`--capability CAPABILITY`
:    A capability of the Satellite location.

`--coreos-enabled`
:    Enable Red Hat CoreOS features for the Satellite location. This action cannot be undone. See [https://ibm.biz/infra-os](https://ibm.biz/infra-os).

`--cos-bucket BUCKET`
:    Specify the name of the IBM Cloud Object Storage bucket to store your Satellite location control plane data. Otherwise, a new bucket is created for you.

`--description DESCRIPTION`
:    Enter a description for the Satellite location.

`--ha-zone ZONE`
:    Specify the zone for your location. For high availability, specify 3 zones for your location as `--ha-zone ZONE1_NAME --ha-zone ZONE2_NAME --ha-zone ZONE3_NAME`. The names of the zones must match exactly the names of the corresponding zones in your infrastructure provider where you plan to create hosts.

`--managed-from REGION`
:    Select the IBM Cloud region to manage your Satellite location from. Choose a region close to your on-prem data center for better performance. See [https://ibm.biz/sat-region](https://ibm.biz/sat-region).

`--name NAME`
:    Specify a name for the Satellite location. Location names must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be fewer than 36 characters. Do not reuse names, even if the other location is deleted.

`--physical-address ADDRESS`
:    The physical address of the Satellite location.

`--pod-network-interface-selection SELECTION`
:    The method for selecting the node network interface for the internal pod network. This option can be used only if you also enable Red Hat CoreOS with the `--coreos-enabled` option. To provide a direct URL or IP address, specify `can-reach=<url>` or `can-reach=<ip_address>`. To choose a network interface, specify `interface=<network_interface>`.

`--pod-subnet SUBNET`
:    Specify a custom subnet CIDR to provide private IP addresses for pods. This option is used only if you enable Red Hat CoreOS with the `--coreos-enabled` option. The subnet must be `/23` or larger. See [https://ibm.biz/sat-location-create](https://ibm.biz/sat-location-create). Default value: '172.16.0.0/16

`--provider PROVIDER`
:    Indicate the infrastructure provider to use for the Satellite location. If you include this option, you must also include the `--provider-credential` option. Accepted values: `aws`, `azure`, `gcp`, `vmware`

`--provider-credential CREDENTIAL`
:    Specify the path to a JSON file on your local machine that has the credentials of the infrastructure provider for the Satellite location. The credential format is provider-specific. See [http://ibm.biz/sat-infra-creds](http://ibm.biz/sat-infra-creds).

`--provider-region REGION`
:    Specify the region in the infrastructure provider where you plan to create the hosts for the Satellite location. If you include this option, you must also include the `--provider` option.

`-q`
:    Do not show the message of the day or update reminders.

`--service-subnet SUBNET`
:    Specify a custom subnet CIDR to provide private IP addresses for services. This option is used only if you enable Red Hat CoreOS with the `--coreos-enabled` option. The subnet must be `/24` or larger. See [https://ibm.biz/sat-location-create](https://ibm.biz/sat-location-create). Default value: `172.20.0.0/16`


#### Examples
{: #location-create-examples}

Create a Satellite location.

```sh
ibmcloud sat location create \
  --pod-subnet SUBNET_CIDR \
  --service-subnet SUBNET_CIDR \
  --name NAME
```
{: pre}


### `ibmcloud sat location dns get`
{: #location-dns-get-cli}



View the details of a registered subdomain in a Satellite location.
{: shortdesc}

```sh
ibmcloud sat location dns get --location LOCATION --subdomain SUBDOMAIN [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #location-dns-get-options}


`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--subdomain SUBDOMAIN`
:    Specify the subdomain name. To list existing subdomains, run `ibmcloud sat location dns ls --location <ID>`.


#### Examples
{: #location-dns-get-examples}

View the details of a registered subdomain in a Satellite location.

```sh
ibmcloud sat location dns get --location LOCATION --subdomain DOMAIN --output json
```
{: pre}


### `ibmcloud sat location dns ls`
{: #location-dns-ls-cli}



List the registered subdomains in a Satellite location.
{: shortdesc}

```sh
ibmcloud sat location dns ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #location-dns-ls-options}


`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-dns-ls-examples}

List the registered subdomains in a Satellite location.

```sh
ibmcloud sat location dns ls --location LOCATION --output json -q
```
{: pre}


### `ibmcloud sat location dns register`
{: #location-dns-register-cli}



Set a subdomain for the hosts assigned to the control plane in a Satellite location.
{: shortdesc}

```sh
ibmcloud sat location dns register --ip IP [--ip IP ...] --location LOCATION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #location-dns-register-options}


`--ip IP`
:    Specify the IP address for each control plane host, in the format `--ip x.x.x.1 --ip x.x.x.2 --ip x.x.x.3`. For multizone clusters, use one IP address from each zone. To find the IP address, run `ibmcloud sat host ls --location <location_ID_or_name>` and look for `Worker IP` for hosts labeled `infrastructure`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-dns-register-examples}

Set a subdomain for the hosts assigned to the control plane in a Satellite location.

```sh
ibmcloud sat location dns register --location LOCATION --ip IP_ADDRESS --output json
```
{: pre}


### `ibmcloud sat location get`
{: #location-get-cli}



View the details of a Satellite location.
{: shortdesc}

```sh
ibmcloud sat location get --location LOCATION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #location-get-options}


`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-get-examples}

View the details of a Satellite location.

```sh
ibmcloud sat location get --location LOCATION --output json -q
```
{: pre}


### `ibmcloud sat location ls`
{: #location-ls-cli}



List all Satellite locations in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat location ls [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #location-ls-options}


`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-ls-examples}

List all Satellite locations in your IBM Cloud account.

```sh
ibmcloud sat location ls --output json -q
```
{: pre}


### `ibmcloud sat location rm`
{: #location-rm-cli}



Delete a location. Before you run this command, back up your configurations and remove any hosts and clusters that run in the location. The underlying host infrastructure is not automatically deleted when you delete a location. This action cannot be undone.
{: shortdesc}

```sh
ibmcloud sat location rm --location LOCATION [-f] [-q]
```
{: pre}

#### Command options
{: #location-rm-options}


`-f`
:    Force the command to run without user prompts.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-rm-examples}

Delete a location.

```sh
ibmcloud sat location rm --location LOCATION -f -q
```
{: pre}


### `ibmcloud sat location update`
{: #location-update-cli}



Update the name or description of a Satellite location.
{: shortdesc}

```sh
ibmcloud sat location update --location-id ID [--description DESCRIPTION] [--name NAME] [-q]
```
{: pre}

#### Command options
{: #location-update-options}


`--description DESCRIPTION`
:    Enter a new description for the Satellite location. The length of the description is limited to 400 bytes.

`--location-id ID`
:    The ID of the Satellite location. To find the location ID, run `ibmcloud sat location ls`.

`--name NAME`
:    Specify a new name for the Satellite location. Location names must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be fewer than 36 characters. Do not reuse names, including names of deleted locations.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-update-examples}

Update the name or description of a Satellite location.

```sh
ibmcloud sat location update \
  --location-id LOCATION \
  --name NAME \
  --description IP_ADDRESS
```
{: pre}



## `ibmcloud sat messages` commands
{: #messages-cli}

View the current user messages.

### `ibmcloud sat messages`
{: #messages-cli}



View the current user messages.
{: shortdesc}

```sh
ibmcloud sat messages [-q]
```
{: pre}

#### Command options
{: #messages-options}


`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #messages-examples}

View the current user messages.

```sh
ibmcloud sat messages -q
```
{: pre}



## `ibmcloud sat resource` commands
{: #resource-cli}

Search and view Kubernetes resources that are managed by a Satellite configuration.

### `ibmcloud sat resource get`
{: #resource-get-cli}



View the details of a Kubernetes resource that is managed by a Satellite configuration.
{: shortdesc}

```sh
ibmcloud sat resource get --resource RESOURCE [--history HISTORY] [--output OUTPUT] [-q] [--save-data]
```
{: pre}

#### Command options
{: #resource-get-options}


`--history HISTORY`
:    The history ID for the resource.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--resource RESOURCE`
:    Specify the Kubernetes resource ID. To find Kubernetes resources, run `ibmcloud sat resource ls`.

`--save-data`
:    Download and save a Kubernetes resource definition to a temporary file.


#### Examples
{: #resource-get-examples}

View the details of a Kubernetes resource that is managed by a Satellite configuration.

```sh
ibmcloud sat resource get \
  --resource RESOURCE \
  --history HISTORY \
  --save-data SAVE-DATA
```
{: pre}


### `ibmcloud sat resource history get`
{: #resource-history-get-cli}



Get history for a Kubernetes resource.
{: shortdesc}

```sh
ibmcloud sat resource history get --resource RESOURCE [--limit LIMIT] [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #resource-history-get-options}


`--limit LIMIT`
:    Specify the maximum number of history entries to return.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--resource RESOURCE`
:    The Kubernetes resource ID.


#### Examples
{: #resource-history-get-examples}

Get history for a Kubernetes resource.

```sh
ibmcloud sat resource history get --resource RESOURCE --limit LIMIT --output json
```
{: pre}


### `ibmcloud sat resource ls`
{: #resource-ls-cli}



Search Kubernetes resources that are managed by Satellite.
{: shortdesc}

```sh
ibmcloud sat resource ls [--limit LIMIT] [--output OUTPUT] [-q] [--search SEARCH] (--cluster CLUSTER | --subscription SUBSCRIPTION)
```
{: pre}

#### Command options
{: #resource-ls-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the name or ID of the registered cluster that the Kubernetes resource runs in. To find registered clusters, run `ibmcloud sat cluster ls`.

`--limit LIMIT`
:    Specify the maximum number of resource entries for the search to return.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--search SEARCH`
:    Indicate the string to filter search results of Kubernetes resources, such as a pod or namespace name.

`--subscription SUBSCRIPTION`
:    Specify the Satellite subscription ID or name.  To find subscriptions, run `ibmcloud sat cluster ls`.


#### Examples
{: #resource-ls-examples}

Search Kubernetes resources that are managed by Satellite.

```sh
ibmcloud sat resource ls --search SEARCH --limit LIMIT --cluster CLUSTER_NAME_OR_ID
```
{: pre}



## `ibmcloud sat service` commands
{: #service-cli}

View Satellite service clusters.

### `ibmcloud sat service ls`
{: #service-ls-cli}



List all Satellite service clusters in your location to review details, such as requested host resources.
{: shortdesc}

```sh
ibmcloud sat service ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #service-ls-options}


`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #service-ls-examples}

List all Satellite service clusters in your location to review details, such as requested host resources.

```sh
ibmcloud sat service ls --location LOCATION --output json -q
```
{: pre}



## `ibmcloud sat storage` commands
{: #storage-cli}

View and manage Satellite storage resources.

### `ibmcloud sat storage assignment autopatch disable`
{: #storage-assignment-autopatch-disable-cli}

The `storage assignment autopatch disable` command is a beta feature.
{: beta}



Disable automatic patches for a Satellite storage assignment.
{: shortdesc}

```sh
ibmcloud sat storage assignment autopatch disable --config CONFIG [-q] (--all | --assignment ASSIGNMENT)
```
{: pre}

#### Command options
{: #storage-assignment-autopatch-disable-options}


`--all`
:    Disable automatic patches for all Satellite storage assignments of a storage configuration.

`--assignment ASSIGNMENT`
:    The ID of a Satellite storage assignment. To list available storage assignments of the configuration, run `ibmcloud sat storage assignment ls  --config CONFIG`.

`--config CONFIG`
:    The name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-autopatch-disable-examples}

Disable automatic patches for a Satellite storage assignment.

```sh
ibmcloud sat storage assignment autopatch disable \
  --config CONFIG \
  --assignment ASSIGNMENT \
  --all
```
{: pre}


### `ibmcloud sat storage assignment autopatch enable`
{: #storage-assignment-autopatch-enable-cli}

The `storage assignment autopatch enable` command is a beta feature.
{: beta}



Enable automatic patches for a Satellite storage assignment.
{: shortdesc}

```sh
ibmcloud sat storage assignment autopatch enable --config CONFIG [-q] (--all | --assignment ASSIGNMENT)
```
{: pre}

#### Command options
{: #storage-assignment-autopatch-enable-options}


`--all`
:    Enable automatic patches for all Satellite storage assignments of a storage configuration.

`--assignment ASSIGNMENT`
:    The ID of a Satellite storage assignment. To list available storage assignments of the configuration, run `ibmcloud sat storage assignment ls  --config CONFIG`.

`--config CONFIG`
:    The name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-autopatch-enable-examples}

Enable automatic patches for a Satellite storage assignment.

```sh
ibmcloud sat storage assignment autopatch enable \
  --config CONFIG \
  --assignment ASSIGNMENT \
  --all
```
{: pre}


### `ibmcloud sat storage assignment create`
{: #storage-assignment-create-cli}



Create an assignment to deploy your storage configurations to clusters in your Satellite location.
{: shortdesc}

```sh
ibmcloud sat storage assignment create --config CONFIG [--name NAME] [-q] (--cluster CLUSTER | --group GROUP | --service-cluster-id CLUSTER)
```
{: pre}

#### Command options
{: #storage-assignment-create-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the ID of the Satellite cluster for the assignment. To find the cluster ID, run `ibmcloud oc cluster ls --provider satellite`.

`--config CONFIG`
:    Specify the Satellite storage configuration for the assignment. to find configurations, run `ibmcloud sat storage config ls`.

`--group GROUP`, `-g GROUP`
:    Specify the cluster groups for the assignment. To find cluster groups, run `ibmcloud sat group ls`.

`--name NAME`
:    Provide a name for Satellite storage assignment.

`-q`
:    Do not show the message of the day or update reminders.

`--service-cluster-id CLUSTER`
:    Specify the ID of the service cluster for the assignment. To find the service cluster ID, run `ibmcloud sat service ls --location <location>`.


#### Examples
{: #storage-assignment-create-examples}

Create an assignment to deploy your storage configurations to clusters in your Satellite location.

```sh
ibmcloud sat storage assignment create \
  --name NAME \
  --group GROUP \
  --service-cluster-id CLUSTER_NAME_OR_ID
```
{: pre}


### `ibmcloud sat storage assignment get`
{: #storage-assignment-get-cli}



Get the details of a Satellite storage assignment.
{: shortdesc}

```sh
ibmcloud sat storage assignment get --assignment ASSIGNMENT [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #storage-assignment-get-options}


`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-get-examples}

Get the details of a Satellite storage assignment.

```sh
ibmcloud sat storage assignment get --assignment ASSIGNMENT --output json -q
```
{: pre}


### `ibmcloud sat storage assignment ls`
{: #storage-assignment-ls-cli}



List the Satellite storage assignments in your IBM Cloud account.

To list all assignments for a service cluster as Service Admin: ibmcloud sat storage assignment ls --service-cluster-id CLUSTER.

To list all assignments for a service cluster as Location Admin: ibmcloud sat storage assignment ls --location LOCATION --service-cluster-id CLUSTER.

To list all assignments for a configuration: ibmcloud sat storage assignment ls --config CONFIG.
{: shortdesc}

```sh
ibmcloud sat storage assignment ls [--output OUTPUT] [-q] (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
```
{: pre}

#### Command options
{: #storage-assignment-ls-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the ID of the Satellite cluster for the assignments. To get the cluster ID, run `ibmcloud oc cluster ls --provider satellite`.

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`--location LOCATION`
:    Specify the name of a Satellite location. To list available locations, run `ibmcloud sat location ls`. This option cannot be used by service administrator.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--service-cluster-id CLUSTER`
:    Specify the ID of the service cluster for the assignments. To find the service cluster ID, run `ibmcloud sat service ls --location <location>`.


#### Examples
{: #storage-assignment-ls-examples}

List the Satellite storage assignments in your IBM Cloud account.

```sh
ibmcloud sat storage assignment ls \
  --service-cluster-id CLUSTER_NAME_OR_ID \
  --cluster CLUSTER_NAME_OR_ID \
  --config CONFIG
```
{: pre}


### `ibmcloud sat storage assignment patch`
{: #storage-assignment-patch-cli}



Apply storage configuration changes to the associated assignments.
{: shortdesc}

```sh
ibmcloud sat storage assignment patch --assignment ASSIGNMENT [-f] [-q]
```
{: pre}

Aliases: `ibmcloud sat storage assignment upgrade`, `ibmcloud sat upgrade`

#### Command options
{: #storage-assignment-patch-options}


`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment. To list available assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-patch-examples}

Apply storage configuration changes to the associated assignments.

```sh
ibmcloud sat storage assignment patch --assignment ASSIGNMENT -f -q
```
{: pre}


### `ibmcloud sat storage assignment rm`
{: #storage-assignment-rm-cli}



Remove a Satellite storage assignment. The Kubernetes resources are deleted from all the clusters in your Satellite location, but the configuration remains.
{: shortdesc}

```sh
ibmcloud sat storage assignment rm --assignment ASSIGNMENT [-f] [-q]
```
{: pre}

#### Command options
{: #storage-assignment-rm-options}


`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment. To find assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-rm-examples}

Remove a Satellite storage assignment.

```sh
ibmcloud sat storage assignment rm --assignment ASSIGNMENT -f -q
```
{: pre}


### `ibmcloud sat storage assignment update`
{: #storage-assignment-update-cli}



Update a Satellite storage assignment.
{: shortdesc}

```sh
ibmcloud sat storage assignment update --assignment ASSIGNMENT [-f] [--group GROUP ...] [--name NAME] [-q]
```
{: pre}

#### Command options
{: #storage-assignment-update-options}


`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment.

`-f`
:    Force the command to run without user prompts.

`--group GROUP`, `-g GROUP`
:    Specify the new cluster groups for the assignment. To list available groups, run `ibmcloud sat group ls`.

`--name NAME`
:    Provide a new name for the Satellite storage assignment.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-update-examples}

Update a Satellite storage assignment.

```sh
ibmcloud sat storage assignment update \
  --assignment ASSIGNMENT \
  --name NAME \
  --group GROUP
```
{: pre}


### `ibmcloud sat storage config class add`
{: #storage-config-class-add-cli}



Create a custom Satellite storage class.
{: shortdesc}

```sh
ibmcloud sat storage config class add --config-name NAME --name NAME --param PARAM [--param PARAM ...] [-q]
```
{: pre}

Aliases: `ibmcloud sat storage config sc add`

#### Command options
{: #storage-config-class-add-options}


`--config-name NAME`
:    Specify the name of the storage configuration for the custom storage class. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--name NAME`
:    Provide a name for the custom storage class.

`--param PARAM`, `-p PARAM`
:    Specify a `key=value` pair for storage class parameters. To see the storage class parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-class-add-examples}

Create a custom Satellite storage class.

```sh
ibmcloud sat storage config class add --name NAME --config-name NAME --param PARAM
```
{: pre}


### `ibmcloud sat storage config class get`
{: #storage-config-class-get-cli}



Get the details of a Satellite storage class.
{: shortdesc}

```sh
ibmcloud sat storage config class get --class CLASS --config CONFIG [--output OUTPUT] [-q]
```
{: pre}

Aliases: `ibmcloud sat storage config sc get`

#### Command options
{: #storage-config-class-get-options}


`--class CLASS`
:    Specify the name of a Satellite storage class.

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-class-get-examples}

Get the details of a Satellite storage class.

```sh
ibmcloud sat storage config class get --class CLASS --config CONFIG --output json
```
{: pre}


### `ibmcloud sat storage config class ls`
{: #storage-config-class-ls-cli}



List the storage classes in a Satellite storage configuration
{: shortdesc}

```sh
ibmcloud sat storage config class ls --config CONFIG [--output OUTPUT] [-q] [--show-params]
```
{: pre}

Aliases: `ibmcloud sat storage config sc ls`

#### Command options
{: #storage-config-class-ls-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--show-params`
:    Include this option to list all storage class parameter details.


#### Examples
{: #storage-config-class-ls-examples}

List the storage classes in a Satellite storage configuration.

```sh
ibmcloud sat storage config class ls \
  --config CONFIG \
  --show-params SHOW-PARAMS \
  --output json
```
{: pre}


### `ibmcloud sat storage config create`
{: #storage-config-create-cli}



Create a Satellite storage configuration to install storage drivers in your clusters.
{: shortdesc}

```sh
ibmcloud sat storage config create --location LOCATION --name NAME --template-name NAME [--param PARAM ...] [-q] [--template-version VERSION]
```
{: pre}

#### Command options
{: #storage-config-create-options}


`--location LOCATION`
:    Enter the ID or name of the location for the storage configuration. To find available locations, run `ibmcloud sat location ls`.

`--name NAME`
:    Specify the name of the storage configuration.

`--param PARAM`, `-p PARAM`
:    Specify a `key=value` pair for configuration parameters. To see the configuration parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.

`--template-name NAME`
:    Specify the Satellite storage configuration template name. To list available storage configuration templates, run `ibmcloud sat storage template ls`.

`--template-version VERSION`
:    Specify the Satellite storage configuration template version. If you do not include this option, the default version is used. To list available storage configuration templates, run `ibmcloud sat storage template ls`.


#### Examples
{: #storage-config-create-examples}

Create a Satellite storage configuration to install storage drivers in your clusters.

```sh
ibmcloud sat storage config create \
  --name NAME \
  --template-name NAME \
  --template-version VERSION
```
{: pre}


### `ibmcloud sat storage config get`
{: #storage-config-get-cli}



Get the details of a Satellite storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config get --config CONFIG [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #storage-config-get-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-get-examples}

Get the details of a Satellite storage configuration.

```sh
ibmcloud sat storage config get --config CONFIG --output json -q
```
{: pre}


### `ibmcloud sat storage config ls`
{: #storage-config-ls-cli}



List the Satellite storage configurations in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat storage config ls [--location LOCATION] [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #storage-config-ls-options}


`--location LOCATION`
:    Specify the ID or name of the location that contains the configurations you want to list. To find available locations, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-ls-examples}

List the Satellite storage configurations in your IBM Cloud account.

```sh
ibmcloud sat storage config ls --location LOCATION --output json -q
```
{: pre}


### `ibmcloud sat storage config param set`
{: #storage-config-param-set-cli}



Set the configuration and secret parameters of a Satellite storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config param set --config CONFIG --param PARAM [--param PARAM ...] [--apply] [-f] [-q]
```
{: pre}

#### Command options
{: #storage-config-param-set-options}


`--apply`
:    Apply the latest Satellite storage configuration version to all assignments of a configuration. To list a configuration's assignments, run `ibmcloud sat storage assignment ls --config CONFIG`.

`--config CONFIG`
:    Specify the name or ID of the storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--param PARAM`, `-p PARAM`
:    Specify a `key=value` pair for configuration parameters. To see the configuration parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-param-set-examples}

Set the configuration and secret parameters of a Satellite storage configuration.

```sh
ibmcloud sat storage config param set --config CONFIG --param PARAM --apply APPLY
```
{: pre}


### `ibmcloud sat storage config patch`
{: #storage-config-patch-cli}



Apply the latest patch updates to a Satellite storage configuration. Patch updates contain vulnerability remediations and bug fixes within the same major version.
{: shortdesc}

```sh
ibmcloud sat storage config patch --config CONFIG [-f] [--include-assignments] [-q]
```
{: pre}

Aliases: `ibmcloud sat storage config upgrade`, `ibmcloud sat upgrade`

#### Command options
{: #storage-config-patch-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--include-assignments`
:    Include this option to patch the assignments of the storage configuration to the latest configuration version.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-patch-examples}

Apply the latest patch updates to a Satellite storage configuration.

```sh
ibmcloud sat storage config patch \
  --config CONFIG \
  --include-assignments INCLUDE-ASSIGNMENTS \
  -f
```
{: pre}


### `ibmcloud sat storage config rm`
{: #storage-config-rm-cli}



Remove a Satellite storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config rm --config CONFIG [-f] [--include-assignments] [-q]
```
{: pre}

#### Command options
{: #storage-config-rm-options}


`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--include-assignments`
:    Include this option to remove the storage configuration as well as any associated assignments.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-rm-examples}

Remove a Satellite storage configuration.

```sh
ibmcloud sat storage config rm \
  --config CONFIG \
  --include-assignments INCLUDE-ASSIGNMENTS \
  -f
```
{: pre}


### `ibmcloud sat storage template get`
{: #storage-template-get-cli}



Get the details of a Satellite storage template
{: shortdesc}

```sh
ibmcloud sat storage template get --name NAME --version VERSION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #storage-template-get-options}


`--name NAME`
:    Specify the storage template name. To list available storage templates, run `ibmcloud sat storage template ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--version VERSION`
:    Specify the storage template version. To list available storage templates, run `ibmcloud sat storage template ls`.


#### Examples
{: #storage-template-get-examples}

Get the details of a Satellite storage template.

```sh
ibmcloud sat storage template get --name NAME --version VERSION --output json
```
{: pre}


### `ibmcloud sat storage template ls`
{: #storage-template-ls-cli}



List the available Satellite storage templates.
{: shortdesc}

```sh
ibmcloud sat storage template ls [-q]
```
{: pre}

#### Command options
{: #storage-template-ls-options}


`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-template-ls-examples}

List the available Satellite storage templates.

```sh
ibmcloud sat storage template ls -q
```
{: pre}



## `ibmcloud sat subscription` commands
{: #subscription-cli}

View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters.

### `ibmcloud sat subscription create`
{: #subscription-create-cli}



Create a Satellite subscription for clusters. After you create the subscription, the associated Satellite configuration version is automatically deployed to the subscribed clusters.
{: shortdesc}

```sh
ibmcloud sat subscription create --config CONFIG --group GROUP [--group GROUP ...] --name NAME [-q] (--auth-required --gitref GITREF --gitref-type TYPE --path PATH --repository REPOSITORY | --version VERSION)
```
{: pre}

#### Command options
{: #subscription-create-options}


`--auth-required`
:    Provide the authentication secret required to connect to the remote repository. See [https://ibm.biz/sat-config-private-repo](https://ibm.biz/sat-config-private-repo) for details. Strategy: GitOps.

`--config CONFIG`
:    Specify the name of the configuration to use for the subscription. To find available configurations, run `ibmcloud sat config ls`.

`--gitref GITREF`
:    Specify the GitRef to use for the Satellite subscription. Strategy: GitOps.

`--gitref-type TYPE`
:    Indicate the type of GitRef to use for the Satellite subscription. Strategy: GitOps. Allowed values: branch, commit, tag, release

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of the cluster groups to subscribe to your configuration. To find available cluster groups, run `ibmcloud sat group ls`.

`--name NAME`
:    Enter a name for the subscription.

`--path PATH`
:    Provide the path to the repository files or release assets in the remote repository to use for the Satellite subscription. Strategy: GitOps.

`-q`
:    Do not show the message of the day or update reminders.

`--repository REPOSITORY`
:    Specify the URL of the remote repository to use for the subscription. Strategy: GitOps.

`--version VERSION`
:    Indicate the name or ID of the existing configuration version to use for the subscription. To find versions, run `ibmcloud sat config get --config <configuration_name_or_ID>`. Strategy: Direct Upload.


#### Examples
{: #subscription-create-examples}

Create a Satellite subscription for clusters.

```sh
ibmcloud sat subscription create \
  --auth-required AUTH-REQUIRED \
  --name NAME \
  --group GROUP \
  --config CONFIG
```
{: pre}


### `ibmcloud sat subscription get`
{: #subscription-get-cli}



Get detailed information for a Satellite subscription.
{: shortdesc}

```sh
ibmcloud sat subscription get --subscription SUBSCRIPTION [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #subscription-get-options}


`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--subscription SUBSCRIPTION`
:    Enter the name or ID of a Satellite subscription. To find subscriptions, run `ibmcloud sat subscription ls`.


#### Examples
{: #subscription-get-examples}

Get detailed information for a Satellite subscription.

```sh
ibmcloud sat subscription get --subscription IP_ADDRESS --output json -q
```
{: pre}


### `ibmcloud sat subscription identity set`
{: #subscription-identity-set-cli}



Update the Satellite subscription to use your identity to manage resources.
{: shortdesc}

```sh
ibmcloud sat subscription identity set --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}

#### Command options
{: #subscription-identity-set-options}


`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--subscription SUBSCRIPTION`
:    Specify the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.


#### Examples
{: #subscription-identity-set-examples}

Update the Satellite subscription to use your identity to manage resources.

```sh
ibmcloud sat subscription identity set --subscription IP_ADDRESS -f -q
```
{: pre}


### `ibmcloud sat subscription ls`
{: #subscription-ls-cli}



List all Satellite subscriptions in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat subscription ls [--cluster CLUSTER] [--output OUTPUT] [-q]
```
{: pre}

#### Command options
{: #subscription-ls-options}


`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the Satellite cluster name or ID. To find registered clusters, run `ibmcloud sat cluster ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #subscription-ls-examples}

List all Satellite subscriptions in your IBM Cloud account.

```sh
ibmcloud sat subscription ls --cluster CLUSTER_NAME_OR_ID --output json -q
```
{: pre}


### `ibmcloud sat subscription rm`
{: #subscription-rm-cli}



Remove a Satellite subscription. The Kubernetes resources are no longer deployed to your clusters.
{: shortdesc}

```sh
ibmcloud sat subscription rm --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}

#### Command options
{: #subscription-rm-options}


`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--subscription SUBSCRIPTION`
:    Provide the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.


#### Examples
{: #subscription-rm-examples}

Remove a Satellite subscription.

```sh
ibmcloud sat subscription rm --subscription IP_ADDRESS -f -q
```
{: pre}


### `ibmcloud sat subscription update`
{: #subscription-update-cli}



Update a Satellite subscription.
{: shortdesc}

```sh
ibmcloud sat subscription update --subscription SUBSCRIPTION [-f] [--group GROUP] [--name NAME] [-q] (--auth-required --gitref GITREF --gitref-type TYPE --path PATH --repository REPOSITORY | --version VERSION)
```
{: pre}

#### Command options
{: #subscription-update-options}


`--auth-required`
:    Provide the authentication secret required to connect to the remote repository. Strategy: GitOps.

`-f`
:    Force the command to run without user prompts.

`--gitref GITREF`
:    Specify the GitRef to use for the Satellite subscription. Strategy: GitOps.

`--gitref-type TYPE`
:    Indicate the type of GitRef to use for this Satellite subscription. Strategy: GitOps. Allowed values: branch, commit, tag, release

`--group GROUP`, `-g GROUP`
:    Specify the new cluster groups to subscribe to your configuration.

`--name NAME`
:    Provide a new name of the Satellite subscription.

`--path PATH`
:    Indicate the path to the repository files or release assets in the remote repository to use for the Satellite subscription. Strategy: GitOps.

`-q`
:    Do not show the message of the day or update reminders.

`--repository REPOSITORY`
:    Provide the URL of the remote repository to use for the Satellite subscription. Strategy: GitOps.

`--subscription SUBSCRIPTION`
:    Specify the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.

`--version VERSION`
:    Indicate the existing configuration version to use for the Satellite subscription. Strategy: Direct Upload.


#### Examples
{: #subscription-update-examples}

Update a Satellite subscription.

```sh
ibmcloud sat subscription update \
  --auth-required AUTH-REQUIRED \
  --subscription IP_ADDRESS \
  --name NAME \
  --group GROUP
```
{: pre}

  
