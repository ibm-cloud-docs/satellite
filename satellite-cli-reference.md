---

copyright:
  years: 2022
lastupdated: "2022-07-15"

keywords: satellite cli reference, satellite commands, satellite cli, satellite reference

subcollection: satellite

---


{{site.data.keyword.attribute-definition-list}}


# CLI reference for {{site.data.keyword.satelliteshort}} commands
{: #satellite-cli-reference}

Refer to these commands when you want to automate the creation and management of your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

To install the CLI, see [Setting up the CLI](/docs/satellite?topic=satellite-setup-cli). 	
{: tip}

## `ibmcloud sat` commands
{: #satellite-cli-map}

The tables below list the `ibmcloud sat` command groups. For a complete list of all `ibmcloud sat` commands as they are structured in the CLI, see the [{{site.data.keyword.satelliteshort}} CLI map](/docs/satellite?topic=satellite-icsat_map).
{: shortdesc}

| Command group | Description | 
| --- | --- |
| [Cluster commands](#sat-cluster-commands) | View and manage Satellite clusters. |
| [Cluster group commands](#cluster-group-commands)| View and manage Satellite cluster groups. |
| [Config commands](#sat-config-configuration-commands)| Create, view, and manage Satellite configurations. |
| [Endpoint commands](#sat-endpoint-commands)| Create, view, and manage Satellite endpoints. |
| [Host commands](#sat-host-commands)| View and modify Satellite host settings. |
| [Location commands](#sat-location-commands)| Create, view, and modify Satellite locations. |
| [Resource commands](#sat-resource-commands)| Search and view Kubernetes resources that are managed by Satellite. |
| [Service commands](#sat-service-commands)| View Satellite service clusters. |
| [Storage commands](#sat-storage-commands)| View and manage Satellite storage resources. |
| [Subscription commands](#sat-config-subscription-commands)| View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters. |
{: summary="The rows are read from left to right. The first column is the command group. The second column is a description of the command group."}
{: caption="{{site.data.keyword.satelliteshort}} CLI command groups" caption-side="top"}

## Cluster commands
{: #sat-cluster-commands}

Use these commands to register clusters for use with [{{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-setup-clusters-satconfig). You can use configurations to consistently deploy and update apps across clusters.
{: shortdesc}

### `ibmcloud sat cluster get`
{: #cli-cluster-get}

Get the details of a cluster that is registered with the {{site.data.keyword.satelliteshort}} Config component.
{: shortdesc}

```sh
ibmcloud sat cluster get --cluster CLUSTER [-q] [--output JSON]
```
{: pre}


#### Minimum required permissions
{: #cli-cluster-get-min-permissions}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-cluster-get-command-options}

`--cluster, -c CLUSTER`
:   Required. The name or ID of the cluster. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:   Optional. Do not show the message of the day or update reminders.

#### Example
{: #cli-cluster-get-example}

```sh
ibmcloud sat cluster get -c mycluster
```
{: pre}


### `ibmcloud sat cluster ls`
{: #cli-cluster-ls}

View a list of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component. You can use {{site.data.keyword.satelliteshort}} configurations to consistently deploy and update apps to these clusters.
{: shortdesc}

```sh
ibmcloud sat cluster ls [--filter FILTER] [--limit NUMBER] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-cluster-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-cluster-ls-command-options}

`--filter FILTER`
:    Optional. Filter results by a cluster property. Currently, the only supported cluster property is the cluster ID.

`--limit NUMBER`
:    Optional. Limit the number of clusters that are returned. The default value is 50.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-cluster-ls-example}

```sh
ibmcloud sat cluster ls
```
{: pre}



### `ibmcloud sat cluster register`
{: #cli-cluster-register}

Get a `kubectl` command to run in your cluster to install the {{site.data.keyword.satelliteshort}} Config agent. The {{site.data.keyword.satelliteshort}} Config agent is automatically installed in clusters that you run in your {{site.data.keyword.satelliteshort}} location. For all clusters that run in {{site.data.keyword.cloud_notm}}, you must use this command to install the {{site.data.keyword.satelliteshort}} Config agent so that you can include these clusters in {{site.data.keyword.satelliteshort}} configurations. After you get the command, [log in to your cluster](/docs/openshift?topic=openshift-access_cluster#access_public_se) and run the command.
{: shortdesc}

```sh
ibmcloud sat cluster register --name NAME [--location LOCATION] [--silent] [-q]
```
{: pre}



#### Minimum required permissions
{: #cli-cluster-register-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-cluster-register-command-options}

`--name NAME`
:    The name of the cluster that you want to register.

`--location LOCATION`
:    Optional. The name or ID of the Satellite location. 

`--silent`
:    Optional. Return only the registration command in the CLI output.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-cluster-register-example}

```sh
ibmcloud sat cluster register --name mycluster
```
{: pre}

### `ibmcloud sat cluster unregister`
{: #cli-cluster-unregister}

Unregister a cluster from the {{site.data.keyword.satelliteshort}} Config component. You can no longer subscribe the cluster to automatically deploy Kubernetes resources from a configuration, but the cluster and its existing resources still run.
{: shortdesc}

```sh
ibmcloud sat cluster unregister --cluster CLUSTER [-f] [-q]
```
{: pre}



#### Minimum required permissions
{: #cli-cluster-unregister-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-cluster-unregister-command-options}

`--cluster, -c CLUSTER`
:    Required. The cluster that you want to unregister from {{site.data.keyword.satelliteshort}}. To list registered clusters, run `ibmcloud sat cluster ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-cluster-unregister-example}

```sh
ibmcloud sat cluster unregister -c mycluster
```
{: pre}



## Cluster group commands
{: #cluster-group-commands}

Use these commands to create cluster groups. Then, subscribe your cluster group to a {{site.data.keyword.satelliteshort}} configuration to automatically deploy Kubernetes resources to these clusters.
{: shortdesc}

### `ibmcloud sat group attach`
{: #cluster-group-attach}

Add a {{site.data.keyword.openshiftlong_notm}} cluster to your cluster group. The cluster can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. To add a cluster that runs in {{site.data.keyword.cloud_notm}}, you must first [register the cluster](#cli-cluster-register) with the {{site.data.keyword.satelliteshort}} Config component.
{: shortdesc}

```sh
ibmcloud sat group attach --cluster CLUSTER [--cluster CLUSTER] --group GROUP [-q]
```
{: pre}



#### Minimum required permissions
{: #cluster-group-attach-min-perm}
 
 {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Cluster group** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cluster-group-attach-command-options}

`--cluster, -c CLUSTER`
:    Required. The cluster that you want to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`--group, -g GROUP`
:    Required. The name or ID of the cluster group where you want to add the cluster. To list available cluster groups, run `ibmcloud sat group ls`.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cluster-group-attach-example}

```sh
ibmcloud sat group attach --cluster mycluster --group mygroup
```
{: pre}


### `ibmcloud sat group create`
{: #cluster-group-create}

Create a cluster group. After you created the cluster group, you can subscribe the cluster group to a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```sh
ibmcloud sat group create --name NAME [--cluster CLUSTER] [-q]
```
{: pre}


#### Minimum required permissions
{: #cluster-group-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Cluster group** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cluster-group-create-command-options}

`--name NAME`
:    Required. The name for the cluster group.  

`--cluster, -c CLUSTER`
:    Optional. The name or ID of a cluster that you want to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cluster-group-create-example}

```sh
ibmcloud sat group create --name mygroup
```
{: pre}



### `ibmcloud sat group detach`
{: #cluster-group-detach}

Remove a {{site.data.keyword.openshiftlong_notm}} cluster from a cluster group.
{: shortdesc}

```sh
ibmcloud sat group detach --group GROUP --cluster CLUSTER [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cluster-group-detach-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Cluster group** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cluster-group-detach-command-options}

`--group GROUP`
:    Required. The name or ID of the cluster group where you want to remove a cluster. To list available cluster groups, run `ibmcloud sat group ls`.  

`--cluster, -c CLUSTER`
:    Optional. The name or ID of the cluster that you want to remove from your cluster group. To list the clusters that are included in your cluster group, run `ibmcloud sat group get --group <cluster_group_name_or_ID>`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cluster-group-detach-example}

```sh
ibmcloud sat group detach --group mygroup --cluster mycluster
```
{: pre}


### `ibmcloud sat group get`
{: #cluster-group-get}

Retrieve details of a cluster group, such as the clusters that are included in your cluster group.
{: shortdesc}

```sh
ibmcloud sat group get --group GROUP [-q]
```
{: pre}

#### Minimum required permissions
{: #cluster-group-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster group** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cluster-group-get-command-options}

`--group, -g GROUP`
:    Required. The name or ID of the cluster group that you want to retrieve details for. To list available cluster groups, run `ibmcloud sat group ls`.  

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cluster-group-get-example}

```sh
ibmcloud sat group get --group mygroup
```
{: pre}

### `ibmcloud sat group ls`
{: #cluster-group-ls}

List all cluster groups in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

```sh
ibmcloud sat group ls [-q]
```
{: pre}

#### Minimum required permissions
{: #cluster-group-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster group** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cluster-group-ls-command-options}

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cluster-group-ls-example}

```sh
ibmcloud sat group ls
```
{: pre}


### `ibmcloud sat group rm`
{: #cluster-group-rm}

Remove a cluster group.
{: shortdesc}

```sh
ibmcloud sat group rm --group GROUP [-f] [-q]
```
{: pre}


#### Minimum required permissions
{: #cluster-group-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Cluster group** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cluster-group-rm-command-options}

`--group, -g GROUP`
:    Required. The name or ID of the cluster group that you want to remove. To list available cluster groups, run `ibmcloud sat group ls`.  

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cluster-group-rm-example}

```sh
ibmcloud sat group rm --group mygroup
```
{: pre}

## Config commands
{: #sat-config-configuration-commands}

Use these commands to create and manage {{site.data.keyword.satelliteshort}} configurations and upload Kubernetes resource definitions as versions to the configuration. Then, use [{{site.data.keyword.satelliteshort}} subscription commands](#sat-config-subscription-commands) to specify the {{site.data.keyword.openshiftlong_notm}} clusters where you want to deploy your Kubernetes resources. For more information, see [Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-setup-clusters-satconfig).
{: shortdesc}

### `ibmcloud sat config create`
{: #cli-config-configuration-create}

Create a {{site.data.keyword.satelliteshort}} configuration. After you create a configuration, you can add Kubernetes resource definitions as versions to the configuration.
{: shortdesc}

```sh
ibmcloud sat config create --name NAME [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-create-command-options}

`--name NAME`
:    Required. The name for your configuration.

`--data-location DATA-LOCATION`
:    Optional. The location to store the data associated with the {{site.data.keyword.satelliteshort}} configuration, such as the definitions of Kubernetes resources to be deployed to your clusters. For example: `tok`. For a list of {{site.data.keyword.satelliteshort}} locations, see [Supported IBM Cloud locations](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions).

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-create-example}

```sh
ibmcloud sat config create --name myconfig
```
{: pre}

### `ibmcloud sat config get`
{: #cli-config-configuration-get}

Get the details of a {{site.data.keyword.satelliteshort}} configuration, such as the versions that you added or the {{site.data.keyword.satelliteshort}} subscriptions that are associated with the configuration.
{: shortdesc}

```sh
ibmcloud sat config get --config CONFIG [--output JSON] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-get-command-options}

`--config CONFIG`
:    Required. The name or ID of your configuration. To list available configurations, run `ibmcloud sat config ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`--q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-get-example}

```sh
ibmcloud sat config get --config myapp_prod
```
{: pre}


### `ibmcloud sat config ls`
{: #cli-config-configuration-ls}

List your {{site.data.keyword.satelliteshort}} configurations.
{: shortdesc}

```sh
ibmcloud sat config ls [--output JSON]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-ls-command-options}

`--output JSON`
:    Optional. Displays the command output in JSON format.


#### Example
{: #cli-config-configuration-ls-example}

```sh
ibmcloud sat config ls
```
{: pre}

### `ibmcloud sat config rename`
{: #cli-config-configuration-rename}

Rename a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

Clusters that are subscribed to the existing configuration name are not automatically updated. Before you rename your configuration, check which clusters are subscribed to your configuration with the `ibmcloud sat config get` command. After you rename the configuration, use the `ibmcloud sat subscription create` command to subscribe these clusters to your new configuration name.
{: note}

```sh
ibmcloud sat config rename --config CONFIG --name NEW_NAME [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-rename-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-rename-command-option}

`--config CONFIG`
:    Required. The name of the configuration that you want to change. To list available configurations, run `ibmcloud sat config ls`.

`--name NEW_NAME`
:    Required. The new name that you want to give your configuration.

`--q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-rename-example}

```sh
ibmcloud sat config rename --config myapp_prod --name myapp_staging
```
{: pre}

### `ibmcloud sat config rm`
{: #cli-config-configuration-rm}

Remove a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

Before you can remove a configuration, all associated subscriptions must be removed. To list the subscriptions that are associated with your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`. If you remove a configuration, all versions of Kubernetes resources that you added to the configuration as versions are permanently removed. Make sure that you back up these resource definitions if you plan to use them later.
{: important}

```sh
ibmcloud sat config rm --config CONFIG [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-rm-command-options}

`--config CONFIG`
:    Required. The name of your configuration that you want to remove. To list available configurations, run `ibmcloud sat config ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`--q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-rm-example}

```sh
ibmcloud sat config rm --config myapp_prod
```
{: pre}

### `ibmcloud sat config version create`
{: #cli-config-configuration-version-create}

Add a Kubernetes resource definition as a version to your {{site.data.keyword.satelliteshort}} configuration. You can later use the [`ibmcloud sat subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create) command to specify the clusters where you want to deploy the version.
{: shortdesc}

```sh
ibmcloud sat config version create --name NAME --read-config FILEPATH --config CONFIG --file-format TYPE [--description DESCRIPTION] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-version-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-version-create-command-options}

`--name NAME`
:    Required. A name for your version, such as `1.0` or `black friday`.

`--read-config FILEPATH`
:    Required. The file path to the Kubernetes resource configuration file that you want to upload as a version. You can upload only one configuration file, but the configuration file can have multiple Kubernetes resource definitions, such as a deployment, service, and security context constraint. Supported file types are YAML. Files must not exceed 3MB in size.

`--config CONFIG`
:    Required. The name of the configuration to add the version to. To list available configurations, run `ibmcloud sat config ls`.

`--file-format TYPE`
:    Required. The file extension of the Kubernetes resource file that you want to add. Supported file types are `yaml`.

`--description DESCRIPTION`
:    Optional. A description for your version, such as the purpose of the Kubernetes resources that the version contains. For example, `Added replicas and zone affinity for HA version of app for multizone clusters`.

`--q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-version-create-example}

```sh
ibmcloud sat config version create --name 1.0 --read-config ~/file/path/myapp.yaml --config myapp_prod --file-format yaml
```
{: pre}

### `ibmcloud sat config version get`
{: #cli-config-configuration-version-get}

Get the details of a particular version that you added to your {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```sh
ibmcloud sat config version get --config CONFIG --version VERSION [--output JSON] [--save-config] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-version-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-version-get-command-options}

`--config CONFIG`
:    Required. The name or ID of the configuration that the version belongs to. To list available configurations, run `ibmcloud sat config ls`.

`--version VERSION`
:    Required. The name or ID of your version. To list versions in your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`--save-config`
:    Optional. Download and save the Kubernetes resource configuration file of the version to a temporary file.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-version-get-example}

```sh
ibmcloud sat config version get --config myapp_prod --version 1.0
```
{: pre}

### `ibmcloud sat config version rm`
{: #cli-config-configuration-version-rm}

Remove a version from your {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```sh
ibmcloud sat config version rm --config CONFIG --version VERSION [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-configuration-version-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-configuration-version-rm-command-options}

`--config CONFIG`
:    Required. The name or ID of the configuration where you want to remove a version. To list available configurations, run `ibmcloud sat config ls`.

`--version VERSION`
:    Required. The name or ID of your version. To list versions in your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-configuration-version-rm-example}

```sh
ibmcloud sat config version rm --config myapp_prod --version 1.0
```
{: pre}

## Endpoint commands
{: #sat-endpoint-commands}

Use these commands to create and manage {{site.data.keyword.satelliteshort}} Link endpoints to allow network traffic between your {{site.data.keyword.satellitelong}} location and services, servers, or apps that run in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

### `ibmcloud sat endpoint create`
{: #cli-endpoint-create}

Create a {{site.data.keyword.satelliteshort}} endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint create --location LOCATION_ID --name NAME --dest-type CLOUD|LOCATION --dest-hostname FQDN_OR_IP --dest-port PORT [--dest-protocol PROTOCOL] --source-protocol PROTOCOL [--sni SNI] [--output JSON] [-q] 
```
{: pre}

#### Minimum required permissions
{: #cli-endpoint-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Link** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-endpoint-create-command-options}

`--location LOCATION_ID`
:    Required. The ID of your location. To list locations, run `ibmcloud sat location ls`.

`--name NAME`
:    Required. A name for your {{site.data.keyword.satelliteshort}} endpoint.

`--dest-type CLOUD|LOCATION`
:    Required. The place where the destination resource runs, either in {{site.data.keyword.cloud_notm}} (`cloud`) or your {{site.data.keyword.satelliteshort}} location (`location`).

`--dest-hostname FQDN_OR_IP`
:    Required. The fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `cloud` endpoints, this value must resolve to a public IP address or to a private IP address that is accessible within {{site.data.keyword.cloud_notm}} such as a private cloud service endpoint.

`--dest-port PORT`
:    Required. The port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol PROTOCOL`
:    Optional. The protocol of the destination resource. If you do not specify this option, the destination protocol is inherited from the source protocol. Supported protocols include `tcp` and `tls`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).

`--source-protocol PROTOCOL`
:    Required. The protocol that the source must use to connect to the destination resource. Supported protocols include `tcp`, `tls`, `http`, `https`, and `http-tunnel`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).

`--sni`
:    Optional. If you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake, include the server name indicator.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-endpoint-create-example}

```sh
ibmcloud sat endpoint create --location aaaaaaaa1111a1aaaa11a --name demo-svc --dest-type cloud --dest-hostname myhost.example.com --dest-port 80 --source-protocol tls --sni myhost.example.com
```
{: pre}


### `ibmcloud sat endpoint get`
{: #cli-endpoint-get}

View the details of an endpoint.
{: shortdesc}

```sh
ibmcloud sat endpoint get --endpoint ENDPOINT_ID --location LOCATION_ID [--output JSON] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-endpoint-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Link** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-endpoint-get-command-options}

`--endpoint ENDPOINT_ID`
:    Required. The ID of the endpoint. To list available endpoints, run `ibmcloud sat endpoint ls --location <location_ID>`.

`--location LOCATION_ID`
:    Required. The ID of your location. To list locations, run `ibmcloud sat location ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-endpoint-get-example}

```sh
ibmcloud sat endpoint get --endpoint aaaaaaaa1111a1aaaa11a_bb22b --location aaaaaaaa1111a1aaaa11a
```
{: pre}


### `ibmcloud sat endpoint ls`
{: #cli-endpoint-ls}

View a list of all {{site.data.keyword.satelliteshort}} endpoints for a location.
{: shortdesc}

```sh
ibmcloud sat endpoint ls --location LOCATION_ID [--output JSON] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-endpoint-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Link** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-endpoint-ls-command-options}

`--location LOCATION_ID`
:    Required. The ID of your location. To list locations, run `ibmcloud sat location ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-endpoint-ls-example}

```sh
ibmcloud sat endpoint ls --location aaaaaaaa1111a1aaaa11a
```
{: pre}

### `ibmcloud sat endpoint rm`
{: #cli-endpoint-rm}

Delete an endpoint from your location. The connection between the source and destination resources is removed.
{: shortdesc}

```sh
ibmcloud sat endpoint rm --endpoint ENDPOINT_ID --location LOCATION_ID [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-endpoint-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Link** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-endpoint-rm-command-options}

`--endpoint ENDPOINT_ID`
:    Required. The ID of the endpoint. To list endpoint, run `ibmcloud sat endpoint ls --location <location_ID>`.

`--location LOCATION_ID`
:    Required. The ID of your location. To list locations, run `ibmcloud sat location ls`.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-endpoint-rm-example}

```sh
ibmcloud sat endpoint rm --endpoint aaaaaaaa1111a1aaaa11a_bb22b --location aaaaaaaa1111a1aaaa11a
```
{: pre}


### `ibmcloud sat endpoint update`
{: #cli-endpoint-update}

Update an endpoint. Only the options that you specify are updated.
{: shortdesc}

```sh
ibmcloud sat endpoint update --location LOCATION_ID --endpoint ENDPOINT_ID [--name NAME] [--dest-hostname FQDN_OR_IP] [--dest-port PORT] [--dest-protocol PROTOCOL] [--source-protocol PROTOCOL] [--sni SNI] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-endpoint-update-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Link** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-endpoint-update-command-options}

`--location LOCATION_ID`
:    Required. The ID of your location. To list locations, run `ibmcloud sat location ls`.

`--endpoint ID`
:    Required. The ID of the endpoint. To list endpoint, run `ibmcloud sat endpoint ls`.

`--name NAME`
:    Optional. A new name for your {{site.data.keyword.satelliteshort}} endpoint.

`--dest-hostname FQDN_OR_IP`
:    Optional. The fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `cloud` endpoints, this value must resolve to a public IP address or to a private IP address that is accessible within {{site.data.keyword.cloud_notm}} such as a private cloud service endpoint.

`--dest-port PORT`
:    Optional. The port that destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol PROTOCOL`
:    Optional. The protocol of the destination resource. Supported protocols include `tcp` and `tls`. If you do not specify this option, the destination protocol is inherited from the source protocol. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).

`--source-protocol PROTOCOL`
:    Optional. The protocol that the source must use to connect to the destination resource. Supported protocols include `tcp`, `tls`, `http`, `https`, and `http-tunnel`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).

`--sni`
:    Optional. If you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake, include the server name indicator.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-endpoint-update-example}

```sh
ibmcloud sat endpoint update --location aaaaaaaa1111a1aaaa11a --endpoint aaaaaaaa1111a1aaaa11a_bb22b --name new_demo_svc --dest-hostname myupdatedhost.example.com --dest-port 8080 --source-protocol tls --sni myhost.example.com
```
{: pre}


## Host commands
{: #sat-host-commands}

### `ibmcloud sat host assign`
{: #host-assign}

Add your compute host to the {{site.data.keyword.satelliteshort}} control plane or any other {{site.data.keyword.redhat_openshift_notm}} cluster that you created in your location.
{: shortdesc}

You can't change the zone of a host while it is assigned to the control plane or to a service. If you want to change a host's zone, you must first [unassign the host from the control plane or service](/docs/satellite?topic=satellite-host-remove). Then, reassign the host to a different zone. You don't need to delete the host from the location.
{: important}

```sh
ibmcloud sat host assign --location LOCATION --cluster CLUSTER --host HOST --zone ZONE [--worker-pool WORKER_POOL] [--host-label "LABEL"] [-q]
```
{: pre}

#### Minimum required permissions
{: #host-assign-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #host-assign-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location where the {{site.data.keyword.satelliteshort}} control plane or {{site.data.keyword.redhat_openshift_notm}} cluster exists to which you want to assign the compute host. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--cluster CLUSTER`
:    Required. Enter the ID or name of the cluster where you want to assign your compute host. If you want to assign your compute host to the {{site.data.keyword.satelliteshort}} control plane, use the location ID or name. To assign the compute host to a {{site.data.keyword.redhat_openshift_notm}} cluster, use the ID or name of the cluster. To retrieve the cluster ID or name, run `ibmcloud sat cluster ls`.  

`--host HOST`
:    Required. Enter the ID of the host that you want to assign to the {{site.data.keyword.satelliteshort}} control plane or {{site.data.keyword.redhat_openshift_notm}} cluster. To retrieve the host ID, run `ibmcloud sat host ls --location <location_ID_or_name>`.  

`--zone ZONE`
:    Required. The name of the zone where you want to assign the compute host. To see the zone names for your location, run `ibmcloud sat location get --location <location_name_or_ID>` and look for the `Host Zones` field.

`--worker-pool WORKER_POOL`
:    Optional. Enter the name or ID of the worker pool in your {{site.data.keyword.redhat_openshift_notm}} cluster to which you want to add your compute host. If you want to assign hosts to your {{site.data.keyword.satelliteshort}} control plane, this option is not required. When you assign hosts to a {{site.data.keyword.redhat_openshift_notm}} cluster, you can include this option to specify the worker pool. If no worker pool is specified, the host is assigned to the default worker pool of the cluster.  

`--host-label LABEL`, `-hl LABEL`
:    Optional. Enter any labels as a key-value pair that you want to use to identify the host that you want to assign to your {{site.data.keyword.satelliteshort}} control plane or {{site.data.keyword.redhat_openshift_notm}} cluster. The first host that has this label and is in an unassigned state it automatically assigned to the control plane or cluster. To find available host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`.  

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #host-assign-example}

```sh
ibmcloud sat host assign --location aaaaaaaa1111a1aaaa11a --host myhost1 --zone us-east-1 --cluster aaaaaaaa1111a1aaaa11a --host-label "use=satloc"
```
{: pre}

### `ibmcloud sat host attach`
{: #host-attach}

Create and download a script that you run on all the compute hosts that you want to make visible to your location.
{: shortdesc}

```sh
ibmcloud sat host attach --location LOCATION [--host-label "LABEL"]  [--operating-system SYSTEM] [-q] [--reset-key]
```
{: pre}

#### Minimum required permissions
{: #host-attach-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #host-attach-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the {{site.data.keyword.satelliteshort}} location where you want to add compute hosts. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--host-label LABEL`, `-hl LABEL`
:    Optional. Enter any labels as a key-value-pair that you want to add to your compute hosts. Labels can help find hosts more easily later.

`--operating-system`
:    Optional. The operating system for the hosts you want to attach to your location. The available options are `RHEL` and `RHCOS`. For default locations without Red Hat CoreOS enabled, use `RHEL`. If no option is specified, the `RHEL` operating system is applied by default. 

`-q`
:    Optional. Do not show the message of the day or update reminders.

`--reset-key`
:    Optional. Reset the key that the control plane uses to communicate with all the hosts in the location. Then, run the script from this command to attach new hosts and all existing hosts back to the location. Until they are reattached, existing hosts have authentication errors and cannot be managed by the control plane, such as for updates.


#### Example
{: #host-attach-example}

```sh
ibmcloud sat host attach --location aaaaaaaa1111a1aaaa11a --host-label "use=satloc"
```
{: pre}


### `ibmcloud sat host get`
{: #host-get}

View the details of a compute host that was made visible to a location.
{: shortdesc}

To make a host visible to a location, you must run a script on each host machine. For more information, see the [`ibmcloud sat host attach`](#host-attach) command.
{: tip}

```sh
ibmcloud sat host get --location LOCATION --host HOST [--output json] [-q]
```
{: pre}

#### Minimum required permissions
{: #host-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #host-get-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location that the host belongs to. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--host HOST`
:    Required. Enter the ID of the host that you want to retrieve details for. To retrieve the host ID, run `ibmcloud sat host ls <location_ID_or_name>`.  

`--output json`
    :    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #host-get-example}

```sh
ibmcloud sat host get --location aaaaaaaa1111a1aaaa11a --host myhost1
```
{: pre}

### `ibmcloud sat host ls`
{: #host-ls}

List all compute hosts that are visible to your location.
{: shortdesc}

To make a host visible to a location, you must run a script on each host machine. For more information, see the [`ibmcloud sat host attach`](#host-attach) command.
{: tip}

```sh
ibmcloud sat host ls --location LOCATION [--output json] [-q]
```
{: pre}

#### Minimum required permissions
{: #host-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #host-ls-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location that you want to list compute hosts for. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--output json`
    :    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example to list all hosts in a location
{: #host-ls-example-listall}

```sh
ibmcloud sat host ls --location aaaaaaaa1111a1aaaa11a
```
{: pre}

#### Example to list available hosts in a location (unassigned)
{: #host-ls-example-list-available}

If no hosts are returned, you do not have any available hosts in the location. You can [attach more hosts](#host-attach).

```sh
ibmcloud sat host ls --location aaaaaaaa1111a1aaaa11a | grep -i unassigned
```
{: pre}


### `ibmcloud sat host rm`
{: #host-rm}

Remove a host from a location.
{: shortdesc}

```sh
ibmcloud sat host rm --location LOCATION --host HOST [-f ] [-q]
```
{: pre}

#### Minimum required permissions
{: #host-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #host-rm-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location where you want to remove a compute host. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--host HOST`
:    Required. Enter the ID of the host that you want to remove. To retrieve the host ID, run `ibmcloud sat host ls <location_ID_or_nam>`.  

`-f`
    :    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #host-rm-example}

```sh
ibmcloud sat host rm --location aaaaaaaa1111a1aaaa11a --host myhost1
```
{: pre}


### `ibmcloud sat host update`
{: #host-update}

Update information about your compute host, such as the zones and host labels that are used for [host auto assignment](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov). You can update only available hosts, not hosts that are assigned to a resource such as a cluster.
{: shortdesc}

You can't change the zone of a host while it is assigned to the control plane or to a service. If you want to change a host's zone, you must first [unassign the host from the control plane or service](/docs/satellite?topic=satellite-host-remove). Then, reassign the host to a different zone. You don't need to delete the host from the location.
{: important}

```sh
ibmcloud sat host update --location LOCATION --host HOST [--host-label "KEY=VALUE"] [--zone ZONE] [-q]
```
{: pre}

#### Minimum required permissions
{: #host-update-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #host-update-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location that the compute host is assigned to. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--host HOST`
:    Required. Enter the ID of the host that you want to update. To retrieve the host ID, run `ibmcloud sat host ls --location <location_ID_or_name>`.  

`--host-label KEY=VALUE`, `-hl KEY=VALUE`
:    Optional. Label the host with a key-value pair to use for host auto assignment. To find existing host labels, run `ibmcloud sat host get --host <host_name_or_ID> --location <location_name_or_ID>`. Repeat this option for multiple host labels.

`--zone ZONE`
:    Optional. Specify the zone that you want the host to use for auto assignment. Generally, this zone matches the zone of the infrastructure provider where the host machine is, such as a cloud provider zone or on-prem rack. To find the zones in your {{site.data.keyword.satelliteshort}} location, run `ibmcloud sat location get --location <location_name_or_ID>` and check the **Host zones**.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #host-update-example}

```sh
ibmcloud sat host update --location aaaaaaaa1111a1aaaa11a --host myhost1
```
{: pre}

## {{site.data.keyword.satelliteshort}} Config key commands
{: #config-key-commands}

View and manage {{site.data.keyword.satelliteshort}} Config keys.
{: shortdesc}

### `ibmcloud sat key ls`
{: #key-ls}

List all Satellite Config keys in your IBM Cloud account.
{: shortdesc}

```sh
ibmcloud sat key ls [--output OUTPUT] [-q]
```
{: pre}

#### Minimum required permissions
{: #key-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #key-ls-command-options}

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #key-ls-example}

```sh
ibmcloud sat key ls
```
{: pre}

### `ibmcloud sat key rm`
{: #key-rm}

Delete a {{site.data.keyword.satelliteshort}} Config key. Once the key is deleted, any cluster still using this key is unable to connect to Satellite Config.
{: shortdesc}

```sh
ibmcloud sat key rm --key KEY [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #key-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #key-rm-command-options}

`--key`
:    The name or the ID of a Satellite Config key.

`-f`
    :    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #key-rm-example}

```sh
ibmcloud sat key rm --key my_key
```
{: pre}

### `ibmcloud sat key rotate`
{: #key-rotate}

Generate a new key for use by managed clusters to connect to {{site.data.keyword.satelliteshort}} Config.
{: shortdesc}

```sh
ibmcloud sat key rotate --name NAME [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #key-rotate-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Administrator** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #key-rotate-command-options}

`--name`
:    The name of the new {{site.data.keyword.satelliteshort}} Config key.

`-f`
    :    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #key-rotate-example}

```sh
ibmcloud sat key rotate --name my_key_name
```
{: pre}

## Location commands
{: #sat-location-commands}

Use these commands to create and manage {{site.data.keyword.satelliteshort}} locations.
{: shortdesc}


### `ibmcloud sat location create`
{: #location-create}

Create a {{site.data.keyword.satelliteshort}} location. When you create a location, a location master is automatically deployed in one of the {{site.data.keyword.cloud_notm}} regions that you select during location creation. The location master is used to manage the location from the public {{site.data.keyword.cloud_notm}}.
{: shortdesc}

```sh
ibmcloud sat location create --managed-from REGION --name NAME [--cos-bucket COS_BUCKET_NAME] [--description DESCRIPTION] [--ha-zone ZONE1_NAME --ha-zone ZONE2_NAME --ha-zone ZONE3_NAME] [--coreos-enabled] [--logging-account-id LOGGING_ACCOUNT] [--provider INFRASTRUCTURE_PROVIDER] [--provider-region PROVIDER_REGION] [--provider-credential PATH_TO_PROVIDER_CREDENTIAL] [-q]
```
{: pre}


#### Minimum required permissions
{: #location-create-min-perm}

- To run this operation, {{site.data.keyword.cloud_notm}} IAM **Administrator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
- To create a location, you also need to set up [permissions to other cloud services](/docs/satellite?topic=satellite-iam#iam-roles-usecases).

#### Command options
{: #location-create-command-options}

`--managed-from REGION`
:    Required. The {{site.data.keyword.cloud_notm}} region, such as `wdc` or `lon`, that your {{site.data.keyword.satelliteshort}} location is managed from. You can use any region, but to reduce latency between {{site.data.keyword.cloud_notm}} and your location, choose the region that is closest to the compute hosts that you plan to attach to your location later. For a list of supported {{site.data.keyword.cloud_notm}} regions, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).

`--name NAME`
:    Required. Enter a name for your location. The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.

`--cos-bucket COS_BUCKET_NAME`
:    Optional. Enter the name of the {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in your {{site.data.keyword.cos_short}} instance.
:    Do not delete your {{site.data.keyword.cos_short}} instance or this bucket. If the service instance or bucket is deleted, your {{site.data.keyword.satelliteshort}} location control plane data cannot be backed up.
     {: important}

`--description DESCRIPTION`
:    Optional. A description of the Satellite location. 

`--ha-zone ZONE1_NAME --ha-zone ZONE2_NAME --ha-zone ZONE3_NAME`
:    Specify three names for high availability zones in your location. The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider.
     1. [Alibaba regions and zones](https://www.alibabacloud.com/help/en/doc-detail/188196.htm.){: external}, such as `us-east-1` and `us-west-1`.
     2. [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, `us-east-1c`.
     3. [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. Do **not** use only the location name (`eastus`) or the zone number (`1`).
     4. [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.
     
:    Optional: If you use this option, zone names must be specified in three repeated flags. If you do not use this option, the zones in your location are assigned names, such as `zone-1`.

`--coreos-enabled`

:    Optional. Enable [Red Hat CoreOS](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os) features within the {{site.data.keyword.satelliteshort}} location. This action cannot be undone.

`--logging-account-id LOGGING_ACCOUNT`
:    Optional. The {{site.data.keyword.cloud_notm}} account ID with the instance of {{site.data.keyword.la_full_notm}} that you want to forward your {{site.data.keyword.satelliteshort}} logs to. This option is available only in select environments.

`--provider INFRASTRUCTURE_PROVIDER`
:    Optional. The name of the infrastructure provider to create the {{site.data.keyword.satelliteshort}} location in. Accepted values are `aws`, `azure`, `gcp`. If you include this option, you must also include the `--provider-credential` option.

`--provider-region PROVIDER_REGION`
:    Optional. The name of the region in the infrastructure provider where you plan to create all the hosts for the {{site.data.keyword.satelliteshort}} location, such as `us-east-1` in AWS. Consult your infrastructure provider for the region name. If you include this option, you must also include the `--provider` option.

`--provider-credential PATH_TO_PROVIDER_CREDENTIAL`
:    Optional. The path to a JSON file on your local machine that has the credentials of the infrastructure provider for the {{site.data.keyword.satelliteshort}} location. The credential format is provider-specific. For more information, see [Providing {{site.data.keyword.satelliteshort}} with credentials to your infrastructure provider](/docs/satellite?topic=satellite-infrastructure-plan). If you include this option, you must also include the `--provider` option.


`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #location-create-example}

```sh
ibmcloud sat location create --managed-from wdc --name mylocation
```
{: pre}

### `ibmcloud sat location dns get`
{: #location-dns-get}

View the details of a registered subdomain in a Satellite location.
{: shortdesc}

```sh
ibmcloud sat location dns get --location LOCATION --subdomain SUBDOMAIN [--output OUTPUT] [-q]
```
{: pre}


#### Minimum required permissions
{: #location-dns-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #location-dns-get-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location that you want to retrieve DNS record for. To retrieve the location ID or name, run `ibmcloud sat location ls`.

`--subdomain SUBDOMAIN`
:    Required. Enter the ID or name of the subdomain that you want to retrieve DNS record for. To list existing subdomains, run `ibmcloud sat location dns ls`.

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #location-dns-get-example}

```sh
ibmcloud sat location dns get --location aaaaaaaa1111a1aaaa11a --subdomain s1b9782f9f75feeb8a5a4-d683ff82e51c94176a53d
```
{: pre}

### `ibmcloud sat location dns ls`
{: #location-dns-ls}

List the DNS record for your location.

```sh
ibmcloud sat location dns ls --location LOCATION [--output json] [-q]
```
{: pre}

#### Minimum required permissions
{: #location-dns-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #location-dns-ls-command-options}


`--location LOCATION`
:    Required. Enter the ID or name of the location that you want to show the DNS record for. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #location-dns-ls-example}

```sh
ibmcloud sat location dns ls --location aaaaaaaa1111a1aaaa11a
```
{: pre}


### `ibmcloud sat location dns register`
{: #location-dns-register}

Create a DNS record for your location and register the public or private IP addresses of your compute hosts that you added to the {{site.data.keyword.satelliteshort}} control plane to enable load balancing and health checking of your location.

A DNS record is automatically created for your location when you add compute hosts to your {{site.data.keyword.satelliteshort}} control plane with the public or private IP addresses that are assigned to your compute hosts. Use this command only if you want to register different public or private IP addresses for your {{site.data.keyword.satelliteshort}} control plane hosts.
{: important}

```sh
ibmcloud sat location dns register --location LOCATION --ip HOST_IP_ADDRESS [--output json] [-q]
```
{: pre}


#### Minimum required permissions
{: #location-dns-register-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #location-dns-register-command-options}

`--location LOCATION`
:    Required. Enter the name or ID of the location for which you want to create a DNS record and register the public or private IP addresses of your control plane hosts. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--ip HOST_IP_ADDRESS`
:    Required. Enter the IP address of a compute host that you added to your {{site.data.keyword.satelliteshort}} control plane. To retrieve the IP, run `ibmcloud sat host ls --location <location_ID_or_name>`ID. To register multiple IP addresses, you can use multiple `--ip` flags in the same command.   

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.



#### Example
{: #location-dns-register-example}

```sh
ibmcloud sat location dns register --location aaaaaaaa1111a1aaaa11a --ip 169.67.23.145. --ip 1669.67.23.167
```
{: pre}


### `ibmcloud sat location get`
{: #location-get}

Retrieve the details of a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

```sh
ibmcloud sat location get --location LOCATION [--output json] [-q]
```
{: pre}


#### Minimum required permissions
{: #location-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #location-get-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location that you want to retrieve details for. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #location-get-example}

```sh
ibmcloud sat location get --location aaaaaaaa1111a1aaaa11a
```


### `ibmcloud sat location ls`
{: #location-ls}

List all {{site.data.keyword.satelliteshort}} location in your account.
{: shortdesc}

```sh
ibmcloud sat location ls [--output json] [-q]
```
{: pre}

#### Minimum required permissions
{: #location-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #location-ls-command-options}


`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #location-ls-example}

```sh
ibmcloud sat location ls
```
{: pre}

### `ibmcloud sat location rm`
{: #location-rm}

Remove a {{site.data.keyword.satelliteshort}} location from your account.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts that run in the location.
{: important}

```sh
ibmcloud sat location rm --location LOCATION [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #location-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Administrator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #location-rm-command-options}


`--location LOCATION`
:    Required. Enter the ID or name of the location that you want to remove. To retrieve the location ID or name, run `ibmcloud sat location ls`.  

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #location-rm-example}

```sh
ibmcloud sat location rm --location mylocation
```
{: pre}

## Resource commands
{: #sat-resource-commands}

Use these commands to view the Kubernetes resources that run in clusters that are registered with [{{site.data.keyword.satelliteshort}} Configuration](/docs/satellite?topic=satellite-setup-clusters-satconfig).
{: shortdesc}

### `ibmcloud sat resource get`
{: #cli-resource-get}

Get the details of a Kubernetes resource that is managed by a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```sh
ibmcloud sat resource get --resource RESOURCE [--save-data] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-resource-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-resource-get-command-options}

`--resource RESOURCE`
:    Required. The ID of the Kubernetes resource. To list Kubernetes resources, run `ibmcloud sat resource ls`.

`--save-data`
:    Optional. Download and save the Kubernetes resource definition to a temporary file.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-resource-get-example}

```sh
ibmcloud sat resource get --resource 1234567
```
{: pre}

### `ibmcloud sat resource history get`
{: #cli-resource-history-get}

Get the history of a Kubernetes resource.
{: shortdesc}

```sh
ibmcloud sat resource history --resource RESOURCE [--limit LIMIT] [--output json] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-resource-history-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-resource-history-get-command-options}

`--resource RESOURCE`
:    Required. The ID of the Kubernetes resource. To list Kubernetes resources, run `ibmcloud sat resource ls`.

`--limit LIMIT`
:    Optional. The maximum number of history entries to return. 

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #cli-resource-history-get-example}

```sh
ibmcloud sat resource history get --resource 123456 --limit 200
```
{: pre}

### `ibmcloud sat resource ls`
{: #cli-resource-ls}

Search for and list Kubernetes resources that are managed by a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```sh
ibmcloud sat resource ls [--cluster CLUSTER] [--limit NUMBER] [--search STRING] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-resource-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-resource-ls-command-options}

`--cluster CLUSTER`
:    Optional. The name or ID of the registered cluster that the Kubernetes resource runs in. To list registered clusters, run `ibmcloud sat cluster ls`.

`--limit NUMBER`
:    Optional. Limit the number of Kubernetes resources that are returned. 

`--search STRING`
:    Optional. A string to filter results by, such as the name of a `pod`, `namespace`, or other string.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-resource-ls-example}

```sh
ibmcloud sat resource ls
```
{: pre}

## Service commands
{: #sat-service-commands}

Use these commands to view the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters that are deployed to your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### `ibmcloud sat service ls`
{: #cli-service-ls}

List all {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters in your location to review details such as requested host resources. For more information about how {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters request resources, see [Using host auto assignment](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov).
{: shortdesc}

```sh
ibmcloud sat service ls --location LOCATION [--output OUTPUT] [-q]
```
{: pre}


#### Minimum required permissions
{: #cli-service-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-service-ls-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location where the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters exist. To retrieve the location ID or name, run `ibmcloud sat location ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-service-ls-example}

```sh
ibmcloud sat service ls --location mylocation
```
{: pre}


## Storage commands
{: #sat-storage-commands}

Use these commands to view the storage resources that run in clusters that are registered with [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
{: shortdesc}

The `ibmcloud sat storage assignment` group of commands are available in beta.
{: beta}

Before working with your {{site.data.keyword.satelliteshort}} storage assignments and configurations, be sure to target the `Managed from` region of your {{site.data.keyword.satelliteshort}} by running `ibmcloud sat location ls` then `ibmcloud target -r <region>`.
{: note}


### `ibmcloud sat storage assignment create`
{: #cli-storage-assign-create}

Create a {{site.data.keyword.satelliteshort}} storage assignment to deploy storage drivers to your clusters.
{: shortdesc}

```sh
ibmcloud sat storage assignment create --config CONFIG (--cluster CLUSTER_ID | --group GROUP [--group GROUP ...] | --service-cluster-id CLUSTER_ID) [--name NAME] [-q]
```
{: pre}


#### Minimum required permissions
{: #cli-storage-assign-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-assign-create-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration that you want to assign to your cluster group. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`--cluster CLUSTER_ID`
:    The ID of a {{site.data.keyword.satelliteshort}} cluster. To list {{site.data.keyword.satelliteshort}} clusters, run `ibmcloud oc cluster ls --provider satellite`. To assign the storage configuration to multiple clusters at once, create a cluster group or run this command with this option multiple times. If you do not include this option, you must specify the `--group` or the `--service-cluster-id` option.

`--group GROUP`
:    The ID of the cluster group. To list {{site.data.keyword.satelliteshort}} cluster groups, run `ibmcloud sat group ls`. To assign the storage configuration to multiple cluster groups at the same time, repeat this option. If you do not include this option, you must specify the `--cluster` or the `--service-cluster-id` option.

`--service-cluster-id CLUSTER_ID`
:    The ID of a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster. To find the cluster ID, run `ibmcloud sat service ls --location <location>`. If you do not include this option, you must specify the `--cluster` or the `--group` option.

`--name NAME`
:    Optional. Enter a name for your storage assignment.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-assign-create-example}

```sh
ibmcloud sat storage assignment create --group staging --config file100 --name file100-staging
```
{: pre}



### `ibmcloud sat storage assignment get`
{: #cli-storage-assign-get}

Get the details of a {{site.data.keyword.satelliteshort}} storage assignment.
{: shortdesc}

```sh
ibmcloud sat storage assignment get --assignment ASSIGNMENT
```
{: pre}

#### Minimum required permissions
{: #cli-storage-assign-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-assign-get-command-options}

`--assignment ASSIGNMENT`
:    Required. The name of the storage assignment. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage assignment ls`.

`--output json`
:    Optional. Prints the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-assign-get-example}

```sh
ibmcloud sat storage assignment get --assignment my-assignment
```
{: pre}


### `ibmcloud sat storage assignment ls`
{: #cli-storage-assign-ls}

List your {{site.data.keyword.satelliteshort}} storage assignments.
{: shortdesc}

```sh
ibmcloud sat storage assignment ls [--output OUTPUT] (--cluster CLUSTER_ID | --config CONFIG |--location LOCATION | --service-cluster-id CLUSTER) [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-assign-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-assign-ls-command-options}

`--output OUTPUT`
:     Prints the command output in the provided format. Available options: json

`--cluster CLUSTER_ID`
:    The ID of a {{site.data.keyword.satelliteshort}} cluster that you created for which you want to list the assignments. To find the cluster ID, run `ibmcloud oc cluster ls --provider satellite`.  If you do not include this option, you must specify the `--config` option, the `--location` option, or the `--service-cluster-id`option.

`--config CONFIG`
:    The name or ID of a {{site.data.keyword.satelliteshort}} storage configuration for which you want to list the assignments. To list available storage configurations, run `ibmcloud sat storage config ls`. If you do not include this option, you must specify the `--cluster` option, the `--location` option, or the `--service-cluster-id` option.

`--location LOCATION`
:    The name of the {{site.data.keyword.satelliteshort}} location for which you want to list the assignments. To list the available locations, run `ibmcloud sat location ls`. This option is not available for service admin. If you do not include this option, you must specify the `--cluster` option, the `--config` option, or the `--service-cluster-id` option.

`--service-cluster-id CLUSTER_ID`
:    The ID of a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster for which you want to list the assignments. To find the cluster ID, run `ibmcloud sat service ls --location <location>`. If you do not include this option, you must specify the `--cluster` option,the `--config` option, or the `--location` option.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-assign-ls-example}

```sh
ibmcloud sat storage assignment ls [-q]
```
{: pre}


### `ibmcloud sat storage assignment rm`
{: #cli-storage-assign-rm}

Remove a {{site.data.keyword.satelliteshort}} storage assignment. When you remove a storage assignment from a cluster or cluster groups, the storage configuration resources such as storage drivers and storage classes are removed.
{: shortdesc}

```sh
ibmcloud sat storage assignment rm --assignment ASSIGNMENT [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-assign-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-assign-rm-command-options}

`--assignment ASSIGNMENT`
:    Required. The name of the storage assignment that you want to remove. To list storage assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-assign-rm-example}

```sh
ibmcloud sat storage assignment rm --assignment my-storage-assignment
```
{: pre}


### `ibmcloud sat storage assignment update`
{: #cli-storage-assign-update}

Update a {{site.data.keyword.satelliteshort}} storage assignment. You can use the `assignment update` command to add clusters or cluster groups to your storage assignments or change the assignment name.
{: shortdesc}

```sh
ibmcloud sat storage assignment update --assignment ASSIGNMENT [--group GROUP] [--name NAME] [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-assign-update-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-assign-update-command-options}

`--assignment ASSIGNMENT`
:    Required. The name of the storage assignment. To list storage assignments, run `ibmcloud sat storage assignment ls`.

`--group GROUP`
:    Optional. The ID of the cluster group that you want to add to your assignment. To list cluster groups, run `ibmcloud sat group ls`.

`--name NAME`
:    Optional. Enter a new name for your storage assignment.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-assign-update-example}

```sh
ibmcloud sat storage assignment update --assignment ASSIGNMENT --group GROUP --name NAME [-f] [-q]
```
{: pre}

### `ibmcloud sat storage assignment upgrade`
{: #cli-storage-assign-upgrade}

Upgrade a {{site.data.keyword.satelliteshort}} storage assignment. You can use the `assignment upgrade` command to upgrade a Satellite storage assignment to the latest storage configuration version.
{: shortdesc}

```sh
ibmcloud sat storage assignment upgrade --assignment ASSIGNMENT [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-assign-upgrade-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-assign-upgrade-command-options}

`--assignment ASSIGNMENT`
:    Required. The name of the storage assignment. To list storage assignments, run `ibmcloud sat storage assignment ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.

#### Example
{: #cli-storage-assign-upgrade-example}

```sh
ibmcloud sat storage assignment upgrade --assignment my-storage-assignment
```
{: pre}


### `ibmcloud sat storage config create`
{: #cli-storage-config-create}

Create a {{site.data.keyword.satelliteshort}} storage configuration that you can assign to your clusters to install storage drivers in your clusters.
{: shortdesc}

Before you can use storage templates and configurations to manage your storage resources across locations and clusters, make sure you [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
{: note}

```sh
ibmcloud sat storage config create --location LOCATION --name NAME --template-name NAME [--template-version VERSION] [--param PARAM ...] [-q] [--source-branch BRANCH] [--source-org ORG]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-create-command-options}

`--location LOCATION`
:    Required. Enter the ID or name of the location where you want to create the storage configuration. To retrieve the location ID or name, run `ibmcloud sat location ls`.

`--name NAME`
:    Required. Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name for your config that uses only lowercase letters, numbers, hyphens or periods.

`--template-name TEMPLATE NAME`
:    Required. Enter the name of your storage template.

`--template-version TEMPLATE VERSION`
:    Optional. Enter the version of your storage template.

`--source-org SOURCE ORG`
:    Optional. Enter the name of the GitHub organization where you forked the `ibm-satellite-storage` repo. You can use the templates in `ibm-satellite-storage` repo to test {{site.data.keyword.satelliteshort}} storage configurations in your cluster.

`--source-branch SOURCE BRANCH`
:    Optional. Enter the name of the branch in your `ibm-satellite-storage` repo that contains the storage templates that you want to use for your configuration. You can use the templates in `ibm-satellite-storage` repo to test {{site.data.keyword.satelliteshort}} storage configurations in your clusters.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-create-example}

```sh
ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-local --template-version 4.6 --source-branch main --source-org my-github-org
```
{: pre}


### `ibmcloud sat storage config get`
{: #cli-storage-config-get}

Get the details of a {{site.data.keyword.satelliteshort}} storage config.
{: shortdesc}

```sh
ibmcloud sat storage config get --config CONFIG [--output OUTPUT] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-get-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-get-example}

```sh
ibmcloud sat storage config get --config ocs-config
```
{: pre}


### `ibmcloud sat storage config ls`
{: #cli-storage-config-ls}

List your {{site.data.keyword.satelliteshort}} storage configurations.
{: shortdesc}

```sh
ibmcloud sat storage config ls [--location LOCATION] [--output OUTPUT] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-ls-command-options}

`--location LOCATION`
:    Optional. Enter the ID or name of the location where you want to list storage configurations. To retrieve the location ID or name, run `ibmcloud sat location ls`.

`--output OUTPUT`
:     Prints the command output in the provided format. Available options: json

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-ls-example-1}

```sh
ibmcloud sat storage config ls
```
{: pre}

### **Beta** `ibmcloud sat storage config param set`
{: #cli-storage-config-param-set}

Set the config and secret parameters of a Satellite storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config param set --config CONFIG --param PARAM [--param PARAM...] [--apply] [-f]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-param-set-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-param-set-command-options}

`--config CONFIG`
:    Required. The name or ID of the storage configuration that you want to set parameters for. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`--param PARAM`
:    Required. The configuration parameter you want to set. Enter the parameter in the "key=value" format. To see the configuration parameters in a storage template, run 'ibmcloud sat storage template get'.

`--apply`
:    Optional. Specify this option to apply the latest storage configuration version to all assignments that belong to the specified configuration. To list all assignments for a configuration, run 'ibmcloud sat storage assignment ls --config CONFIG'

`-f`
:    Optional. Force the command to run with no user prompts.

#### Example
{: #cli-storage-config-ls-example-2}

```sh
ibmcloud sat storage config param set --config <config_name> --param <"key=value">
```
{: pre}

### `ibmcloud sat storage config rm`
{: #cli-storage-config-rm}

Remove a {{site.data.keyword.satelliteshort}} storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config rm --config CONFIG [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-rm-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration that you want to remove. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-rm-example}

```sh
ibmcloud sat storage config rm --config ocs-config
```
{: pre}

### `ibmcloud sat storage config upgrade`
{: #cli-storage-config-upgrade}

Upgrade a {{site.data.keyword.satelliteshort}} storage configuration to the latest template revision. Template revisions, if available, include the latest storage driver patch updates.
{: shortdesc}

```sh
ibmcloud sat storage config upgrade --config CONFIG [-f] [--include-assignments] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-upgrade-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-upgrade-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration that you want to upgrade. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`--include-assignments`
:    Optional. Include this option to also update the assignments of the storage configuration to the latest configuration version.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-upgrade-example}

```sh
ibmcloud sat storage config upgrade --config ocs-config
```
{: pre}
  


### `ibmcloud sat storage config sc add`
{: #cli-storage-config-sc-add}

Add a custom storage class to a {{site.data.keyword.satelliteshort}} storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config sc add --config-name CONFIG --name NAME [--param PARAM ...] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-sc-add-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Configuration** resource type in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-sc-add-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration that you want to add a storage class to. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`--name`
:    Required. The name of the custom storage class that you want to add to your configuration.

`--param`
:    Optional. The custom parameters to provide to the storage class. Parameters are passed as KEY=VALUE pairs. You can pass multiple parameters by specifying the `--param` option multiple times.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-sc-add-example}

```sh
ibmcloud sat storage config sc add --config ocs-config --name my-sc --param key=value --param key=value
```
{: pre}


### `ibmcloud sat storage config sc get`
{: #cli-storage-config-sc-get}

Get the details of a custom storage class in a {{site.data.keyword.satelliteshort}} storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config sc get --config CONFIG --sc SC [--output OUTPUT] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-sc-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-sc-get-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration that you want to get storage class details from. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`--sc`
:    Required. The custom storage class that you want to retrieve. To list the storage classes of a {{site.data.keyword.satelliteshort}} storage configuration, run `ibmcloud sat storage config sc ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-sc-get-example}

```sh
ibmcloud sat storage config sc get --config ocs-config --sc my-sc
```
{: pre}


### `ibmcloud sat storage config sc ls`
{: #cli-storage-config-sc-ls}

List the custom storage classes in a {{site.data.keyword.satelliteshort}} storage configuration.
{: shortdesc}

```sh
ibmcloud sat storage config sc ls --config CONFIG [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-storage-config-sc-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-config-sc-ls-command-options}

`--config CONFIG`
:    Required. The name of the storage configuration that you want to retrieve storage classes from. To list {{site.data.keyword.satelliteshort}} storage configurations, run `ibmcloud sat storage config ls`.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-config-sc-ls-example}

```sh
ibmcloud sat storage config sc ls --config ocs-config
```
{: pre}


### `ibmcloud sat storage template get`
{: #cli-storage-template-get}

Get the details of a {{site.data.keyword.satelliteshort}} storage template.
{: shortdesc}

```sh
ibmcloud sat storage template get --name NAME --version VERSION
```
{: pre}

#### Minimum required permissions
{: #cli-storage-template-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-template-get-command-options}

`--name NAME`
:    Required. The name of the storage template that you want to retrieve.

`--version VERSION`
:    Required. The version of the storage template.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-template-get-example}

```sh
ibmcloud sat storage template get --name ocs --version 4.3
```
{: pre}


### `ibmcloud sat storage template ls`
{: #cli-storage-template-ls}

List your {{site.data.keyword.satelliteshort}} storage templates.
{: shortdesc}

```sh
ibmcloud sat storage template ls [-q]
```
{: pre}

You can run `ibmcloud sat storage templates` as an alias of the `ibmcloud sat storage template ls` command.
{: tip}

#### Minimum required permissions
{: #cli-storage-template-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-storage-template-ls-command-options}

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-storage-template-ls-example}

```sh
ibmcloud sat storage template ls
```
{: pre}


## Subscription commands
{: #sat-config-subscription-commands}

Use these commands to manage subscriptions to {{site.data.keyword.satelliteshort}} configurations for your registered clusters.
{: shortdesc}

You can use the `sub` alias for `subscription` commands.
{: tip}

### `ibmcloud sat subscription create`
{: #cli-config-subscription-create}

Create a subscription for {{site.data.keyword.openshiftlong_notm}} clusters to deploy a {{site.data.keyword.satelliteshort}} configuration version. After you create the subscription, the associated {{site.data.keyword.satelliteshort}} configuration version is automatically deployed to the subscribed clusters.
{: shortdesc}

```sh
ibmcloud sat subscription create --name NAME --group GROUP [--group GROUP] --config CONFIG --version VERSION [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-subscription-create-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-subscription-create-command-options}

`--name NAME`
:    Required. The name to give your subscription.

`--group GROUP`
:    Required. The name or ID of the cluster group that includes the {{site.data.keyword.openshiftlong_notm}} clusters where you want to deploy the configuration version. To list available cluster groups, run `ibmcloud sat group ls`.

`--config CONFIG`
:    Required. The name of the configuration where you added the Kubernetes resource definition as a version that you want to deploy to your clusters. To list available configurations, run `ibmcloud sat config ls`.

`--version VERSION`
:    Required. The name or ID of the version that you want to deploy to the clusters in your cluster group. To list versions in your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-subscription-create-example}

```sh
ibmcloud sat subscription create --name myapp_prod_subscription --group mygroup --config myapp_prod --version 1.0
```
{: pre}


### `ibmcloud sat subscription get`
{: #cli-config-subscription-get}

Get the details of a subscription, such as the {{site.data.keyword.satelliteshort}} configuration that the subscription is for or the registered clusters that are subscribed.
{: shortdesc}

```sh
ibmcloud sat subscription get --subscription SUBSCRIPTION [--output JSON] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-subscription-get-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-subscription-get-command-options}

`--subscription SUBSCRIPTION`
:    Required. The name or ID of your subscription. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run `ibmcloud sat subscription ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-subscription-get-example}

```sh
ibmcloud sat subscription get --subscription myapp_prod_subscription
```
{: pre}

### `ibmcloud sat subscription identity set`
{: #cli-config-subscription-identity-set}

Update a Satellite subscription to use your identity to manage resources. 
{: shortdesc}

```sh
ibmcloud sat subscription identity set --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-subscription-identity-set-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-subscription-identity-set-command-options}

`--subscription SUBSCRIPTION`
:    Required. The name or ID of your subscription. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run `ibmcloud sat subscription ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-subscription-identity-set-example}

```sh
ibmcloud sat subscription identity set --subscription my_subscription
```
{: pre}

### `ibmcloud sat subscription ls`
{: #cli-config-subscription-ls}

List the subscriptions to {{site.data.keyword.satelliteshort}} configurations in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

```sh
ibmcloud sat subscription ls [--cluster CLUSTER] [--output JSON] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-subscription-ls-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-subscription-ls-command-options}

`--cluster, -c CLUSTER`
:    Optional. The name or ID of the cluster that you want to list subscriptions from. To list registered clusters, run `ibmcloud sat cluster ls`.

`--output JSON`
:    Optional. Displays the command output in JSON format.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-subscription-ls-example}

```sh
ibmcloud sat subscription ls
```
{: pre}


### `ibmcloud sat subscription rm`
{: #cli-config-subscription-rm}

Remove a subscription.
{: shortdesc}

The Kubernetes resources for the subscription, such as pods and services, are deleted from all your subscribed clusters that are registered in {{site.data.keyword.satelliteshort}}. Before you remove a subscription, make sure that you do not need these resources. The {{site.data.keyword.satelliteshort}} configuration remains, so if you want to restore the resources, you can create another subscription.
{: important}

```sh
ibmcloud sat subscription rm --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-subscription-rm-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-subscription-rm-command-options}

`--subscription SUBSCRIPTION`
:    Required. The name or ID of your subscription. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run `ibmcloud sat subscription ls`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-subscription-rm-example}

```sh
ibmcloud sat subscription rm --subscription myapp_prod_subscription
```
{: pre}


### `ibmcloud sat subscription update`
{: #cli-config-subscription-update}

Update a subscription, such as to change the subscription name, the configuration version, or the subscribed cluster group. After you update the subscription, the configuration version is automatically deployed to all subscribed clusters.
{: shortdesc}

```sh
ibmcloud sat subscription update --subscription SUBSCRIPTION [--name NAME] [--group GROUP] [--version VERSION] [-f] [-q]
```
{: pre}

#### Minimum required permissions
{: #cli-config-subscription-update-min-perm}

{{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).

#### Command options
{: #cli-config-subscription-update-command-options}

`--subscription SUBSCRIPTION`
:    Required. The name or ID of the subscription that you want to update. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run `ibmcloud sat subscription ls`.

`--name NAME`
:    Optional. The new name to give your subscription. 

`--group GROUP`
:    Optional. The name or ID of the cluster group that you want to subscribe. To list available cluster groups, run `ibmcloud sat group ls`.

`--version VERSION`
:    Required. The name or ID of your configuration version that you want to subscribe clusters to. To list versions in your configuration, run `ibmcloud sat config get --config <configuration_name_or_ID>`.

`-f`
:    Optional. Force the command to run with no user prompts.

`-q`
:    Optional. Do not show the message of the day or update reminders.


#### Example
{: #cli-config-subscription-update-example}

```sh
ibmcloud sat subscription update --subscription myapp_prod_subscription --name myapp_staging_subscription --group mygroup --version 1.0
```
{: pre}

## Other commands
{: #other-commands}

Review other commands for managing {{site.data.keyword.satelliteshort}} resources, such as commands from other {{site.data.keyword.cloud_notm}} services that might be useful.
{: shortdesc}

### `ibmcloud sat messages`
{: #cli-messages}

View current messages from {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

```sh
ibmcloud sat messages
```
{: pre}

#### Minimum required permissions
{: #cli-messages-min-perm}

None

#### Command options
{: #cli-messages-command-options}

None

### {{site.data.keyword.openshiftlong_notm}} commands (`ibmcloud oc`)
{: #cluster-create}

For commands to manage {{site.data.keyword.redhat_openshift_notm}} clusters in your {{site.data.keyword.satelliteshort}} locations, such as `ibmcloud oc cluster create satellite`, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-kubernetes-service-cli#sat_commands).
{: shortdesc}
