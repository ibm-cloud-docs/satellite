---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: file storage, satellite storage, local file storage, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Local Storage Operator - File
{: #config-storage-local-file}

Set up [Persistent storage using local volumes](https://docs.openshift.com/container-platform/4.6/storage/persistent_storage/persistent-storage-local.html#create-local-pvc_persistent-storage-local){: external} for {{site.data.keyword.satellitelong}} clusters.You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

When you create a local file storage configuration, you specify the local storage devices that you want to make available as persistent volumes (PVs) in your clusters. After you assign the storage configuration to a cluster, {{site.data.keyword.satelliteshort}} deploys the local storage operator which mounts the local disks that you specified in your configuration. The operator further creates the persistent volumes with the file system type that you specify, and creates the `sat-local-file-gold` storage class which you can use to create persistent volume claims (PVCs). You can then reference your PVCs in your Kubernetes workloads.

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}


## Prerequisites for local file storage
{: #config-storage-local-file-prereqs}

Before you can create a local file storage configuration, you must identify the worker nodes in your clusters that have the required available disks. Then, label these worker nodes so that the local storage drivers are installed on only these worker nodes.
1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing). Ensure that the worker nodes in your cluster that you want to use in your storage configuration have at least one available local disk in addition to the disks required by {{site.data.keyword.satelliteshort}}. The extra disks must be unformatted. 
1. [Get the device details of your worker nodes](#sat-storage-file-local-devices).
1. [Label the worker nodes](#sat-storage-file-local-labels) that have an available disk and that you want to use in your configuration. The local storage drivers are installed only on the labeled worker nodes.



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

    2. List your devices.
    
        ```sh
        lsblk
        ```
        {: pre}

    3. Get the details of your devices. Verify that the devices that you want to use are unmounted and unformatted.
    
        ```sh
        fdisk -l
        ```
        {: pre}

4. Review the command output for available disks. You must use unmounted disks for the local storage configuration. In the following example output from the `lsblk` command, the `xvdc` disk is unmounted and has no partitions.

    ```sh
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    xvda    202:0    0  100G  0 disk 
    |-xvda1 202:1    0    1G  0 part /boot
    `-xvda2 202:2    0   99G  0 part /
    xvdb    202:16   0    2G  0 disk 
    `-xvdb1 202:17   0    2G  0 part 
    xvdc    202:32   0   100G  0 disk 
    xvde    202:64   0   50G  0 disk /var/data
    xvdh    202:112  0   64M  0 disk
    ```
    {: screen}

5. Repeat the previous steps for each worker node that you want to use for your local file storage configuration.




### Labeling your worker nodes when using local file storage
{: #sat-storage-file-local-labels}

After you have [retrieved the device paths for the disks that you want to use in your configuration](#sat-storage-file-local-devices), label the worker nodes where the disks are located.
{: shortdesc}

1. Get the worker node IP addresses.

    ```sh
    oc get nodes
    ```
    {: pre}

2. Label the worker nodes that you retrieved earlier. The local storage drivers are deployed to the worker nodes with this label. You can use the `storage=local-file` label in the example command or you can create your own label in the `key=value` format.

    ```sh
    oc label nodes <worker-IP> <worker-IP> "storage=local-file"
    ```
    {: pre}

    Example output

    ```sh
    node/<worker-IP> labeled
    node/<worker-IP> labeled
    ```
    {: screen}

3. Verify that the label is added to the worker nodes that you want to use. Run the following command to display the labels on your worker nodes and highlight the label that you added in the previous step.

    ```sh
    oc get nodes --show-labels | grep --color=always storage=local-file
    ```
    {: pre}


## Creating a local file storage configuration
{: #sat-storage-local-file}

You can use the [console](#sat-storage-local-file-ui) or [CLI](#sat-storage-local-file-cli) to create a local file storage configuration in your location and assign the configuration to your clusters.
{: shortdesc}

### Creating a local file storage configuration from the console
{: #sat-storage-local-file-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.


## Creating a local file storage configuration in the command line
{: #sat-storage-local-file-cli}
{: cli}

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
    
1. Ensure that the worker nodes in your cluster that you want to use in your storage configuration have at least one available local disk in addition to the disks required by {{site.data.keyword.satelliteshort}}. The extra disks must be unformatted. 
1. List the available templates and versions and review the output. Make a note of the template and version that you want to use. Your storage template version and cluster version must match. 

    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
    
1. Review the [Local file storage configuration parameters](#sat-storage-local-file-params-cli).
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a configuration with template version 4.6-4.8.

    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name local-volume-file --template-version <template-version> --param "label-key=storage" --param "label-value=local-file" --param "devicepath=<devicepath>" --param "fstype=<fstype>"
    ```
    {: pre}

    Example command to create a configuration with template version 4.9 and automatically discover local storage devices.

    In version 4.9 of the Local Storage Operator - File template you have the option to pass the `devicepath` parameter or the `auto-discover-devices` parameter. The `auto-discover-devices` parameter can automatically discover available storage devices on your worker nodes. If you don't pass this parameter, or if you pass `auto-discover-devices=false` you must specify the `device-path` parameter. 

    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name local-volume-file --template-version <template-version> --param "label-key=storage" --param "label-value=local-file" --param "autodiscoverdevices=true" --param "fstype=<fstype>"
    ```
    {: pre}

1. Verify that your storage configuration is created.

    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your configuration to clusters](/docs/satellite?topic=satellite-config-storage-local-file#assign-storage-local-file).



## Assigning your local file storage configuration to a cluster
{: #assign-storage-local-file}

After you [create a local {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-local-file), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.


### Assigning a local file storage configuration in the console
{: #assign-storage-local-file-ui}
{: ui}


1. Open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} in your browser.
1. Select the location where you want to create a storage configuration.
1. Click the **Configurations** tab and click the storage configuration that you want to assign to a cluster group.
1. On the **Configuration details** page, click **Create subscription**.
1. In the **Create a subscription** pane, enter a name for your subscription. When you create a subscription you assign your storage configuration to your clusters.
1. From the **Version** drop-down list, select the storage configuration version that you want to assign.
1. From the **Cluster group** drop-down list, select the cluster group that you want to assign to the storage configuration. Note that the clusters in your cluster group where you want to assign storage must all be in the same {{site.data.keyword.satelliteshort}} location.
1. Click **Create** to create the subscription.
1. Verify that your storage configuration is deployed to your cluster. 
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, navigate to the **Configurations** tab.
    1. Click the storage configuration that you created and review the **Subscriptions** tab.
    1. Click the **Subscription** that you created and review the **Rollout status** for your configuration.



### Assigning a local file storage configuration in the command line
{: #assign-storage-local-file-cli}
{: cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    - Group
      ```sh
      ibmcloud sat group ls
      ```
      {: pre}

    - Cluster
      ```sh
      ibmcloud oc cluster ls --provider satellite
      ```
      {: pre}

    - {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster
      ```sh
      ibmcloud sat service ls --location <location>
      ```
      {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. Replace `<group>` with the ID of your cluster group or `<cluster>` with the ID of your cluster. Replace `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    - Group
      ```sh
      ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
      ```
      {: pre}

    - Cluster
      ```sh
      ibmcloud sat storage assignment create --cluster <cluster> --config <config> --name <name>
      ```
      {: pre}

    - {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER) | grep <storage-assignment-name>
    ```
    {: pre}

1. Verify that the storage configuration resources are deployed. Get a list of all the resources in the `local-storage` namespace.

    ```sh
    oc get all -n local-storage
    ```
    {: pre}

    Example output

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

1. List the storage classes that are available.

    ```sh
    oc get sc -n local-storage | grep local
    ```
    {: pre}

    Example output

    ```sh
    sat-local-file-gold       kubernetes.io/no-provisioner   Delete          WaitForFirstConsumer   false                  21m
    ```
    {: screen}

1. List the PVs and verify that the status is `Available`. The local disks that you specified when you created your configuration are available as persistent volumes.

    ```sh
    oc get pv
    ```
    {: pre}

    Example output

    ```sh
    NAME               CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS          REASON   AGE
    local-pv-1d14680   50Gi       RWO            Delete           Available           sat-local-file-gold            50s
    ```
    {: screen}

1. [Create a PVC that references your local PV, then deploy an app that uses your local storage](#deploy-app-local-file).




## Deploying an app that uses your local file storage
{: #deploy-app-local-file}
{: cli}

After you create a local file storage configuration and assign it to your clusters, you can then create an app that uses your local file storage.
{: shortdesc}

You can map your PVCs to specific persistent volumes by adding labels to your persistent volumes. For more information, see the [Kubernetes documentation for selectors](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#selector).
{: tip}

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
          storage: 20Gi # Important: Ensure that size of your claim is not larger than the local disk.
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

    To ensure that your pods are scheduled to worker nodes with storage, or to ensure that the apps that require storage are not preempted by other pods, you can specify `nodeAffinity` and set up pod priority. For more information, see the Kubernetes documentation for [pod priority and preemption](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/) and setting [node affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-isolation-restriction).
    {: tip}

4. Deploy an app pod that uses your local storage PVC. Save the following example app YAML as a file on your local machine called `app.yaml`. This pod writes the date to a file called `test.txt`. Be sure to enter the name of the PVC that you created earlier. In this example, the `nodeAffinity` spec ensures that this pod is only scheduled to a worker node with the label is the specified.

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
              - local-file # Enter the 'value' of the worker label that you created earlier.
      volumes:
        - name: local-pvc
          persistentVolumeClaim:
            claimName: local-pvc 
      containers:
        - name: local-disks 
          image: nginx
          ports:
            - containerPort: 80
              name: "http-server"
          volumeMounts:
            - mountPath: <mount-path-to-local-disk> # Example /dev/xvdc
              name: local-pvc
    ```
    {: codeblock}

5. Create the app pod in your cluster.

    ```sh
    oc create -f app.yaml
    ```
    {: pre}

6. Log in to your app pod and verify that you can write to your local disk.

    ```sh
    kubectl exec <app-pod> -it bash
    ```
    {: pre}

7. Run the following command to change directories to the location of your local disk, write the test.txt file, and display the contents of the file.

    ```sh
    cd /<mount-path-to-local-disk> && echo "This is a test." >> test.txt && cat test.txt
    ```
    {: pre}

    Example output

    ```sh
    This is a test.
    ```
    {: screen}

8. Remove the test file and log out of the pod.

    ```sh
    rm test.txt && exit
    ```
    {: pre}


## Upgrading a local file storage configuration
{: #sat-storage-local-file-upgrade-config}
{: cli}

You can upgrade your {{site.data.keyword.satelliteshort}} storage configurations to use the latest storage template revision within the same major version. 

1. List your {{site.data.keyword.satelliteshort}} storage configurations, make a note of the {{site.data.keyword.satelliteshort}} configuration you want to upgrade.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} configuration. Note, only the configuration is updated. If you want to upgrade the assignments that use this configuration, you can specify the `--include-assignments` option or you can manually update each assignment using the `assignment update` command.
    ```sh
    ibmcloud sat storage config upgrade --config CONFIG [--include-assignments]
    ```
    {: pre}

## Upgrading a local file storage assignment
{: #sat-storage-local-file-upgrade-assignment}
{: cli}

You can use the `storage assignment upgrade` command to upgrade an assignment to the latest version of the storage configuration it uses. 

1. List your {{site.data.keyword.satelliteshort}} storage assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to upgrade.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. List the {{site.data.keyword.satelliteshort}} storage templates to see the latest available versions.
    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} assignment.
    ```sh
   ibmcloud sat storage assignment upgrade --assignment ASSIGNMENT
    ```
    {: pre}

## Updating a local file storage assignment
{: #sat-storage-local-file-update-assignment}
{: cli}

## Removing the local file storage configuration from your cluster
{: #sat-storage-remove-local-file-config}

If you no longer plan on using local file storage in your cluster, you can unassign your cluster from the storage configuration. 
{: shortdesc}

Note that if you remove the storage configuration, the local storage operator resources and the `sat-local-file-gold` storage class is then uninstalled from all assigned clusters. Your PVCs, PVs and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again. 
{: important}


### Remove the local file storage configuration from the console
{: #sat-storage-rm-local-file-ui}
{: ui}


Use the console to remove a storage configuration. 
{: shortdesc}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Remove the local file storage configuration from the command line
{: #rm-local-file-temp-cli}
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
    {: screen}

2. List your storage assignments and find the one that you used for your cluster. 

    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

3. Remove the assignment. After the assignment is removed, the local storage driver pods and storage classes are removed from all clusters that were part of the storage assignment. 

    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

4. List the resources in the `local-storage` namespace and verify that the local storage driver pods are removed.
 
    ```sh
    oc get all -n local-storage
    ```
    {: pre}

    Example output

    ```sh
    No resources found in local-storage namespace.
    ```
    {: screen}

5. List of the storage classes in your cluster and verify that the local storage classes are removed. 

    ```sh
    oc get sc
    ```
    {: pre}

6. Optional: Remove the storage configuration. 
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

7. List your PVCs and note the name of the PVC that you want to remove. 
    ```sh
    oc get pvc
    ```
    {: pre}

8. Remove any pods that currently mount the PVC.
 
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

9. Delete the PVC. Because all {{site.data.keyword.IBM_notm}}-provided local block storage classes are specified with a `Retain` reclaim policy, the PV and PVC are not automatically deleted when you delete your app or deployment. 

    ```sh
    oc delete pvc <pvc-name>
    ```
    {: pre}

10. Verify that your PVC is removed.

    ```sh
    oc get pvc
    ```
    {: pre}

11. List your PVs and note the name of the PVs that you want to remove.

    ```sh
    oc get pv
    ```
    {: pre}

12. Delete the PVs. Deleting your PVs will make your disks available for other workloads.

    ```sh
    oc delete pv <pv-name>
    ```
    {: pre}

13. Verify that your PV is removed.

    ```sh
    oc get pv
    ```
    {: pre}


## Local file storage configuration parameter reference
{: #sat-storage-local-file-params-cli}

### Local file storage version 4.9 parameters
{: #file-49-params}

| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `local-volume-file`. |
| `--template-version` | Required | Enter the version of the `local-volume-file` template that you want to use. The template version that you specify must match your OCP version. For example, if your OCP version is `4.5.X`, specify template version `4.5`.  To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `label-key` | Required | Enter the node label key that you added to the worker nodes where you want to install the local storage drivers. The local storage drivers are installed only on the worker nodes that have the corresponding label. In the previous example, the `label-key` is `storage`. |
| `label-value` | Required | Enter the node label value that you added to the worker nodes where you want to install the local storage driver. The local storage drivers are installed only on the worker nodes that have the corresponding label. In the previous example, the `label-value` is `local-file`. |
| `devicepath` | Required | Enter the local storage device paths. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/nvme2n1`,`/dev/nvme3n1`. For more information on how to retrieve this value, see [Getting the device details](#sat-storage-file-local-devices). Example: `/dev/sdc`. |
| `fstype` | Required | Enter the file system type that you want to use on your local disks. The supported file system types are: `xfs`, `ext`, `ext3`, and `ext4`.  Example: `ext4`. |
| `auto-discover-devices` | Optional | Set to `true` if you want to automatically discover available devices on your worker nodes. You must have unformatted disks available on your worker nodes to use this feature. If you don't pass this parameter, or if you pass `auto-discover-devices=false`, you must specify the `osd-device-path` parameter. | false | boolean |
{: caption="Table 1. Local file storage parameter reference." caption-side="top"}

### Local file storage versions 4.6-4.8 parameters
{: #file-46-48-params}

| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `local-volume-file`. |
| `--template-version` | Required | Enter the version of the `local-volume-file` template that you want to use. The template version that you specify must match your OCP version. For example, if your OCP version is `4.5.X`, specify template version `4.5`.  To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `label-key` | Required | Enter the node label key that you added to the worker nodes where you want to install the local storage drivers. The local storage drivers are installed only on the worker nodes that have the corresponding label. In the previous example, the `label-key` is `storage`. |
| `label-value` | Required | Enter the node label value that you added to the worker nodes where you want to install the local storage driver. The local storage drivers are installed only on the worker nodes that have the corresponding label. In the previous example, the `label-value` is `local-file`. |
| `devicepath` | Required | Enter the local storage device paths. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/nvme2n1`,`/dev/nvme3n1`. For more information on how to retrieve this value, see [Getting the device details](#sat-storage-file-local-devices). Example: `/dev/sdc`. |
| `fstype` | Required | Enter the file system type that you want to use on your local disks. The supported file system types are: `xfs`, `ext`, `ext3`, and `ext4`.  Example: `ext4`. |
{: caption="Table 2. Local file storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates if the parameter is a required parameter. The third column is a brief description of the parameter."}



## Storage class reference for local file storage
{: #local-file-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for local file storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-local-file-gold` | `ext4` or `xfs` | Retain |
{: caption="Table 2. Local file storage class reference." caption-side="top"}


## Getting help and support for local file storage
{: #sat-local-block-support}

If you run into an issue with using the Local Storage Operator - File template, you can open an issue in the [Red Hat Customer Portal](https://access.redhat.com/){: external}. 
