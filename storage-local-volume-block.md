---


copyright:
  years: 2020, 2025
lastupdated: "2025-07-09"

keywords: block storage, satellite storage, local block storage, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Local Storage Operator - Block
{: #storage-local-volume-block}

Set up Persistent storage using local volumes for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

When you create a local block storage configuration, you specify the local block storage device paths that you want to make available as persistent volumes (PVs) in your clusters. After you assign the storage configuration to a cluster, {{site.data.keyword.satelliteshort}} deploys the local storage operator which mounts the local disks that you specified in your configuration. The operator further creates the local persistent volumes, and creates the `sat-local-block-gold` storage class which you can use to create persistent volumes claims (PVCs). You can then reference your PVCs in your Kubernetes workloads.

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}


## Prerequisites for using local block storage
{: #storage-local-volume-block-prereqs}

Before you can create a local block storage configuration, you must identify the worker nodes in your clusters that have the required available disks. Then, label these worker nodes so that the local storage drivers are installed on only these worker nodes.

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).


1. Ensure that the worker nodes in your cluster that you want to use in your storage configuration have at least one available local disk in addition to the disks required by {{site.data.keyword.satelliteshort}}. The extra disks must be unformatted. 
1. [Get the device details of your worker nodes](#sat-storage-block-local-devices).
1. [Label the worker nodes](#sat-storage-block-local-labels) that have an available disk and that you want to use in your configuration. The local storage drivers are installed only on the labeled worker nodes.



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

5. Repeat the previous steps for each worker node that you want to use for your local block storage configuration.  



### Labeling your worker nodes when using local block storage
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
{: #local-volume-block-config-create-console}
{: ui}


1. Review the [parameter reference](#local-volume-block-parameter-reference).


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
{: #local-volume-block-config-create-cli}
{: cli}


1. Review the [parameter reference](#local-volume-block-parameter-reference) for the template version that you want to use.


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


    Example command to create a version 4.9 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name local-volume-block --template-version 4.9 --param "auto-discover-devices=AUTO-DISCOVER-DEVICES"  --param "label-key=LABEL-KEY"  --param "label-value=LABEL-VALUE"  --param "devicepath=DEVICEPATH" 
    ```
    {: pre}


    Example command to create a version 4.10 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name local-volume-block --template-version 4.10 --param "auto-discover-devices=AUTO-DISCOVER-DEVICES"  --param "label-key=LABEL-KEY"  --param "label-value=LABEL-VALUE"  --param "devicepath=DEVICEPATH" 
    ```
    {: pre}


    Example command to create a version 4.11 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name local-volume-block --template-version 4.11 --param "auto-discover-devices=AUTO-DISCOVER-DEVICES"  --param "label-key=LABEL-KEY"  --param "label-value=LABEL-VALUE"  --param "devicepath=DEVICEPATH" 
    ```
    {: pre}


    Example command to create a version 4.12 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name local-volume-block --template-version 4.12 --param "auto-discover-devices=AUTO-DISCOVER-DEVICES"  --param "label-key=LABEL-KEY"  --param "label-value=LABEL-VALUE"  --param "devicepath=DEVICEPATH" 
    ```
    {: pre}


    Example command to create a version 4.13 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name local-volume-block --template-version 4.13 --param "auto-discover-devices=AUTO-DISCOVER-DEVICES"  --param "label-key=LABEL-KEY"  --param "label-value=LABEL-VALUE"  --param "devicepath=DEVICEPATH" 
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
{: #local-volume-block-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#local-volume-block-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 4.9 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"local-volume-block\", \"storage-template-version\": \"4.9\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AUTO-DISCOVER-DEVICES\", { \"entry.name\": \"LABEL-KEY\", { \"entry.name\": \"LABEL-VALUE\", { \"entry.name\": \"DEVICEPATH\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 4.10 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"local-volume-block\", \"storage-template-version\": \"4.10\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AUTO-DISCOVER-DEVICES\", { \"entry.name\": \"LABEL-KEY\", { \"entry.name\": \"LABEL-VALUE\", { \"entry.name\": \"DEVICEPATH\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 4.11 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"local-volume-block\", \"storage-template-version\": \"4.11\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AUTO-DISCOVER-DEVICES\", { \"entry.name\": \"LABEL-KEY\", { \"entry.name\": \"LABEL-VALUE\", { \"entry.name\": \"DEVICEPATH\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 4.12 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"local-volume-block\", \"storage-template-version\": \"4.12\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AUTO-DISCOVER-DEVICES\", { \"entry.name\": \"LABEL-KEY\", { \"entry.name\": \"LABEL-VALUE\", { \"entry.name\": \"DEVICEPATH\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 4.13 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"local-volume-block\", \"storage-template-version\": \"4.13\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AUTO-DISCOVER-DEVICES\", { \"entry.name\": \"LABEL-KEY\", { \"entry.name\": \"LABEL-VALUE\", { \"entry.name\": \"DEVICEPATH\",\"user-secret-parameters\": }
    ```
    {: pre}










{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-autopatch-cli}}
{{site.data.content.assignment-upgrade-api}}

## Deploying an app that uses local block storage
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

    To ensure that your pods are scheduled to worker nodes with storage, or to ensure that the apps that require storage are not preempted by other pods, you can specify `nodeAffinity` and set up pod priority. For more information, see the Kubernetes documentation for [pod priority and preemption](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/) and setting [node affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-isolation-restriction).
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
{{site.data.content.configuration-upgrade-console}}

## Removing the local block storage configuration from your cluster
{: #sat-storage-remove-local-block-config}

If you no longer plan on using local block storage in your cluster, you can unassign your cluster from the storage configuration. 
{: shortdesc}

Note that if you remove the storage configuration, the local storage operator resources and the `sat-local-block-gold` storage class are then uninstalled from all assigned clusters. Your PVCs, PVs and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again. 
{: important}


### Removing the local block storage configuration from the console
{: #sat-storage-rm-local-block-ui}

Use the console to remove a storage configuration. 
{: shortdesc}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Removing the local block storage configuration from the command line
{: #rm-local-block-temp-cli}
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
{: #local-volume-block-parameter-reference}

### 4.9 parameter reference
{: #local-volume-block-4.9-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Automatic storage volume discovery | `auto-discover-devices` | Config | Set to `true` if you want to automatically discover and use the storage volumes on your worker nodes. | `false` |
| Node Label Key | `label-key` | Config | The `key` of the worker node `key=value` label. | N/A |
| Node Label Key Value | `label-value` | Config | The `value` of the worker node `key=value` label. | N/A |
| Device Path | `devicepath` | Config | The local storage device path. Example: `/dev/sdc`. Required when `auto-discover-devices` is set to `false`. | N/A |
{: caption="Table 1. 4.9 parameter reference" caption-side="bottom"}


### 4.10 parameter reference
{: #local-volume-block-4.10-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Automatic storage volume discovery | `auto-discover-devices` | Config | Set to `true` if you want to automatically discover and use the storage volumes on your worker nodes. | `false` |
| Node Label Key | `label-key` | Config | The `key` of the worker node `key=value` label. | N/A |
| Node Label Key Value | `label-value` | Config | The `value` of the worker node `key=value` label. | N/A |
| Device Path | `devicepath` | Config | The local storage device path. Example: `/dev/sdc`. Required when `auto-discover-devices` is set to false. | N/A |
{: caption="Table 2. 4.10 parameter reference" caption-side="bottom"}


### 4.11 parameter reference
{: #local-volume-block-4.11-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Automatic storage volume discovery | `auto-discover-devices` | Config | Set to `true` if you want to automatically discover and use the storage volumes on your worker nodes. | `false` |
| Node Label Key | `label-key` | Config | The `key` of the worker node `key=value` label. | N/A |
| Node Label Key Value | `label-value` | Config | The `value` of the worker node `key=value` label. | N/A |
| Device Path | `devicepath` | Config | The local storage device path. Example: `/dev/sdc`. Required when `auto-discover-devices` is set to false. | N/A |
{: caption="Table 3. 4.11 parameter reference" caption-side="bottom"}


### 4.12 parameter reference
{: #local-volume-block-4.12-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Automatic storage volume discovery | `auto-discover-devices` | Config | Set to `true` if you want to automatically discover and use the storage volumes on your worker nodes. | `false` |
| Node Label Key | `label-key` | Config | The `key` of the worker node `key=value` label. | N/A |
| Node Label Key Value | `label-value` | Config | The `value` of the worker node `key=value` label. | N/A |
| Device Path | `devicepath` | Config | The local storage device path. Example: `/dev/sdc`. Required when `auto-discover-devices` is set to false. | N/A |
{: caption="Table 4. 4.12 parameter reference" caption-side="bottom"}


### 4.13 parameter reference
{: #local-volume-block-4.13-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Automatic storage volume discovery | `auto-discover-devices` | Config | Set to `true` if you want to automatically discover and use the storage volumes on your worker nodes. | `false` |
| Node Label Key | `label-key` | Config | The `key` of the worker node `key=value` label. | N/A |
| Node Label Key Value | `label-value` | Config | The `value` of the worker node `key=value` label. | N/A |
| Device Path | `devicepath` | Config | The local storage device path. Example: `/dev/sdc`. Required when `auto-discover-devices` is set to false. | N/A |
{: caption="Table 5. 4.13 parameter reference" caption-side="bottom"}




## Storage class reference for local block storage
{: #local-block-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for local block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `sat-local-block-gold` | Block | Retain |
{: caption="Local block storage class reference" caption-side="bottom"}


## Getting help and support for local block storage
{: #sat-local-block-support}


1. Review the FAQs in the [Red Hat OpenShift docs](https://docs.openshift.com/container-platform/4.9/storage/persistent_storage/persistent-storage-local.html){: external}.
1. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-storage-must-gather) to troubleshoot and resolve common issues.
1. Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
1. Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. Tag any questions with ibm-cloud, so that it's seen by the {{site.data.keyword.Bluemix_notm}} development teams.
1. If you run into an issue with the Local Storage Operator - Block template, you can open an issue in the [Red Hat Customer Portal](https://access.redhat.com/){: external}. 
