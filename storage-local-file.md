---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-15"

keywords: file storage, satellite storage, local file storage, satellite config, satellite configurations,

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


# Local Storage Operator - File
{: #config-storage-local-file}

Set up [Persistent storage using local volumes](https://docs.openshift.com/container-platform/4.6/storage/persistent_storage/persistent-storage-local.html#create-local-pvc_persistent-storage-local){: external} for {{site.data.keyword.satelliteshort}} clusters.You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. After you create a storage configuration, you can assign it to your clusters. When you assign a storage configuration to your clusters, the storage drivers for the provider that you used to create your configuration are installed on your cluster.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

<br />

## Prerequisites
Before you can create a local file storage configuration, you must identify the worker nodes in your clusters that have the required available disks. Once you identify the worker nodes that you want to use in your configuration, you must add a label to those worker nodes. When the local storage drivers are installed in your cluster, they are installed only on the worker nodes that have the label you specified.

- [Get the device details of your worker nodes](#sat-storage-file-local-devices). If your worker nodes do not have an available disk, add worker nodes to your cluster or add disks to your worker nodes.
- [Label the worker nodes](#sat-storage-file-local-labels) that have an available disk and that you want to use in your configuration.


<br />
### Getting the device details for your local file storage configuration
{: #sat-storage-file-local-devices}

When you create your file storage configuration, you must specify which devices that you want to use. The device paths that you retrieve in the following steps are specified as parameters when you create your configuration.

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

4. Review the command output for available disks. Disks that can be used for your local file storage configuration must be unmounted. In the following example output from the `lsblk` command, the `sdc` disk unformatted partitions that we can use for local file storage configuration.
    ```
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
    {: screen}

6. Repeat the previous steps for each worker node that you want to use for your local file storage configuration.


<br />
### Labeling your worker nodes
{: #sat-storage-file-local-labels}

After you have [retrieved the device paths for the disks that you want to use in your configuration](#sat-storage-file-local-devices), label the worker nodes where the disks are located.
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


<br />



<br />

## Creating a local file storage configuration in the command line
{: #sat-storage-local-file-cli}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
2. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).
3. Review the [Local file storage configuration parameters](#sat-storage-local-file-params-cli).
4. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name local-volume-file --template-version <template-version> --param "label-key=<label-key>" --param "lable-value=<label-value>" --param "devicepath=<devicepath>" --param "fstype=<fstype>"
  ```
  {: pre}
5. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}
6. [Assign your configuration to clusters](/docs/satellite?topic=satellite-config-storage-local-file#assign-storage-local-file).


<br />
### Local file storage configuration parameter reference
{: #sat-storage-local-file-params-cli}

| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `local-volume-file`. |
| `--template-version` | Required | Enter the version of the `local-volume-file` template that you want to use. The template version that you specify must match your OCP version. For example, if your OCP version is `4.5.X`, specify template version `4.5`.  To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `label-key` | Required | Enter the node label key that you added to the worker nodes where you want to install the local storage drivers. The local storage drivers are installed only on the worker nodes that have the corresponding label. |
| `label-value` | Required | Enter the node label value that you added to the worker nodes where you want to install the local storage driver. The local storage drivers are installed only on the worker nodes that have the corresponding label. |
| `devicepath` | Required | Enter the local storage device paths. For more information on how to retrieve this value, see [Getting the device details](#sat-storage-file-local-devices). Example: `/dev/sdc`. |
| `fstype` | Required | Enter the file system type that you want to use on your local disks. The supported file system types are: `xfs`, `ext`, `ext3`, and `ext4`.  Example: `ext4`. |
{: caption="Table 1. Local file storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates if the parameter is a required parameter. The third column is a brief description of the parameter."}


<br />


## Assigning your storage configuration to a cluster
{: #assign-storage-local-file}

After you [create a local {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-local-file), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />



### Assigning a storage configuraton in the command line
{: #assign-storage-local-file-cli}

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
5. Verify that the storage configuration resources are deployed. Get a list of all the resources in the `local-storage` namespace.

  ```sh
  oc get all -n local-storage
  ```
  {: pre}

  Example output:
  ```sh
  NAME                                         READY   STATUS    RESTARTS   AGE
  pod/local-disk-local-diskmaker-cpk4r         1/1     Running   0          30s
  pod/local-disk-local-provisioner-xstjh       1/1     Running   0          30s
  pod/local-storage-operator-96c444dfc-ttpmq   1/1     Running   0          35s

  NAME                             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)     AGE
  service/local-storage-operator   ClusterIP   172.21.173.238   <none>        60000/TCP   32s

  NAME                                          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
  daemonset.apps/local-disk-local-diskmaker     1         1         1       1            1           <none>          31s
  daemonset.apps/local-disk-local-provisioner   1         1         1       1            1           <none>          31s

  NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
  deployment.apps/local-storage-operator   1/1     1            1           36s

  NAME                                               DESIRED   CURRENT   READY   AGE
  replicaset.apps/local-storage-operator-96c444dfc   1         1         1       37s
  ```
  {: screen}

6. List the storage classes that are available.
  ```sh
  oc get sc -n local-storage | grep local
  ```
  {: pre}

  Example output:
  ```sh
  sat-local-file-gold       kubernetes.io/no-provisioner   Delete          WaitForFirstConsumer   false                  21m
  ```
  {: pre}

7. List the PVs and verify that the status is `Available`. The local disks that you specified when you created your configuration are available as persistent volumes.
  ```sh
  oc get pv
  ```
  {: pre}

  Example output:
  ```sh
  NAME               CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS          REASON   AGE
  local-pv-1d14680   50Gi       RWO            Delete           Available           sat-local-file-gold            50s
  ```
  {: screen}

8. [Create a PVC that references your local PV, then deploy an app that uses your local storage](#deploy-app-local-file).


<br />
## Deploying an app that uses your local file storage
{: #deploy-app-local-file}

After you create a local file storage configuration and assign it to your clusters, you can then create an app that uses your local file storage.
{: shortdesc}

1. Save the following YAML to a file on your local machine called `local-pvc.yaml`.
  ```yaml
  kind: PersistentVolumeClaim
  apiVersion: v1
  metadata:
    name: local-pvc
  spec:
    accessModes:
    - ReadWriteOnce
    volumeMode: Filesystem
    resources:
      requests:
        storage: 50Gi
    storageClassName: sat-local-file-gold
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
        volumeMounts:
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
  kubectl exec <local-pv-pod> -it bash
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
{: #local-file-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for local file storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the storage class YAML in [GitHub](https://github.com/IBM/ibm-satellite-storage/tree/develop/config-templates/redhat/local-volume-file).
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-local-file-gold` | `ext4` or `xfs` | Retain |
{: caption="Table 2. Local file storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}
