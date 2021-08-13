---
copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

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


# NetApp ONTAP-SAN 20.07
{: #config-storage-netapp}

Set up [NetApp ONTAP-SAN storage](https://netapp-trident.readthedocs.io/en/stable-v20.07/){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp Trident template](/docs/satellite?topic=satellite-config-storage-netapp-trident) which installs the required operator.
{: important}

<br />



## Creating a NetApp Trident SAN storage configuration in the command line
{: #sat-storage-netapp-cli-san}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.
    ```sh
    ibmcloud login
    ```
    {: pre}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.
    ```
    ibmcloud sat location ls
    ```
    {: pre}

1. Target the `Managed from` region of your {{site.data.keyword.satelliteshort}} location. For example, for `wdc` target `us-east`. For more information, see [{{site.data.keyword.satelliteshort}} regions](/docs/satellite?topic=satellite-sat-regions).
    ```sh
    ibmcloud target -r us-east
    ```
    {: pre}

1. If you use a resource group other than `default`, target it.
    ```sh
    ibmcloud target -g <resource-group>
    ```
    {: pre}
1. List the available templates and versions and review the output. Make a note of the template and version that you want to use.
    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
3. Review the [NetApp Trident storage configuration parameters](#sat-storage-netapp-params-cli-san).
4. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name netapp-ontap-san --template-version <template_version> --param "managementLIF=<managementLIF>" --param "dataLIF=<dataLIF>" --param "svm=<svm>" --param "username=<username>" --param "password=<password>"
    ```
    {: pre}

5. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

## Assigning your NetApp storage configuration to a cluster
{: #assign-storage-netapp-san}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-netapp), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />



### Assigning a storage configuration in the command line
{: #assign-storage-netapp-cli-san}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig).
    * **Group**
    ```sh
    ibmcloud sat group ls
    ```
    {: pre}

    * **Cluster**
    ```sh
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}

    * **{{site.data.keyword.satelliteshort}}-enabled service cluster**
    ```sh
    ibmcloud sat service ls --location <location>
    ```
    {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. Replace `<group>` with the ID of your cluster group or `<cluster>` with the ID of your cluster. Replace `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    * **Group**
    ```sh
    ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
    ```
    {: pre}

    * **Cluster**
    ```sh
    ibmcloud sat storage assignment create --cluster <cluster> --config <config> --name <name>
    ```
    {: pre}

    * **{{site.data.keyword.satelliteshort}}-enabled service cluster**
    ```sh
    ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
    ```
    {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>) | grep <storage-assignment-name>
    ```
    {: pre}
5. Verify that the storage configuration resources are deployed.
    ```sh
    oc get pods -A | grep <template-name>
    ```
    {: pre}

<br />

## NetApp Trident storage configuration parameter reference
{: #sat-storage-netapp-params-cli-san}

For more information about the NetApp Trident configuration parameters, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v20.07/docker/install/ndvp_ontap_config.html#configuration-file-options){: external}.

| Parameter name | Required? | Description | Default if not provided |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. | N/A |
| `--template-name` | Required | Enter `netapp-ontap-san` | N/A |
| `--template-version` | Required | Enter the template version number. To get a list of templates, run `ibmcloud sat storage template ls`. | N/A |
| `namespace` | Optional | Enter the namespace where you want to install the NetApp Trident storage drivers. | `trident` |
| `managementLIF` | Required | Enter the IP address of the management LIF. Example: `10.0.0.1`. | N/A |
| `dataLIF` | Required | Enter the IP address of the protocol LIF. Example: `10.0.0.2`. | N/A |
| `svm` | Required | Enter the name of the storage virtual machine. Example: `svm`. | N/A |
| `username` | Required | Enter your username. | N/A |
| `password` | Required | Enter your user password. | N/A |
| `limitVolumeSize` | Optional | Maximum volume size that can be requested and qtree parent volume size. | `50Gi` |
| `limitAggregateUsage` | Optional | Limit provisioning of volumes if the parent volume usage exceeds this value. For example, if a volume is requested that causes parent volume usage to exceed this value, the volume provisioning fails.  | `80%` |
{: caption="Table 1. NetApp Trident storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}

<br />
## Storage class reference
{: #netapp-sc-reference-san}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-SAN. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-netapp-block-gold` | ONTAP-SAN | Block | Delete |
| `sat-netapp-block-silver` | ONTAP-SAN | Block | Delete |
| `sat-netapp-block-bronze` | ONTAP-SAN | Block | Delete |
{: caption="Table 2. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}




