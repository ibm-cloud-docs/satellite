---

copyright:
  years: 2020
lastupdated: "2020-09-17"

keywords: satellite cli reference, satellite commands, satellite cli, satellite reference

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# {{site.data.keyword.satelliteshort}} CLI command reference
{: #satellite-cli-reference}

Refer to these commands when you want to automate the creation and management of your {{site.data.keyword.satelliteshort}} location. 	
{: shortdesc}

To install the CLI, see [Setting up the CLI](/docs/satellite?topic=satellite-setup-cli). 	
{: tip}

## `ibmcloud sat` commands
{: #satellite-cli-map}

The following image depicts the structure and grouping of the `ibmcloud sat` commands.
{: shortdesc}

![Image of the structure and groupings of commands in the `ibmcloud sat` CLI plug-in.](images/cli_ref_imagemap.png)

## Config configuration commands
{: #sat-config-configuration-commands}

Use these commands to create and manage {{site.data.keyword.satelliteshort}} configurations and upload Kubernetes resource definitions as versions to the configuration. Then, use {{site.data.keyword.satelliteshort}} subscriptions to specify the {{site.data.keyword.openshiftlong_notm}} clusters where you want to deploy your Kubernetes resources. For more information, see [Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config).
{: shortdesc}

### `ibmcloud sat config configuration create`
{: #cli-config-configuration-create}

Create a {{site.data.keyword.satelliteshort}} configuration. After you create a configuration, you can add Kubernetes resource definitions as versions to the configuration.
{: shortdesc}

```
ibmcloud sat config configuration create --name NAME [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--name <em>NAME</em></code></dt>
<dd>Required. The name for your configuration.</dd>
<dt><code>-q </code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration create --name myconfig
```
{: pre}

<br />


### `ibmcloud sat config configuration get`
{: #cli-config-configuration-get}

Get the details of a {{site.data.keyword.satelliteshort}} configuration, such as the versions that you added or the {{site.data.keyword.satelliteshort}} subscriptions that are associated with the configuration.
{: shortdesc}

```
ibmcloud sat config configuration get --configuration CONFIGURATION [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--configuration <em>CONFIGURATION</em></code></dt>
<dd>Required. The name or ID of your configuration. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>
<dt><code>--q </code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration get --configuration myapp_prod
```
{: pre}

<br />


### `ibmcloud sat config configuration ls`
{: #cli-config-configuration-ls}

List your {{site.data.keyword.satelliteshort}} configurations.
{: shortdesc}

```
ibmcloud sat config configuration ls
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options**: N/A

**Example:**
```
ibmcloud sat config configuration ls
```
{: pre}

<br />


### `ibmcloud sat config configuration rename`
{: #cli-config-configuration-rename}

Rename a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

Clusters that are subscribed to the existing configuration name are not automatically updated. Before you rename your configuration, check which clusters are subscribed to your configuration with the `ibmcloud sat config configuration get` command. After you rename the configuration, use the `ibmcloud sat config subscription create` command to subscribe these clusters to your new configuration name.
{: note}

```
ibmcloud sat config configuration rename --configuration CONFIGURATION --name NEW_NAME [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--configuration <em>CONFIGURATION</em></code></dt>
<dd>Required. The name of the configuration that you want to change. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>

<dt><code>--name <em>NEW_NAME</em></code></dt>
<dd>Required. The new name that you want to give your configuration.</dd>

<dt><code>--q </code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration rename --configuration myapp_prod --name myapp_staging
```
{: pre}

<br />


### `ibmcloud sat config configuration rm`
{: #cli-config-configuration-rm}

Remove a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

Before you can remove a configuration, all associated subscriptions must be removed. To list the subscriptions that are associated with your configuration, run `ibmcloud sat config configuration get --configuration <configuration_name_or_ID>`. If you remove a configuration, all versions of Kubernetes resources that you added to the configuration as versions are permanently removed. Make sure that you back up these resource definitions if you plan to use them later.
{: important}

```
ibmcloud sat config configuration rm --configuration CONFIGURATION [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--configuration <em>CONFIGURATION</em></code></dt>
<dd>Required. The name of your configuration that you want to remove. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>--q </code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration rm --configuration myapp_prod
```
{: pre}

<br />


### `ibmcloud sat config configuration version create`
{: #cli-config-configuration-version-create}

Add a Kubernetes resource definition as a version to your {{site.data.keyword.satelliteshort}} configuration. You can later use the [`ibmcloud sat config subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create) command to specify the clusters where you want to deploy the version.
{: shortdesc}

```
ibmcloud sat config configuration version create --name NAME --read-config FILEPATH --configuration CONFIGURATION --type TYPE [--description DESCRIPTION] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--name <em>NAME</em></code></dt>
<dd>Required. A name for your version, such as `1.0` or `black friday`.</dd>

<dt><code>--read-config <em>FILEPATH</em></code></dt>
<dd>Required. The file path to the Kubernetes resource configuration file that you want to upload as a version. You can upload only one configuration file, but the configuration file can have multiple Kubernetes resource definitions, such as a deployment, service, and security context constraint. Supported file types are YAML.</dd>

<dt><code>--configuration <em>configuration</em></code></dt>
<dd>Required. The name of the configuration to add the version to. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>

<dt><code>--type <em>TYPE</em></code></dt>
<dd>Required. The file extension of the Kubernetes resource file that you want to add. Supported file types are <code>yaml</code>.</dd>

<dt><code>--description <em>DESCRIPTION</em></code></dt>
<dd>Optional. A description for your version, such as the purpose of the Kubernetes resources that the version contains. For example, `Added replicas and zone affinity for HA version of app for multizone clusters`.</dd>

<dt><code>--q </code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration version create --name 1.0 --read-config ~/file/path/myapp.yaml --configuration myapp_prod --type yaml
```
{: pre}

<br />


### `ibmcloud sat config configuration version get`
{: #cli-config-configuration-version-get}

Get the details of a particular version that you added to your {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```
ibmcloud sat config configuration version get --configuration CONFIGURATION --version VERSION [--save-config] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--configuration <em>CONFIGURATION</em></code></dt>
<dd>Required. The name or ID of the configuration that the version belongs to. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Required. The name or ID of your version. To list versions in your configuration, run <code>ibmcloud sat config configuration get --configuration &lt;configuration_name_or_ID&gt;</code>.</dd>

<dt><code>--save-config</code></dt>
<dd>Optional. Download and save the Kubernetes resource configuration file of the version to a temporary file.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration version get --configuration myapp_prod --version 1.0
```
{: pre}

<br />


### `ibmcloud sat config configuration version rm`
{: #cli-config-configuration-version-rm}

Remove a version from your {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```
ibmcloud sat config configuration version rm --configuration CONFIGURATION --version VERSION [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Configuration** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--configuration <em>CONFIGURATION</em></code></dt>
<dd>Required. The name or ID of the configuration where you want to remove a version. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Required. The name or ID of your version. To list versions in your configuration, run <code>ibmcloud sat config configuration get --configuration &lt;configuration_name_or_ID&gt;</code>.</dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config configuration version rm --configuration myapp_prod --version 1.0
```
{: pre}

<br />


## Config subscription commands
{: #sat-config-subscription-commands}

Use these commands to manage subscriptions to {{site.data.keyword.satelliteshort}} configurations for your registered clusters.
{: shortdesc}

### `ibmcloud sat config subscription create`
{: #cli-config-subscription-create}

Create a subscription for {{site.data.keyword.openshiftlong_notm}} clusters to deploy a {{site.data.keyword.satelliteshort}} configuration version. After you create the subscription, the associated {{site.data.keyword.satelliteshort}} configuration version is automatically deployed to the subscribed clusters.
{: shortdesc}

```
ibmcloud sat config subscription create --name NAME --cluster-group CLUSTERGROUP [--cluster-group CLUSTERGROUP] --configuration CONFIGURATION --version VERSION [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--name <em>NAME</em></code></dt>
<dd>Required. The name to give your subscription.</dd>

<dt><code>--cluster-group <em>CLUSTERGROUP</em></code></dt>
<dd>Required. The name or ID of the cluster group that includes the {{site.data.keyword.openshiftlong_notm}} clusters where you want to deploy the configuration version. To list available cluster groups, run <code>ibmcloud sat cluster-group ls</code>.</dd>

<dt><code>--configuration <em>CONFIGURATION</em></code></dt>
<dd>Required. The name of the configuration where you added the Kubernetes resource definition as a version that you want to deploy to your clusters. To list available configurations, run <code>ibmcloud sat config configuration ls</code>.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Required. The name or ID of the version that you want to deploy to the clusters in your cluster group. To list versions in your configuration, run <code>ibmcloud sat config configuration get --configuration &lt;configuration_name_or_ID&gt;</code>.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config subscription create --name myapp_prod_subscription --cluster-group mygroup --configuration myapp_prod --version 1.0
```
{: pre}

<br />


### `ibmcloud sat config subscription get`
{: #cli-config-subscription-get}

Get the details of a subscription, such as the {{site.data.keyword.satelliteshort}} configuration that the subscription is for or the registered clusters that are subscribed.
{: shortdesc}

```
ibmcloud sat config subscription get --subscription SUBSCRIPTION [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--subscription <em>SUBSCRIPTION</em></code></dt>
<dd>Required. The name or ID of your subscription. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run <code>ibmcloud sat config subscription ls</code>.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config subscription get --subscription myapp_prod_subscription
```
{: pre}

<br />


### `ibmcloud sat config subscription ls`
{: #cli-config-subscription-ls}

List the subscriptions to {{site.data.keyword.satelliteshort}} configurations in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

```
ibmcloud sat config subscription ls
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}.

**Command options:** N/A

**Example:**
```
ibmcloud sat config subscription ls
```
{: pre}

<br />


### `ibmcloud sat config subscription rm`
{: #cli-config-subscription-rm}

Remove a subscription.
{: shortdesc}

The Kubernetes resources for the subscription, such as pods and services, are deleted from all your subscribed clusters that are registered in {{site.data.keyword.satelliteshort}}. Before you remove a subscription, make sure that you do not need these resources. The {{site.data.keyword.satelliteshort}} configuration remains, so if you want to restore the resources, you can create another subscription.
{: important}

```
ibmcloud sat config subscription rm --subscription SUBSCRIPTION [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--subscription <em>SUBSCRIPTION</em></code></dt>
<dd>Required. The name or ID of your subscription. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run <code>ibmcloud sat config subscription ls</code>.</dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config subscription rm --subscription myapp_prod_subscription
```
{: pre}

<br />


### `ibmcloud sat config subscription update`
{: #cli-config-subscription-update}

Update a subscription, such as to change the subscription name, the configuration version, or the subscribed cluster group. After you update the subscription, the configuration version is automatically deployed to all subscribed clusters.
{: shortdesc}

```
ibmcloud sat config subscription update --subscription SUBSCRIPTION [--name NAME] [--cluster-group CLUSTERGROUP] [--version VERSION] [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Subscription** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--subscription <em>SUBSCRIPTION</em></code></dt>
<dd>Required. The name or ID of the subscription that you want to update. To list subscriptions in your {{site.data.keyword.cloud_notm}} account, run <code>ibmcloud sat config subscription ls</code>.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Optional. The new name to give your subscription. </dd>

<dt><code>--cluster-group <em>CLUSTERGROUP</em></code></dt>
<dd>Optional. The name or ID of the cluster group that you want to subscribe. To list available cluster groups, run <code>ibmcloud sat cluster-group ls</code>.</dd>

<dt><code>--version <em>VERSION</em></code></dt>
<dd>Required. The name or ID of your configuration version that you want to subscribe clusters to. To list versions in your configuration, run <code>ibmcloud sat config configuration get --configuration &lt;configuration_name_or_ID&gt;</code>.</dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat config subscription update --subscription myapp_prod_subscription --name myapp_staging_subscription --cluster-group mygroup --version 1.0
```
{: pre}

<br />


## Cluster commands
{: #sat-cluster-commands}

Use these commands to register {{site.data.keyword.openshiftshort}} clusters for use with [{{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config), to consistently deploy and update apps across clusters.
{: shortdesc}

### `ibmcloud sat cluster get`
{: #cli-cluster-get}

Get the details of a cluster that is registered with the {{site.data.keyword.satelliteshort}} Config component.
{: shortdesc}

```
ibmcloud sat cluster get --cluster CLUSTER [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster, -c <em>CLUSTER</em></code></dt>
<dd>Required. The name or ID of the cluster. To list registered clusters, run <code>ibmcloud sat cluster ls</code>.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster get -c mycluster
```
{: pre}

<br />


### `ibmcloud sat cluster ls`
{: #cli-cluster-ls}

View a list of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component. You can use {{site.data.keyword.satelliteshort}} configurations to consistently deploy and update apps to these clusters.
{: shortdesc}

```
ibmcloud sat cluster ls [--filter FILTER] [--limit NUMBER] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--filter <em>FILTER</em></code></dt>
<dd>Optional. Filter results by a cluster property. Currently, the only supported cluster property is the cluster ID.</dd>

<dt><code>--limit <em>NUMBER</em></code></dt>
<dd>Optional. Limit the number of clusters that are returned. The default value is 50.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster ls
```
{: pre}

<br />


### `ibmcloud sat cluster register`
{: #cli-cluster-register}

Get a `kubectl` command to run in your cluster to install the {{site.data.keyword.satelliteshort}} Config agent. The {{site.data.keyword.satelliteshort}} Config agent is automatically installed in clusters that you run in your {{site.data.keyword.satelliteshort}} location. For all clusters that run in {{site.data.keyword.cloud_notm}}, you must use this command to install the {{site.data.keyword.satelliteshort}} Config agent so that you can include these clusters in {{site.data.keyword.satelliteshort}} configurations. After you get the command, [log in to your cluster](/docs/openshift?topic=openshift-access_cluster#access_public_se) and run the command.
{: shortdesc}

```
ibmcloud sat cluster register [--silent] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--silent</code></dt>
  <dd>Return only the registration command in the CLI output.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster register
```
{: pre}

<br />


### `ibmcloud sat cluster unregister`
{: #cli-cluster-unregister}

Unregister a cluster from the {{site.data.keyword.satelliteshort}} Config component. You can no longer subscribe the cluster to automatically deploy Kubernetes resources from a configuration, but the cluster and its existing resources still run.
{: shortdesc}

```
ibmcloud sat cluster unregister --cluster CLUSTER [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Cluster** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster, -c <em>CLUSTER</em></code></dt>
<dd>Required. The cluster that you want to unregister from {{site.data.keyword.satelliteshort}}. To list registered clusters, run `ibmcloud sat cluster ls`.</dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster unregister -c mycluster
```
{: pre}

<br />


## Cluster group commands
{: #cluster-group-commands}

Use these commands to create cluster groups. Then, subscribe your cluster group to a {{site.data.keyword.satelliteshort}} configuration to automatically deploy Kubernetes resources to these clusters.
{: shortdesc}

### `ibmcloud sat cluster-group attach`
{: #cluster-group-attach}

Add a {{site.data.keyword.openshiftlong_notm}} cluster to your cluster group. The cluster can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. To add a cluster that runs in {{site.data.keyword.cloud_notm}}, you must first [register the cluster](#cli-cluster-register) with the {{site.data.keyword.satelliteshort}} Config component.
{: shortdesc}

```
ibmcloud sat cluster-group attach --cluster CLUSTER [--cluster CLUSTER] --cluster-group CLUSTERGROUP [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Clustergroup** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster, -c <em>CLUSTER</em></code></dt>
<dd>Required. The cluster that you want to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.</dd>

<dt><code>--cluster-group, -g <em>CLUSTERGROUP</em></code></dt>
<dd>Required. The name or ID of the cluster group where you want to add the cluster. To list available cluster groups, run `ibmcloud sat cluster-group ls`.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster-group attach --cluster mycluster --cluster-group mygroup
```
{: pre}

<br />



### `ibmcloud sat cluster-group create`
{: #cluster-group-create}

Create a cluster group. After you created the cluster group, you can subscribe the cluster group to a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```
ibmcloud sat cluster-group create --name NAME [--cluster CLUSTER] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Clustergroup** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--name <em>NAME</em></code></dt>
<dd>Required. The name for the cluster group.</dd>  

<dt><code>--cluster, -c <em>CLUSTER</em></code></dt>
<dd>Optional. The name or ID of a cluster that you want to add to the cluster group. To list registered clusters, run `ibmcloud sat cluster ls`.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster-group create --name mygroup
```
{: pre}

<br />


### `ibmcloud sat cluster-group detach`
{: #cluster-group-detach}

Remove a {{site.data.keyword.openshiftlong_notm}} cluster from a cluster group.
{: shortdesc}

```
ibmcloud sat cluster-group detach --cluster-group CLUSTERGROUP --cluster CLUSTER [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Clustergroup** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster-group <em>CLUSTERGROUP</em></code></dt>
<dd>Required. The name or ID of the cluster group where you want to remove a cluster. To list available cluster groups, run <code>ibmcloud sat cluster-group ls</code>.</dd>  

<dt><code>--cluster, -c <em>CLUSTER</em></code></dt>
<dd>Optional. The name or ID of the cluster that you want to remove from your cluster group. To list the clusters that are included in your cluster group, run <code>ibmcloud sat cluster-group get --cluster-group &lt;cluster_group_name_or_ID&gt;</code>.</dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster-group detach --cluster-group mygroup --cluster mycluster
```
{: pre}

<br />


### `ibmcloud sat cluster-group get`
{: #cluster-group-get}

Retrieve details of a cluster group, such as the clusters that are included in your cluster group.
{: shortdesc}

```
ibmcloud sat cluster-group get --cluster-group CLUSTERGROUP [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Clustergroup** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster-group, -g <em>CLUSTERGROUP</em></code></dt>
<dd>Required. The name or ID of the cluster group that you want to retrieve details for. To list available cluster groups, run <code>ibmcloud sat cluster-group ls</code>.</dd>  

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster-group get --cluster-group mygroup
```
{: pre}

<br />


### `ibmcloud sat cluster-group ls`
{: #cluster-group-ls}

List all cluster groups in your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

```
ibmcloud sat cluster-group ls [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Clustergroup** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster-group ls
```
{: pre}

<br />



### `ibmcloud sat cluster-group rm`
{: #cluster-group-rm}

Remove a cluster group.
{: shortdesc}

```
ibmcloud sat cluster-group rm --cluster-group CLUSTERGROUP [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Clustergroup** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster-group, -g <em>CLUSTERGROUP</em></code></dt>
<dd>Required. The name or ID of the cluster group that you want to remove. To list available cluster groups, run <code>ibmcloud sat cluster-group ls</code>.</dd>  

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat cluster-group rm --cluster-group mygroup
```
{: pre}

<br />


## Endpoint commands
{: #sat-endpoint-commands}

Use these commands to create and manage {{site.data.keyword.satelliteshort}} Link endpoints to allow network traffic between your {{site.data.keyword.satellitelong}} location and services, servers, or apps that run in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

### `ibmcloud sat endpoint create`
{: #cli-endpoint-create}

Create a {{site.data.keyword.satelliteshort}} endpoint.
{: shortdesc}

```
ibmcloud sat endpoint create --location LOCATION_ID --name NAME --dest-type CLOUD|LOCATION --dest-hostname HOSTNAME_OR_IP --dest-port PORT [--dest-protocol PROTOCOL] --source-protocol PROTOCOL [--output JSON] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--location <em>LOCATION_ID</em></code></dt>
<dd>Required. The ID of your location. To list locations, run <code>ibmcloud sat location ls</code>.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Required. A name for your {{site.data.keyword.satelliteshort}} endpoint.</dd>

<dt><code>--dest-type <em>CLOUD|LOCATION</em></code></dt>
<dd>Required. The place where the destination resource runs, either in {{site.data.keyword.cloud_notm}} (`cloud`) or your {{site.data.keyword.satelliteshort}} location (`location`).</dd>

<dt><code>--dest-hostname <em>HOSTNAME_OR_IP</em></code></dt>
<dd>Required. The URL or the externally accessible IP address of the destination resource that you want to connect to. Make sure to enter the URL without <code>http://</code> or <code>https://</code>.</dd>

<dt><code>--dest-port <em>PORT</em></code></dt>
<dd>Required. The port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.</dd>

<dt><code>--dest-protocol <em>PROTOCOL</em></code></dt>
<dd>Optional. The protocol of the destination resource. If you do not specify this flag, the destination protocol is inherited from the source protocol. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).</dd>

<dt><code>--source-protocol <em>PROTOCOL</em></code></dt>
<dd>Required. The protocol that the source must use to connect to the destination resource. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).</dd>

<dt><code>--output <em>JSON</em></code></dt>
<dd>Optional. Displays the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat endpoint create --location aaaaaaaa1111a1aaaa11a --name demo-svc --dest-type cloud --dest-hostname myhost.example.com --dest-port 80 --source-protocol TCP
```
{: pre}

<br />


### `ibmcloud sat endpoint get`
{: #cli-endpoint-get}

View the details of an endpoint.
{: shortdesc}

```
ibmcloud sat endpoint get --endpoint ENDPOINT_ID --location LOCATION_ID [--output JSON] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--endpoint <em>ENDPOINT_ID</em></code></dt>
<dd>Required. The ID of the endpoint. To list available endpoints, run <code>ibmcloud sat endpoint ls --location &lt;location_ID&gt;</code>.</dd>

<dt><code>--location <em>LOCATION_ID</em></code></dt>
<dd>Required. The ID of your location. To list locations, run <code>ibmcloud sat location ls</code>.</dd>

<dt><code>--output <em>JSON</em></code></dt>
<dd>Optional. Displays the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat endpoint get --endpoint aaaaaaaa1111a1aaaa11a_bb22b --location aaaaaaaa1111a1aaaa11a
```
{: pre}

<br />


### `ibmcloud sat endpoint ls`
{: #cli-endpoint-ls}

View a list of all {{site.data.keyword.satelliteshort}} endpoints for a location.
{: shortdesc}

```
ibmcloud sat endpoint ls --location LOCATION_ID [--output JSON] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--location <em>LOCATION_ID</em></code></dt>
<dd>Required. The ID of your location. To list locations, run <code>ibmcloud sat location ls</code>.</dd>

<dt><code>--output <em>JSON</em></code></dt>
<dd>Optional. Displays the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat endpoint ls --location aaaaaaaa1111a1aaaa11a
```
{: pre}

<br />


### `ibmcloud sat endpoint rm`
{: #cli-endpoint-rm}

Delete an endpoint from your location. The connection between the source and destination resources is removed.
{: shortdesc}

```
ibmcloud sat endpoint rm --endpoint ENDPOINT_ID --location LOCATION_ID [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--endpoint <em>ENDPOINT_ID</em></code></dt>
<dd>Required. The ID of the endpoint. To list endpoint, run <code>ibmcloud sat endpoint ls --location &lt;location_ID&gt;</code>.</dd>

<dt><code>--location <em>LOCATION_ID</em></code></dt>
<dd>Required. The ID of your location. To list locations, run <code>ibmcloud sat location ls</code>.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat endpoint rm --endpoint aaaaaaaa1111a1aaaa11a_bb22b --location aaaaaaaa1111a1aaaa11a
```
{: pre}

<br />


### `ibmcloud sat endpoint update`
{: #cli-endpoint-update}

Update an endpoint. Only the options that you specify are updated.
{: shortdesc}

```
ibmcloud sat endpoint update --location LOCATION_ID --endpoint ENDPOINT_ID [--name NAME] [--dest-type CLOUD|LOCATION] [--dest-hostname HOSTNAME_OR_IP] [--dest-port PORT] [--dest-protocol PROTOCOL] [--source-protocol PROTOCOL] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Editor** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--location <em>LOCATION_ID</em></code></dt>
<dd>Required. The ID of your location. To list locations, run <code>ibmcloud sat location ls</code>.</dd>

<dt><code>--endpoint <em>ID</em></code></dt>
<dd>Required. The ID of the endpoint. To list endpoint, run <code>ibmcloud sat endpoint ls</code>.</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Optional. A new name for your {{site.data.keyword.satelliteshort}} endpoint.</dd>

<dt><code>--dest-type <em>CLOUD|LOCATION</em></code></dt>
<dd>Optional. Where the destination resource runs.</dd>

<dt><code>--dest-hostname <em>HOSTNAME_OR_IP</em></code></dt>
<dd>Optional. The URL or the externally accessible IP address of the destination resource that you want to connect to. Make sure to enter the URL without <code>http://</code> or <code>https://</code>.</dd>

<dt><code>--dest-port <em>PORT</em></code></dt>
<dd>Optional. The port that destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.</dd>

<dt><code>--dest-protocol <em>PROTOCOL</em></code></dt>
<dd>Optional. The protocol of the destination resource. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).</dd>

<dt><code>--source-protocol <em>PROTOCOL</em></code></dt>
<dd>Optional. The protocol that the source must use to connect to the destination resource. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat endpoint update --location aaaaaaaa1111a1aaaa11a --endpoint aaaaaaaa1111a1aaaa11a_bb22b --name new_demo_svc --dest-hostname myupdatedhost.example.com --dest-port 8080
```
{: pre}

<br />



## Host commands
{: #sat-host-commands}

### `ibmcloud sat host assign`
{: #host-assign}

Add your compute host to the {{site.data.keyword.satelliteshort}} control plane or any other {{site.data.keyword.openshiftshort}} cluster that you created in your location.
{: shortdesc}

```
ibmcloud sat host assign --location LOCATION --cluster CLUSTER --host HOST --zone ZONE [--worker-pool WORKER_POOL] [--label "LABEL"] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location where the {{site.data.keyword.satelliteshort}} control plane or {{site.data.keyword.openshiftshort}} cluster exists to which you want to assign the compute host. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Required. Enter the ID or name of the cluster where you want to assign your compute host. If you want to assign your compute host to the {{site.data.keyword.satelliteshort}} control plane, use the location ID or name. To assign the compute host to an {{site.data.keyword.openshiftshort}} cluster, use the ID or name of the cluster. To retrieve the cluster ID or name, run <code>ibmcloud sat cluster ls</code>.  </dd>

<dt><code>--host <em>HOST</em></code></dt>
<dd>Required. Enter the ID of the host that you want to assign to the {{site.data.keyword.satelliteshort}} control plane or {{site.data.keyword.openshiftshort}} cluster. To retrieve the host ID, run <code>ibmcloud sat host ls --location &lt;location_ID_or_name&gt;</code>.  </dd>

<dt><code>--zone <em>ZONE</em></code></dt>
<dd>Required. Enter the name of the zone where you want to assign the compute host. The zone must belong to the {{site.data.keyword.cloud_notm}} multizone metro that you selected when you created the location. </dd>

<dt><code>--worker-pool <em>WORKER_POOL</em></code></dt>
<dd>Optional. Enter the name or ID of the worker pool in your {{site.data.keyword.openshiftshort}} cluster to which you want to add your compute host. If you want to assign hosts to your {{site.data.keyword.satelliteshort}} control plane, this flag is not required. When you assign hosts to an {{site.data.keyword.openshiftshort}} cluster, you can include this flag to specify the worker pool. If no worker pool is specified, the host is assigned to the default worker pool of the cluster.  </dd>

<dt><code>--label <em>LABEL</em></code>, <code>-l <em>LABEL</em></code></dt>
<dd>Optional. Enter any labels as a key-value-pair that you want to use to identify the host that you want to assign to your  {{site.data.keyword.satelliteshort}} control plane or {{site.data.keyword.openshiftshort}} cluster. The first host that has this label and is in an unassigned state it automatically assigned to the control plane or cluster. To find available host labels, run <code>ibmcloud sat host get --host &lt;host_name_or_ID&gt; --location &lt;location_name_or_ID&gt;</code>.  </dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat host assign --location aaaaaaaa1111a1aaaa11a --host myhost1 --zone us-east-1 --cluster aaaaaaaa1111a1aaaa11a --label "use=satloc"
```
{: pre}

<br />


### `ibmcloud sat host attach`
{: #host-attach}

Create and download a script that you run on all the compute hosts that you want to make visible to your location.
{: shortdesc}

```
ibmcloud sat host attach --location LOCATION [--label "LABEL"] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location where you want to add compute hosts. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--label <em>LABEL</em></code>, <code>-l <em>LABEL</em></code></dt>
<dd>Optional. Enter any labels as a key-value-pair that you want to add to your compute hosts. Labels can help find hosts more easily later.  </dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat host attach --location aaaaaaaa1111a1aaaa11a --label "use=satloc"
```
{: pre}

<br />


### `ibmcloud sat host get`
{: #host-get}

View the details of a compute host that was made visible to a location.
{: shortdesc}

To make a host visible to a location, you must run a script on each host machine. For more information, see the [`ibmcloud sat host attach`](#host-attach) command.
{: tip}

```
ibmcloud sat host get --location LOCATION --host HOST [--output json] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location that the host belongs to. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--host <em>HOST</em></code></dt>
<dd>Required. Enter the ID of the host that you want to retrieve details for. To retrieve the host ID, run <code>ibmcloud sat host ls &lt;location_ID_or_name&gt;</code>.  </dd>

<dt><code>--output json</code></dt>
 <dd>Optional. Prints the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat host get --location aaaaaaaa1111a1aaaa11a --host myhost1
```
{: pre}

<br />


### `ibmcloud sat host ls`
{: #host-ls}

List all compute hosts that are visible to your location.
{: shortdesc}

To make a host visible to a location, you must run a script on each host machine. For more information, see the [`ibmcloud sat host attach`](#host-attach) command.
{: tip}

```
ibmcloud sat host ls --location LOCATION [--output json] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location that you want to list compute hosts for. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--output json</code></dt>
 <dd>Optional. Prints the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat host ls --location aaaaaaaa1111a1aaaa11a
```
{: pre}

<br />


### `ibmcloud sat host rm`
{: #host-rm}

Remove a host from a location.
{: shortdesc}

```
ibmcloud sat host rm --location LOCATION --host HOST [-f ] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location where you want to remove a compute host. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--host <em>HOST</em></code></dt>
<dd>Required. Enter the ID of the host that you want to remove. To retrieve the host ID, run <code>ibmcloud sat host ls &lt;location_ID_or_name&gt;</code>.  </dd>

<dt><code>-f</code></dt>
 <dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>


**Example:**
```
ibmcloud sat host rm --location aaaaaaaa1111a1aaaa11a --host myhost1
```
{: pre}

<br />


### `ibmcloud sat host update`
{: #host-update}

Update information about your compute host, such as labels.
{: shortdesc}

```
ibmcloud sat host update --location LOCATION --host HOST [--label "LABEL"] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location that the compute host is assigned to. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--host <em>HOST</em></code></dt>
<dd>Required. Enter the ID of the host that you want to update. To retrieve the host ID, run <code>ibmcloud sat host ls --location &lt;location_ID_or_name&gt;</code>.  </dd>

<dt><code>--label <em>LABEL</em></code>, <code>-l <em>LABEL</em></code></dt>
<dd>Optional. Enter any labels as a key-value-pair that you want to use to identify the hosts that you want to update. To find available host labels, run <code>ibmcloud sat host get --host &lt;host_name_or_ID&gt; --location &lt;location_name_or_ID&gt;</code>.  </dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat host update --location aaaaaaaa1111a1aaaa11a --host myhost1
```
{: pre}

<br />


## Location commands
{: #sat-location-commands}

Use these commands to create and manage {{site.data.keyword.satelliteshort}} locations.
{: shortdesc}

### `ibmcloud sat location create`
{: #location-create}

Create a {{site.data.keyword.satelliteshort}} location. When you create a location, a location master is automatically deployed in one of the {{site.data.keyword.cloud_notm}} multizone metro zones that you select during location creation. The location master is used to manage the location from the public {{site.data.keyword.cloud_notm}}.
{: shortdesc}

```
ibmcloud sat location create --managed-from METRO --name NAME [--cos-key COS_SECRET_KEY] [--cos-key-id COS_ACCESS_KEY_ID] [--cos-region COS__BUCKET_REGION] [--cos-bucket COS_BUCKET_NAME] [--cos-endpoint COS_BUCKET_ENDPOINT] [--logging-account-id LOGGING_ACCOUNT] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Administrator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--managed-from <em>METRO</em></code></dt>
<dd>Required. The {{site.data.keyword.cloud_notm}} multizone metro that your {{site.data.keyword.satelliteshort}} control plane resources are managed from. Select the {{site.data.keyword.cloud_notm}} multizone metro that is nearest to where your physical machines are. For a list of supported metros, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).</dd>

<dt><code>--name <em>NAME</em></code></dt>
<dd>Required. Enter a name for your location. The name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the name of a previously deleted location.</dd>

<dt><code>--cos-key <em>COS_SECRET_KEY</em></code></dt>
<dd>Optional. Enter the HMAC secret access key credentials of the {{site.data.keyword.cos_full_notm}} service instance that you want to use to back up data of your {{site.data.keyword.satelliteshort}} control plane. If you specify the secret access key, make sure to also specify the HMAC access key ID, bucket name, bucket endpoint, and bucket region of your {{site.data.keyword.cos_full_notm}} service instance.   </dd>

<dt><code>--cos-key-id <em>COS_ACCESS_KEY_ID</em></code></dt>
<dd>Optional. Enter the HMAC access key ID credentials of the {{site.data.keyword.cos_full_notm}} service instance that you want to use to back up data of your {{site.data.keyword.satelliteshort}} control plane. If you specify the access key ID, make sure to also specify the HMAC secret access key, bucket name, bucket endpoint, and bucket region of your {{site.data.keyword.cos_full_notm}} service instance.   </dd>

<dt><code>--cos-region <em>COS_BUCKET_REGION</em></code></dt>
<dd>Optional. Enter the region that your {{site.data.keyword.cos_full_notm}} bucket is in. From the {{site.data.keyword.cos_full_notm}} console, click **Buckets > Configuration** and look for the **Location**. If you specify the bucket region, make sure to also specify the HMAC secret access key, access key ID, bucket name, and bucket endpoint of your {{site.data.keyword.cos_full_notm}} service instance.   </dd>

<dt><code>--cos-bucket <em>COS_BUCKET_NAME</em></code></dt>
<dd>Optional. Enter the name of the {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up the control plane data. If you specify the bucket name, make sure to also specify the HMAC secret access key, access key ID, bucket region, and bucket endpoint of your {{site.data.keyword.cos_full_notm}} service instance.   </dd>

<dt><code>--cos-endpoint <em>COS_BUCKET_ENDPOINT</em></code></dt>
<dd>Optional. Enter the API endpoint of the {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up the control plane data. From the {{site.data.keyword.cos_full_notm}} console, click **Buckets > Configuration** and look for the **Public Endpoints** value. If you specify the bucket endpoint, make sure to also specify the HMAC secret access key, access key ID, bucket region, and bucket name of your {{site.data.keyword.cos_full_notm}} service instance.   </dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat location create --managed-from wdc --name mylocation
```
{: pre}

<br />


### `ibmcloud sat location dns ls`
{: #location-dns-ls}

List the DNS record for your location.

```
ibmcloud sat location dns ls --location LOCATION [--output json] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location that you want to show the DNS record for. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--output json</code></dt>
 <dd>Optional. Prints the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat location dns ls --location aaaaaaaa1111a1aaaa11a
```
{: pre}

<br />


### `ibmcloud sat location dns register`
{: #location-dns-register}

Create a DNS record for your location and register the public IP addresses of your compute hosts that you added to the {{site.data.keyword.satelliteshort}} control plane to enable load balancing and health checking of your location.

A DNS record is automatically created for your location when you add compute hosts to your {{site.data.keyword.satelliteshort}} control plane with the public IP addresses that are assigned to your compute hosts. Use this command only if you want to register different public IP addresses for your {{site.data.keyword.satelliteshort}} control plane hosts.
{: important}

```
ibmcloud sat location dns register --location LOCATION --ip HOST_PUBLIC_IP [--output json] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Operator** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the name or ID of the location for which you want to create a DNS record and register the public IP addresses of your control plane hosts. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--ip <em>HOST_PUBLIC_IP</em></code></dt>
<dd>Required. Enter the public IP address of your a compute host that you added to your {{site.data.keyword.satelliteshort}} control plane. To retrieve the IP, run <code>ibmcloud sat host ls --location &lt;location_ID_or_name&gt;</code>ID. To register multiple public IP addresses, you can use multiple <code>--ip</code> flags in the same command.   </dd>

<dt><code>--output json</code></dt>
 <dd>Optional. Prints the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat location dns register --location aaaaaaaa1111a1aaaa11a --ip 169.67.23.145. --ip 1669.67.23.167
```
{: pre}

<br />


### `ibmcloud sat location get`
{: #location-get}

Retrieve the details of a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

```
ibmcloud sat location get --location LOCATION [--output json] [-q]
```
{: pre}


</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in the {{site.data.keyword.satelliteshort}} location.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location that you want to retrieve details for. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>--output json</code></dt>
 <dd>Optional. Prints the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat location get --location aaaaaaaa1111a1aaaa11a
```

<br />


### `ibmcloud sat location ls`
{: #location-ls}

List all {{site.data.keyword.satelliteshort}} location in your account.
{: shortdesc}

```
ibmcloud sat location ls [--output json] [-q]
```
{: pre}


</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--output json</code></dt>
<dd>Optional. Prints the command output in JSON format.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat location ls
```
{: pre}

<br />


### `ibmcloud sat location rm`
{: #location-rm}

Remove a {{site.data.keyword.satelliteshort}} location from your account.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts that run in the location.
{: important}

```
ibmcloud sat location rm --location LOCATION [-f] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Administrator** platform role for the **Location** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>

<dt><code>--location <em>LOCATION</em></code></dt>
<dd>Required. Enter the ID or name of the location that you want to remove. To retrieve the location ID or name, run <code>ibmcloud sat location ls</code>.  </dd>

<dt><code>-f</code></dt>
<dd>Optional. Force the command to run with no user prompts.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>

</dl>

**Example:**
```
ibmcloud sat location rm --location mylocation
```
{: pre}

<br />


## Resource commands
{: #sat-resource-commands}

Use these commands to view the Kubernetes resources that run in clusters that are registered with [{{site.data.keyword.satelliteshort}} Configuration](/docs/satellite?topic=satellite-cluster-config).
{: shortdesc}

### `ibmcloud sat resource get`
{: #cli-resource-get}

Get the details of a Kubernetes resource that is managed by a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```
ibmcloud sat resource get --resource RESOURCE [--save-data] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--resource <em>RESOURCE</em></code></dt>
<dd>Required. The ID of the Kubernetes resource. To list Kubernetes resources, run <code>ibmcloud sat resource ls</code>.</dd>

<dt><code>--save-data</code></dt>
<dd>Optional. Download and save the Kubernetes resource definition to a temporary file.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat resource get --resource 1234567
```
{: pre}

<br />


### `ibmcloud sat resource ls`
{: #cli-resource-ls}

Search for and list Kubernetes resource that are managed by a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

```
ibmcloud sat resource ls [--cluster CLUSTER] [--limit NUMBER] [--search STRING] [-q]
```
{: pre}

</br>

**Minimum required permissions**: {{site.data.keyword.cloud_notm}} IAM **Viewer** platform role for the **Resource** resource in {{site.data.keyword.satelliteshort}}.

**Command options:**

<dl>
<dt><code>--cluster <em>CLUSTER</em></code></dt>
<dd>Optional. The name or ID of the registered cluster that the Kubernetes resource runs in. To list registered clusters, run <code>ibmcloud sat cluster ls</code>.</dd>

<dt><code>--limit <em>NUMBER</em></code></dt>
<dd>Optional. Limit the number of Kubernetes resources that are returned. Enter a number less than 500.</dd>

<dt><code>--search <em>STRING</em></code></dt>
<dd>Optional. A string to filter results by, such as the name of a `pod`, `namespace`, or other string.</dd>

<dt><code>-q</code></dt>
<dd>Optional. Do not show the message of the day or update reminders.</dd>
</dl>

**Example:**
```
ibmcloud sat resource ls
```
{: pre}

<br />


## Other commands
{: #other-commands}

Review other commands for managing {{site.data.keyword.satelliteshort}} resources, such as commands from other {{site.data.keyword.cloud_notm}} services that might be useful.
{: shortdesc}

### `ibmcloud ks cluster create satellite`
{: #cluster-create}

See the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite).
{: shortdesc}