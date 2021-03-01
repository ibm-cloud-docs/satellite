---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-01"

keywords: block storage, satellite storage, local block storage, satellite config, satellite configurations, 

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


# Local Storage Operator - Block
{: #config-storage-local-block}

Set up [Persistent storage using local volumes](https://docs.openshift.com/container-platform/4.6/storage/persistent_storage/persistent-storage-local.html#create-local-pvc_persistent-storage-local){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. After you create a storage configuration, you can assign it to your clusters. When you assign a storage configuration to your clusters, the storage drivers for the provider that you used to create your configuration are installed on your cluster.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

<br />

## Prerequisites
{: #sat-storage-local-prereqs}

Before you can create a local block storage configuration, you must identify the worker nodes in your clusters that have the required available disks. Then, label these worker nodes so that the local storage drivers are installed on only these worker nodes.
1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
2. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters). Ensure that the worker nodes in your cluster have at least one available local disk.
3. [Get the device details of your worker nodes](#sat-storage-block-local-devices).
4. [Label the worker nodes](#sat-storage-file-local-labels) that have an available disk and that you want to use in your configuration.


<br />
### Getting the device details for your local block storage configuration
{: #sat-storage-block-local-devices}

When you create your local block storage configuration, you must specify which devices that you want to use. The device paths that you retrieve in the following steps are specified as parameters when you create your configuration.
{: shortdesc}

1. Log in to your cluster and get a list of available worker nodes. Make a note of the worker nodes that you want to use in your configuration.
    ```sh
    oc get nodes
    ```
    {: pre}

2. Log in to each worker node that you want to use for your local storage configuration. 
    ```sh
    oc debug node/<node-name>
    ```
    {: pre}
    
3. When the debug pod is deployed on the worker node, run the following commands to list the available disks on the worker node.
  1. Allow host binaries.
    ```sh
    chroot /host
    ```
    {: pre}

  1. List your devices.
    ```sh
    lsblk
    ```
    {: pre}

  1. Get the details of your devices. Verify that the devices that you want to use are unmounted and unformatted.
    ```sh
    fdisk -l
    ```
    {: pre}

4. Review the command output for available disks. You must use unmounted disks for the local storage configuration. In the following example output from the `lsblk` command, the `sdc` disk is the only disk that has only unmounted partitions and can be used for local storage.
    ```sh
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   931G  0 disk
    |-sda1   8:1    0   256M  0 part /boot
    |-sda2   8:2    0     1G  0 part
    `-sda3   8:3    0 929.8G  0 part /
    sdb      8:16   0 744.7G  0 disk
    `-sdb1   8:17   0 744.7G  0 part /disk1
    sdc      8:32   0 744.7G  0 disk
    |-sdc1   8:33   0  18.6G  0 part
    `-sdc2   8:34   0 260.8G  0 part
    ```
    {: pre}

6. Repeat the previous steps for each worker node that you want to use for your local block storage configuration.  


<br />
### Labeling your worker nodes
{: #sat-storage-file-local-labels}
After you have [retrieved the device paths for the disks that you want to use in your configuration](#sat-storage-block-local-devices), label the worker nodes where the disks are located.
{: shortdesc}

1. Get the worker node IP addresses.
  ```sh
  oc get nodes
  ```
  {: pre}

2. Label the worker nodes that you retrieved earlier. The label that you add to your worker nodes is used to deploy the local storage drivers. The label must be in the format `key=value`.
  ```sh
  oc label nodes <worker-IP> <worker-IP> "key=value"
  ```
  {: pre}

  Example output:
  ```sh
  node/<worker-IP> labeled
  node/<worker-IP> labeled
  ```
  {: screen}



## Creating a local block storage configuration in the command line
{: #sat-storage-local-block-cli}

1. Review the [Local block storage configuration parameters](#sat-storage-local-params-cli).
2. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name local-volume-block --template-version <template-version> --param "label-key=<label-key>" --param "lable-value=<label-value>" --param "devicepath=<devicepath>"
  ```
  {: pre}
3. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}
4. [Assign your configuration to clusters](#assign-storage-local-block).


<br />
### Local block storage configuration parameter reference
{: #sat-storage-local-block-params-cli}

| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `local-volume-block`. |
| `--template-version` | Required | Enter the version of the `local-volume-block` template that you want to use. The template version that you specify must match your OCP version. For example, if your OCP version is `4.5.X`, specify template version `4.5`. To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `label-key` | Required | Enter the node label key that you added to the worker nodes where you want to install the local storage drivers. The local storage drivers are installed only on the worker nodes that have the corresponding label. |
| `label-value` | Required | Enter the node label value that you added to the worker nodes where you want to install the local storage driver. The local storage drivers are installed only on the worker nodes that have the corresponding label. |
| `devicepath` | Required | Enter the local storage device paths. For more information on how to retrieve this value, see [Getting the device details](#sat-storage-block-local-devices). |
{: caption="Table 1. Local block storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates if the parameter is a required parameter. The third column is a brief description of the parameter. The third column is the default value of the parameter."}


<br />


## Assigning your storage configuration to a cluster
{: #assign-storage-local-block}

After you [create a local block storage configuration](#config-storage-local-block), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />




### Assigning a storage configuraton in the command line
{: #assign-storage-local-block-cli}

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
5. List all of the resources in the `local-storage` namespace and verify the driver pods are `Running`.
   ```sh
   oc get all -n local-storage
   ```
   {: pre}
   
   Example output:
   ```sh
   NAME                                         READY   STATUS    RESTARTS   AGE
   pod/local-disk-local-diskmaker-qdrjs         1/1     Running   0          100s
   pod/local-disk-local-provisioner-b6v4n       1/1     Running   0          100s
   pod/local-storage-operator-96c444dfc-m25g7   1/1     Running   0          104s

   NAME                             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)     AGE
   service/local-storage-operator   ClusterIP   172.21.92.28   <none>        60000/TCP   101s

   NAME                                          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
   daemonset.apps/local-disk-local-diskmaker     1         1         1       1            1           <none>          101s
   daemonset.apps/local-disk-local-provisioner   1         1         1       1            1           <none>          101s

   NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
   deployment.apps/local-storage-operator   1/1     1            1           106s

   NAME                                               DESIRED   CURRENT   READY   AGE
   replicaset.apps/local-storage-operator-96c444dfc   1         1         1       106s

   ```
   {: screen}
   
6. List the PVs and verify that the status is `Available`.
   ```sh
   oc get pv
   ```
   {: pre}

   Example output:
   ```sh
   NAME                CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS           REASON   AGE
   local-pv-88842685   50Gi       RWO            Delete           Available           sat-local-block-gold            90s
   ```
   {: screen}

8. [Create a PVC that references your local PV, then deploy an app that uses your local storage](#deploy-app-local-block).


<br />
## Deploying an app that uses your local block storage
{: #deploy-app-local-block}

After you create a local file storage configuration and assign it to your clusters, you can then create an app that uses your local file storage.
{: shortdesc}

1. Save the following PVC YAML file on your local machine called `local-pvc.yaml`.
  ```yaml
  kind: PersistentVolumeClaim
  apiVersion: v1
  metadata:
    name: local-pvc 
  spec:
    accessModes:
    - ReadWriteOnce
    volumeMode: Block
    resources:
      requests:
        storage: 50Gi 
    storageClassName: sat-local-block-gold
  ```
  {: codeblock}

2. Create the PVC in your cluster.
  ```sh
  oc create -f local-pvc.yaml
  ```
  {: pre}

3. Verify that your PVC is created.
  ```sh
  oc get pvc | grep local
  ```
  {: pre}

4. Deploy an app pod that uses your local storage PVC. Save the following example app YAML as a file on your local machine called `app.yaml`.
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: local-pod
  spec:
    volumes:
      - name: localpvc
        persistentVolumeClaim:
          claimName: local-pvc 
    containers:
      - name: local-disks 
        image: nginx
        ports:
          - containerPort: 80
            name: "http-server"
        volumeDevices:
          - mountPath: /test
            name: localpvc
  ```
  {: codeblock}

5. Create the app pod in your cluster.
  ```sh
  oc create -f app.yaml
  ```
  {: pre}

6. Log in to your app pod and verify that you can write to your local disk.
  ```sh
  kubectl exec <local-pod> -it bash
  ```
  {: pre}

7. Change directories to the directory where you mounted the local disk inside the container. In this example, the disk is mounted at `/test`. Write a test.txt file to your local disk.
  ```sh
  cd /mnt/test && touch test.txt && echo "This is a test." >> test.txt
  ```
  {: pre}

8. View the test file that you created.
  ```sh
  cat test.txt
  ```
  {: pre}

  Example output:
  ```
  This is a test.
  ```
  {: screen}

9. Remove your test file and log out of the pod.
  ```sh
  rm test.txt && exit
  ```
  {: pre}


<br />

## Storage class reference
{: #local-block-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the storage class YAML in [GitHub](https://github.com/IBM/ibm-satellite-storage/blob/develop/config-templates/redhat/local-volume-block).
{: shortdesc}

| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `sat-local-block-gold ` | Block | Retain |
{: caption="Table 2. Local block storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}


