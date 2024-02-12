---


copyright:
  years: 2023, 2024
lastupdated: "2024-02-12"

keywords: satellite storage, local file storage, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Local Storage - File and Block
{: #storage-local-storage}

Set up persistent storage using local block or file volumes for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

## Prerequisites
{: #sat-storage-local-prereqs}

Before you can create a local storage configuration, you must identify the worker nodes in your clusters that have the required available disks. Then, label these worker nodes so that the local storage drivers are installed on only these worker nodes. 

1. Make sure you have the following permissions.
    - **Editor** for the Billing service.
    - **Manager** and **Editor** for Kubernetes service.
    - **Satellite Link Administrator** and **Reader** for the {{site.data.keyword.satelliteshort}} service.
1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
    - Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage.

1. [Deploy the Local Storage Operator](/docs/satellite?topic=satellite-storage-local-storage-operator&interface=ui). To set up local file or block storage, you must deploy the Local Storage Operator.

1. Ensure that the worker nodes in your cluster that you want to use in your storage configuration have at least one available local disk in addition to the disks required by {{site.data.keyword.satelliteshort}}. The extra disks must be unformatted. 

1. [Get the device details of your worker nodes](#sat-storage-local-devices).
1. [Label the worker nodes](#sat-storage-local-labels) that have an available disk and that you want to use in your configuration. The local storage drivers are installed only on the labeled worker nodes.

### Getting the device details for your local storage configuration
{: #sat-storage-local-devices}

When you create your local storage configuration, you must specify which devices that you want to use. The device paths that you retrieve in the following steps are specified as parameters when you create your configuration.
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

5. Repeat the previous steps for each worker node that you want to use for your local storage configuration.  



### Labeling your worker nodes
{: #sat-storage-local-labels}

After you have [retrieved the device paths for the disks that you want to use in your configuration](#sat-storage-local-devices), label the worker nodes where the disks are located.
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

    Example output

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





## Creating and assigning a configuration in the console
{: #local-storage-config-create-console}
{: ui}


1. Review the [parameter reference](#local-storage-parameter-reference).


1. [From the Locations console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type**.
1. Select the **Version** and click **Next**
1. If the **Storage type** that you selected accepts custom parameters, enter them on the **Parameters** tab.
1. If the **Storage type** that you selected requires secrets, enter them on the **Secrets** tab.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

## Creating a configuration in the CLI
{: #local-storage-config-create-cli}
{: cli}


1. Review the [parameter reference](#local-storage-parameter-reference) for the template version that you want to use.


1. Log in to the {{site.data.keyword.cloud_notm}} CLI.

    ```sh
    ibmcloud login
    ```
    {: pre}

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.

    ```sh
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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-create-cli).


    Example command to create a version 1.0.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name local-storage --template-version 1.0.0 --param "install-local-storage-file=INSTALL-LOCAL-STORAGE-FILE"  --param "auto-discover-devices-file=AUTO-DISCOVER-DEVICES-FILE"  [--param "file-nodes-label-key=FILE-NODES-LABEL-KEY"]  [--param "file-nodes-label-value=FILE-NODES-LABEL-VALUE"]  [--param "file-devicepath=FILE-DEVICEPATH"]  --param "fstype=FSTYPE"  --param "install-local-storage-block=INSTALL-LOCAL-STORAGE-BLOCK"  --param "auto-discover-devices-block=AUTO-DISCOVER-DEVICES-BLOCK"  [--param "block-nodes-label-key=BLOCK-NODES-LABEL-KEY"]  [--param "block-nodes-label-value=BLOCK-NODES-LABEL-VALUE"]  [--param "block-devicepath=BLOCK-DEVICEPATH"] 
    ```
    {: pre}



1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}

## Creating a configuration in the API
{: #local-storage-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#local-storage-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.0.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"local-storage\", \"storage-template-version\": \"1.0.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"INSTALL-LOCAL-STORAGE-FILE\", { \"entry.name\": \"AUTO-DISCOVER-DEVICES-FILE\", { \"entry.name\": \"FILE-NODES-LABEL-KEY\", { \"entry.name\": \"FILE-NODES-LABEL-VALUE\", { \"entry.name\": \"FILE-DEVICEPATH\", { \"entry.name\": \"FSTYPE\", { \"entry.name\": \"INSTALL-LOCAL-STORAGE-BLOCK\", { \"entry.name\": \"AUTO-DISCOVER-DEVICES-BLOCK\", { \"entry.name\": \"BLOCK-NODES-LABEL-KEY\", { \"entry.name\": \"BLOCK-NODES-LABEL-VALUE\", { \"entry.name\": \"BLOCK-DEVICEPATH\",\"user-secret-parameters\": }
    ```
    {: pre}








{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-upgrade-api}}
{{site.data.content.assignment-autopatch-api}}




## Deploying an app that uses local storage
{: #deploy-app-local}


After you create a local storage configuration and assign it to your clusters, you can then create an app that uses your local block storage.
{: shortdesc}

You can map your PVCs to specific persistent volumes by adding labels to your persistent volumes. For more information, see the [Kubernetes documentation for selectors](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#selector){: external}.
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

    To ensure that your pods are scheduled to worker nodes with storage, or to ensure that the apps that require storage are not preempted by other pods, you can specify `nodeAffinity` and set up pod priority. For more information, see the Kubernetes documentation for [pod priority and preemption](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/){: external} and setting [node affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-isolation-restriction){: external}.
    {: tip}

4. Deploy an app pod that uses your local storage PVC. Save the following example app YAML as a file on your local machine called `app.yaml`. In this example, the `nodeAffinity` spec ensures that this pod is scheduled only to a worker node with the specified label.

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

8. Run the `ls -lR <device-path>` command to verify your device details and that your app pod can read and write permissions to your block device, which is indicated by `brw` in the command output.

    ```sh
    ls -lR /dev/nvme2n1
    ```
    {: pre}

    Example output

    ```sh
    brw-rw-rw-. 1 root disk 202, 32 Mar  3 21:24 /dev/nvme2n1
    ```
    {: screen}

9. **Optional** Run the following commands to write data to your block device.

    1. Write `"block_data"` to the local storage device that you mounted to your app. Replace `<device-path>` with the path to your storage device. Example: `/dev/nvme2n1`.
    
        ```sh
        kubectl exec <pod_name> -- bash -c "echo "block_data" | dd conv=unblock of=<device-path>"
        ```
        {: pre}

    2. Verify the data is written to your device. Replace `<device-path>` with the path to your storage device. Example: `/dev/nvme2n1`.

        ```sh
        kubectl exec <pod_name> -- bash -c "od -An -c -N 10 <device-path>"
        ```
        {: pre}

        Example output
        
        ```sh
        b   l   o   c   k   _   d   a   t   a
        ```
        {: screen}



10. Delete the `test` pod.

    ```sh
    oc delete pod <pod_name>
    ```
    {: pre}

{{site.data.content.configuration-upgrade-manual-cli}}
{{site.data.content.configuration-upgrade-console2}}


{{site.data.content.configuration-remove-console}}

## Removing local storage configuration from the command line
{: #rm-local-temp-cli}
{: cli}

1. List the resources in the `local-storage` namespace. When you delete your storage assignment, these resources are removed.

    ```sh
    oc get all -n local-storage
    ```
    {: pre}

    Example output
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
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

1. Remove the assignment. After the assignment is removed, the local storage driver pods and storage classes are removed from all clusters that were part of the storage assignment. 

    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

1. List the resources in the `local-storage` namespace and verify that the local storage driver pods are removed. 

    ```sh
    oc get all -n local-storage
    ```
    {: pre}

    Example output

    ```sh
    No resources found in local-storage namespace.
    ```
    {: pre}

1. List of the storage classes in your cluster and verify that the local storage classes are removed. 

    ```sh
    oc get sc
    ```
    {: pre}

1. Optional: Remove the storage configuration. 

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

1. List your PVCs and note the name of the PVC that you want to remove. 

    ```sh
    oc get pvc
    ```
    {: pre}

1. Remove any pods that currently mount the PVC. 


    1. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC. 
    
        ```sh
        oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
        ```
        {: pre}

        Example output
        
        ```sh
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

1. Delete the PVC. Because all {{site.data.keyword.IBM_notm}}-provided local block storage classes are specified with a `Retain` reclaim policy, the PV and PVC are not automatically deleted when you delete your app or deployment. 

    ```sh
    oc delete pvc <pvc-name>
    ```
    {: pre}

1. Verify that your PVC is removed.

    ```sh
    oc get pvc
    ```
    {: pre}

1. List your PVs and note the name of the PVs that you want to remove.

    ```sh
    oc get pv
    ```
    {: pre}

1. Delete the PVs. Deleting your PVs will make your disks available for other workloads.

    ```sh
    oc delete pv <pv-name>
    ```
    {: pre}

1. Verify that your PV is removed.

    ```sh
    oc get pv
    ```
    {: pre}




## Parameter reference
{: #local-storage-parameter-reference}

### 1.0.0 parameter reference
{: #local-storage-1.0.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Install file storage driver | `install-local-storage-file` | Config | Set to `true` to install the file storage driver. | true | `true` |
| Automatic volume discovery for file storage | `auto-discover-devices-file` | Config | Set to `true` if you want to automatically discover and use the volumes on your worker nodes for file storage. | true | `false` |
| File storage worker node label key | `file-nodes-label-key` | Config | The `key` of the worker node `key=value` label that you want to use for file storage. | false | N/A |
| File storage worker node label value | `file-nodes-label-value` | Config | The `value` of the worker node `key=value` label that you want to use for file storage. | false | N/A |
| Device path for file storage | `file-devicepath` | Config | The path to the storage devices on your worker node that you want to use for file storage. Example: `/dev/sdc`. This option is required when `auto-discover-devices-file` is set to `false`. | false | N/A |
| File system type | `fstype` | Config | The file system type. Specify `ext3`, `ext4`, or `xfs`. | true | `ext4` |
| Install block storage driver | `install-local-storage-block` | Config | Set to `true` to install the block storage driver. | true | `true` |
| Automatic volume discovery for block storage | `auto-discover-devices-block` | Config | Set to `true` if you want to automatically discover and use the volumes on your worker nodes for block storage. | true | `false` |
| Block storage worker node label key | `block-nodes-label-key` | Config | The `key` of the worker node `key=value` label that you want to use for block storage. | false | N/A |
| Block storage worker node label value | `block-nodes-label-value` | Config | The `value` of the worker node `key=value` label that you want to use for block storage. | false | N/A |
| Device Path for Local Storage Block | `block-devicepath` | Config | The path to the storage devices on your worker node that you want to use for block storage. Example: `/dev/sdc`. This option is required when `auto-discover-devices-block` is set to false. | false | N/A |
{: caption="Table 1. 1.0.0 parameter reference" caption-side="bottom"}


