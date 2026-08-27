---


copyright:
  years: 2019, 2026
lastupdated: "2026-08-27"

keywords: satellite cli reference, satellite commands, satellite cli, satellite reference

subcollection: satellite

content-type: cli-docs

---



{{site.data.keyword.attribute-definition-list}}

# IBM Cloud CLI reference for {{site.data.keyword.satelliteshort}} commands
{: #satellite-cli-reference}

Automate the creation and management of your IBM Cloud Satellite location with CLI commands for location, host, cluster, and endpoint operations.
{: shortdesc}

To install the CLI, see [Installing the the CLI](/docs/satellite?topic=satellite-cli-install). To view a high-level map of all the {{site.data.keyword.satellitelong_notm}} commands, see the [CLI map](/docs/satellite?topic=satellite-icsat_map).
{: tip}



1. Install the {{site.data.keyword.cloud_notm}} CLI. See [Getting started with the IBM Cloud CLI](/docs/satellite?topic=satellite-cli-install).

2. Install the `ks` plug-in.

    ```console
    ibmcloud plugin install ks
    ```
    {: pre}



## Acl commands
{: #acl-cli}

View and manage Satellite access control lists (ACLs).


### `ibmcloud sat acl create`
{: #acl-create-cli}



Create an ACL.

```sh
ibmcloud sat acl create --name NAME --subnet SUBNET [--subnet SUBNET ...] [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-create-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    A name or ID of an endpoint to enable for this ACL.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name`
:    The name for the ACL.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #acl-create-examples}

Create an ACL

```sh
ibmcloud sat acl create --name NAME --subnet SUBNET --connector-id ID
```
{: pre}


### `ibmcloud sat acl endpoint add`
{: #acl-endpoint-add-cli}



Add one or more enabled endpoints to an ACL.

```sh
ibmcloud sat acl endpoint add --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-endpoint-add-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    A name or ID of an endpoint to enable for this ACL.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-endpoint-add-examples}

Add one or more enabled endpoints to an ACL

```sh
ibmcloud sat acl endpoint add --acl-id ID --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat acl endpoint help`
{: #acl-endpoint-help-cli}



Show help

```sh
ibmcloud sat acl endpoint help
```


#### Examples
{: #acl-endpoint-help-examples}

Show help

```sh
ibmcloud sat acl endpoint help
```
{: pre}


### `ibmcloud sat acl endpoint ls`
{: #acl-endpoint-ls-cli}



List all enabled endpoints for an ACL.

```sh
ibmcloud sat acl endpoint ls --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-endpoint-ls-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-endpoint-ls-examples}

List all enabled endpoints for an ACL

```sh
ibmcloud sat acl endpoint ls --acl-id ID --connector-id ID
```
{: pre}


### `ibmcloud sat acl endpoint rm`
{: #acl-endpoint-rm-cli}



Remove one or more enabled endpoints from an ACL.

```sh
ibmcloud sat acl endpoint rm --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-endpoint-rm-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    A name or ID of an endpoint to disable for this ACL.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-endpoint-rm-examples}

Remove one or more enabled endpoints from an ACL

```sh
ibmcloud sat acl endpoint rm --acl-id ID --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat acl get`
{: #acl-get-cli}



View the details of an ACL.

```sh
ibmcloud sat acl get --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-get-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-get-examples}

View the details of an ACL

```sh
ibmcloud sat acl get --acl-id ID --connector-id ID
```
{: pre}


### `ibmcloud sat acl help`
{: #acl-help-cli}



Show help

```sh
ibmcloud sat acl help
```


#### Examples
{: #acl-help-examples}

Show help

```sh
ibmcloud sat acl help
```
{: pre}


### `ibmcloud sat acl ls`
{: #acl-ls-cli}



List all ACLs for a Satellite connector or location.

```sh
ibmcloud sat acl ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-ls-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-ls-examples}

List all ACLs for a Satellite connector or location

```sh
ibmcloud sat acl ls --connector-id ID
```
{: pre}


### `ibmcloud sat acl rm`
{: #acl-rm-cli}



Delete an ACL.

```sh
ibmcloud sat acl rm --acl-id ID [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-rm-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-rm-examples}

Delete an ACL

```sh
ibmcloud sat acl rm --acl-id ID --connector-id ID
```
{: pre}


### `ibmcloud sat acl subnet add`
{: #acl-subnet-add-cli}



Add one or more subnets to an ACL.

```sh
ibmcloud sat acl subnet add --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-subnet-add-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #acl-subnet-add-examples}

Add one or more subnets to an ACL

```sh
ibmcloud sat acl subnet add --acl-id ID --subnet SUBNET --connector-id ID
```
{: pre}


### `ibmcloud sat acl subnet help`
{: #acl-subnet-help-cli}



Show help

```sh
ibmcloud sat acl subnet help
```


#### Examples
{: #acl-subnet-help-examples}

Show help

```sh
ibmcloud sat acl subnet help
```
{: pre}


### `ibmcloud sat acl subnet rm`
{: #acl-subnet-rm-cli}



Remove one or more subnets from an ACL.

```sh
ibmcloud sat acl subnet rm --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-subnet-rm-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--subnet`
:    An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.


#### Examples
{: #acl-subnet-rm-examples}

Remove one or more subnets from an ACL

```sh
ibmcloud sat acl subnet rm --acl-id ID --subnet SUBNET --connector-id ID
```
{: pre}


### `ibmcloud sat acl update`
{: #acl-update-cli}



Update the name of an ACL.

```sh
ibmcloud sat acl update --acl-id ID --name NAME [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #acl-update-options}


`--acl-id`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name`
:    The new name for the ACL.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #acl-update-examples}

Update the name of an ACL

```sh
ibmcloud sat acl update --acl-id ID --name NAME --connector-id ID
```
{: pre}


## Agent commands
{: #agent-cli}

Attach or view Satellite Connector Agents.


### `ibmcloud sat agent attach`
{: #agent-attach-cli}



Get a Satellite Connector Agent for a specific platform. Download the Agent `.zip` for Windows or get a link to the documentation for Docker environments.

```sh
ibmcloud sat agent attach --platform PLATFORM [-q]
```

#### Command options
{: #agent-attach-options}


`--platform`
:    The platform for the Satellite Connector Agent. For more information about Docker, see the documentation at https://ibm.biz/satconagent Available options: windows, docker

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #agent-attach-examples}

Get a Satellite Connector Agent for a specific platform

```sh
ibmcloud sat agent attach --platform PLATFORM
```
{: pre}


### `ibmcloud sat agent help`
{: #agent-help-cli}



Show help

```sh
ibmcloud sat agent help
```


#### Examples
{: #agent-help-examples}

Show help

```sh
ibmcloud sat agent help
```
{: pre}


### `ibmcloud sat agent ls`
{: #agent-ls-cli}



List all Agents for a Satellite Connector.

```sh
ibmcloud sat agent ls --connector-id ID [--output OUTPUT] [-q]
```

#### Command options
{: #agent-ls-options}


`--connector-id`
:    The ID of a Satellite connector.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #agent-ls-examples}

List all Agents for a Satellite Connector

```sh
ibmcloud sat agent ls --connector-id ID
```
{: pre}


## Cluster commands
{: #cluster-cli}

Register and manage clusters for use with Satellite configurations.


### `ibmcloud sat cluster get`
{: #cluster-get-cli}

[Virtual Private Cloud]{: tag-vpc} [Classic infrastructure]{: tag-classic-inf} [Satellite]{: tag-satellite} 

Get the details of a registered cluster.

```sh
ibmcloud sat cluster get --cluster CLUSTER [--output OUTPUT] [-q]
```

#### Command options
{: #cluster-get-options}


`-c`, `--cluster`
:    Specify the cluster name or the ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #cluster-get-examples}

Get the details of a registered cluster

```sh
ibmcloud sat cluster get --cluster CLUSTER
```
{: pre}


### `ibmcloud sat cluster help`
{: #cluster-help-cli}



Show help

```sh
ibmcloud sat cluster help
```


#### Examples
{: #cluster-help-examples}

Show help

```sh
ibmcloud sat cluster help
```
{: pre}


### `ibmcloud sat cluster ls`
{: #cluster-ls-cli}

[Virtual Private Cloud]{: tag-vpc} [Classic infrastructure]{: tag-classic-inf} [Satellite]{: tag-satellite} 

List all registered clusters in your IBM Cloud account.

```sh
ibmcloud sat cluster ls [--filter FILTER] [--limit LIMIT] [--output OUTPUT] [-q]
```

#### Command options
{: #cluster-ls-options}


`--filter`
:    Filter registered clusters by cluster ID.

`--limit`
:    Limit the number of clusters that are returned.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #cluster-ls-examples}

List all registered clusters in your IBM Cloud account

```sh
ibmcloud sat cluster ls
```
{: pre}


### `ibmcloud sat cluster register`
{: #cluster-register-cli}



Get a `kubectl` command to register your cluster in a Satellite configuration. Log in to your cluster and run this command to install a Satellite Config agent. Clusters that you run in your Satellite location automatically install this agent.

```sh
ibmcloud sat cluster register --name NAME [-q] [--silent]
```

#### Command options
{: #cluster-register-options}


`--name`
:    Specify the name of the cluster that you want to register

`-q`
:    Do not show the message of the day or update reminders.

`--silent`
:    Silent. Return only the registration command in the output.


#### Examples
{: #cluster-register-examples}

Get a `kubectl` command to register your cluster in a Satellite configuration

```sh
ibmcloud sat cluster register --name NAME
```
{: pre}


### `ibmcloud sat cluster unregister`
{: #cluster-unregister-cli}



Remove a cluster registration. The cluster is no longer subscribed to a Satellite configuration, but the cluster and its existing resources still run.

```sh
ibmcloud sat cluster unregister --cluster CLUSTER [-f] [-q]
```

#### Command options
{: #cluster-unregister-options}


`-c`, `--cluster`
:    Specify the cluster name or the ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #cluster-unregister-examples}

Remove a cluster registration

```sh
ibmcloud sat cluster unregister --cluster CLUSTER
```
{: pre}


## Config commands
{: #config-cli}

View and manage Satellite Configuration.


### `ibmcloud sat config create`
{: #config-create-cli}



Create a configuration to specify what Kubernetes resources you want to deploy to your clusters in your Satellite workloads.

```sh
ibmcloud sat config create --name NAME [-q] (--data-location LOCATION | --provider PROVIDER)
```

#### Command options
{: #config-create-options}


`--data-location`
:    Specify the IBM region to store the Satellite configuration data. Strategy: Direct Upload.

`--name`
:    Provide a name for the Satellite configuration.

`--provider`
:    Indicate the remote GitOps provider for the Satellite configuration. This provider stores the Kubernetes resource definitions. Strategy: GitOps. Allowed values: github, gitlab

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-create-examples}

Create a configuration to specify what Kubernetes resources you want to deploy to your clusters in your Satellite workloads

```sh
ibmcloud sat config create --name NAME --data-location LOCATION
```
{: pre}


### `ibmcloud sat config get`
{: #config-get-cli}



Get details of a Satellite configuration, such as the versions or subscriptions that are associated with the configuration.

```sh
ibmcloud sat config get --config CONFIG [--output OUTPUT] [-q]
```

#### Command options
{: #config-get-options}


`--config`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-get-examples}

Get details of a Satellite configuration, such as the versions or subscriptions that are associated with the configuration

```sh
ibmcloud sat config get --config CONFIG
```
{: pre}


### `ibmcloud sat config help`
{: #config-help-cli}



Show help

```sh
ibmcloud sat config help
```


#### Examples
{: #config-help-examples}

Show help

```sh
ibmcloud sat config help
```
{: pre}


### `ibmcloud sat config ls`
{: #config-ls-cli}



List all Satellite configurations in your IBM Cloud account.

```sh
ibmcloud sat config ls [--output OUTPUT] [-q]
```

#### Command options
{: #config-ls-options}


`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-ls-examples}

List all Satellite configurations in your IBM Cloud account

```sh
ibmcloud sat config ls
```
{: pre}


### `ibmcloud sat config rename`
{: #config-rename-cli}



Rename a Satellite configuration.

```sh
ibmcloud sat config rename --config CONFIG --name NAME [-q]
```

#### Command options
{: #config-rename-options}


`--config`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--name`
:    Provide a new name for the Satellite configuration.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-rename-examples}

Rename a Satellite configuration

```sh
ibmcloud sat config rename --config CONFIG --name NAME
```
{: pre}


### `ibmcloud sat config rm`
{: #config-rm-cli}



Remove a Satellite configuration. All associated subscriptions must be removed first. All versions are deleted. Back up any resource definitions that you want to keep.

```sh
ibmcloud sat config rm --config CONFIG [-f] [-q]
```

#### Command options
{: #config-rm-options}


`--config`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #config-rm-examples}

Remove a Satellite configuration

```sh
ibmcloud sat config rm --config CONFIG
```
{: pre}


### `ibmcloud sat config version create`
{: #config-version-create-cli}



Create a configuration version to update existing Kubernetes resources for your Satellite workloads.

```sh
ibmcloud sat config version create --config CONFIG --file-format FORMAT --name NAME --read-config CONFIG [--description DESCRIPTION] [-q]
```

#### Command options
{: #config-version-create-options}


`--config`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--description`
:    Add a description for the Satellite configuration version.

`--file-format`
:    Indicate the file format of the configuration version. Available options: yaml

`--name`
:    Provide a name for the Satellite configuration version.

`-q`
:    Do not show the message of the day or update reminders.

`--read-config`
:    Specify the file path for the configuration version file.


#### Examples
{: #config-version-create-examples}

Create a configuration version to update existing Kubernetes resources for your Satellite workloads

```sh
ibmcloud sat config version create \
  --config CONFIG \
  --file-format FORMAT \
  --name NAME \
  --read-config CONFIG
```
{: pre}


### `ibmcloud sat config version get`
{: #config-version-get-cli}



Get details for a Satellite configuration version.

```sh
ibmcloud sat config version get --config CONFIG --version VERSION [--output OUTPUT] [-q] [--save-config]
```

#### Command options
{: #config-version-get-options}


`--config`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--save-config`
:    Download and save the configuration version to a temporary file.

`--version`
:    Specify the name or ID of the Satellite configuration version. To list versions in your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`.


#### Examples
{: #config-version-get-examples}

Get details for a Satellite configuration version

```sh
ibmcloud sat config version get --config CONFIG --version VERSION
```
{: pre}


### `ibmcloud sat config version help`
{: #config-version-help-cli}



Show help

```sh
ibmcloud sat config version help
```


#### Examples
{: #config-version-help-examples}

Show help

```sh
ibmcloud sat config version help
```
{: pre}


### `ibmcloud sat config version rm`
{: #config-version-rm-cli}



Remove a Satellite configuration version.

```sh
ibmcloud sat config version rm --config CONFIG --version VERSION [-f] [-q]
```

#### Command options
{: #config-version-rm-options}


`--config`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--version`
:    Indicate the name or ID of the Satellite configuration version. To list versions, run `ibmcloud sat config get --config <configuration_name_or_ID>`.


#### Examples
{: #config-version-rm-examples}

Remove a Satellite configuration version

```sh
ibmcloud sat config version rm --config CONFIG --version VERSION
```
{: pre}


## Connector commands
{: #connector-cli}

Create, view, and modify Satellite connectors.


### `ibmcloud sat connector create`
{: #connector-create-cli}



Create a Satellite connector.

```sh
ibmcloud sat connector create --name NAME --region REGION [-q]
```

#### Command options
{: #connector-create-options}


`--name`
:    The name for the Satellite connector.

`-q`
:    Do not show the message of the day or update reminders.

`--region`
:    The IBM Cloud region to manage your Satellite connector.


#### Examples
{: #connector-create-examples}

Create a Satellite connector

```sh
ibmcloud sat connector create --name NAME --region REGION
```
{: pre}


### `ibmcloud sat connector get`
{: #connector-get-cli}



View the details of a Satellite Connector.

```sh
ibmcloud sat connector get --connector-id ID [--output OUTPUT] [-q]
```

#### Command options
{: #connector-get-options}


`--connector-id`
:    The ID of a Satellite connector.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #connector-get-examples}

View the details of a Satellite Connector

```sh
ibmcloud sat connector get --connector-id ID
```
{: pre}


### `ibmcloud sat connector help`
{: #connector-help-cli}



Show help

```sh
ibmcloud sat connector help
```


#### Examples
{: #connector-help-examples}

Show help

```sh
ibmcloud sat connector help
```
{: pre}


### `ibmcloud sat connector ls`
{: #connector-ls-cli}



View the Satellite Connectors in your IBM Cloud account.

```sh
ibmcloud sat connector ls [--after AFTER] [--first FIRST] [--output OUTPUT] [-q]
```

#### Command options
{: #connector-ls-options}


`--after`
:    Show Satellite Connectors after the given cursor.

`--first`
:    View the next Satellite Connectors, up to the first number of Connectors.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #connector-ls-examples}

View the Satellite Connectors in your IBM Cloud account

```sh
ibmcloud sat connector ls
```
{: pre}


### `ibmcloud sat connector rm`
{: #connector-rm-cli}



Delete a Satellite connector.

```sh
ibmcloud sat connector rm --connector-id ID [-f] [-q]
```

#### Command options
{: #connector-rm-options}


`--connector-id`
:    The ID of a Satellite connector.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #connector-rm-examples}

Delete a Satellite connector

```sh
ibmcloud sat connector rm --connector-id ID
```
{: pre}


## Endpoint commands
{: #endpoint-cli}

View and manage Satellite endpoints.


### `ibmcloud sat endpoint authn get`
{: #endpoint-authn-get-cli}



Get the authentication settings for an endpoint.

```sh
ibmcloud sat endpoint authn get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-authn-get-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-authn-get-examples}

Get the authentication settings for an endpoint

```sh
ibmcloud sat endpoint authn get --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint authn help`
{: #endpoint-authn-help-cli}



Show help

```sh
ibmcloud sat endpoint authn help
```


#### Examples
{: #endpoint-authn-help-examples}

Show help

```sh
ibmcloud sat endpoint authn help
```
{: pre}


### `ibmcloud sat endpoint authn rotate`
{: #endpoint-authn-rotate-cli}



Replace existing authentication certificates with new ones. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Only the certificates that you specify are replaced.

```sh
ibmcloud sat endpoint authn rotate --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-authn-rotate-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-ca-cert-file`
:    Trusted CA certificate or chain used to validate the destination server's certificate. For example `myCA.pem`.

`--dest-cert-file`
:    The client certificate used to authenticate with the destination server. For example `myCert.pem`.

`--dest-key-file`
:    The client private key used to encrypt the client certificate. For example `myKey.pem`.

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--source-ca-cert-file`
:    Trusted CA certificate or chain used to validate the source client's certificate when source-tls-mode is mutual. For example `myCA.pem`.

`--source-cert-file`
:    The server certificate to present to the source client. For example `myCert.pem`.

`--source-key-file`
:    The server private key used to encrypt the server certificate. For example `myKey.pem`.


#### Examples
{: #endpoint-authn-rotate-examples}

Replace existing authentication certificates with new ones

```sh
ibmcloud sat endpoint authn rotate --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint authn set`
{: #endpoint-authn-set-cli}



Set authentication settings for an endpoint. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.

```sh
ibmcloud sat endpoint authn set --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [--dest-tls-mode MODE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] [--source-tls-mode MODE] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-authn-set-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-ca-cert-file`
:    Trusted CA certificate or chain used to validate the destination server's certificate. For example `myCA.pem`.

`--dest-cert-file`
:    The client certificate used to authenticate with the destination server. For example `myCert.pem`.

`--dest-key-file`
:    The client private key used to encrypt the client certificate. For example `myKey.pem`.

`--dest-tls-mode`
:    The destination TLS mode. Accepted values: `simple`, `mutual`, `none`

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--source-ca-cert-file`
:    Trusted CA certificate or chain used to validate the source client's certificate when source-tls-mode is mutual. For example `myCA.pem`.

`--source-cert-file`
:    The server certificate to present to the source client. For example `myCert.pem`.

`--source-key-file`
:    The server private key used to encrypt the server certificate. For example `myKey.pem`.

`--source-tls-mode`
:    The source TLS mode. Accepted values: `simple`, `mutual`


#### Examples
{: #endpoint-authn-set-examples}

Set authentication settings for an endpoint

```sh
ibmcloud sat endpoint authn set --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint create`
{: #endpoint-create-cli}



Create an endpoint.

```sh
ibmcloud sat endpoint create --dest-hostname HOSTNAME --dest-port PORT --dest-type TYPE --name NAME --source-protocol PROTOCOL [--dest-protocol PROTOCOL] [--idle-timeout-seconds SECONDS] [--output OUTPUT] [-q] [--sni SNI] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-create-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-hostname`
:    Indicate the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `cloud` endpoints, this value must resolve to a public IP address or to a private IP address that is accessible within IBM Cloud such as a private cloud service endpoint. For `location` endpoints, this value must resolve from and be reachable from the control plane hosts for Satellite locations or where the agent runs for Satellite Connector.

`--dest-port`
:    Provide the port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol`
:    Specify the destination's protocol. If you do not specify this option, the destination protocol is inherited from the source protocol. Accepted values: `TCP`, `TLS`

`--dest-type`
:    Specify where the destination resource runs, either in IBM Cloud (`cloud`) or your Satellite location (`location`). Available options: location, cloud

`--idle-timeout-seconds`
:    Specify the timeout interval in seconds for active connections to the destination. Make sure your timeout is compatible with the destination service and protocol `keep-alive` settings.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name`
:    Provide a name for the endpoint.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--sni`
:    Specify the server name indicator, if you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake.

`--source-protocol`
:    Provide the protocol that the source uses to connect the destination resource. See [http://ibm.biz/endpoint-protocols](http://ibm.biz/endpoint-protocols). Available options: TCP, TLS, HTTP, HTTPS, HTTP-tunnel


#### Examples
{: #endpoint-create-examples}

Create an endpoint

```sh
ibmcloud sat endpoint create \
  --dest-hostname HOSTNAME \
  --dest-port PORT \
  --dest-type TYPE \
  --name NAME \
  --source-protocol PROTOCOL \
  --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint disable`
{: #endpoint-disable-cli}



Disable an endpoint.

```sh
ibmcloud sat endpoint disable --endpoint ENDPOINT [-f] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-disable-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`-f`
:    Force the command to run without user prompts.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-disable-examples}

Disable an endpoint

```sh
ibmcloud sat endpoint disable --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint enable`
{: #endpoint-enable-cli}



Enable an endpoint.

```sh
ibmcloud sat endpoint enable --endpoint ENDPOINT [-f] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-enable-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`-f`
:    Force the command to run without user prompts.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-enable-examples}

Enable an endpoint

```sh
ibmcloud sat endpoint enable --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint get`
{: #endpoint-get-cli}



View the details of an endpoint.

```sh
ibmcloud sat endpoint get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-get-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-get-examples}

View the details of an endpoint

```sh
ibmcloud sat endpoint get --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint help`
{: #endpoint-help-cli}



Show help

```sh
ibmcloud sat endpoint help
```


#### Examples
{: #endpoint-help-examples}

Show help

```sh
ibmcloud sat endpoint help
```
{: pre}


### `ibmcloud sat endpoint ls`
{: #endpoint-ls-cli}



List all endpoints in a Satellite location.

```sh
ibmcloud sat endpoint ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-ls-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-ls-examples}

List all endpoints in a Satellite location

```sh
ibmcloud sat endpoint ls --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint rm`
{: #endpoint-rm-cli}



Delete an endpoint.

```sh
ibmcloud sat endpoint rm --endpoint ENDPOINT [-q] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-rm-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #endpoint-rm-examples}

Delete an endpoint

```sh
ibmcloud sat endpoint rm --endpoint ENDPOINT --connector-id ID
```
{: pre}


### `ibmcloud sat endpoint update`
{: #endpoint-update-cli}



Update an endpoint. Only the options that you specify are updated.

```sh
ibmcloud sat endpoint update --endpoint ENDPOINT [--dest-hostname HOSTNAME] [--dest-port PORT] [--dest-protocol PROTOCOL] [--idle-timeout-seconds SECONDS] [--name NAME] [-q] [--sni SNI] [--source-protocol PROTOCOL] (--connector-id ID | --location LOCATION)
```

#### Command options
{: #endpoint-update-options}


`--connector-id`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--dest-hostname`
:    Indicate the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `cloud` endpoints, this value must resolve to a public IP address or to a private IP address that is accessible within IBM Cloud such as a private cloud service endpoint. For `location` endpoints, this value must resolve from and be reachable from the control plane hosts for Satellite locations or where the agent runs for Satellite Connector.

`--dest-port`
:    Provide the port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol`
:    Specify the destination's protocol. If you do not specify this option, the destination protocol is inherited from the source protocol. Accepted values: `TCP`, `TLS`

`--endpoint`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--idle-timeout-seconds`
:    Specify the timeout interval in seconds for active connections to the destination. Make sure your timeout is compatible with the destination service and protocol `keep-alive` settings.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--name`
:    Provide a new name for the endpoint.

`-q`
:    Do not show the message of the day or update reminders.

`--sni`
:    Specify the server name indicator, if you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake.

`--source-protocol`
:    Provide the protocol that the source uses to connect the destination resource. See [http://ibm.biz/endpoint-protocols](http://ibm.biz/endpoint-protocols). Accepted values: `TCP`, `TLS`, `HTTP`, `HTTPS`, `HTTP-tunnel`


#### Examples
{: #endpoint-update-examples}

Update an endpoint

```sh
ibmcloud sat endpoint update --endpoint ENDPOINT --connector-id ID
```
{: pre}


## Experimental commands
{: #experimental-cli}

[Expires on 2024-11-25] Experiment with new commands. IMPORTANT: Commands here will retire after the [date] in their description.


### `ibmcloud sat experimental endpoint help`
{: #experimental-endpoint-help-cli}



Show help

```sh
ibmcloud sat experimental endpoint help
```


#### Examples
{: #experimental-endpoint-help-examples}

Show help

```sh
ibmcloud sat experimental endpoint help
```
{: pre}


### `ibmcloud sat experimental help`
{: #experimental-help-cli}



Show help

```sh
ibmcloud sat experimental help
```


#### Examples
{: #experimental-help-examples}

Show help

```sh
ibmcloud sat experimental help
```
{: pre}


## Group commands
{: #group-cli}

View and manage Satellite cluster groups. Cluster groups are used to subscribe clusters to Satellite configurations of Kubernetes resources.


### `ibmcloud sat group attach`
{: #group-attach-cli}



Add a cluster to your cluster group. The cluster can run in your Satellite location or in IBM Cloud. To add a cluster that runs in IBM Cloud, you must first register the cluster with Satellite Config.

```sh
ibmcloud sat group attach --cluster CLUSTER [--cluster CLUSTER ...] --group GROUP [-q]
```

#### Command options
{: #group-attach-options}


`-c`, `--cluster`
:    Specify the cluster name or ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`-g`, `--group`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-attach-examples}

Add a cluster to your cluster group

```sh
ibmcloud sat group attach --cluster CLUSTER --group GROUP
```
{: pre}


### `ibmcloud sat group create`
{: #group-create-cli}



Create a cluster group. Then, you can subscribe the cluster group to a Satellite configuration.

```sh
ibmcloud sat group create --name NAME [--cluster CLUSTER ...] [-q]
```

#### Command options
{: #group-create-options}


`-c`, `--cluster`
:    Specify the cluster name or ID to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--name`
:    Provide a name of the Satellite cluster group.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-create-examples}

Create a cluster group

```sh
ibmcloud sat group create --name NAME
```
{: pre}


### `ibmcloud sat group detach`
{: #group-detach-cli}



Removes one or more clusters from your Satellite cluster group and deletes the Kubernetes resources that were managed by the group's subscriptions.

```sh
ibmcloud sat group detach --cluster CLUSTER [--cluster CLUSTER ...] --group GROUP [-f] [-q]
```

#### Command options
{: #group-detach-options}


`-c`, `--cluster`
:    Specify the cluster name or ID. To list the clusters in your cluster group, run `ibmcloud sat group get --group <cluster_group_name_or_ID>`.

`-f`
:    Force the command to run without user prompts.

`-g`, `--group`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-detach-examples}

Removes one or more clusters from your Satellite cluster group and deletes the Kubernetes resources that were managed by the group's subscriptions

```sh
ibmcloud sat group detach --cluster CLUSTER --group GROUP
```
{: pre}


### `ibmcloud sat group get`
{: #group-get-cli}



Get detailed information for a Satellite cluster group.

```sh
ibmcloud sat group get --group GROUP [--output OUTPUT] [-q]
```

#### Command options
{: #group-get-options}


`-g`, `--group`
:    Specify the name or ID of a Satellite cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-get-examples}

Get detailed information for a Satellite cluster group

```sh
ibmcloud sat group get --group GROUP
```
{: pre}


### `ibmcloud sat group help`
{: #group-help-cli}



Show help

```sh
ibmcloud sat group help
```


#### Examples
{: #group-help-examples}

Show help

```sh
ibmcloud sat group help
```
{: pre}


### `ibmcloud sat group ls`
{: #group-ls-cli}



List all Satellite cluster groups in your IBM Cloud account.

```sh
ibmcloud sat group ls [--output OUTPUT] [-q]
```

#### Command options
{: #group-ls-options}


`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-ls-examples}

List all Satellite cluster groups in your IBM Cloud account

```sh
ibmcloud sat group ls
```
{: pre}


### `ibmcloud sat group rm`
{: #group-rm-cli}



Remove a Satellite cluster group, which unsubscribes clusters and deletes the Kubernetes resources that were managed by the group's subscriptions.

```sh
ibmcloud sat group rm --group GROUP [-f] [-q]
```

#### Command options
{: #group-rm-options}


`-f`
:    Force the command to run without user prompts.

`-g`, `--group`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #group-rm-examples}

Remove a Satellite cluster group, which unsubscribes clusters and deletes the Kubernetes resources that were managed by the group's subscriptions

```sh
ibmcloud sat group rm --group GROUP
```
{: pre}


## Host commands
{: #host-cli}

View and modify Satellite hosts.


### `ibmcloud sat host assign`
{: #host-assign-cli}



Assign a host to a Satellite location control plane or cluster.

```sh
ibmcloud sat host assign --location LOCATION [--cluster CLUSTER] [--host HOST] [--host-label LABEL ...] [-q] [--worker-pool POOL] [--zone ZONE]
```

#### Command options
{: #host-assign-options}


`--cluster`
:    The name or ID of the cluster to assign the host to. To list available clusters, run `ibmcloud sat cluster ls`. If no cluster is provided, the host is automatically assigned to the Satellite control plane.

`--host`
:    The name or ID of the host to assign. To automatically assign hosts based on labels, do not include this option. To retrieve the host ID, run `ibmcloud sat host ls --location <location_ID_or_name>`.

`--host-label`, `--hl`
:    Enter any labels as key-value pairs to identify the host to assign to your Satellite control plane or Red Hat OpenShift cluster. The first host that has this label and is unassigned is automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-p`, `--worker-pool`
:    The name or ID of the worker pool within the cluster to assign the host. If no worker pool is specified, the host is assigned to the default worker pool.

`-q`
:    Do not show the message of the day or update reminders.

`--zone`
:    The name or ID of the zone to assign the host. To find available zones, run `ibmcloud sat location get --location <location_name_or_ID>` and look for the `Host Zones` field.


#### Examples
{: #host-assign-examples}

Assign a host to a Satellite location control plane or cluster

```sh
ibmcloud sat host assign --location LOCATION
```
{: pre}


### `ibmcloud sat host attach`
{: #host-attach-cli}



Create and download a script that you can run on your hosts to attach them to your location. For CoreOS enabled locations, the script is an ignition file.

```sh
ibmcloud sat host attach --location LOCATION [--host-label LABEL ...] [--host-link-agent-endpoint ENDPOINT] [--operating-system SYSTEM] [-q] [--reset-key]
```

#### Command options
{: #host-attach-options}


`--host-label`, `--hl`
:    Enter any labels as key-value pairs to identify the host to assign to your Satellite control plane or Red Hat OpenShift cluster. The first host that has this label and is unassigned is automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.

`--host-link-agent-endpoint`
:    The endpoint that the link agent uses to connect to the link tunnel server.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--operating-system`
:    The operating system of the hosts you want to attach to your location. To attach RHCOS hosts, your location must be RHCOS enabled. Accepted values: `RHEL`, `RHCOS`

`-q`
:    Do not show the message of the day or update reminders.

`--reset-key`
:    Reset the key that the control plane uses to attach and assign hosts in the location. See https://ibm.biz/reset-key.


#### Examples
{: #host-attach-examples}

Create and download a script that you can run on your hosts to attach them to your location

```sh
ibmcloud sat host attach --location LOCATION
```
{: pre}


### `ibmcloud sat host get`
{: #host-get-cli}



View the details of a Satellite host.

```sh
ibmcloud sat host get --host HOST --location LOCATION [--output OUTPUT] [-q]
```

#### Command options
{: #host-get-options}


`--host`
:    The Satellite host ID. To find the host ID, run `ibmcloud sat host ls <location_ID_or_name>`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #host-get-examples}

View the details of a Satellite host

```sh
ibmcloud sat host get --host HOST --location LOCATION
```
{: pre}


### `ibmcloud sat host help`
{: #host-help-cli}



Show help

```sh
ibmcloud sat host help
```


#### Examples
{: #host-help-examples}

Show help

```sh
ibmcloud sat host help
```
{: pre}


### `ibmcloud sat host ls`
{: #host-ls-cli}



List all hosts that are attached to a Satellite location, including hosts that are assigned to clusters or the control plane.

```sh
ibmcloud sat host ls --location LOCATION [--output OUTPUT] [-q]
```

#### Command options
{: #host-ls-options}


`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #host-ls-examples}

List all hosts that are attached to a Satellite location, including hosts that are assigned to clusters or the control plane

```sh
ibmcloud sat host ls --location LOCATION
```
{: pre}


### `ibmcloud sat host rm`
{: #host-rm-cli}



Remove a host from a Satellite location.

```sh
ibmcloud sat host rm --host HOST --location LOCATION [-f] [-q]
```

#### Command options
{: #host-rm-options}


`-f`
:    Force the command to run without user prompts.

`--host`
:    The name or ID of the host to remove.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #host-rm-examples}

Remove a host from a Satellite location

```sh
ibmcloud sat host rm --host HOST --location LOCATION
```
{: pre}


### `ibmcloud sat host update`
{: #host-update-cli}



Update host information, such as zones and labels.

```sh
ibmcloud sat host update --host HOST --location LOCATION [--host-label LABEL ...] [-q] [--zone ZONE]
```

#### Command options
{: #host-update-options}


`--host`
:    The name or ID of the host to assign. To automatically assign hosts based on labels, do not include this option.

`--host-label`, `--hl`
:    Enter any labels as key-value pairs to identify the host to assign to your Satellite control plane or Red Hat OpenShift cluster. The first host that has this label and is unassigned is automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.

`--zone`
:    The name or ID of the zone to associate the host. You cannot change the zone of hosts that are assigned to a resource, such as a cluster. You must unassign them first. To list available zones, run `ibmcloud sat location get --location <ID>`.


#### Examples
{: #host-update-examples}

Update host information, such as zones and labels

```sh
ibmcloud sat host update --host HOST --location LOCATION
```
{: pre}


## Key commands
{: #key-cli}

View and manage Satellite Config keys.


### `ibmcloud sat key help`
{: #key-help-cli}



Show help

```sh
ibmcloud sat key help
```


#### Examples
{: #key-help-examples}

Show help

```sh
ibmcloud sat key help
```
{: pre}


### `ibmcloud sat key ls`
{: #key-ls-cli}



List all Satellite Config keys in your IBM Cloud account.

```sh
ibmcloud sat key ls [--output OUTPUT] [-q]
```

#### Command options
{: #key-ls-options}


`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #key-ls-examples}

List all Satellite Config keys in your IBM Cloud account

```sh
ibmcloud sat key ls
```
{: pre}


### `ibmcloud sat key rm`
{: #key-rm-cli}



Remove a Satellite Config key. Any cluster that still uses this key cannot connect to Satellite Config.

```sh
ibmcloud sat key rm --key KEY [-f] [-q]
```

#### Command options
{: #key-rm-options}


`-f`
:    Force the command to run without user prompts.

`--key`
:    The name or ID of a Satellite Config key.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #key-rm-examples}

Remove a Satellite Config key

```sh
ibmcloud sat key rm --key KEY
```
{: pre}


### `ibmcloud sat key rotate`
{: #key-rotate-cli}



Generate a new key for use by managed clusters to connect to Satellite Config.

```sh
ibmcloud sat key rotate --name NAME [-f] [-q]
```

#### Command options
{: #key-rotate-options}


`-f`
:    Force the command to run without user prompts.

`--name`
:    The name of the new Satellite Config key.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #key-rotate-examples}

Generate a new key for use by managed clusters to connect to Satellite Config

```sh
ibmcloud sat key rotate --name NAME
```
{: pre}


## Location commands
{: #location-cli}

Create, view, and modify Satellite locations.


### `ibmcloud sat location create`
{: #location-create-cli}



Create a Satellite location. A Satellite location is a representation of an environment in your infrastructure provider. After you create a location, attach hosts from separate zones of your backing infrastructure environment with the `ibmcloud sat host attach` command.

```sh
ibmcloud sat location create --managed-from REGION --name NAME [--capability CAPABILITY ...] [--coreos-enabled] [--cos-bucket BUCKET] [--description DESCRIPTION] [--ha-zone ZONE ...] [--physical-address ADDRESS] [--pod-network-interface-selection SELECTION] [--pod-subnet SUBNET] [--provider PROVIDER] [--provider-credential CREDENTIAL] [--provider-region REGION] [-q] [--service-subnet SUBNET]
```

#### Command options
{: #location-create-options}


`--capability`
:    A capability of the Satellite location.

`--coreos-enabled`
:    Enable Red Hat CoreOS features for the Satellite location. This action cannot be undone. See [https://ibm.biz/infra-os](https://ibm.biz/infra-os).

`--cos-bucket`
:    Specify the name of the IBM Cloud Object Storage bucket to store your Satellite location control plane data. Otherwise, a new bucket is created for you.

`--description`
:    Enter a description for the Satellite location.

`--ha-zone`
:    Specify the zone for your location. For high availability, specify 3 zones for your location as `--ha-zone ZONE1_NAME --ha-zone ZONE2_NAME --ha-zone ZONE3_NAME`. The names of the zones must match exactly the names of the corresponding zones in your infrastructure provider where you plan to create hosts.

`--managed-from`
:    Select the IBM Cloud region to manage your Satellite location from. Choose a region close to your on-prem data center for better performance. See [https://ibm.biz/sat-region](https://ibm.biz/sat-region).

`--name`
:    Specify a name for the Satellite location. Location names must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be fewer than 36 characters. Do not reuse names, even if the other location is deleted.

`--physical-address`
:    The physical address of the Satellite location.

`--pod-network-interface-selection`
:    The method for selecting the node network interface for the internal pod network. This option can be used only if you also enable Red Hat CoreOS with the `--coreos-enabled` option. To provide a direct URL or IP address, specify `can-reach=<url>` or `can-reach=<ip_address>`. To choose a network interface, specify `interface=<network_interface>`.

`--pod-subnet`
:    Specify a custom subnet CIDR to provide private IP addresses for pods. This option is used only if you enable Red Hat CoreOS with the `--coreos-enabled` option. The subnet must be `/23` or larger. See [https://ibm.biz/sat-location-create](https://ibm.biz/sat-location-create). Default value: '172.16.0.0/16

`--provider`
:    Indicate the infrastructure provider to use for the Satellite location. If you include this option, you must also include the `--provider-credential` option. Accepted values: `aws`, `azure`, `gcp`, `vmware`

`--provider-credential`
:    Specify the path to a JSON file on your local machine that has the credentials of the infrastructure provider for the Satellite location. The credential format is provider-specific. See [http://ibm.biz/sat-infra-creds](http://ibm.biz/sat-infra-creds).

`--provider-region`
:    Specify the region in the infrastructure provider where you plan to create the hosts for the Satellite location. If you include this option, you must also include the `--provider` option.

`-q`
:    Do not show the message of the day or update reminders.

`--service-subnet`
:    Specify a custom subnet CIDR to provide private IP addresses for services. This option is used only if you enable Red Hat CoreOS with the `--coreos-enabled` option. The subnet must be `/24` or larger. See [https://ibm.biz/sat-location-create](https://ibm.biz/sat-location-create). Default value: `172.20.0.0/16`


#### Examples
{: #location-create-examples}

Create a Satellite location

```sh
ibmcloud sat location create --managed-from REGION --name NAME
```
{: pre}


### `ibmcloud sat location dns get`
{: #location-dns-get-cli}



View the details of a registered subdomain in a Satellite location.

```sh
ibmcloud sat location dns get --location LOCATION --subdomain SUBDOMAIN [--output OUTPUT] [-q]
```

#### Command options
{: #location-dns-get-options}


`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--subdomain`
:    Specify the subdomain name. To list existing subdomains, run `ibmcloud sat location dns ls --location <ID>`.


#### Examples
{: #location-dns-get-examples}

View the details of a registered subdomain in a Satellite location

```sh
ibmcloud sat location dns get --location LOCATION --subdomain SUBDOMAIN
```
{: pre}


### `ibmcloud sat location dns help`
{: #location-dns-help-cli}



Show help

```sh
ibmcloud sat location dns help
```


#### Examples
{: #location-dns-help-examples}

Show help

```sh
ibmcloud sat location dns help
```
{: pre}


### `ibmcloud sat location dns ls`
{: #location-dns-ls-cli}



List the registered subdomains in a Satellite location.

```sh
ibmcloud sat location dns ls --location LOCATION [--output OUTPUT] [-q]
```

#### Command options
{: #location-dns-ls-options}


`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-dns-ls-examples}

List the registered subdomains in a Satellite location

```sh
ibmcloud sat location dns ls --location LOCATION
```
{: pre}


### `ibmcloud sat location dns register`
{: #location-dns-register-cli}



Set a subdomain for the hosts assigned to the control plane in a Satellite location.

```sh
ibmcloud sat location dns register --ip IP [--ip IP ...] --location LOCATION [--output OUTPUT] [-q]
```

#### Command options
{: #location-dns-register-options}


`--ip`
:    Specify the IP address for each control plane host, in the format `--ip x.x.x.1 --ip x.x.x.2 --ip x.x.x.3`. For multizone clusters, use one IP address from each zone. To find the IP address, run `ibmcloud sat host ls --location <location_ID_or_name>` and look for `Worker IP` for hosts labeled `infrastructure`.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-dns-register-examples}

Set a subdomain for the hosts assigned to the control plane in a Satellite location

```sh
ibmcloud sat location dns register --ip IP --location LOCATION
```
{: pre}


### `ibmcloud sat location get`
{: #location-get-cli}



View the details of a Satellite location.

```sh
ibmcloud sat location get --location LOCATION [--output OUTPUT] [-q]
```

#### Command options
{: #location-get-options}


`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-get-examples}

View the details of a Satellite location

```sh
ibmcloud sat location get --location LOCATION
```
{: pre}


### `ibmcloud sat location help`
{: #location-help-cli}



Show help

```sh
ibmcloud sat location help
```


#### Examples
{: #location-help-examples}

Show help

```sh
ibmcloud sat location help
```
{: pre}


### `ibmcloud sat location ls`
{: #location-ls-cli}



List all Satellite locations in your IBM Cloud account.

```sh
ibmcloud sat location ls [--output OUTPUT] [-q]
```

#### Command options
{: #location-ls-options}


`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-ls-examples}

List all Satellite locations in your IBM Cloud account

```sh
ibmcloud sat location ls
```
{: pre}


### `ibmcloud sat location rm`
{: #location-rm-cli}



Delete a location. Before you run this command, back up your configurations and remove any hosts and clusters that run in the location. The underlying host infrastructure is not automatically deleted when you delete a location. This action cannot be undone.

```sh
ibmcloud sat location rm --location LOCATION [-f] [-q]
```

#### Command options
{: #location-rm-options}


`-f`
:    Force the command to run without user prompts.

`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-rm-examples}

Delete a location

```sh
ibmcloud sat location rm --location LOCATION
```
{: pre}


### `ibmcloud sat location update`
{: #location-update-cli}



Update the name or description of a Satellite location.

```sh
ibmcloud sat location update --location-id ID [--description DESCRIPTION] [--name NAME] [-q]
```

#### Command options
{: #location-update-options}


`--description`
:    Enter a new description for the Satellite location. The length of the description is limited to 400 bytes.

`--location-id`
:    The ID of the Satellite location. To find the location ID, run `ibmcloud sat location ls`.

`--name`
:    Specify a new name for the Satellite location. Location names must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be fewer than 36 characters. Do not reuse names, including names of deleted locations.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #location-update-examples}

Update the name or description of a Satellite location

```sh
ibmcloud sat location update --location-id ID
```
{: pre}


## Messages commands
{: #messages-cli}

View the current user messages.


### `ibmcloud sat messages`
{: #messages-cli}



View the current user messages.

```sh
ibmcloud sat messages [-q]
```

#### Command options
{: #messages-options}


`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #messages-examples}

View the current user messages

```sh
ibmcloud sat messages
```
{: pre}


## Resource commands
{: #resource-cli}

Search and view Kubernetes resources that are managed by a Satellite configuration.


### `ibmcloud sat resource get`
{: #resource-get-cli}



View the details of a Kubernetes resource that is managed by a Satellite configuration.

```sh
ibmcloud sat resource get --resource RESOURCE [--history HISTORY] [--output OUTPUT] [-q] [--save-data]
```

#### Command options
{: #resource-get-options}


`--history`
:    The history ID for the resource.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--resource`
:    Specify the Kubernetes resource ID. To find Kubernetes resources, run `ibmcloud sat resource ls`.

`--save-data`
:    Download and save a Kubernetes resource definition to a temporary file.


#### Examples
{: #resource-get-examples}

View the details of a Kubernetes resource that is managed by a Satellite configuration

```sh
ibmcloud sat resource get --resource RESOURCE
```
{: pre}


### `ibmcloud sat resource help`
{: #resource-help-cli}



Show help

```sh
ibmcloud sat resource help
```


#### Examples
{: #resource-help-examples}

Show help

```sh
ibmcloud sat resource help
```
{: pre}


### `ibmcloud sat resource history get`
{: #resource-history-get-cli}



Get history for a Kubernetes resource.

```sh
ibmcloud sat resource history get --resource RESOURCE [--limit LIMIT] [--output OUTPUT] [-q]
```

#### Command options
{: #resource-history-get-options}


`--limit`
:    Specify the maximum number of history entries to return.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--resource`
:    The Kubernetes resource ID.


#### Examples
{: #resource-history-get-examples}

Get history for a Kubernetes resource

```sh
ibmcloud sat resource history get --resource RESOURCE
```
{: pre}


### `ibmcloud sat resource history help`
{: #resource-history-help-cli}



Show help

```sh
ibmcloud sat resource history help
```


#### Examples
{: #resource-history-help-examples}

Show help

```sh
ibmcloud sat resource history help
```
{: pre}


### `ibmcloud sat resource ls`
{: #resource-ls-cli}



Search Kubernetes resources that are managed by Satellite.

```sh
ibmcloud sat resource ls [--limit LIMIT] [--output OUTPUT] [-q] [--search SEARCH] (--cluster CLUSTER | --subscription SUBSCRIPTION)
```

#### Command options
{: #resource-ls-options}


`-c`, `--cluster`
:    Specify the name or ID of the registered cluster that the Kubernetes resource runs in. To find registered clusters, run `ibmcloud sat cluster ls`.

`--limit`
:    Specify the maximum number of resource entries for the search to return.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--search`
:    Indicate the string to filter search results of Kubernetes resources, such as a pod or namespace name.

`--subscription`
:    Specify the Satellite subscription ID or name.  To find subscriptions, run `ibmcloud sat cluster ls`.


#### Examples
{: #resource-ls-examples}

Search Kubernetes resources that are managed by Satellite

```sh
ibmcloud sat resource ls --cluster CLUSTER
```
{: pre}


## Service commands
{: #service-cli}

View Satellite service clusters.


### `ibmcloud sat service help`
{: #service-help-cli}



Show help

```sh
ibmcloud sat service help
```


#### Examples
{: #service-help-examples}

Show help

```sh
ibmcloud sat service help
```
{: pre}


### `ibmcloud sat service ls`
{: #service-ls-cli}



List all Satellite service clusters in your location to review details, such as requested host resources.

```sh
ibmcloud sat service ls --location LOCATION [--output OUTPUT] [-q]
```

#### Command options
{: #service-ls-options}


`--location`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #service-ls-examples}

List all Satellite service clusters in your location to review details, such as requested host resources

```sh
ibmcloud sat service ls --location LOCATION
```
{: pre}


## Storage commands
{: #storage-cli}

View and manage Satellite storage resources.


### `ibmcloud sat storage assignment autopatch disable`
{: #storage-assignment-autopatch-disable-cli}

The `storage assignment autopatch disable` command is a beta feature.
{: beta}



Disable automatic patches for a Satellite storage assignment.

```sh
ibmcloud sat storage assignment autopatch disable --config CONFIG [-q] (--all | --assignment ASSIGNMENT)
```

#### Command options
{: #storage-assignment-autopatch-disable-options}


`--all`
:    Disable automatic patches for all Satellite storage assignments of a storage configuration.

`--assignment`
:    The ID of a Satellite storage assignment. To list available storage assignments of the configuration, run `ibmcloud sat storage assignment ls  --config CONFIG`.

`--config`
:    The name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-autopatch-disable-examples}

Disable automatic patches for a Satellite storage assignment

```sh
ibmcloud sat storage assignment autopatch disable --config CONFIG --all
```
{: pre}


### `ibmcloud sat storage assignment autopatch enable`
{: #storage-assignment-autopatch-enable-cli}

The `storage assignment autopatch enable` command is a beta feature.
{: beta}



Enable automatic patches for a Satellite storage assignment.

```sh
ibmcloud sat storage assignment autopatch enable --config CONFIG [-q] (--all | --assignment ASSIGNMENT)
```

#### Command options
{: #storage-assignment-autopatch-enable-options}


`--all`
:    Enable automatic patches for all Satellite storage assignments of a storage configuration.

`--assignment`
:    The ID of a Satellite storage assignment. To list available storage assignments of the configuration, run `ibmcloud sat storage assignment ls  --config CONFIG`.

`--config`
:    The name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-autopatch-enable-examples}

Enable automatic patches for a Satellite storage assignment

```sh
ibmcloud sat storage assignment autopatch enable --config CONFIG --all
```
{: pre}


### `ibmcloud sat storage assignment autopatch help`
{: #storage-assignment-autopatch-help-cli}



Show help

```sh
ibmcloud sat storage assignment autopatch help
```


#### Examples
{: #storage-assignment-autopatch-help-examples}

Show help

```sh
ibmcloud sat storage assignment autopatch help
```
{: pre}


### `ibmcloud sat storage assignment create`
{: #storage-assignment-create-cli}



Create an assignment to deploy your storage configurations to clusters in your Satellite location.

```sh
ibmcloud sat storage assignment create --config CONFIG [--name NAME] [-q] (--cluster CLUSTER | --group GROUP | --service-cluster-id CLUSTER)
```

#### Command options
{: #storage-assignment-create-options}


`-c`, `--cluster`
:    Specify the ID of the Satellite cluster for the assignment. To find the cluster ID, run `ibmcloud oc cluster ls --provider satellite`.

`--config`
:    Specify the Satellite storage configuration for the assignment. to find configurations, run `ibmcloud sat storage config ls`.

`-g`, `--group`
:    Specify the cluster groups for the assignment. To find cluster groups, run `ibmcloud sat group ls`.

`--name`
:    Provide a name for Satellite storage assignment.

`-q`
:    Do not show the message of the day or update reminders.

`--service-cluster-id`
:    Specify the ID of the service cluster for the assignment. To find the service cluster ID, run `ibmcloud sat service ls --location <location>`.


#### Examples
{: #storage-assignment-create-examples}

Create an assignment to deploy your storage configurations to clusters in your Satellite location

```sh
ibmcloud sat storage assignment create --config CONFIG --cluster CLUSTER
```
{: pre}


### `ibmcloud sat storage assignment get`
{: #storage-assignment-get-cli}



Get the details of a Satellite storage assignment.

```sh
ibmcloud sat storage assignment get --assignment ASSIGNMENT [--output OUTPUT] [-q]
```

#### Command options
{: #storage-assignment-get-options}


`--assignment`
:    Specify the ID of a Satellite storage assignment.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-get-examples}

Get the details of a Satellite storage assignment

```sh
ibmcloud sat storage assignment get --assignment ASSIGNMENT
```
{: pre}


### `ibmcloud sat storage assignment help`
{: #storage-assignment-help-cli}



Show help

```sh
ibmcloud sat storage assignment help
```


#### Examples
{: #storage-assignment-help-examples}

Show help

```sh
ibmcloud sat storage assignment help
```
{: pre}


### `ibmcloud sat storage assignment ls`
{: #storage-assignment-ls-cli}



List the Satellite storage assignments in your IBM Cloud account.

To list all assignments for a service cluster as Service Admin: ibmcloud sat storage assignment ls --service-cluster-id CLUSTER.

To list all assignments for a service cluster as Location Admin: ibmcloud sat storage assignment ls --location LOCATION --service-cluster-id CLUSTER.

To list all assignments for a configuration: ibmcloud sat storage assignment ls --config CONFIG.

```sh
ibmcloud sat storage assignment ls [--output OUTPUT] [-q] (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
```

#### Command options
{: #storage-assignment-ls-options}


`-c`, `--cluster`
:    Specify the ID of the Satellite cluster for the assignments. To get the cluster ID, run `ibmcloud oc cluster ls --provider satellite`.

`--config`
:    Specify the name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`--location`
:    Specify the name of a Satellite location. To list available locations, run `ibmcloud sat location ls`. This option cannot be used by service administrator.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--service-cluster-id`
:    Specify the ID of the service cluster for the assignments. To find the service cluster ID, run `ibmcloud sat service ls --location <location>`.


#### Examples
{: #storage-assignment-ls-examples}

List the Satellite storage assignments in your IBM Cloud account

```sh
ibmcloud sat storage assignment ls --cluster CLUSTER
```
{: pre}


### `ibmcloud sat storage assignment patch`
{: #storage-assignment-patch-cli}



Apply storage configuration changes to the associated assignments.

```sh
ibmcloud sat storage assignment patch --assignment ASSIGNMENT [-f] [-q]
```

Aliases: `ibmcloud sat upgrade`

#### Command options
{: #storage-assignment-patch-options}


`--assignment`
:    Specify the ID of a Satellite storage assignment. To list available assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-patch-examples}

Apply storage configuration changes to the associated assignments

```sh
ibmcloud sat storage assignment patch --assignment ASSIGNMENT
```
{: pre}


### `ibmcloud sat storage assignment rm`
{: #storage-assignment-rm-cli}



Remove a Satellite storage assignment. The Kubernetes resources are deleted from all the clusters in your Satellite location, but the configuration remains.

```sh
ibmcloud sat storage assignment rm --assignment ASSIGNMENT [-f] [-q]
```

#### Command options
{: #storage-assignment-rm-options}


`--assignment`
:    Specify the ID of a Satellite storage assignment. To find assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-rm-examples}

Remove a Satellite storage assignment

```sh
ibmcloud sat storage assignment rm --assignment ASSIGNMENT
```
{: pre}


### `ibmcloud sat storage assignment update`
{: #storage-assignment-update-cli}



Update a Satellite storage assignment.

```sh
ibmcloud sat storage assignment update --assignment ASSIGNMENT [-f] [--group GROUP ...] [--name NAME] [-q]
```

#### Command options
{: #storage-assignment-update-options}


`--assignment`
:    Specify the ID of a Satellite storage assignment.

`-f`
:    Force the command to run without user prompts.

`-g`, `--group`
:    Specify the new cluster groups for the assignment. To list available groups, run `ibmcloud sat group ls`.

`--name`
:    Provide a new name for the Satellite storage assignment.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-assignment-update-examples}

Update a Satellite storage assignment

```sh
ibmcloud sat storage assignment update --assignment ASSIGNMENT
```
{: pre}


### `ibmcloud sat storage config class add`
{: #storage-config-class-add-cli}



Create a custom Satellite storage class.

```sh
ibmcloud sat storage config class add --config-name NAME --name NAME --param PARAM [--param PARAM ...] [-q]
```

#### Command options
{: #storage-config-class-add-options}


`--config-name`
:    Specify the name of the storage configuration for the custom storage class. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--name`
:    Provide a name for the custom storage class.

`-p`, `--param`
:    Specify a `key=value` pair for storage class parameters. To see the storage class parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-class-add-examples}

Create a custom Satellite storage class

```sh
ibmcloud sat storage config class add --config-name NAME --name NAME --param PARAM
```
{: pre}


### `ibmcloud sat storage config class get`
{: #storage-config-class-get-cli}



Get the details of a Satellite storage class.

```sh
ibmcloud sat storage config class get --class CLASS --config CONFIG [--output OUTPUT] [-q]
```

#### Command options
{: #storage-config-class-get-options}


`--class`
:    Specify the name of a Satellite storage class.

`--config`
:    Specify the name or ID of a Satellite storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-class-get-examples}

Get the details of a Satellite storage class

```sh
ibmcloud sat storage config class get --class CLASS --config CONFIG
```
{: pre}


### `ibmcloud sat storage config class help`
{: #storage-config-class-help-cli}



Show help

```sh
ibmcloud sat storage config class help
```


#### Examples
{: #storage-config-class-help-examples}

Show help

```sh
ibmcloud sat storage config class help
```
{: pre}


### `ibmcloud sat storage config class ls`
{: #storage-config-class-ls-cli}



List the storage classes in a Satellite storage configuration

```sh
ibmcloud sat storage config class ls --config CONFIG [--output OUTPUT] [-q] [--show-params]
```

#### Command options
{: #storage-config-class-ls-options}


`--config`
:    Specify the name or ID of a Satellite storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--show-params`
:    Include this option to list all storage class parameter details.


#### Examples
{: #storage-config-class-ls-examples}

List the storage classes in a Satellite storage configuration

```sh
ibmcloud sat storage config class ls --config CONFIG
```
{: pre}


### `ibmcloud sat storage config create`
{: #storage-config-create-cli}



Create a Satellite storage configuration to install storage drivers in your clusters.

```sh
ibmcloud sat storage config create --location LOCATION --name NAME --template-name NAME [--param PARAM ...] [-q] [--template-version VERSION]
```

#### Command options
{: #storage-config-create-options}


`--location`
:    Enter the ID or name of the location for the storage configuration. To find available locations, run `ibmcloud sat location ls`.

`--name`
:    Specify the name of the storage configuration.

`-p`, `--param`
:    Specify a `key=value` pair for configuration parameters. To see the configuration parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.

`--template-name`
:    Specify the Satellite storage configuration template name. To list available storage configuration templates, run `ibmcloud sat storage template ls`.

`--template-version`
:    Specify the Satellite storage configuration template version. If you do not include this option, the default version is used. To list available storage configuration templates, run `ibmcloud sat storage template ls`.


#### Examples
{: #storage-config-create-examples}

Create a Satellite storage configuration to install storage drivers in your clusters

```sh
ibmcloud sat storage config create --location LOCATION --name NAME --template-name NAME
```
{: pre}


### `ibmcloud sat storage config get`
{: #storage-config-get-cli}



Get the details of a Satellite storage configuration.

```sh
ibmcloud sat storage config get --config CONFIG [--output OUTPUT] [-q]
```

#### Command options
{: #storage-config-get-options}


`--config`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-get-examples}

Get the details of a Satellite storage configuration

```sh
ibmcloud sat storage config get --config CONFIG
```
{: pre}


### `ibmcloud sat storage config help`
{: #storage-config-help-cli}



Show help

```sh
ibmcloud sat storage config help
```


#### Examples
{: #storage-config-help-examples}

Show help

```sh
ibmcloud sat storage config help
```
{: pre}


### `ibmcloud sat storage config ls`
{: #storage-config-ls-cli}



List the Satellite storage configurations in your IBM Cloud account.

```sh
ibmcloud sat storage config ls [--location LOCATION] [--output OUTPUT] [-q]
```

#### Command options
{: #storage-config-ls-options}


`--location`
:    Specify the ID or name of the location that contains the configurations you want to list. To find available locations, run `ibmcloud sat location ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-ls-examples}

List the Satellite storage configurations in your IBM Cloud account

```sh
ibmcloud sat storage config ls
```
{: pre}


### `ibmcloud sat storage config param help`
{: #storage-config-param-help-cli}



Show help

```sh
ibmcloud sat storage config param help
```


#### Examples
{: #storage-config-param-help-examples}

Show help

```sh
ibmcloud sat storage config param help
```
{: pre}


### `ibmcloud sat storage config param set`
{: #storage-config-param-set-cli}



Set the configuration and secret parameters of a Satellite storage configuration.

```sh
ibmcloud sat storage config param set --config CONFIG --param PARAM [--param PARAM ...] [--apply] [-f] [-q]
```

#### Command options
{: #storage-config-param-set-options}


`--apply`
:    Apply the latest Satellite storage configuration version to all assignments of a configuration. To list a configuration's assignments, run `ibmcloud sat storage assignment ls --config CONFIG`.

`--config`
:    Specify the name or ID of the storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`-p`, `--param`
:    Specify a `key=value` pair for configuration parameters. To see the configuration parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-param-set-examples}

Set the configuration and secret parameters of a Satellite storage configuration

```sh
ibmcloud sat storage config param set --config CONFIG --param PARAM
```
{: pre}


### `ibmcloud sat storage config patch`
{: #storage-config-patch-cli}



Apply the latest patch updates to a Satellite storage configuration. Patch updates contain vulnerability remediations and bug fixes within the same major version.

```sh
ibmcloud sat storage config patch --config CONFIG [-f] [--include-assignments] [-q]
```

Aliases: `ibmcloud sat upgrade`

#### Command options
{: #storage-config-patch-options}


`--config`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--include-assignments`
:    Include this option to patch the assignments of the storage configuration to the latest configuration version.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-patch-examples}

Apply the latest patch updates to a Satellite storage configuration

```sh
ibmcloud sat storage config patch --config CONFIG
```
{: pre}


### `ibmcloud sat storage config rm`
{: #storage-config-rm-cli}



Remove a Satellite storage configuration.

```sh
ibmcloud sat storage config rm --config CONFIG [-f] [--include-assignments] [-q]
```

#### Command options
{: #storage-config-rm-options}


`--config`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--include-assignments`
:    Include this option to remove the storage configuration as well as any associated assignments.

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-config-rm-examples}

Remove a Satellite storage configuration

```sh
ibmcloud sat storage config rm --config CONFIG
```
{: pre}


### `ibmcloud sat storage help`
{: #storage-help-cli}



Show help

```sh
ibmcloud sat storage help
```


#### Examples
{: #storage-help-examples}

Show help

```sh
ibmcloud sat storage help
```
{: pre}


### `ibmcloud sat storage template get`
{: #storage-template-get-cli}



Get the details of a Satellite storage template

```sh
ibmcloud sat storage template get --name NAME --version VERSION [--output OUTPUT] [-q]
```

#### Command options
{: #storage-template-get-options}


`--name`
:    Specify the storage template name. To list available storage templates, run `ibmcloud sat storage template ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--version`
:    Specify the storage template version. To list available storage templates, run `ibmcloud sat storage template ls`.


#### Examples
{: #storage-template-get-examples}

Get the details of a Satellite storage template

```sh
ibmcloud sat storage template get --name NAME --version VERSION
```
{: pre}


### `ibmcloud sat storage template help`
{: #storage-template-help-cli}



Show help

```sh
ibmcloud sat storage template help
```


#### Examples
{: #storage-template-help-examples}

Show help

```sh
ibmcloud sat storage template help
```
{: pre}


### `ibmcloud sat storage template ls`
{: #storage-template-ls-cli}



List the available Satellite storage templates.

```sh
ibmcloud sat storage template ls [-q]
```

#### Command options
{: #storage-template-ls-options}


`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #storage-template-ls-examples}

List the available Satellite storage templates

```sh
ibmcloud sat storage template ls
```
{: pre}


## Subscription commands
{: #subscription-cli}

View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters.


### `ibmcloud sat subscription create`
{: #subscription-create-cli}



Create a Satellite subscription for clusters. After you create the subscription, the associated Satellite configuration version is automatically deployed to the subscribed clusters.

```sh
ibmcloud sat subscription create --config CONFIG --group GROUP [--group GROUP ...] --name NAME [-q] (--auth-required --gitref GITREF --gitref-type TYPE --path PATH --repository REPOSITORY | --version VERSION)
```

#### Command options
{: #subscription-create-options}


`--auth-required`
:    Provide the authentication secret required to connect to the remote repository. See [https://ibm.biz/sat-config-private-repo](https://ibm.biz/sat-config-private-repo) for details. Strategy: GitOps.

`--config`
:    Specify the name of the configuration to use for the subscription. To find available configurations, run `ibmcloud sat config ls`.

`-g`, `--group`
:    Specify the name or ID of the cluster groups to subscribe to your configuration. To find available cluster groups, run `ibmcloud sat group ls`.

`--gitref`
:    Specify the GitRef to use for the Satellite subscription. Strategy: GitOps.

`--gitref-type`
:    Indicate the type of GitRef to use for the Satellite subscription. Strategy: GitOps. Allowed values: branch, commit, tag, release

`--name`
:    Enter a name for the subscription.

`--path`
:    Provide the path to the repository files or release assets in the remote repository to use for the Satellite subscription. Strategy: GitOps.

`-q`
:    Do not show the message of the day or update reminders.

`--repository`
:    Specify the URL of the remote repository to use for the subscription. Strategy: GitOps.

`--version`
:    Indicate the name or ID of the existing configuration version to use for the subscription. To find versions, run `ibmcloud sat config get --config <configuration_name_or_ID>`. Strategy: Direct Upload.


#### Examples
{: #subscription-create-examples}

Create a Satellite subscription for clusters

```sh
ibmcloud sat subscription create \
  --config CONFIG \
  --group GROUP \
  --name NAME \
  --auth-required \
  --gitref GITREF \
  --gitref-type TYPE \
  --path PATH \
  --repository REPOSITORY
```
{: pre}


### `ibmcloud sat subscription get`
{: #subscription-get-cli}



Get detailed information for a Satellite subscription.

```sh
ibmcloud sat subscription get --subscription SUBSCRIPTION [--output OUTPUT] [-q]
```

#### Command options
{: #subscription-get-options}


`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--subscription`
:    Enter the name or ID of a Satellite subscription. To find subscriptions, run `ibmcloud sat subscription ls`.


#### Examples
{: #subscription-get-examples}

Get detailed information for a Satellite subscription

```sh
ibmcloud sat subscription get --subscription SUBSCRIPTION
```
{: pre}


### `ibmcloud sat subscription help`
{: #subscription-help-cli}



Show help

```sh
ibmcloud sat subscription help
```


#### Examples
{: #subscription-help-examples}

Show help

```sh
ibmcloud sat subscription help
```
{: pre}


### `ibmcloud sat subscription identity help`
{: #subscription-identity-help-cli}



Show help

```sh
ibmcloud sat subscription identity help
```


#### Examples
{: #subscription-identity-help-examples}

Show help

```sh
ibmcloud sat subscription identity help
```
{: pre}


### `ibmcloud sat subscription identity set`
{: #subscription-identity-set-cli}



Update the Satellite subscription to use your identity to manage resources.

```sh
ibmcloud sat subscription identity set --subscription SUBSCRIPTION [-f] [-q]
```

#### Command options
{: #subscription-identity-set-options}


`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--subscription`
:    Specify the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.


#### Examples
{: #subscription-identity-set-examples}

Update the Satellite subscription to use your identity to manage resources

```sh
ibmcloud sat subscription identity set --subscription SUBSCRIPTION
```
{: pre}


### `ibmcloud sat subscription ls`
{: #subscription-ls-cli}



List all Satellite subscriptions in your IBM Cloud account.

```sh
ibmcloud sat subscription ls [--cluster CLUSTER] [--output OUTPUT] [-q]
```

#### Command options
{: #subscription-ls-options}


`-c`, `--cluster`
:    Specify the Satellite cluster name or ID. To find registered clusters, run `ibmcloud sat cluster ls`.

`--output`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.


#### Examples
{: #subscription-ls-examples}

List all Satellite subscriptions in your IBM Cloud account

```sh
ibmcloud sat subscription ls
```
{: pre}


### `ibmcloud sat subscription rm`
{: #subscription-rm-cli}



Remove a Satellite subscription. The Kubernetes resources are no longer deployed to your clusters.

```sh
ibmcloud sat subscription rm --subscription SUBSCRIPTION [-f] [-q]
```

#### Command options
{: #subscription-rm-options}


`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--subscription`
:    Provide the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.


#### Examples
{: #subscription-rm-examples}

Remove a Satellite subscription

```sh
ibmcloud sat subscription rm --subscription SUBSCRIPTION
```
{: pre}


### `ibmcloud sat subscription update`
{: #subscription-update-cli}



Update a Satellite subscription.

```sh
ibmcloud sat subscription update --subscription SUBSCRIPTION [-f] [--group GROUP] [--name NAME] [-q] (--auth-required --gitref GITREF --gitref-type TYPE --path PATH --repository REPOSITORY | --version VERSION)
```

#### Command options
{: #subscription-update-options}


`--auth-required`
:    Provide the authentication secret required to connect to the remote repository. Strategy: GitOps.

`-f`
:    Force the command to run without user prompts.

`-g`, `--group`
:    Specify the new cluster groups to subscribe to your configuration.

`--gitref`
:    Specify the GitRef to use for the Satellite subscription. Strategy: GitOps.

`--gitref-type`
:    Indicate the type of GitRef to use for this Satellite subscription. Strategy: GitOps. Allowed values: branch, commit, tag, release

`--name`
:    Provide a new name of the Satellite subscription.

`--path`
:    Indicate the path to the repository files or release assets in the remote repository to use for the Satellite subscription. Strategy: GitOps.

`-q`
:    Do not show the message of the day or update reminders.

`--repository`
:    Provide the URL of the remote repository to use for the Satellite subscription. Strategy: GitOps.

`--subscription`
:    Specify the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.

`--version`
:    Indicate the existing configuration version to use for the Satellite subscription. Strategy: Direct Upload.


#### Examples
{: #subscription-update-examples}

Update a Satellite subscription

```sh
ibmcloud sat subscription update \
  --subscription SUBSCRIPTION \
  --auth-required \
  --gitref GITREF \
  --gitref-type TYPE \
  --path PATH \
  --repository REPOSITORY
```
{: pre}
