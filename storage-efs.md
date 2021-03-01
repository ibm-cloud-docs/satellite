---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-01"

keywords: satellite storage, satellite config, satellite configurations, aws, efs, file storage

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


# AWS EFS
{: #config-storage-efs}

Set up [Amazon Elastic File System (EFS)](https://docs.aws.amazon.com/efs/?id=docs_gateway){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. After you create a storage configuration, you can assign it to your clusters. When you assign a storage configuration to your clusters, the storage drivers for the provider that you used to create your configuration are installed on your cluster.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

Only static provisioning is supported. You must manually provision an [AWS EFS file system](https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html){: external} on AWS before you create your {{site.data.keyword.satelliteshort}} storage configuration.
{: note}

<br />

## Prerequisites
{: #sat-storage-efs-prereqs}
Only static provisioning is supported. You must manually provision an [AWS EFS file system](https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html){: external} on AWS before you create your {{site.data.keyword.satelliteshort}} storage configuration. After you create your AWS EFS instance, you can create a {{site.data.keyword.satelliteshort}} storage configuration to install the AWS EFS drivers in your cluster.

<br />



## Creating an AWS EFS storage configuration in the command line
{: #sat-storage-aws-efs-cli}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
2. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).
3. Review the [AWS EFS storage configuration parameters](#sat-storage-aws-efs-params-cli).
4. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name aws-efs-csi-driver --template-version 1.0.0
  ```
  {: pre}

5. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}

6. [Assign your configuration to clusters](#assign-storage-efs).


<br />
### AWS EFS storage configuration parameter reference
{: #sat-storage-aws-efs-params-cli}

| Parameter | Required? | Description | Default if not provided |
| --- | --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. | N/A |
| `--template-name` | Required | Enter `aws-efs-csi-driver`. | N/A |
| `--template-version` | Required | Enter the version of the `aws-efs-csi-driver` template that you want to use. To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. | N/A |
{: caption="Table 1. AWS EFS parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}

<br />

## Assigning your EFS storage configuration to a cluster
{: #assign-storage-efs}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-efs), you can assign you configuration to your {{site.data.keyword.satelliteshort}}
{: shortdesc}



<br />
### Assigning a storage configuraton in the command line
{: #assign-storage-efs-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

2. List your {{site.data.keyword.satelliteshort}} cluster groups and make a note of the group that you want to assign storage.
  ```sh
  ibmcloud sat group ls
  ```
  {: pre}

3. Assign storage to the clusters that you retrieved in step 2. Replace `<group>` with the name of your cluster group, `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).
  ```sh
  ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
  ```
  {: pre}

4. Verify that your assignment is created.
  ```sh
  ibmcloud sat storage assignment ls | grep <storage-assignment-name>
  ```
  {: pre}
5. Verify that the storage configuration resources are deployed. List the EFS driver pods in the `kube-system` namespace and verify that the status is `Running`.
  ```sh
  oc get pods -n kube-system | grep efs
  ```
  {: pre}

  Example output:
  ```sh   
  efs-csi-node-4gkzx                      3/3     Running   0          2d22h
  efs-csi-node-r8g5d                      3/3     Running   0          2d22h
  efs-csi-node-td4wc                      3/3     Running   0          2d22h
  ```
  {: screen}

6. List the EFS storage classes.
  ```sh
  oc get sc | grep aws-file
  ```
  {: pre}

  Example output:
  ```sh
  sat-aws-file-gold              efs.csi.aws.com    Delete          Immediate              false                  20h
  ```
  {: screen}

<br />

## Storage class reference
{: #efs-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EFS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the EFS storage classes in [GitHub](https://github.com/IBM/ibm-satellite-storage/blob/develop/config-templates/aws/aws-efs-csi-driver/1.0.0/storage-class.yaml).
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-aws-file-gold` | NFS | Delete |
{: caption="Table 2. AWS EFS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the file system type. The third column is the reclaim policy."}




