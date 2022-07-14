---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-14"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations, netapp nas trident

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp ONTAP-NAS 21.04
{: #config-storage-netapp-nas-2104}

Set up [NetApp ONTAP-NAS storage](https://netapp-trident.readthedocs.io/en/stable-v21.04/){: external} for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp ONTAP-NAS template](/docs/satellite?topic=satellite-config-storage-netapp-trident) which installs the required operator.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for NetApp ONTAP-NAS
{: #netapp-nas-2104-pre}

- You must configure your backend ONTAP cluster as a Trident backend.
- You must have a dedicated Storage Virtual Machine (SVM) for Trident. Volumes created by Trident are created in this SVM.
- You must have one or more aggregates assigned to the SVM. You can add aggregates with the `netapp1::> vserver modify -vs <svm_name> -aggr-list <aggregate(s)_to_be_added>` command.
- You must configure permissions on the default export policy or create your own custom export policy.
- You must have one or more dataLIFs for the SVM.
- You must have NFS services enabled on the SVM.
- You must set up a snapshot policy on the SVM.


1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters). 
    - Your cluster must meet the requirements for ONTAP-NAS. For more information, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/support/requirements.html).
    - Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to the requirements for ONTAP-NAS.
1. [Add your {{site.data.keyword.satelliteshort}} to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups).



## Creating a NetApp ONTAP-NAS storage configuration in the console
{: #sat-storage-netapp-ui-nas-2104}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} locations dashboard, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type** that you want to use to create your configuration and the **Version**.
1. On the **Parameters** tab, enter the parameters for your configuration.
1. On the **Secrets** tab, enter the secrets, if required, for your configuration.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

## Creating a NetApp ONTAP-NAS storage configuration in the command line
{: #sat-storage-netapp-cli-nas-2104}
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
    
1. List the available templates and versions and review the output. Make a note of the template and version that you want to use.

    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
    

1. Review the [NetApp ONTAP-NAS storage configuration parameters](#sat-storage-netapp-params-cli-nas-2104).
1. Copy the following command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    ```sh
    ibmcloud sat storage config create --name <config> --location <location-id> --template-name 'netapp-ontap-nas' --template-version '21.04' -p 'managementLIF=<mgmt-LIF' -p 'dataLIF=<data-LIF>' -p 'svm=svm-nas' -p 'username=<username>' -p 'password=<password>' -p 'exportPolicy=<export-policy>'
    ```
    {: pre}

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}
    
    


## Assigning your NetApp ONTAP-NAS storage configuration to a cluster
{: #assign-storage-netapp-nas-2104}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-netapp-nas-2104), you can assign your configuration to your {{site.data.keyword.satelliteshort}} clusters.


### Assigning a NetApp ONTAP-NAS storage configuration in the console
{: #assign-storage-netapp-ui-nas-2104}
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



### Assigning a NetApp ONTAP-NAS storage configuration in the command line
{: #assign-storage-netapp-cli-nas-2104}
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

1. Verify that the `trident-kubectl-nas` pod is deployed in the `trident` namespace.

    ```sh
    oc get pods -n trident | grep trident-kubectl-nas
    ```
    {: pre}

    Example output

    ```sh
    trident-kubectl-nas                 1/1     Running   0          2m32s
    ```
    {: screen}

1. Verify that the `sat-netapp` storage classes are deployed.

    ```sh
    oc get sc | grep netapp
    ```
    {: pre}

1. Verify that all resources in the `trident` namespace are `Running` or `Ready`.

    ```sh
    oc get all -n trident
    ```
    {: pre}

    Example output
    ```sh
    NAME                                    READY   STATUS    RESTARTS   AGE
    pod/trident-csi-2nrt4                   2/2     Running   0          14m
    pod/trident-csi-7f999bfb96-z4dr5        6/6     Running   0          14m
    pod/trident-csi-cd5mx                   2/2     Running   0          14m
    pod/trident-csi-zlwwn                   2/2     Running   0          14m
    pod/trident-kubectl-nas                 1/1     Running   0          4m14s
    pod/trident-operator-794f74cd4b-zpnt4   1/1     Running   0          14m

    NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)              AGE
    service/trident-csi   ClusterIP   172.21.106.252   <none>        34571/TCP,9220/TCP   14m

    NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                                     AGE
    daemonset.apps/trident-csi   3         3         3       3            3           kubernetes.io/arch=amd64,kubernetes.io/os=linux   14m

    NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/trident-csi        1/1     1            1           14m
    deployment.apps/trident-operator   1/1     1            1           14m

    NAME                                          DESIRED   CURRENT   READY   AGE
    replicaset.apps/trident-csi-7f999bfb96        1         1         1       14m
    replicaset.apps/trident-operator-794f74cd4b   1         1         1       14m
    ```
    {: screen}


## Deploying an app that uses ONTAP-NAS storage
{: #sat-storage-netapp-nas-deploy-2104}
{: cli}

You can use the `trident-kubectl-nas` driver to deploy apps that use your NetApp ONTAP-NAS storage in your clusters.
{: shortdesc}

1. Create a PVC configuration file that uses one of the `sat-netapp` storage classes. 

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: netapp-pvc
    spec:
      accessModes:
      - ReadWriteMany
      storageClassName: sat-netapp-file-gold
      resources:
      requests:
        storage: 10Gi
    ```
    {: pre}

1. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc.yaml
    ```
    {: pre}

1. Verify that the PVC is created. Make sure that the PVC is in a `Bound` status.

    ```sh
    oc get pvc
    ```
    {: pre}

    Example output

    ```sh
    NAME         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS           AGE
    netapp-pvc   Bound    pvc-acd9e5b4-0b24-4e20-ac00-69a05148c799   10Gi       RWX            sat-netapp-file-gold   39s
    ```
    {: screen}

1. Create a YAML configuration file for a pod that mounts the PVC that you created. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file on your ONTAP-NAS volume mount path.

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
        name: app
    spec:
        containers:
        - name: app
        image: nginx
        command: ["/bin/sh"]
        args: ["-c", "while true; do echo $(date -u) >> /test/test.txt; sleep 5; done"]
        volumeMounts:
        - name: persistent-storage
            mountPath: /test
        volumes:
        - name: persistent-storage
        persistentVolumeClaim:
            claimName: netapp-pvc
    ```
    {: codeblock}

1. Create the pod in your cluster.

    ```sh
    oc apply -f pod.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}

    Example output
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    app   1/1     Running   0          50s
    ```
    {: screen}

1. Verify that the app can write to your ONTAP-NAS instance.

    1. Log in to your pod.
    
        ```sh
        oc exec app -it bash
        ```
        {: pre}

    2. Display the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
    
        ```sh
        cat /test/test.txt
        ```
        {: pre}

        Example output
        ```sh
        Wed May 19 13:28:31 UTC 2021
        Wed May 19 13:28:37 UTC 2021
        Wed May 19 13:28:42 UTC 2021
        Wed May 19 13:28:47 UTC 2021
        ```
        {: screen}

    3. Exit the pod.
    
        ```sh
        exit
        ```
        {: pre}

## Upgrading a NetApp ONTAP-NAS storage configuration
{: #netapp-nas-2104-upgrade-config}
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

## Upgrading a NetApp ONTAP-NAS storage assignment
{: #netapp-nas-2104-upgrade-assignment}
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

## Updating a NetApp ONTAP-NAS storage assignment
{: #netapp-nas-2104-update-assignment}
{: cli}

You can use the `storage assignment update` command to rename your assignment or assign it to a new cluster or cluster group. 

1. List your {{site.data.keyword.satelliteshort}} storage assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to update and the clusters or cluster groups included in the assignment.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Update the {{site.data.keyword.satelliteshort}} assignment. 
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT [--group GROUP ...] [--name NAME]
    ```
    {: pre}

    Example command to update assignment name and assign different cluster groups.
    
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT --name new-name --group group-1 --group group-2 --group group-3
    ```
    {: pre}


## Removing NetApp ONTAP-NAS storage from your apps
{: #netapp-nas-rm-2104}

Before you remove your storage configuration, remove the app pods and PVCs that are using your NetApp storage.
{: shortdesc}

1. List your PVCs and note the name of the PVC and the corresponding PV that you want to remove.

    ```sh
    oc get pvc
    ```
    {: pre}

2. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC.

    ```sh
    oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
    ```
    {: pre}

    Example output
    ```sh
    app    sat-netapp-file-gold
    ```
    {: screen}

3. If the pod is part of a deployment, delete the deployment.

    ```sh
    oc delete deployment <deployment_name>
    ```
    {: pre}

4. Verify that the pod or the deployment is removed.

    ```sh
    oc get pods
    ```
    {: pre}

    ```sh
    oc get deployments
    ```
    {: pre}

5. Delete the PVC.

    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

6. Delete the corresponding PV.

    ```sh
    oc delete pv <pv_name>
    ```
    {: pre}

7. [Remove your NetApp ONTAP-NAS storage configuration from your cluster](#netapp-nas-template-rm-cli-2104)



### Removing the NetApp ONTAP-NAS storage assignment and configuration from the CLI
{: #netapp-nas-template-rm-cli-2104}
{: cli}

Use the CLI to remove a storage assignment and storage configuration.
{: shortdesc}


1. List your storage assignments and find the one that you used for your cluster.

    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the NetApp ONTAP-NAS driver pods and storage class are removed from all clusters that were part of the storage assignment.

    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. Verify that the NetApp ONTAP-NAS driver is removed from your cluster. List the storage classes in your cluster and verify that the NetApp ONTAP-NAS storage class is removed.

    ```sh
    oc get sc
    ```
    {: pre}

4. List the pods in the `trident` namespace and verify that the NetApp ONTAP-NAS storage driver pods are removed.

    ```sh
    oc get pods -n trident
    ```
    {: pre}

5. **Optional**: List your storage configurations and remove your NetApp configuration.

    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

    ```sh
    ibmcloud sat storage config rm --config <config_name>
    ```
    {: pre}

6. **Next steps**: [Remove the NetApp Trident operator from your cluster](/docs/satellite?topic=satellite-config-storage-netapp-trident).

### Removing the NetApp ONTAP-NAS storage assignment and configuration from the console
{: #netapp-nas-template-rm-ui-2104}
{: ui}

Use the console to remove a storage assignment and storage configuration.
{: shortdesc}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

## NetApp ONTAP-NAS storage configuration parameter reference
{: #sat-storage-netapp-params-cli-nas-2104}

For more information about the NetApp ONTAP-NAS configuration parameters, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/docker/install/ndvp_ontap_config.html#configuration-file-options){: external}.

| Parameter name | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `managementLIF` | Required | The IP address of the management LIF. Example: `10.0.0.1`. | N/A |
| `dataLIF` | Required | The IP address of data LIF. Example: `10.0.0.2`. | N/A | 
| `svm` | Required | The name of the storage virtual machine. Example: `svm-nfs`. | N/A | 
| `export-policy` | Optional | Provide the name of an NFS export policy to use. Example: `remote-workers`. | `default` |
| `username` | Required | The username to connect to the storage device. | N/A |
| `password` | Required | The password to connect to the storage device. | N/A |
| `limitVolumeSize` | Optional | Maximum volume size that can be requested per PVC. Example: `500Gi` | `default is 50Gi` |
| `limitAggregateUsage` | Optional | Limit provisioning of volumes if parent volume usage exceeds this value. For example, if a volume is requested that causes parent volume usage to exceed this value, the volume provisioning fails. Example: `70%` | `default is 80%` |
| `nfsMountOptions` | Optional | Specify the NFS mount version. Example: `nfsvers=4` | `""` |
{: caption="Table 1. NetApp ONTAP-NAS storage parameter reference." caption-side="top"}



## Storage class reference for NetApp ONTAP-NAS
{: #netapp-sc-reference-nas-2104}

Before you deploy an app that uses the `sat-netapp` storage classes, review the following notes.
{: shortdesc}

By default, the `sat-netapp-file-gold` storage class doesn't include any QoS limits (unlimited IOPS).
{: note}

To use the `sat-netapp-file-silver` and `sat-netapp-file-bronze` storage classes, you must create corresponding `silver` and `bronze` QoS policy groups on the storage controller and define the QoS limits. To create a policy group on the storage system, log in to the system CLI and run the `netapp1::> qos policy-group create -policy-group <policy_group_name> -vserver <svm_name> [-min-throughput <min_IOPS>] -max-throughput <max_IOPS>` command.
{: note}

The ***min-throughput*** optino is supported only on all-flash systems. For more information about creating and managing QoS Policy groups, see the [ONTAP 9 Storage Management documentation](https://docs.netapp.com/us-en/ontap/index.html){: external}.
{: note}

To use an ***encrypted*** storage class, NetApp Volume Encryption (NVE) must be enabled on your storage system by using either the NetApp ONTAP onboard key manager or a supported (off-box) third-party key manager, such as {{site.data.keyword.IBM_notm}} 's TKLM key manager. To enable the onboard key manager, run the `netapp1::> security key-manager onboard enable` command. For more information about configuring encryption, see the [ONTAP 9 Security and Data Encryption documentation](https://docs.netapp.com/us-en/ontap/security-encryption/index.html){: external}.
{: note}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-NAS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.

| Storage class name | Type | File system | IOPs | Encryption | Reclaim policy |
| --- | --- | --- | --- | --- | --- |
| `sat-netapp-file-gold` **Default** | ONTAP-NAS | NFS | no QoS limits | Encryption disabled. | Delete |
| `sat-netapp-file-gold-encrypted` | ONTAP-NAS | NFS | no QoS limits | Encryption enabled. | Delete |
| `sat-netapp-file-silver` | ONTAP-NAS | NFS | User defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-file-silver-encrypted` | ONTAP-NAS | NFS | User defined QoS limit. | Encryption enabled. | Delete |
| `sat-netapp-file-bronze` | ONTAP-NAS | NFS | User-defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-file-bronze-encrypted` | ONTAP-NAS | NFS | User-defined QoS limit.| Encryption enabled. | Delete |
{: caption="NetApp ONTAP-NAS storage class reference." caption-side="top"}


## Getting help and support for NetApp ONTAP-NAS
{: #sat-nas-2104-support}

If you run into an issue with using NetApp Trident, you can visit the [NetApp support page](https://netapp-trident.readthedocs.io/en/stable-v20.04/support/support.html){: external}. 

