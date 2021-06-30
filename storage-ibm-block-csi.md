---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-30"

keywords: block storage, satellite storage, satellite config, satellite configurations, 

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


# IBM Systems {{site.data.keyword.blockstorageshort}} CSI driver
{: #config-storage-block-csi}

The {{site.data.keyword.blockstorageshort}} CSI driver is based on an IBM open-source project, and integrated into the IBM Storage orchestration for containers. IBM Storage orchestration for containers enables enterprises to implement a modern container-driven hybrid multicloud environment that can reduce IT costs and enhance business agility, while continuing to derive value from existing systems.

For full release notes, compatiblity, installation, and user information, see the [{{site.data.keyword.blockstorageshort}} CSI driver documentation](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0){: external}.

Supported IBM storage systems:
  - IBM Spectrum Virtualize Family including IBM SAN Volume Controller (SVC) and IBM FlashSystem® family members built with IBM Spectrum® Virtualize (FlashSystem 5010, 5030, 5100, 5200, 7200, 9100, 9200, 9200R)
  - IBM FlashSystem A9000 and A9000R
  - IBM DS8000 Family
  
## Prerequisites
{: #sat-storage-block-csi-prereq}

Be sure to complete all prerequisite and installation steps before assigning hosts to your location. Do not create a Kubernetes cluster.
{: important}

Review the [compatibility and requirements documentation](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=installation-compatibility-requirements){: external}.

<br />




<br />

## Creating a {{site.data.keyword.blockstorageshort}} configuration in the command line
{: #sat-storage-block-csi-cli}

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
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name ibm-system-storage-block-csi-driver --template-version <template_version> -p "namespace=<namespace>" 
  ```
  {: pre}

1. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-block-csi).

<br />
## Assigning your {{site.data.keyword.blockstorageshort}} configuration to a cluster
{: #assign-storage-block-csi}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-block-csi), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />



### Assigning a storage configuraton in the command line
{: #assign-storage-block-csi-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig).
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
  oc get all -A | grep block
  ```
  {: pre}

## Deploying an app that uses your IBM {{site.data.keyword.blockstorageshort}}
{: #storage-block-csi-app-deploy}

You can use the `ibm-system-storage-block-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a Kubernetes secret configuration file that contains your {{site.data.keyword.blockstorageshort}} credentials. For more information, see [Creating a Kubernetes secret](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-secret){: external}.
  ```yaml
  kind: Secret
  apiVersion: v1
  metadata:
    name:  demo-secret
    namespace: default
  type: Opaque
  stringData:
    management_address: demo-management-address 
    username: demo-username                     
  data:
    password: AAAA1AAA 
  ```
  {: codeblock}

1. Create the secret in your cluster.
  ```sh
  oc apply -f <secret.yaml>
  ```
  {: pre}

1. [Create a storage class that uses the block storage driver](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-storageclass){: external}.
  ```yaml
  kind: StorageClass
  apiVersion: storage.k8s.io/v1
  metadata:
    name: demo-storageclass
  provisioner: block.csi.ibm.com
  parameters:
    SpaceEfficiency: deduplicated   # Optional.
    pool: demo-pool
    csi.storage.k8s.io/provisioner-secret-name: demo-secret
    csi.storage.k8s.io/provisioner-secret-namespace: default
    csi.storage.k8s.io/controller-publish-secret-name: demo-secret
    csi.storage.k8s.io/controller-publish-secret-namespace: default
    csi.storage.k8s.io/controller-expand-secret-name: demo-secret
    csi.storage.k8s.io/controller-expand-secret-namespace: default
    csi.storage.k8s.io/fstype: xfs   # Optional. Values ext4\xfs. The default is ext4.
    volume_name_prefix: demoPVC      # Optional
  allowVolumeExpansion: true
  ```
  {: codeblock}

1. Create the storage class in your cluster.
   ```sh
   oc apply -f sc.yaml
   ```
   {: pre}

1. Verify that the storage class is created.
  ```sh
  oc get sc
  ```
  {: pre}

1. [Create a PVC](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-persistentvolumeclaim-pvc){: external} that references the storage class that you created earlier.
   ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      name: demo-pvc-file
    spec:
      volumeMode: Filesystem
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi
      storageClassName: demo-storageclass
  ```
  {: codeblock}

1. Create the PVC in your cluster.
   ```sh
   oc apply -f pvc.yaml
   ```
   {: pre}

1. Verify that the PVC is created and the status is `Bound`.
  ```sh
  oc get pvc
  ```
  {: pre}

1. Create a YAML configuration file for a stateful set that mounts the PVC that you created.
  ```yaml
  kind: StatefulSet
  apiVersion: apps/v1
  metadata:
    name: demo-statefulset-file-system
  spec:
    selector:
      matchLabels:
        app: demo-statefulset
    serviceName: demo-statefulset
    replicas: 1
    template:
      metadata:
        labels:
          app: demo-statefulset
      spec:
        containers:
        - name: demo-container
          image: registry.access.redhat.com/ubi8/ubi:latest
          command: [ "/bin/sh", "-c", "--" ]
          args: [ "while true; do sleep 30; done;" ]
          volumeMounts:
            - name: demo-volume-file-system
              mountPath: "/data"
        volumes:
        - name: demo-volume-file-system
          persistentVolumeClaim:
            claimName: demo-pvc-file-system
   ```
   {: codeblock}

1. Create the pod in your cluster.
   ```sh
   oc apply -f <stateful-set>.yaml
   ```
   {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.
   ```sh
   oc get pods
   ```
   {: pre}

   Example output:
   ```sh
   NAME                                READY   STATUS    RESTARTS   AGE
   demo-statefulset-file-system-0       1/1     Running   0          2m58s
   ```
   {: screen}

1. Verify that the app can write to your IBM Block Storage instance.
   1. Log in to your pod.
      ```sh
      oc exec demo-statefulset-file-system-0 -it bash
      ```
      {: pre}

   1. Change to the `/data` directory, write a `test.txt` file, and view the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
      ```sh
      cd data && echo "Testing" >> test.txt && cat test.txt
      ```
      {: pre}

      Example output:
      ```sh
      Testing
      ```
      {: screen}

   1. Exit the pod.
      ```sh
      exit
      ```
      {: pre}


<br />


