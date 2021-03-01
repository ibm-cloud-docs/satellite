---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-01"

keywords: satellite storage, satellite config, satellite configurations, aws, ebs, block storage

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


# AWS EBS
{: #config-storage-ebs}

Set up [Amazon Elastic Block Storage (EBS)](https://docs.aws.amazon.com/ebs/?id=docs_gateway){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. After you create a storage configuration, you can assign it to your clusters. When you assign a storage configuration to your clusters, the storage drivers for the provider that you used to create your configuration are installed on your cluster.
{: shortdesc}

When you create your AWS EBS storage configuration you provide your AWS credentials which are stored as a Kubernetes secret in the clusters that you assign your configuration to. The secret is mounted inside the CSI controller pod so that when you create a PVC by using one of the {{site.data.keyword.satelliteshort}} storage classes, your AWS credentials are used to dynamically provision an EBS instance.

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

<br />



## Creating an AWS EBS storage configuration in the command line
{: #sat-storage-aws-ebs-cli}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
2. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).
3. Review the [AWS EBS storage configuration parameters](#sat-storage-aws-ebs-params-cli).
4. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name aws-ebs-csi-driver --template-version 0.8.2 --param "aws-access-key=<aws-access-key>" --param "aws-secret-access-key=<aws-secret-access-key>"
  ```
  {: pre}
5. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}
6. [Assign your configuration to clusters](#assign-storage-ebs).

<br />
### AWS EBS storage configuration parameter reference
{: #sat-storage-aws-ebs-params-cli}
Review the AWS EBS storage configuration parameters.


| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `aws-ebs-csi-driver`. |
| `--template-version` | Required | Enter the version of the `aws-ebs-csi-driver` template that you want to use. To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `aws-access-key` | Required | Enter your AWS access key. For more information about how retrieve this value, see the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html){: external}. |
| `aws-secret-access-key` | Required | Enter your AWS secret access key. For more information about how retrieve this value, see the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html){: external}. |
{: caption="Table 1. AWS EBS parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}

<br />

## Assigning your AWS EBS storage configuration to a cluster
{: #assign-storage-ebs}

After you [create an EBS storage configuration](#config-storage-ebs), you can assign you configuration to {{site.data.keyword.satelliteshort}} clusters.



<br />
### Assigning a storage configuraton in the command line
{: #assign-storage-ebs-cli}


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
5. Verify that the storage configuration resources are deployed. List the EBS driver pods in the `kube-system` namespace and verify that the status is `Running`.
  ```sh
  oc get pods -n kube-system | grep ebs
  ```
  {: pre}

  Example output:
  ```sh   
  ebs-csi-controller-5dc5468b9-d8c5c      6/6     Running   0          2d8h
  ebs-csi-controller-5dc5468b9-p2tw4      6/6     Running   0          2d8h
  ebs-csi-node-2vt9v                      3/3     Running   0          2d8h
  ebs-csi-node-7wccn                      3/3     Running   0          2d8h
  ebs-csi-node-rllvw                      3/3     Running   0          2d8h
  ebs-snapshot-controller-0               1/1     Running   0          2d8h
  ```
  {: screen}

6. List the EBS storage classes.
  ```sh
  oc get sc | grep aws-block
  ```
  {: pre}

  Example output:
  ```sh             
  sat-aws-block-bronze      ebs.csi.aws.com    Delete          WaitForFirstConsumer   true                   2d8h
  sat-aws-block-gold        ebs.csi.aws.com    Delete          WaitForFirstConsumer   true                   2d8h
  sat-aws-block-silver      ebs.csi.aws.com    Delete          WaitForFirstConsumer   true                   2d8h
  ```
  {: screen}

<br />
## Storage class reference
{: #sat-ebs-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EBS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the EBS storage classes in [GitHub](https://github.com/IBM/ibm-satellite-storage/blob/develop/config-templates/aws/aws-ebs-csi-driver/0.8.2/storage-class.yaml).
{: shortdesc}

| Storage class name | EBS volume type | File system type | Provisioner | Default IOPS per GB | Size range | Hard disk | Encrypted? | Volume binding mode | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `sat-aws-block-gold` | io2 | ext4 | `ebs.csi.aws.com` | 10 | 10 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | 
| `sat-aws-block-silver` | gp3 | ext4 | `ebs.csi.aws.com` | N/A | 1 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | 
| `sat-aws-block-bronze` | st1 | ext4 | `ebs.csi.aws.com` | N/A | 125 GiB - 16 TiB | HDD | True |  WaitforFirstConsumer | Delete |
{: caption="Table 2. AWS EBS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the volume type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the default IOPs per GB. The size column is the supported size range. The seventh column is disk type. The eigth column is encryption support. The ninth column is the volume binding mode. The tenth column is the reclaim policy."}


