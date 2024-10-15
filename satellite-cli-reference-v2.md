---


copyright:
  years: 2019, 2024
lastupdated: "2024-10-11"

keywords: satellite cli reference, satellite commands, satellite cli, satellite reference

subcollection: satellite

---



{{site.data.keyword.attribute-definition-list}}


# CLI reference for {{site.data.keyword.satelliteshort}} commands
{: #satellite-cli-reference}

Refer to these commands when you want to automate the creation and management of your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

- To install the CLI, see [Installing the the CLI](/docs/satellite?topic=satellite-cli-install).
- To view a high-level map of all the {{site.data.keyword.satellitelong_notm}} commands, see the [CLI map](/docs/satellite?topic=satellite-icsat_map).
{: tip}

## ibmcloud sat commands
{: #cli_commands}

The following tables list the `ibmcloud sat` command groups. For a complete list of all `ibmcloud sat` commands as they are structured in the CLI, see the [CLI map](/docs/satellite?topic=satellite-icsat_map).
{: shortdesc}


## `ibmcloud sat acl create`
{: #acl-create-cli}

Create an ACL.

```txt
ibmcloud sat acl create --name NAME --subnet SUBNET [--subnet SUBNET ...] [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-create-usage}

### Command options
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
{: #acl-create-options-dl}


## `ibmcloud sat acl endpoint add`
{: #acl-endpoint-add-cli}

Add one or more enabled endpoints to an ACL.

```txt
ibmcloud sat acl endpoint add --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-endpoint-add-usage}

### Command options
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
{: #acl-endpoint-add-options-dl}


## `ibmcloud sat acl endpoint ls`
{: #acl-endpoint-ls-cli}

List all enabled endpoints for an ACL.

```txt
ibmcloud sat acl endpoint ls --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-endpoint-ls-usage}

### Command options
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
{: #acl-endpoint-ls-options-dl}


## `ibmcloud sat acl endpoint rm`
{: #acl-endpoint-rm-cli}

Remove one or more enabled endpoints from an ACL.

```txt
ibmcloud sat acl endpoint rm --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-endpoint-rm-usage}

### Command options
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
{: #acl-endpoint-rm-options-dl}


## `ibmcloud sat acl get`
{: #acl-get-cli}

View the details of an ACL.

```txt
ibmcloud sat acl get --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-get-usage}

### Command options
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
{: #acl-get-options-dl}


## `ibmcloud sat acl ls`
{: #acl-ls-cli}

List all ACLs for a Satellite connector or location.

```txt
ibmcloud sat acl ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-ls-usage}

### Command options
{: #acl-ls-options}

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #acl-ls-options-dl}


## `ibmcloud sat acl rm`
{: #acl-rm-cli}

Delete an ACL.

```txt
ibmcloud sat acl rm --acl-id ID [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-rm-usage}

### Command options
{: #acl-rm-options}

`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #acl-rm-options-dl}


## `ibmcloud sat acl subnet add`
{: #acl-subnet-add-cli}

Add one or more subnets to an ACL.

```txt
ibmcloud sat acl subnet add --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-subnet-add-usage}

### Command options
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
{: #acl-subnet-add-options-dl}


## `ibmcloud sat acl subnet rm`
{: #acl-subnet-rm-cli}

Remove one or more subnets from an ACL.

```txt
ibmcloud sat acl subnet rm --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-subnet-rm-usage}

### Command options
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
{: #acl-subnet-rm-options-dl}


## `ibmcloud sat acl update`
{: #acl-update-cli}

Update the name of an ACL.

```txt
ibmcloud sat acl update --acl-id ID --name NAME [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #acl-update-usage}

### Command options
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
{: #acl-update-options-dl}


## `ibmcloud sat agent attach`
{: #agent-attach-cli}

Get a Satellite Connector Agent for a specific platform. Download the Agent `.zip` for Windows or get a link to the documentation for Docker environments.

```txt
ibmcloud sat agent attach --platform PLATFORM [-q]
```
{: pre}
{: #agent-attach-usage}

### Command options
{: #agent-attach-options}

`--platform PLATFORM`
:    The platform for the Satellite Connector Agent. For more information about Docker, see the documentation at https://ibm.biz/satconagent Available options: windows, docker

`-q`
:    Do not show the message of the day or update reminders.
{: #agent-attach-options-dl}


## `ibmcloud sat agent ls`
{: #agent-ls-cli}

List all Agents for a Satellite Connector.

```txt
ibmcloud sat agent ls --connector-id ID [--output OUTPUT] [-q]
```
{: pre}
{: #agent-ls-usage}

### Command options
{: #agent-ls-options}

`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #agent-ls-options-dl}


## `ibmcloud sat cluster get`
{: #cluster-get-cli}

Get the details of a registered cluster.

```txt
ibmcloud sat cluster get --cluster CLUSTER [--output OUTPUT] [-q]
```
{: pre}
{: #cluster-get-usage}

### Command options
{: #cluster-get-options}

`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or the ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #cluster-get-options-dl}


## `ibmcloud sat cluster ls`
{: #cluster-ls-cli}

List all registered clusters in your IBM Cloud account.

```txt
ibmcloud sat cluster ls [--filter FILTER] [--limit LIMIT] [--output OUTPUT] [-q]
```
{: pre}
{: #cluster-ls-usage}

### Command options
{: #cluster-ls-options}

`--filter FILTER`
:    Filter registered clusters by cluster ID.

`--limit LIMIT`
:    Limit the number of clusters that are returned.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #cluster-ls-options-dl}


## `ibmcloud sat cluster register`
{: #cluster-register-cli}

Get a `kubectl` command to register your cluster in a Satellite configuration. Log in to your cluster and run this command to install a Satellite Config agent. Clusters that you run in your Satellite location automatically install this agent.

```txt
ibmcloud sat cluster register --name NAME [-q] [--silent]
```
{: pre}
{: #cluster-register-usage}

### Command options
{: #cluster-register-options}

`--name NAME`
:    Specify the name of the cluster that you want to register

`-q`
:    Do not show the message of the day or update reminders.

`--silent`
:    Silent. Return only the registration command in the output.
{: #cluster-register-options-dl}


## `ibmcloud sat cluster unregister`
{: #cluster-unregister-cli}

Remove a cluster registration. The cluster is no longer subscribed to a Satellite configuration, but the cluster and its existing resources still run.

```txt
ibmcloud sat cluster unregister --cluster CLUSTER [-f] [-q]
```
{: pre}
{: #cluster-unregister-usage}

### Command options
{: #cluster-unregister-options}

`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or the ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.
{: #cluster-unregister-options-dl}


## `ibmcloud sat config create`
{: #config-create-cli}

Create a configuration to specify what Kubernetes resources you want to deploy to your clusters in your Satellite workloads.

```txt
ibmcloud sat config create --name NAME [-q] (--data-location LOCATION | --provider PROVIDER)
```
{: pre}
{: #config-create-usage}

### Command options
{: #config-create-options}

`--data-location LOCATION`
:    Specify the IBM region to store the Satellite configuration data. Strategy: Direct Upload.

`--name NAME`
:    Provide a name for the Satellite configuration.

`--provider PROVIDER`
:    Indicate the remote GitOps provider for the Satellite configuration. This provider stores the Kubernetes resource definitions. Strategy: GitOps. Allowed values: github, gitlab

`-q`
:    Do not show the message of the day or update reminders.
{: #config-create-options-dl}


## `ibmcloud sat config get`
{: #config-get-cli}

Get details of a Satellite configuration, such as the versions or subscriptions that are associated with the configuration.

```txt
ibmcloud sat config get --config CONFIG [--output OUTPUT] [-q]
```
{: pre}
{: #config-get-usage}

### Command options
{: #config-get-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #config-get-options-dl}


## `ibmcloud sat config ls`
{: #config-ls-cli}

List all Satellite configurations in your IBM Cloud account.

```txt
ibmcloud sat config ls [--output OUTPUT] [-q]
```
{: pre}
{: #config-ls-usage}

### Command options
{: #config-ls-options}

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #config-ls-options-dl}


## `ibmcloud sat config rename`
{: #config-rename-cli}

Rename a Satellite configuration.

```txt
ibmcloud sat config rename --config CONFIG --name NAME [-q]
```
{: pre}
{: #config-rename-usage}

### Command options
{: #config-rename-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`--name NAME`
:    Provide a new name for the Satellite configuration.

`-q`
:    Do not show the message of the day or update reminders.
{: #config-rename-options-dl}


## `ibmcloud sat config rm`
{: #config-rm-cli}

Remove a Satellite configuration. All associated subscriptions must be removed first. All versions are deleted. Back up any resource definitions that you want to keep.

```txt
ibmcloud sat config rm --config CONFIG [-f] [-q]
```
{: pre}
{: #config-rm-usage}

### Command options
{: #config-rm-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.
{: #config-rm-options-dl}


## `ibmcloud sat config version create`
{: #config-version-create-cli}

Create a configuration version to update existing Kubernetes resources for your Satellite workloads.

```txt
ibmcloud sat config version create --config CONFIG --file-format FORMAT --name NAME --read-config CONFIG [--description DESCRIPTION] [-q]
```
{: pre}
{: #config-version-create-usage}

### Command options
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
{: #config-version-create-options-dl}


## `ibmcloud sat config version get`
{: #config-version-get-cli}

Get details for a Satellite configuration version.

```txt
ibmcloud sat config version get --config CONFIG --version VERSION [--output OUTPUT] [-q] [--save-config]
```
{: pre}
{: #config-version-get-usage}

### Command options
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
{: #config-version-get-options-dl}


## `ibmcloud sat config version rm`
{: #config-version-rm-cli}

Remove a Satellite configuration version.

```txt
ibmcloud sat config version rm --config CONFIG --version VERSION [-f] [-q]
```
{: pre}
{: #config-version-rm-usage}

### Command options
{: #config-version-rm-options}

`--config CONFIG`
:    Specify the name or ID of the Satellite configuration. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--version VERSION`
:    Indicate the name or ID of the Satellite configuration version. To list versions, run `ibmcloud sat config get --config <configuration_name_or_ID>`.
{: #config-version-rm-options-dl}


## `ibmcloud sat connector create`
{: #connector-create-cli}

Create a Satellite connector.

```txt
ibmcloud sat connector create --name NAME --region REGION [-q]
```
{: pre}
{: #connector-create-usage}

### Command options
{: #connector-create-options}

`--name NAME`
:    The name for the Satellite connector.

`-q`
:    Do not show the message of the day or update reminders.

`--region REGION`
:    The IBM Cloud region to manage your Satellite connector.
{: #connector-create-options-dl}


## `ibmcloud sat connector get`
{: #connector-get-cli}

View the details of a Satellite Connector.

```txt
ibmcloud sat connector get --connector-id ID [--output OUTPUT] [-q]
```
{: pre}
{: #connector-get-usage}

### Command options
{: #connector-get-options}

`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #connector-get-options-dl}


## `ibmcloud sat connector ls`
{: #connector-ls-cli}

View the Satellite Connectors in your IBM Cloud account.

```txt
ibmcloud sat connector ls [--after AFTER] [--first FIRST] [--output OUTPUT] [-q]
```
{: pre}
{: #connector-ls-usage}

### Command options
{: #connector-ls-options}

`--after AFTER`
:    Show Satellite Connectors after the given cursor.

`--first FIRST`
:    View the next Satellite Connectors, up to the first number of Connectors.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #connector-ls-options-dl}


## `ibmcloud sat connector rm`
{: #connector-rm-cli}

Delete a Satellite connector.

```txt
ibmcloud sat connector rm --connector-id ID [-f] [-q]
```
{: pre}
{: #connector-rm-usage}

### Command options
{: #connector-rm-options}

`--connector-id ID`
:    The ID of a Satellite connector.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.
{: #connector-rm-options-dl}


## `ibmcloud sat endpoint authn get`
{: #endpoint-authn-get-cli}

Get the authentication settings for an endpoint.

```txt
ibmcloud sat endpoint authn get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-authn-get-usage}

### Command options
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
{: #endpoint-authn-get-options-dl}


## `ibmcloud sat endpoint authn rotate`
{: #endpoint-authn-rotate-cli}

Replace existing authentication certificates with new ones. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Only the certificates that you specify are replaced.

```txt
ibmcloud sat endpoint authn rotate --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-authn-rotate-usage}

### Command options
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
{: #endpoint-authn-rotate-options-dl}


## `ibmcloud sat endpoint authn set`
{: #endpoint-authn-set-cli}

Set authentication settings for an endpoint. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.

```txt
ibmcloud sat endpoint authn set --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [--dest-tls-mode MODE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] [--source-tls-mode MODE] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-authn-set-usage}

### Command options
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
{: #endpoint-authn-set-options-dl}


## `ibmcloud sat endpoint create`
{: #endpoint-create-cli}

Create an endpoint.

```txt
ibmcloud sat endpoint create --dest-hostname HOSTNAME --dest-port PORT --dest-type TYPE --name NAME --source-protocol PROTOCOL [--dest-protocol PROTOCOL] [--idle-timeout-seconds SECONDS] [--output OUTPUT] [-q] [--sni SNI] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-create-usage}

### Command options
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
{: #endpoint-create-options-dl}


## `ibmcloud sat endpoint disable`
{: #endpoint-disable-cli}

Disable an endpoint.

```txt
ibmcloud sat endpoint disable --endpoint ENDPOINT [-f] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-disable-usage}

### Command options
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
{: #endpoint-disable-options-dl}


## `ibmcloud sat endpoint enable`
{: #endpoint-enable-cli}

Enable an endpoint.

```txt
ibmcloud sat endpoint enable --endpoint ENDPOINT [-f] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-enable-usage}

### Command options
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
{: #endpoint-enable-options-dl}


## `ibmcloud sat endpoint get`
{: #endpoint-get-cli}

View the details of an endpoint.

```txt
ibmcloud sat endpoint get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-get-usage}

### Command options
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
{: #endpoint-get-options-dl}


## `ibmcloud sat endpoint ls`
{: #endpoint-ls-cli}

List all endpoints in a Satellite location.

```txt
ibmcloud sat endpoint ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-ls-usage}

### Command options
{: #endpoint-ls-options}

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #endpoint-ls-options-dl}


## `ibmcloud sat endpoint rm`
{: #endpoint-rm-cli}

Delete an endpoint.

```txt
ibmcloud sat endpoint rm --endpoint ENDPOINT [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-rm-usage}

### Command options
{: #endpoint-rm-options}

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--endpoint ENDPOINT`
:    Specify the name or ID of the endpoint. To list all endpoints, run `ibmcloud sat endpoint ls (--connector-id ID | --location LOCATION)`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #endpoint-rm-options-dl}


## `ibmcloud sat endpoint update`
{: #endpoint-update-cli}

Update an endpoint. Only the options that you specify are updated.

```txt
ibmcloud sat endpoint update --endpoint ENDPOINT [--dest-hostname HOSTNAME] [--dest-port PORT] [--dest-protocol PROTOCOL] [--idle-timeout-seconds SECONDS] [--name NAME] [-q] [--sni SNI] [--source-protocol PROTOCOL] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #endpoint-update-usage}

### Command options
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
{: #endpoint-update-options-dl}


## `ibmcloud sat experimental acl create`
{: #experimental-acl-create-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl create` instead] Create an ACL.

```txt
ibmcloud sat experimental acl create --name NAME --subnet SUBNET [--subnet SUBNET ...] [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-create-usage}

### Command options
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
{: #experimental-acl-create-options-dl}


## `ibmcloud sat experimental acl endpoint add`
{: #experimental-acl-endpoint-add-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl endpoint add` instead] Add one or more enabled endpoints to an ACL.

```txt
ibmcloud sat experimental acl endpoint add --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-endpoint-add-usage}

### Command options
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
{: #experimental-acl-endpoint-add-options-dl}


## `ibmcloud sat experimental acl endpoint ls`
{: #experimental-acl-endpoint-ls-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl endpoint ls` instead] List all enabled endpoints for an ACL.

```txt
ibmcloud sat experimental acl endpoint ls --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-endpoint-ls-usage}

### Command options
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
{: #experimental-acl-endpoint-ls-options-dl}


## `ibmcloud sat experimental acl endpoint rm`
{: #experimental-acl-endpoint-rm-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl endpoint rm` instead] Remove one or more enabled endpoints from an ACL.

```txt
ibmcloud sat experimental acl endpoint rm --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-endpoint-rm-usage}

### Command options
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
{: #experimental-acl-endpoint-rm-options-dl}


## `ibmcloud sat experimental acl get`
{: #experimental-acl-get-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl get` instead] View the details of an ACL.

```txt
ibmcloud sat experimental acl get --acl-id ID [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-get-usage}

### Command options
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
{: #experimental-acl-get-options-dl}


## `ibmcloud sat experimental acl ls`
{: #experimental-acl-ls-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl ls` instead] List all ACLs for a Satellite connector or location.

```txt
ibmcloud sat experimental acl ls [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-ls-usage}

### Command options
{: #experimental-acl-ls-options}

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-acl-ls-options-dl}


## `ibmcloud sat experimental acl rm`
{: #experimental-acl-rm-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl rm` instead] Delete an ACL.

```txt
ibmcloud sat experimental acl rm --acl-id ID [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-rm-usage}

### Command options
{: #experimental-acl-rm-options}

`--acl-id ID`
:    Specify the ID of the ACL. To list all ACLs, run `ibmcloud sat acl ls`.

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-acl-rm-options-dl}


## `ibmcloud sat experimental acl subnet add`
{: #experimental-acl-subnet-add-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl subnet add` instead] Add one or more subnets to an ACL.

```txt
ibmcloud sat experimental acl subnet add --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-subnet-add-usage}

### Command options
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
{: #experimental-acl-subnet-add-options-dl}


## `ibmcloud sat experimental acl subnet rm`
{: #experimental-acl-subnet-rm-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl subnet rm` instead] Remove one or more subnets from an ACL.

```txt
ibmcloud sat experimental acl subnet rm --acl-id ID --subnet SUBNET [--subnet SUBNET ...] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-subnet-rm-usage}

### Command options
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
{: #experimental-acl-subnet-rm-options-dl}


## `ibmcloud sat experimental acl update`
{: #experimental-acl-update-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat acl update` instead] Update the name of an ACL.

```txt
ibmcloud sat experimental acl update --acl-id ID --name NAME [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-acl-update-usage}

### Command options
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
{: #experimental-acl-update-options-dl}


## `ibmcloud sat experimental agent attach`
{: #experimental-agent-attach-cli}

[Deactivated on 2024-09-01! Use `ibmcloud sat agent attach` instead] Get a Satellite Connector Agent for a specific platform. Download the Agent `.zip` for Windows or get a link to the documentation for Docker environments.

```txt
ibmcloud sat experimental agent attach --platform PLATFORM [-q]
```
{: pre}
{: #experimental-agent-attach-usage}

### Command options
{: #experimental-agent-attach-options}

`--platform PLATFORM`
:    The platform for the Satellite Connector Agent. For more information about Docker, see the documentation at https://ibm.biz/satconagent Available options: windows, docker

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-agent-attach-options-dl}


## `ibmcloud sat experimental agent ls`
{: #experimental-agent-ls-cli}

[Deactivated on 2024-09-01! Use `ibmcloud sat agent ls` instead] List all Agents for a Satellite Connector.

```txt
ibmcloud sat experimental agent ls --connector-id ID [--output OUTPUT] [-q]
```
{: pre}
{: #experimental-agent-ls-usage}

### Command options
{: #experimental-agent-ls-options}

`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-agent-ls-options-dl}


## `ibmcloud sat experimental connector create`
{: #experimental-connector-create-cli}

[Expires on 2024-11-18. Use `ibmcloud sat connector create` instead] Create a Satellite connector.

```txt
ibmcloud sat experimental connector create --name NAME --region REGION [-q]
```
{: pre}
{: #experimental-connector-create-usage}

### Command options
{: #experimental-connector-create-options}

`--name NAME`
:    The name for the Satellite connector.

`-q`
:    Do not show the message of the day or update reminders.

`--region REGION`
:    The IBM Cloud region to manage your Satellite connector.
{: #experimental-connector-create-options-dl}


## `ibmcloud sat experimental connector get`
{: #experimental-connector-get-cli}

[Expires on 2024-11-18. Use `ibmcloud sat connector get` instead] View the details of a Satellite Connector.

```txt
ibmcloud sat experimental connector get --connector-id ID [--output OUTPUT] [-q]
```
{: pre}
{: #experimental-connector-get-usage}

### Command options
{: #experimental-connector-get-options}

`--connector-id ID`
:    The ID of a Satellite connector.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-connector-get-options-dl}


## `ibmcloud sat experimental connector ls`
{: #experimental-connector-ls-cli}

[Expires on 2024-11-18. Use `ibmcloud sat connector ls` instead] View the Satellite Connectors in your IBM Cloud account.

```txt
ibmcloud sat experimental connector ls [--after AFTER] [--first FIRST] [--output OUTPUT] [-q]
```
{: pre}
{: #experimental-connector-ls-usage}

### Command options
{: #experimental-connector-ls-options}

`--after AFTER`
:    Show Satellite Connectors after the given cursor.

`--first FIRST`
:    View the next Satellite Connectors, up to the first number of Connectors.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-connector-ls-options-dl}


## `ibmcloud sat experimental connector rm`
{: #experimental-connector-rm-cli}

[Expires on 2024-11-18. Use `ibmcloud sat connector rm` instead] Delete a Satellite connector.

```txt
ibmcloud sat experimental connector rm --connector-id ID [-f] [-q]
```
{: pre}
{: #experimental-connector-rm-usage}

### Command options
{: #experimental-connector-rm-options}

`--connector-id ID`
:    The ID of a Satellite connector.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-connector-rm-options-dl}


## `ibmcloud sat experimental endpoint authn get`
{: #experimental-endpoint-authn-get-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat endpoint authn get` instead] Get the authentication settings for an endpoint.

```txt
ibmcloud sat experimental endpoint authn get --endpoint ENDPOINT [--output OUTPUT] [-q] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-endpoint-authn-get-usage}

### Command options
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
{: #experimental-endpoint-authn-get-options-dl}


## `ibmcloud sat experimental endpoint authn rotate`
{: #experimental-endpoint-authn-rotate-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat endpoint authn rotate` instead] Replace existing authentication certificates with new ones. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Only the certificates that you specify are replaced.

```txt
ibmcloud sat experimental endpoint authn rotate --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-endpoint-authn-rotate-usage}

### Command options
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
{: #experimental-endpoint-authn-rotate-options-dl}


## `ibmcloud sat experimental endpoint authn set`
{: #experimental-endpoint-authn-set-cli}

[Deactivated on 2024-10-01! Use `ibmcloud sat endpoint authn set` instead] Set authentication settings for an endpoint. There are two TLS connections in the request flow. The `source` options refer to the TLS handshake between the source and the Connector service. The `destination` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.

```txt
ibmcloud sat experimental endpoint authn set --endpoint ENDPOINT [--dest-ca-cert-file FILE] [--dest-cert-file FILE] [--dest-key-file FILE] [--dest-tls-mode MODE] [-q] [--source-ca-cert-file FILE] [--source-cert-file FILE] [--source-key-file FILE] [--source-tls-mode MODE] (--connector-id ID | --location LOCATION)
```
{: pre}
{: #experimental-endpoint-authn-set-usage}

### Command options
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
{: #experimental-endpoint-authn-set-options-dl}


## `ibmcloud sat experimental location update`
{: #experimental-location-update-cli}

[Expires on 2024-11-25] Update the name or description of a Satellite location.

```txt
ibmcloud sat experimental location update --location-id ID [--description DESCRIPTION] [--name NAME] [-q]
```
{: pre}
{: #experimental-location-update-usage}

### Command options
{: #experimental-location-update-options}

`--description DESCRIPTION`
:    Enter a new description for the Satellite location. The length of the description is limited to 400 bytes.

`--location-id ID`
:    The ID of the Satellite location. To find the location ID, run `ibmcloud sat location ls`.

`--name NAME`
:    Specify a new name for the Satellite location. Location names must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be fewer than 36 characters. Do not reuse names, including names of deleted locations.

`-q`
:    Do not show the message of the day or update reminders.
{: #experimental-location-update-options-dl}


## `ibmcloud sat group attach`
{: #group-attach-cli}

Add a cluster to your cluster group. The cluster can run in your Satellite location or in IBM Cloud. To add a cluster that runs in IBM Cloud, you must first register the cluster with Satellite Config.

```txt
ibmcloud sat group attach --cluster CLUSTER [--cluster CLUSTER ...] --group GROUP [-q]
```
{: pre}
{: #group-attach-usage}

### Command options
{: #group-attach-options}

`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or ID. To list registered clusters, run `ibmcloud sat cluster ls`.

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #group-attach-options-dl}


## `ibmcloud sat group create`
{: #group-create-cli}

Create a cluster group. Then, you can subscribe the cluster group to a Satellite configuration.

```txt
ibmcloud sat group create --name NAME [--cluster CLUSTER ...] [-q]
```
{: pre}
{: #group-create-usage}

### Command options
{: #group-create-options}

`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or ID to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--name NAME`
:    Provide a name of the Satellite cluster group.

`-q`
:    Do not show the message of the day or update reminders.
{: #group-create-options-dl}


## `ibmcloud sat group detach`
{: #group-detach-cli}

Removes one or more clusters from your Satellite cluster group and deletes the Kubernetes resources that were managed by the group's subscriptions.

```txt
ibmcloud sat group detach --cluster CLUSTER [--cluster CLUSTER ...] --group GROUP [-f] [-q]
```
{: pre}
{: #group-detach-usage}

### Command options
{: #group-detach-options}

`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the cluster name or ID. To list the clusters in your cluster group, run `ibmcloud sat group get --group <cluster_group_name_or_ID>`.

`-f`
:    Force the command to run without user prompts.

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #group-detach-options-dl}


## `ibmcloud sat group get`
{: #group-get-cli}

Get detailed information for a Satellite cluster group.

```txt
ibmcloud sat group get --group GROUP [--output OUTPUT] [-q]
```
{: pre}
{: #group-get-usage}

### Command options
{: #group-get-options}

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #group-get-options-dl}


## `ibmcloud sat group ls`
{: #group-ls-cli}

List all Satellite cluster groups in your IBM Cloud account.

```txt
ibmcloud sat group ls [--output OUTPUT] [-q]
```
{: pre}
{: #group-ls-usage}

### Command options
{: #group-ls-options}

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #group-ls-options-dl}


## `ibmcloud sat group rm`
{: #group-rm-cli}

Remove a Satellite cluster group, which unsubscribes clusters and deletes the Kubernetes resources that were managed by the group's subscriptions.

```txt
ibmcloud sat group rm --group GROUP [-f] [-q]
```
{: pre}
{: #group-rm-usage}

### Command options
{: #group-rm-options}

`-f`
:    Force the command to run without user prompts.

`--group GROUP`, `-g GROUP`
:    Specify the name or ID of a Satellite cluster group. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #group-rm-options-dl}


## `ibmcloud sat host assign`
{: #host-assign-cli}

Assign a host to a Satellite location control plane or cluster.

```txt
ibmcloud sat host assign --location LOCATION [--cluster CLUSTER] [--host HOST] [--host-label LABEL ...] [-q] [--worker-pool POOL] [--zone ZONE]
```
{: pre}
{: #host-assign-usage}

### Command options
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
{: #host-assign-options-dl}


## `ibmcloud sat host attach`
{: #host-attach-cli}

Create and download a script that you can run on your hosts to attach them to your location. For RHCOS enabled locations, the script is an ignition file.

```txt
ibmcloud sat host attach --location LOCATION [--host-label LABEL ...] [--host-link-agent-endpoint ENDPOINT] [--operating-system SYSTEM] [-q] [--reset-key]
```
{: pre}
{: #host-attach-usage}

### Command options
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
{: #host-attach-options-dl}


## `ibmcloud sat host get`
{: #host-get-cli}

View the details of a Satellite host.

```txt
ibmcloud sat host get --host HOST --location LOCATION [--output OUTPUT] [-q]
```
{: pre}
{: #host-get-usage}

### Command options
{: #host-get-options}

`--host HOST`
:    The Satellite host ID. To find the host ID, run `ibmcloud sat host ls <location_ID_or_name>`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #host-get-options-dl}


## `ibmcloud sat host ls`
{: #host-ls-cli}

List all hosts that are attached to a Satellite location, including hosts that are assigned to clusters or the control plane.

```txt
ibmcloud sat host ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}
{: #host-ls-usage}

### Command options
{: #host-ls-options}

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #host-ls-options-dl}


## `ibmcloud sat host rm`
{: #host-rm-cli}

Remove a host from a Satellite location.

```txt
ibmcloud sat host rm --host HOST --location LOCATION [-f] [-q]
```
{: pre}
{: #host-rm-usage}

### Command options
{: #host-rm-options}

`-f`
:    Force the command to run without user prompts.

`--host HOST`
:    The name or ID of the host to remove.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #host-rm-options-dl}


## `ibmcloud sat host update`
{: #host-update-cli}

Update host information, such as zones and labels.

```txt
ibmcloud sat host update --host HOST --location LOCATION [--host-label LABEL ...] [-q] [--zone ZONE]
```
{: pre}
{: #host-update-usage}

### Command options
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
{: #host-update-options-dl}


## `ibmcloud sat key ls`
{: #key-ls-cli}

List all Satellite Config keys in your IBM Cloud account.

```txt
ibmcloud sat key ls [--output OUTPUT] [-q]
```
{: pre}
{: #key-ls-usage}

### Command options
{: #key-ls-options}

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #key-ls-options-dl}


## `ibmcloud sat key rm`
{: #key-rm-cli}

Remove a Satellite Config key. Any cluster that still uses this key cannot connect to Satellite Config.

```txt
ibmcloud sat key rm --key KEY [-f] [-q]
```
{: pre}
{: #key-rm-usage}

### Command options
{: #key-rm-options}

`-f`
:    Force the command to run without user prompts.

`--key KEY`
:    The name or ID of a Satellite Config key.

`-q`
:    Do not show the message of the day or update reminders.
{: #key-rm-options-dl}


## `ibmcloud sat key rotate`
{: #key-rotate-cli}

Generate a new key for use by managed clusters to connect to Satellite Config.

```txt
ibmcloud sat key rotate --name NAME [-f] [-q]
```
{: pre}
{: #key-rotate-usage}

### Command options
{: #key-rotate-options}

`-f`
:    Force the command to run without user prompts.

`--name NAME`
:    The name of the new Satellite Config key.

`-q`
:    Do not show the message of the day or update reminders.
{: #key-rotate-options-dl}


## `ibmcloud sat location create`
{: #location-create-cli}

Create a Satellite location. A Satellite location is a representation of an environment in your infrastructure provider. After you create a location, attach hosts from separate zones of your backing infrastructure environment with the `ibmcloud sat host attach` command.

```txt
ibmcloud sat location create --managed-from REGION --name NAME [--capability CAPABILITY ...] [--coreos-enabled] [--cos-bucket BUCKET] [--description DESCRIPTION] [--ha-zone ZONE ...] [--physical-address ADDRESS] [--pod-network-interface-selection SELECTION] [--pod-subnet SUBNET] [--provider PROVIDER] [--provider-credential CREDENTIAL] [--provider-region REGION] [-q] [--service-subnet SUBNET]
```
{: pre}
{: #location-create-usage}

### Command options
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
{: #location-create-options-dl}


## `ibmcloud sat location dns get`
{: #location-dns-get-cli}

View the details of a registered subdomain in a Satellite location.

```txt
ibmcloud sat location dns get --location LOCATION --subdomain SUBDOMAIN [--output OUTPUT] [-q]
```
{: pre}
{: #location-dns-get-usage}

### Command options
{: #location-dns-get-options}

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--subdomain SUBDOMAIN`
:    Specify the subdomain name. To list existing subdomains, run `ibmcloud sat location dns ls --location <ID>`.
{: #location-dns-get-options-dl}


## `ibmcloud sat location dns ls`
{: #location-dns-ls-cli}

List the registered subdomains in a Satellite location.

```txt
ibmcloud sat location dns ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}
{: #location-dns-ls-usage}

### Command options
{: #location-dns-ls-options}

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #location-dns-ls-options-dl}


## `ibmcloud sat location dns register`
{: #location-dns-register-cli}

Set a subdomain for the hosts assigned to the control plane in a Satellite location.

```txt
ibmcloud sat location dns register --ip IP [--ip IP ...] --location LOCATION [--output OUTPUT] [-q]
```
{: pre}
{: #location-dns-register-usage}

### Command options
{: #location-dns-register-options}

`--ip IP`
:    Specify the IP address for each control plane host, in the format `--ip x.x.x.1 --ip x.x.x.2 --ip x.x.x.3`. For multizone clusters, use one IP address from each zone. To find the IP address, run `ibmcloud sat host ls --location <location_ID_or_name>` and look for `Worker IP` for hosts labeled `infrastructure`.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #location-dns-register-options-dl}


## `ibmcloud sat location get`
{: #location-get-cli}

View the details of a Satellite location.

```txt
ibmcloud sat location get --location LOCATION [--output OUTPUT] [-q]
```
{: pre}
{: #location-get-usage}

### Command options
{: #location-get-options}

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #location-get-options-dl}


## `ibmcloud sat location ls`
{: #location-ls-cli}

List all Satellite locations in your IBM Cloud account.

```txt
ibmcloud sat location ls [--output OUTPUT] [-q]
```
{: pre}
{: #location-ls-usage}

### Command options
{: #location-ls-options}

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #location-ls-options-dl}


## `ibmcloud sat location rm`
{: #location-rm-cli}

Delete a location. Before you run this command, back up your configurations and remove any hosts and clusters that run in the location. The underlying host infrastructure is not automatically deleted when you delete a location. This action cannot be undone.

```txt
ibmcloud sat location rm --location LOCATION [-f] [-q]
```
{: pre}
{: #location-rm-usage}

### Command options
{: #location-rm-options}

`-f`
:    Force the command to run without user prompts.

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #location-rm-options-dl}


## `ibmcloud sat messages`
{: #messages-cli}

View the current user messages.

```txt
ibmcloud sat messages [-q]
```
{: pre}
{: #messages-usage}

### Command options
{: #messages-options}

`-q`
:    Do not show the message of the day or update reminders.
{: #messages-options-dl}


## `ibmcloud sat resource get`
{: #resource-get-cli}

View the details of a Kubernetes resource that is managed by a Satellite configuration.

```txt
ibmcloud sat resource get --resource RESOURCE [--history HISTORY] [--output OUTPUT] [-q] [--save-data]
```
{: pre}
{: #resource-get-usage}

### Command options
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
{: #resource-get-options-dl}


## `ibmcloud sat resource history get`
{: #resource-history-get-cli}

Get history for a Kubernetes resource.

```txt
ibmcloud sat resource history get --resource RESOURCE [--limit LIMIT] [--output OUTPUT] [-q]
```
{: pre}
{: #resource-history-get-usage}

### Command options
{: #resource-history-get-options}

`--limit LIMIT`
:    Specify the maximum number of history entries to return.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--resource RESOURCE`
:    The Kubernetes resource ID.
{: #resource-history-get-options-dl}


## `ibmcloud sat resource ls`
{: #resource-ls-cli}

Search Kubernetes resources that are managed by Satellite.

```txt
ibmcloud sat resource ls [--limit LIMIT] [--output OUTPUT] [-q] [--search SEARCH] (--cluster CLUSTER | --subscription SUBSCRIPTION)
```
{: pre}
{: #resource-ls-usage}

### Command options
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
{: #resource-ls-options-dl}


## `ibmcloud sat service ls`
{: #service-ls-cli}

List all Satellite service clusters in your location to review details, such as requested host resources.

```txt
ibmcloud sat service ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}
{: #service-ls-usage}

### Command options
{: #service-ls-options}

`--location LOCATION`
:    The name or ID of the Satellite location. To find the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #service-ls-options-dl}


## `ibmcloud sat storage assignment autopatch disable`
{: #storage-assignment-autopatch-disable-cli}

The `storage assignment autopatch disable` command is a beta feature.
{: beta}

Disable automatic patches for a Satellite storage assignment.

```txt
ibmcloud sat storage assignment autopatch disable --config CONFIG [-q] (--all | --assignment ASSIGNMENT)
```
{: pre}
{: #storage-assignment-autopatch-disable-usage}

### Command options
{: #storage-assignment-autopatch-disable-options}

`--all`
:    Disable automatic patches for all Satellite storage assignments of a storage configuration.

`--assignment ASSIGNMENT`
:    The ID of a Satellite storage assignment. To list available storage assignments of the configuration, run `ibmcloud sat storage assignment ls  --config CONFIG`.

`--config CONFIG`
:    The name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-assignment-autopatch-disable-options-dl}


## `ibmcloud sat storage assignment autopatch enable`
{: #storage-assignment-autopatch-enable-cli}

The `storage assignment autopatch enable` command is a beta feature.
{: beta}

Enable automatic patches for a Satellite storage assignment.

```txt
ibmcloud sat storage assignment autopatch enable --config CONFIG [-q] (--all | --assignment ASSIGNMENT)
```
{: pre}
{: #storage-assignment-autopatch-enable-usage}

### Command options
{: #storage-assignment-autopatch-enable-options}

`--all`
:    Enable automatic patches for all Satellite storage assignments of a storage configuration.

`--assignment ASSIGNMENT`
:    The ID of a Satellite storage assignment. To list available storage assignments of the configuration, run `ibmcloud sat storage assignment ls  --config CONFIG`.

`--config CONFIG`
:    The name or ID of a Satellite storage configuration. To list available storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-assignment-autopatch-enable-options-dl}


## `ibmcloud sat storage assignment create`
{: #storage-assignment-create-cli}

Create an assignment to deploy your storage configurations to clusters in your Satellite location.

```txt
ibmcloud sat storage assignment create --config CONFIG [--name NAME] [-q] (--cluster CLUSTER | --group GROUP | --service-cluster-id CLUSTER)
```
{: pre}
{: #storage-assignment-create-usage}

### Command options
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
{: #storage-assignment-create-options-dl}


## `ibmcloud sat storage assignment get`
{: #storage-assignment-get-cli}

Get the details of a Satellite storage assignment.

```txt
ibmcloud sat storage assignment get --assignment ASSIGNMENT [--output OUTPUT] [-q]
```
{: pre}
{: #storage-assignment-get-usage}

### Command options
{: #storage-assignment-get-options}

`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-assignment-get-options-dl}


## `ibmcloud sat storage assignment ls`
{: #storage-assignment-ls-cli}

List the Satellite storage assignments in your IBM Cloud account.

To list all assignments for a service cluster as Service Admin: ibmcloud sat storage assignment ls --service-cluster-id CLUSTER.

To list all assignments for a service cluster as Location Admin: ibmcloud sat storage assignment ls --location LOCATION --service-cluster-id CLUSTER.

To list all assignments for a configuration: ibmcloud sat storage assignment ls --config CONFIG.

```txt
ibmcloud sat storage assignment ls [--output OUTPUT] [-q] (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
```
{: pre}
{: #storage-assignment-ls-usage}

### Command options
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
{: #storage-assignment-ls-options-dl}


## `ibmcloud sat storage assignment patch`
{: #storage-assignment-patch-cli}

Apply storage configuration changes to the associated assignments.

```txt
ibmcloud sat storage assignment patch --assignment ASSIGNMENT [-f] [-q]
```
{: pre}
{: #storage-assignment-patch-usage}

### Command options
{: #storage-assignment-patch-options}

`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment. To list available assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-assignment-patch-options-dl}


## `ibmcloud sat storage assignment rm`
{: #storage-assignment-rm-cli}

Remove a Satellite storage assignment. The Kubernetes resources are deleted from all the clusters in your Satellite location, but the configuration remains.

```txt
ibmcloud sat storage assignment rm --assignment ASSIGNMENT [-f] [-q]
```
{: pre}
{: #storage-assignment-rm-usage}

### Command options
{: #storage-assignment-rm-options}

`--assignment ASSIGNMENT`
:    Specify the ID of a Satellite storage assignment. To find assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-assignment-rm-options-dl}


## `ibmcloud sat storage assignment update`
{: #storage-assignment-update-cli}

Update a Satellite storage assignment.

```txt
ibmcloud sat storage assignment update --assignment ASSIGNMENT [-f] [--group GROUP ...] [--name NAME] [-q]
```
{: pre}
{: #storage-assignment-update-usage}

### Command options
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
{: #storage-assignment-update-options-dl}


## `ibmcloud sat storage config class add`
{: #storage-config-class-add-cli}

Create a custom Satellite storage class.

```txt
ibmcloud sat storage config class add --config-name NAME --name NAME --param PARAM [--param PARAM ...] [-q]
```
{: pre}
{: #storage-config-class-add-usage}

### Command options
{: #storage-config-class-add-options}

`--config-name NAME`
:    Specify the name of the storage configuration for the custom storage class. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--name NAME`
:    Provide a name for the custom storage class.

`--param PARAM`, `-p PARAM`
:    Specify a `key=value` pair for storage class parameters. To see the storage class parameters in a storage template, run `ibmcloud sat storage template get`.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-config-class-add-options-dl}


## `ibmcloud sat storage config class get`
{: #storage-config-class-get-cli}

Get the details of a Satellite storage class.

```txt
ibmcloud sat storage config class get --class CLASS --config CONFIG [--output OUTPUT] [-q]
```
{: pre}
{: #storage-config-class-get-usage}

### Command options
{: #storage-config-class-get-options}

`--class CLASS`
:    Specify the name of a Satellite storage class.

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-config-class-get-options-dl}


## `ibmcloud sat storage config class ls`
{: #storage-config-class-ls-cli}

List the storage classes in a Satellite storage configuration

```txt
ibmcloud sat storage config class ls --config CONFIG [--output OUTPUT] [-q] [--show-params]
```
{: pre}
{: #storage-config-class-ls-usage}

### Command options
{: #storage-config-class-ls-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list Satellite storage configurations, run `ibmcloud sat storage config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--show-params`
:    Include this option to list all storage class parameter details.
{: #storage-config-class-ls-options-dl}


## `ibmcloud sat storage config create`
{: #storage-config-create-cli}

Create a Satellite storage configuration to install storage drivers in your clusters.

```txt
ibmcloud sat storage config create --location LOCATION --name NAME --template-name NAME [--param PARAM ...] [-q] [--template-version VERSION]
```
{: pre}
{: #storage-config-create-usage}

### Command options
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
{: #storage-config-create-options-dl}


## `ibmcloud sat storage config get`
{: #storage-config-get-cli}

Get the details of a Satellite storage configuration.

```txt
ibmcloud sat storage config get --config CONFIG [--output OUTPUT] [-q]
```
{: pre}
{: #storage-config-get-usage}

### Command options
{: #storage-config-get-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-config-get-options-dl}


## `ibmcloud sat storage config ls`
{: #storage-config-ls-cli}

List the Satellite storage configurations in your IBM Cloud account.

```txt
ibmcloud sat storage config ls [--location LOCATION] [--output OUTPUT] [-q]
```
{: pre}
{: #storage-config-ls-usage}

### Command options
{: #storage-config-ls-options}

`--location LOCATION`
:    Specify the ID or name of the location that contains the configurations you want to list. To find available locations, run `ibmcloud sat location ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-config-ls-options-dl}


## `ibmcloud sat storage config param set`
{: #storage-config-param-set-cli}

Set the configuration and secret parameters of a Satellite storage configuration.

```txt
ibmcloud sat storage config param set --config CONFIG --param PARAM [--param PARAM ...] [--apply] [-f] [-q]
```
{: pre}
{: #storage-config-param-set-usage}

### Command options
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
{: #storage-config-param-set-options-dl}


## `ibmcloud sat storage config patch`
{: #storage-config-patch-cli}

Apply the latest patch updates to a Satellite storage configuration. Patch updates contain vulnerability remediations and bug fixes within the same major version.

```txt
ibmcloud sat storage config patch --config CONFIG [-f] [--include-assignments] [-q]
```
{: pre}
{: #storage-config-patch-usage}

### Command options
{: #storage-config-patch-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--include-assignments`
:    Include this option to patch the assignments of the storage configuration to the latest configuration version.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-config-patch-options-dl}


## `ibmcloud sat storage config rm`
{: #storage-config-rm-cli}

Remove a Satellite storage configuration.

```txt
ibmcloud sat storage config rm --config CONFIG [-f] [--include-assignments] [-q]
```
{: pre}
{: #storage-config-rm-usage}

### Command options
{: #storage-config-rm-options}

`--config CONFIG`
:    Specify the name or ID of a Satellite storage configuration. To list available configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Force the command to run without user prompts.

`--include-assignments`
:    Include this option to remove the storage configuration as well as any associated assignments.

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-config-rm-options-dl}


## `ibmcloud sat storage template get`
{: #storage-template-get-cli}

Get the details of a Satellite storage template

```txt
ibmcloud sat storage template get --name NAME --version VERSION [--output OUTPUT] [-q]
```
{: pre}
{: #storage-template-get-usage}

### Command options
{: #storage-template-get-options}

`--name NAME`
:    Specify the storage template name. To list available storage templates, run `ibmcloud sat storage template ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--version VERSION`
:    Specify the storage template version. To list available storage templates, run `ibmcloud sat storage template ls`.
{: #storage-template-get-options-dl}


## `ibmcloud sat storage template ls`
{: #storage-template-ls-cli}

List the available Satellite storage templates.

```txt
ibmcloud sat storage template ls [-q]
```
{: pre}
{: #storage-template-ls-usage}

### Command options
{: #storage-template-ls-options}

`-q`
:    Do not show the message of the day or update reminders.
{: #storage-template-ls-options-dl}


## `ibmcloud sat subscription create`
{: #subscription-create-cli}

Create a Satellite subscription for clusters. After you create the subscription, the associated Satellite configuration version is automatically deployed to the subscribed clusters.

```txt
ibmcloud sat subscription create --config CONFIG --group GROUP [--group GROUP ...] --name NAME [-q] (--auth-required --gitref GITREF --gitref-type TYPE --path PATH --repository REPOSITORY | --version VERSION)
```
{: pre}
{: #subscription-create-usage}

### Command options
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
{: #subscription-create-options-dl}


## `ibmcloud sat subscription get`
{: #subscription-get-cli}

Get detailed information for a Satellite subscription.

```txt
ibmcloud sat subscription get --subscription SUBSCRIPTION [--output OUTPUT] [-q]
```
{: pre}
{: #subscription-get-usage}

### Command options
{: #subscription-get-options}

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.

`--subscription SUBSCRIPTION`
:    Enter the name or ID of a Satellite subscription. To find subscriptions, run `ibmcloud sat subscription ls`.
{: #subscription-get-options-dl}


## `ibmcloud sat subscription identity set`
{: #subscription-identity-set-cli}

Update the Satellite subscription to use your identity to manage resources.

```txt
ibmcloud sat subscription identity set --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}
{: #subscription-identity-set-usage}

### Command options
{: #subscription-identity-set-options}

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--subscription SUBSCRIPTION`
:    Specify the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.
{: #subscription-identity-set-options-dl}


## `ibmcloud sat subscription ls`
{: #subscription-ls-cli}

List all Satellite subscriptions in your IBM Cloud account.

```txt
ibmcloud sat subscription ls [--cluster CLUSTER] [--output OUTPUT] [-q]
```
{: pre}
{: #subscription-ls-usage}

### Command options
{: #subscription-ls-options}

`--cluster CLUSTER`, `-c CLUSTER`
:    Specify the Satellite cluster name or ID. To find registered clusters, run `ibmcloud sat cluster ls`.

`--output OUTPUT`
:    Prints the command output in the provided format. Accepted values: `json`

`-q`
:    Do not show the message of the day or update reminders.
{: #subscription-ls-options-dl}


## `ibmcloud sat subscription rm`
{: #subscription-rm-cli}

Remove a Satellite subscription. The Kubernetes resources are no longer deployed to your clusters.

```txt
ibmcloud sat subscription rm --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}
{: #subscription-rm-usage}

### Command options
{: #subscription-rm-options}

`-f`
:    Force the command to run without user prompts.

`-q`
:    Do not show the message of the day or update reminders.

`--subscription SUBSCRIPTION`
:    Provide the name or ID of a Satellite subscription. To list subscriptions, run `ibmcloud sat subscription ls`.
{: #subscription-rm-options-dl}


## `ibmcloud sat subscription update`
{: #subscription-update-cli}

Update a Satellite subscription.

```txt
ibmcloud sat subscription update --subscription SUBSCRIPTION [-f] [--group GROUP] [--name NAME] [-q] (--auth-required --gitref GITREF --gitref-type TYPE --path PATH --repository REPOSITORY | --version VERSION)
```
{: pre}
{: #subscription-update-usage}

### Command options
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
{: #subscription-update-options-dl}

  
