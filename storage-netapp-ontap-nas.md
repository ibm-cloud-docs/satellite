---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-27"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations, netapp nas trident

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp ONTAP-NAS
{: #storage-netapp-ontap-nas}

Set up [NetApp ONTAP-NAS storage](https://netapp-trident.readthedocs.io/en/stable-v21.04/){: external} for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp ONTAP-NAS template](/docs/satellite?topic=satellite-storage-netapp-trident) which installs the required operator.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

## Prerequisites for NetApp ONTAP-NAS
{: #netapp-nas-2104-pre}

- You must configure your backend ONTAP cluster as a Trident backend.
- You must have a dedicated Storage Virtual Machine (SVM) for Trident. Volumes created by Trident are created in this SVM.
- You must have one or more aggregates assigned to the SVM. You can add aggregates with the `netapp1::> vserver modify -vs <svm_name> -aggr-list <aggregate(s)_to_be_added>` command.
- You must configure permissions on the default export policy or create your own custom export policy.
- You must have one or more `dataLIFs` for the SVM.
- You must have NFS services enabled on the SVM.
- You must set up a snapshot policy on the SVM.


1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
    - Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage.
    - Your cluster must meet the requirements for ONTAP-NAS. For more information, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/support/requirements.html).
    - Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to the requirements for ONTAP-NAS.
1. [Add your {{site.data.keyword.satelliteshort}} to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig-groups).








## Creating and assigning a configuration in the console
{: #netapp-ontap-nas-config-create-console}
{: ui}


1. Review the [parameter reference](#netapp-ontap-nas-parameter-reference).


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
{: #netapp-ontap-nas-config-create-cli}
{: cli}


1. Review the [parameter reference](#netapp-ontap-nas-parameter-reference) for the template version that you want to use.


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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 21.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-nas --template-version 21.04 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  --param "exportPolicy=EXPORTPOLICY"  --param "limitVolumeSize=LIMITVOLUMESIZE"  --param "limitAggregateUsage=LIMITAGGREGATEUSAGE"  --param "nfsMountOptions=NFSMOUNTOPTIONS" 
    ```
    {: pre}


    Example command to create a version 22.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-nas --template-version 22.04 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  --param "exportPolicy=EXPORTPOLICY"  --param "limitVolumeSize=LIMITVOLUMESIZE"  --param "limitAggregateUsage=LIMITAGGREGATEUSAGE"  --param "nfsMountOptions=NFSMOUNTOPTIONS" 
    ```
    {: pre}


    Example command to create a version 22.10 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-nas --template-version 22.10 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  --param "exportPolicy=EXPORTPOLICY"  --param "limitVolumeSize=LIMITVOLUMESIZE"  --param "limitAggregateUsage=LIMITAGGREGATEUSAGE"  --param "nfsMountOptions=NFSMOUNTOPTIONS" 
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
{: #netapp-ontap-nas-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#netapp-ontap-nas-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 21.04 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-nas\", \"storage-template-version\": \"21.04\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\", { \"entry.name\": \"DATALIF\", { \"entry.name\": \"SVM\", { \"entry.name\": \"EXPORTPOLICY\", { \"entry.name\": \"LIMITVOLUMESIZE\", { \"entry.name\": \"LIMITAGGREGATEUSAGE\", { \"entry.name\": \"NFSMOUNTOPTIONS\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",{ \"entry.name\": \"PASSWORD\",}
    ```
    {: pre}


    Example request to create a version 22.04 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-nas\", \"storage-template-version\": \"22.04\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\", { \"entry.name\": \"DATALIF\", { \"entry.name\": \"SVM\", { \"entry.name\": \"EXPORTPOLICY\", { \"entry.name\": \"LIMITVOLUMESIZE\", { \"entry.name\": \"LIMITAGGREGATEUSAGE\", { \"entry.name\": \"NFSMOUNTOPTIONS\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",{ \"entry.name\": \"PASSWORD\",}
    ```
    {: pre}


    Example request to create a version 22.10 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-nas\", \"storage-template-version\": \"22.10\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\", { \"entry.name\": \"DATALIF\", { \"entry.name\": \"SVM\", { \"entry.name\": \"EXPORTPOLICY\", { \"entry.name\": \"LIMITVOLUMESIZE\", { \"entry.name\": \"LIMITAGGREGATEUSAGE\", { \"entry.name\": \"NFSMOUNTOPTIONS\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",{ \"entry.name\": \"PASSWORD\",}
    ```
    {: pre}








1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}
    
    


{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}


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

{{site.data.content.configuration-upgrade-cli}}
{{site.data.content.configuration-upgrade-console}}


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

6. **Next steps**: [Remove the NetApp Trident operator from your cluster](/docs/satellite?topic=satellite-storage-netapp-trident).

### Removing the NetApp ONTAP-NAS storage assignment and configuration from the console
{: #netapp-nas-template-rm-ui-2104}
{: ui}

Use the console to remove a storage assignment and storage configuration.
{: shortdesc}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.


## Parameter reference
{: #netapp-ontap-nas-parameter-reference}

### 21.04 parameter reference
{: #netapp-ontap-nas-21.04-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Management LIF | `managementLIF` | Config | The IP address of the Management LIF. | true | N/A |
| Data LIF | `dataLIF` | Config | The IP address of the Data LIF. | true | N/A |
| SVM | `svm` | Config | The name of the SVM. | true | N/A |
| User Name | `username` | Secret | The username to connect to the storage device. | true | N/A |
| User Password | `password` | Secret | The password to connect to the storage device. | true | N/A |
| Export Policy | `exportPolicy` | Config | The NAS option for the NFS export policy. | true | `default` |
| Limit Volume Size | `limitVolumeSize` | Config | Maximum requestable volume size (in Gibibytes) and qtree parent volume size | true | `50Gi` |
| Limit AggregateUsage | `limitAggregateUsage` | Config | Fail provisioning if usage is above this percentage. | true | `80%` |
| NFS Mount Options | `nfsMountOptions` | Config | The NFS mount options. | true | `nfsvers=4` |
{: caption="Table 1. 21.04 parameter reference" caption-side="bottom"}


### 22.04 parameter reference
{: #netapp-ontap-nas-22.04-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Management LIF | `managementLIF` | Config | The IP address of the Management LIF. | true | N/A |
| Data LIF | `dataLIF` | Config | The IP address of the Data LIF. | true | N/A |
| SVM | `svm` | Config | The name of the SVM. | true | N/A |
| User Name | `username` | Secret | The username to connect to the storage device. | true | N/A |
| User Password | `password` | Secret | The password to connect to the storage device. | true | N/A |
| Export Policy | `exportPolicy` | Config | The NAS option for the NFS export policy. | true | `default` |
| Limit Volume Size | `limitVolumeSize` | Config | Maximum requestable volume size (in Gibibytes) and qtree parent volume size | true | `50Gi` |
| Limit AggregateUsage | `limitAggregateUsage` | Config | Fail provisioning if usage is above this percentage. | true | `80%` |
| NFS Mount Options | `nfsMountOptions` | Config | The NFS mount options. | true | `nfsvers=4` |
{: caption="Table 2. 22.04 parameter reference" caption-side="bottom"}


### 22.10 parameter reference
{: #netapp-ontap-nas-22.10-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Management LIF | `managementLIF` | Config | The IP address of the Management LIF. | true | N/A |
| Data LIF | `dataLIF` | Config | The IP address of the Data LIF. | true | N/A |
| SVM | `svm` | Config | The name of the SVM. | true | N/A |
| User Name | `username` | Secret | The username to connect to the storage device. | true | N/A |
| User Password | `password` | Secret | The password to connect to the storage device. | true | N/A |
| Export Policy | `exportPolicy` | Config | The NAS option for the NFS export policy. | true | `default` |
| Limit Volume Size | `limitVolumeSize` | Config | Maximum requestable volume size (in Gibibytes) and qtree parent volume size | true | `50Gi` |
| Limit AggregateUsage | `limitAggregateUsage` | Config | Fail provisioning if usage is above this percentage. | true | `80%` |
| NFS Mount Options | `nfsMountOptions` | Config | The NFS mount options. | true | `nfsvers=4` |
{: caption="Table 3. 22.10 parameter reference" caption-side="bottom"}




## Storage class reference for NetApp ONTAP-NAS
{: #netapp-sc-reference-nas-2104}

Before you deploy an app that uses the `sat-netapp` storage classes, review the following notes.
{: shortdesc}

- By default, the `sat-netapp-file-gold` storage class doesn't include any QoS limits (unlimited IOPS).
- To use the `sat-netapp-file-silver` and `sat-netapp-file-bronze` storage classes, you must create corresponding `silver` and `bronze` QoS policy groups on the storage controller and define the QoS limits. To create a policy group on the storage system, log in to the system CLI and run the `**netapp1::> qos policy-group create -policy-group <policy_group_name> -vserver <svm_name> [-min-throughput <min_IOPS>] -max-throughput <max_IOPS>**` command.
- The ***min-throughput*** option is supported only on all-flash systems. For more information about creating and managing QoS Policy groups, see the [ONTAP 9 Storage Management documentation](https://docs.netapp.com/us-en/ontap/index.html){: external}.
- To use an ***encrypted*** storage class, NetApp Volume Encryption (NVE) must be enabled on your storage system by using either the NetApp ONTAP onboard key manager or a supported (off-box) third-party key manager, such as {{site.data.keyword.IBM_notm}} 's TKLM key manager. To enable the onboard key manager, run the `netapp1::> security key-manager onboard enable` command. For more information about configuring encryption, see the [ONTAP 9 Security and Data Encryption documentation](https://docs.netapp.com/us-en/ontap/security-encryption/index.html){: external}.


Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-NAS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.

| Storage class name | Type | File system | IOPs | Encryption | Reclaim policy |
| --- | --- | --- | --- | --- | --- |
| `sat-netapp-file-gold` **Default** | ONTAP-NAS | NFS | no QoS limits | Encryption disabled. | Delete |
| `sat-netapp-file-gold-encrypted` | ONTAP-NAS | NFS | no QoS limits | Encryption enabled. | Delete |
| `sat-netapp-file-silver` | ONTAP-NAS | NFS | User defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-file-silver-encrypted` | ONTAP-NAS | NFS | User defined QoS limit. | Encryption enabled. | Delete |
| `sat-netapp-file-bronze` | ONTAP-NAS | NFS | User-defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-file-bronze-encrypted` | ONTAP-NAS | NFS | User-defined QoS limit.| Encryption enabled. | Delete |
{: caption="NetApp ONTAP-NAS storage class reference." caption-side="bottom"}


## Getting help and support for NetApp ONTAP-NAS
{: #sat-nas-2104-support}

If you run into an issue with NetApp Trident, you can visit the [NetApp support page](https://netapp-trident.readthedocs.io/en/stable-v20.04/support/support.html){: external}. 

