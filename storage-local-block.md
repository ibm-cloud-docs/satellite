---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-19"

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

Set up [Persistent storage using local volumes](https://docs.openshift.com/container-platform/4.6/storage/persistent_storage/persistent-storage-local.html#create-local-pvc_persistent-storage-local){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

When you create a local block storage configuration, you specify the local block storage device paths that you want to make available as persistent volumes (PVs) in your clusters. After you assign the storage configuration to a cluster, {{site.data.keyword.satelliteshort}} deploys the local storage operator which mounts the local disks that you specified in your configuration. The operator further creates the local persistent volumes, and creates the `sat-local-block-gold` storage class which you can use to create persistent volumes claims (PVCs). You can then reference your PVCs in your Kubernetes workloads.


The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

<br />

## Prerequisites
{: #sat-storage-local-prereqs}

Before you can create a local block storage configuration, you must identify the worker nodes in your clusters that have the required available disks. Then, label these worker nodes so that the local storage drivers are installed on only these worker nodes.
1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
2. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters). Ensure that the worker nodes in your cluster that you want to use in your storage configuration have at least one available local disk in addition to the disks required by {{site.data.keyword.satelliteshort}}. The extra disks must be unformatted. 
3. [Get the device details of your worker nodes](#sat-storage-block-local-devices).
4. [Label the worker nodes](#sat-storage-block-local-labels) that have an available disk and that you want to use in your configuration. The local storage drivers are installed only on the labeled worker nodes.


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


4. List available block storage disks on your worker node. You must use unmounted disks for the local storage configuration. In the following example output from the `lsblk` command, the `nvme2n1` disk is unmounted and has no partitions.
    ```sh
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    nvme0n1     259:3    0   100G  0 disk 
    |-nvme0n1p1 259:4    0     1M  0 part 
    `-nvme0n1p2 259:5    0   100G  0 part /
    nvme1n1     259:0    0    20G  0 disk 
    nvme2n1     259:1    0    20G  0 disk 
    nvme3n1     259:2    0 139.7G  0 disk /var/data
    ```
    {: screen}

6. Repeat the previous steps for each worker node that you want to use for your local block storage configuration.  


<br />
### Labeling your worker nodes
{: #sat-storage-block-local-labels}
After you have [retrieved the device paths for the disks that you want to use in your configuration](#sat-storage-block-local-devices), label the worker nodes where the disks are located.
{: shortdesc}

1. Get the worker node IP addresses.
  ```sh
  oc get nodes
  ```
  {: pre}

2. Label the worker nodes that you retrieved earlier. The local storage drivers are deployed to the worker nodes with this label. You can use the `storage=local-block` label in the example command or you can create your own label in the `key=value` format.
  ```sh
  oc label nodes <worker-IP> <worker-IP> <worker-IP> "storage=local-block"
  ```
  {: pre}

  Example output:
  ```sh
  node/<worker-IP> labeled
  node/<worker-IP> labeled
  node/<worker-IP> labeled
  ```
  {: screen}

3. Verify that the label is added to the worker nodes that you want to use. Run the following command to display the labels on your worker nodes and highlight the label that you added previous step.
  ```sh
  oc get nodes --show-labels | grep --color=always storage=local-block
  ```
  {: pre}




## Creating a local block storage configuration in the command line
{: #sat-storage-local-block-cli}

1. Review the [Local block storage configuration parameters](#sat-storage-local-block-params-cli).
2. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name local-volume-block --template-version <template-version> --param "label-key=storage" --param "label-value=local-block" --param "devicepath=<devicepath>"
  ```
  {: pre}
3. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}
4. [Assign your configuration to clusters](#assign-storage-local-block).


<br />

## Assigning your storage configuration to a cluster
{: #assign-storage-local-block}

After you [create a local block storage configuration](#config-storage-local-block), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />




### Assigning a storage configuration in the command line
{: #assign-storage-local-block-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

2. List your {{site.data.keyword.satelliteshort}} cluster groups and note the group that you want to assign storage. Note that the clusters in the cluster group where you want to assign storage must all be in the same {{site.data.keyword.satelliteshort}} location. If you have not created a cluster group, see [Setting up cluster groups](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-groups).
  ```sh
  ibmcloud sat group ls
  ```
  {: pre}

3. Get the details of your cluster group and verify the clusters where you want to deploy your storage configuration. To attach clusters to your group, run the `ic sat group attach` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach).
  ```sh
  ibmcloud sat group get <group-name>
  ```
  {: pre}

4. Assign storage to the clusters that you retrieved in step 2. Replace `<group>` with the name of your cluster group, `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).
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
   
   **Example output**:

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
   local-pv-88842685   20Gi       RWO            Delete           Available           sat-local-block-gold            90s
   ```
   {: screen}

8. [Create a PVC that references your local PV, then deploy an app that uses your local storage](#deploy-app-local-block).


<br />
## Deploying an app that uses your local block storage
{: #deploy-app-local-block}

After you create a local block storage configuration and assign it to your clusters, you can then create an app that uses your local block storage.
{: shortdesc}

You can map your PVCs to specific persistent volumes by adding labels to your persistent volumes. For more information, see the [Kubernetes documentation for selectors](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#selector).
{: tip}

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
        storage: 20Gi # Important: Ensure that size of your claim is not larger than the local disk.
    storageClassName: sat-local-block-gold
  ```
  {: codeblock}

2. Create the PVC in your cluster.
  ```sh
  oc create -f local-pvc.yaml
  ```
  {: pre}

3. Verify that your PVC is created. Note that the `volumeBindingMode` for the `sat-local-block-gold` storage class is `waitForFirstConsumer`.
  ```sh
  oc get pvc | grep local
  ```
  {: pre}

  To ensure that your pods are scheduled to worker nodes with storage, or to ensure that the apps that require storage are not pre-empted by other pods, you can specify `nodeAffinity` and set up pod priority. For more information, see the Kubernetes documentation for [pod priority and preemption](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/) and setting [node affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-isolation-restriction).
  {: tip}

4. Deploy an app pod that uses your local storage PVC. Save the following example app YAML as a file on your local machine called `app.yaml`. In this example, the `nodeAffinity` spec ensures that this pod is only scheduled to a worker node with the specified label.
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: app
  spec:
    affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: storage # Enter the 'key' of the worker node label created earlier.
            operator: In
            values:
            - local-block # Enter the 'value' of the worker label that you created earlier.
    containers:
      - name: nginx
        image: nginx
        volumeDevices:
          - name: data
            devicePath: "/dev/nvme2n1" # Enter the path to your local device.
    volumes:
      - name: data
        persistentVolumeClaim:
          claimName: local-pvc
  ```
  {: codeblock}

5. Create the app pod in your cluster.
  ```sh
  oc create -f app.yaml
  ```
  {: pre}

6. Log in to your app pod and verify that you can write to your local disk.
  ```sh
  kubectl exec <pod_name> -it bash
  ```
  {: pre}

7. Change directories to the `dev` folder.
  ```sh
  cd dev
  ```
  {: pre}

8. Run the `ls -lR <device-path>` command to verify your device details and that your app pod can has read and write permissions to your block device which is indicated by `brw` in the command output.
  ```sh
  ls -lR /dev/nvme2n1
  ```
  {: pre}

  **Example output**
  ```sh
  brw-rw-rw-. 1 root disk 202, 32 Mar  3 21:24 /dev/nvme2n1
  ```
  {: screen}

9. **Optional** Run the following commands to write data to your block device.
  1. Write "block_data" to the local storage device that you mounted to your app. Replace `<device-path>` with the path to your storage device. Example: `/dev/nvme2n1`.
    ```sh
    kubectl exec <pod_name> -- bash -c "echo "block_data" | dd conv=unblock of=<device-path>"
    ```
    {: pre}

  2. Verify the data is written to your device. Replace `<device-path>` with the path to your storage device. Example: `/dev/nvme2n1`.
    ```sh
    kubectl exec <pod_name> -- bash -c "od -An -c -N 10 <device-path>"
    ```
    {: pre}

    **Example output**:
    ```sh
    b   l   o   c   k   _   d   a   t   a
    ```
    {: screen}

11. Delete the `test` pod.
  ```sh
  oc delete pod <pod_name>
  ```
  {: pre}

<br />

## Removing the local block storage configuration from your cluster
{: #sat-storage-remove-local-block-config}

If you no longer plan on using local block storage in your cluster, you can unassign your cluster from the storage configuration. 
{: shortdesc}

Removing the storage configuration, uninstalls the local storage operator resources and the `sat-local-block-gold` storage class from all assigned clusters. Your PVCs, PVs and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again. 
{: important}



1. List the resources in the `local-storage` namespace. When you delete your storage assignment, these resources are removed.
  ```sh
  oc get all -n local-storage
  ```
  {: pre}

  **Example output**
  ```sh
  NAME                                         READY   STATUS    RESTARTS   AGE
  pod/local-disk-local-diskmaker-clvg6         1/1     Running   0          29h
  pod/local-disk-local-diskmaker-kqddq         1/1     Running   0          29h
  pod/local-disk-local-diskmaker-p6z9q         1/1     Running   0          29h
  pod/local-disk-local-provisioner-dw5g7       1/1     Running   0          29h
  pod/local-disk-local-provisioner-hxd9n       1/1     Running   0          29h
  pod/local-disk-local-provisioner-tfg95       1/1     Running   0          29h
  pod/local-storage-operator-df4994656-7826l   1/1     Running   0          29h

  NAME                             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)     AGE
  service/local-storage-operator   ClusterIP   172.21.147.17   <none>        60000/TCP   29h

  NAME                                          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
  daemonset.apps/local-disk-local-diskmaker     3         3         3       3            3           <none>          29h
  daemonset.apps/local-disk-local-provisioner   3         3         3       3            3           <none>          29h

  NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
  deployment.apps/local-storage-operator   1/1     1            1           29h

  NAME                                               DESIRED   CURRENT   READY   AGE
  replicaset.apps/local-storage-operator-df4994656   1         1         1       29h
  ```
  {: pre}

1. List your storage assignments and find the one that you used for your cluster. 
  ```sh
  ibmcloud sat storage assignment ls
  ```
  {: pre}

2. Remove the assignment. After the assignment is removed, the local storage driver pods and storage classes are removed from all clusters that were part of the storage assignment. 
  ```sh
  ibmcloud sat storage assignment rm --assignment <assignment_name>
  ```
  {: pre}

3. List the resources in the `local-storage` namespace and verify that the local storage driver pods are removed. 
  ```sh
  oc get all -n local-storage
  ```
  {: pre}

  **Example output**:
  ```sh
  No resources found in local-storage namespace.
  ```
  {: pre}

4. List of the storage classes in your cluster and verify that the local storage classes are removed. 
  ```sh
  oc get sc
  ```
  {: pre}

5. Optional: Remove the storage configuration. 
  1. List the storage configurations. 
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

  2. Remove the storage configuration.
    ```sh
    ibmcloud sat storage config rm --config <config_name>
    ```
    {: pre}

6. List your PVCs and note the name of the PVC that you want to remove. 
  ```sh
  oc get pvc
  ```
  {: pre}

7. Remove any pods that currently mount the PVC. 
  1. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC. 
    ```sh
    oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
    ```
    {: pre}

    Example output: 
    ```
    app    sat-local-block-gold
    ```
    {: screen}

  2. Remove the pod that uses the PVC. If the pod is part of a deployment, remove the deployment.
    ```sh
    oc delete pod <pod_name>
    ```
    {: pre}

    ```sh
    oc delete deployment <deployment-name>
    ```
    {: pre}

  3. Verify that the pod or the deployment is removed. 
    ```sh
    oc get pods
    ```
    {: pre}

    ```sh
    oc get deployments
    ```
    {: pre}

8. Delete the PVC. Because all IBM-provided local block storage classes are specified with a `Retain` reclaim policy, the PV and PVC are not automatically deleted when you delete your app or deployment. 
  ```sh
  oc delete pvc <pvc-name>
  ```
  {: pre}

9. Verify that your PVC is removed.
  ```sh
  oc get pvc
  ```
  {: pre}

10. List your PVs and note the name of the PVs that you want to remove.
  ```sh
  oc get pv
  ```
  {: pre}

11. Delete the PVs. Deleting your PVs will make your disks available for other workloads.
  ```sh
  oc delete pv <pv-name>
  ```
  {: pre}

12. Verify that your PV is removed.
  ```sh
  oc get pv
  ```
  {: pre}
  



<br />

## Local block storage configuration parameter reference
{: #sat-storage-local-block-params-cli}

| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `local-volume-block`. |
| `--template-version` | Required | Enter the version of the `local-volume-block` template that you want to use. The template version that you specify must match your OCP version. For example, if your OCP version is `4.5.X`, specify template version `4.5`. To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `label-key` | Required | Enter the node label key that you added to the worker nodes where you want to install the local storage drivers. The local storage drivers are installed only on the worker nodes that have the corresponding label. In the previous example, the label key is `storage`. |
| `label-value` | Required | Enter the node label value that you added to the worker nodes where you want to install the local storage driver. The local storage drivers are installed only on the worker nodes that have the corresponding label. In the previous example, the label value is `local-block`. |
| `devicepath` | Required | Enter the local storage device paths in the format `/dev/<device>`. The device path begins with `/dev` and includes the disk that you retrieved earlier. For example: `/dev/nvme2n1`. If you specify more than one device path, be sure to separate them with a comma and do not enter any additional spaces between each path. For example: `/dev/nvme2n1`,`/dev/nvme3n1`. For more information on how to retrieve this value, see [Getting the device details](#sat-storage-block-local-devices). |
{: caption="Table 1. Local block storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates if the parameter is a required parameter. The third column is a brief description of the parameter. The third column is the default value of the parameter."}

<br />

## Storage class reference
{: #local-block-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the storage class YAML in [GitHub](https://github.com/IBM/ibm-satellite-storage/tree/develop/config-templates/redhat/local-volume-block).
{: shortdesc}

| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `sat-local-block-gold ` | Block | Retain |
{: caption="Table 2. Local block storage class reference" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the reclaim policy."}
