---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite storage, csi, satellite configurations, block storage,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.block_storage_is_short}} Container Storage Interface (CSI) Driver
{: #config-storage-vpc-csi}

The {{site.data.keyword.block_storage_is_short}} Container Storage Interface (CSI) [Driver](https://github.com/kubernetes-sigs/ibm-vpc-block-csi-driver){: external} in {{site.data.keyword.satellitelong}} allows you to manage the lifecycle of your IBM VPC Block Data volumes.

The template is currently in beta. Do not use it for production workloads. 
{: beta}


Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for {{site.data.keyword.block_storage_is_short}}
{: #sat-storage-vpc-csi-prereq}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create an API key for access to your clusters](https://cloud.ibm.com/iam/apikeys){: external}




## Creating the {{site.data.keyword.block_storage_is_short}} configuration
{: #sat-storage-vpc}

Create a storage configuration in the command line by using the {{site.data.keyword.block_storage_is_short}} configuration template.
{: shortdesc}

## Creating the {{site.data.keyword.block_storage_is_short}} configuration from the console
{: #sat-storage-vpc-create-config-ui}
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

### Creating the {{site.data.keyword.block_storage_is_short}} configuration in the command line
{: #sat-storage-vpc-create-config-cli}
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
    

1. Review the [template parameters](#sat-storage-vpc-csi-params-cli).

1. Create storage configuration. You can pass parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Note that Kubernetes resources can't contain capital letters or special characters. Enter a name for your config that uses only lowercase letters, numbers, hyphens or periods.

    ```sh
    ibmcloud sat storage config create --name vpc-template --template-name ibm-vpc-block-csi-driver --template-version 4.1.1 --param "g2_resource_group_id=<resource-group-for-your-IaaS>" --param "g2_riaas_endpoint_url=https://us-south.iaas.cloud.ibm.com" --param "g2_token_exchange_endpoint_url=https://iam.cloud.ibm.com" --param "g2_api_key=<api-key>" --location c5r9tuow07nq26ji7qqg
    ```
    {: pre}

1. Verify that your storage configuration is created.

    ```sh
    ibmcloud sat storage config get --config <CONFIG>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-vpc-csi-cli)

### Assigning a {{site.data.keyword.block_storage_is_short}} storage configuration from the console
{: #assign-storage-vpc-csi-ui}
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


### Assigning a {{site.data.keyword.block_storage_is_short}} storage configuration in the command line
{: #assign-storage-vpc-csi-cli}
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

1. Verify that the storage configuration resources are deployed. 

    ```sh
    kubectl get pods -n kube-system | grep vpc
    NAME                                         READY   STATUS    RESTARTS   AGE
    ibm-vpc-block-csi-controller-0               5/5     Ready     0          17m
    ```
    {: screen}

1. List the storage classes.

    ```sh
    kubectl get sc -n ibm-vpc-block-csi-driver
    ibmc-vpc-block-metro-5iops-tier               vpc.block.csi.ibm.io   Delete          WaitForFirstConsumer   true                   9m13s
    ibmc-vpc-block-metro-custom                   vpc.block.csi.ibm.io   Delete          WaitForFirstConsumer   true                   9m12s
    ibmc-vpc-block-metro-general-purpose          vpc.block.csi.ibm.io   Delete          WaitForFirstConsumer   true                   9m11s
    ibmc-vpc-block-metro-retain-10iops-tier       vpc.block.csi.ibm.io   Retain          WaitForFirstConsumer   true                   9m10s
    ibmc-vpc-block-metro-retain-5iops-tier        vpc.block.csi.ibm.io   Retain          WaitForFirstConsumer   true                   9m8s
    ibmc-vpc-block-metro-retain-custom            vpc.block.csi.ibm.io   Retain          WaitForFirstConsumer   true                   9m7s
    ibmc-vpc-block-metro-retain-general-purpose   vpc.block.csi.ibm.io   Retain          WaitForFirstConsumer   true                   9m5s
    sat-vpc-block-gold-metro (default)            vpc.block.csi.ibm.io   Delete          WaitForFirstConsumer   true                   9m22s
    ```
    {: screen}

1. [Deploy an app that uses your {{site.data.keyword.block_storage_is_short}}](#sat-storage-vpc-deploy-app)

## Deploying an app that uses {{site.data.keyword.block_storage_is_short}}
{: #sat-storage-vpc-deploy-app}
{: cli}

You can use the `ibm-vpc-block-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references a VPC storage class that you created earlier.

    ```yaml
        apiVersion: v1
        kind: PersistentVolumeClaim
        metadata:
        name: my-pvc
        spec:
        storageClassName: ibmc-vpc-block-5iops-tier
        accessModes:
            - ReadWriteOnce
        resources:
            requests:
            storage: 10Gi

    ```
    {: codeblock}
        
1. Create the PVC in your cluster. 

    ```sh
    oc apply -f pvc.yaml
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you created. 

    ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: my-deployment
            labels:
            app: my-app
        spec:
        replicas: 1
        selector:
            matchLabels:
            app: my-app
        template:
            metadata:
            labels:
                app: my-app
            spec:
            containers:
            - image: ngnix # Your containerized app image.
                name: my-container
                volumeMounts:
                - name: my-volume
                mountPath: /mount-path
            volumes:
            - name: my-volume
                persistentVolumeClaim:
                claimName: my-pvc

    ```
    {: codeblock}

1. Create the pod in your cluster.

    ```sh
    oc apply -f deployment.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}
    
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    my-deployment                       1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your block storage volume by logging in to your pod.

    ```sh
    oc exec XXX
    ```
    {: pre}

1. View the contents of the `outfile` file to confirm that your app can write data to your persistent storage.

    ```sh
    cat /
    ```
    {: pre}

    Example output

    
    ```sh
    Fri Jul 16 07:49:39 EDT 2021
    Fri Jul 16 07:49:39 EDT 2021
    Fri Jul 16 07:49:39 EDT 2021
    ```
    {: screen}

1. Exit the pod.

    ```sh
    exit
    ```
    {: pre}

## Upgrading a {{site.data.keyword.block_storage_is_short}} storage configuration
{: #vpc-csi-upgrade-config}
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

## Upgrading a {{site.data.keyword.block_storage_is_short}} storage assignment
{: #vpc-csi-upgrade-assignment}
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

## Updating a {{site.data.keyword.block_storage_is_short}} storage assignment
{: #vpc-csi-update-assignment}
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

## Removing {{site.data.keyword.block_storage_is_short}} storage from your apps
{: #vpc-csi-rm-apps}

If you no longer need your {{site.data.keyword.block_storage_is_short}} configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters.
{: shortdesc}

1. List your PVCs and note the name of the PVC that you want to remove.

    ```sh
    oc get pvc
    ```
    {: pre}

1. Remove any pods that mount the PVC.

    1. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC.
    
        ```sh
        oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
        ```
        {: pre}

        Example output

        
        ```sh
        app    ibmc-vpc-block-metro-5iops-tier
        ```
        {: screen}

    1. Remove the pod that uses the PVC. If the pod is part of a deployment or statefulset, remove the deployment or statefulset.
    
        ```sh
        oc delete pod <pod_name>
        ```
        {: pre}

        ```sh
        oc delete deployment <deployment_name>
        ```
        {: pre} 

        ```sh
        oc delete statefulset <statefulset_name>
        ```
        {: pre}

    1. Verify that the pod, deployment, or statefulset is removed.
    
        ```sh
        oc get pods
        ```
        {: pre}

        ```sh
        oc get deployments
        ```
        {: pre}

        ```sh
        oc get statefulset
        ```
        {: pre}

1. Delete the PVC.

    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

1. Verify that your PV is automatically removed.

    ```sh
    oc get pv
    ```
    {: pre}

## Removing the {{site.data.keyword.block_storage_is_short}} storage configuration from your cluster
{: #vpc-csi-template-rm}

If you no longer plan on using your {{site.data.keyword.block_storage_is_short}} in your cluster, you can use the CLI unassign your cluster from the storage configuration.
{: shortdesc}

Note that if you remove the storage configuration, the driver is then uninstalled from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}

### Removing the {{site.data.keyword.block_storage_is_short}} storage configuration using the console
{: #vpc-csi-rm-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Removing the {{site.data.keyword.block_storage_is_short}} storage configuration using the cli
{: #vpc-csi-rm-cli}
{: cli}

1. List your storage assignments and find the one that you used for your cluster.

    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the driver pods and storage classes are removed from all clusters that were part of the storage assignment.

    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. Verify that the driver is removed from your cluster.

    1. List of the storage classes in your cluster and verify that the storage classes are removed.
    
        ```sh
        oc get sc
        ```
        {: pre}

    2. List the pods in the `kube-system` namespace and verify that the storage driver pods are removed.
    
        ```sh
        oc get pods -n kube-system | grep vpc
        ```
        {: pre}

4. Optional: Remove the storage configuration.

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


## Parameter reference for {{site.data.keyword.block_storage_is_short}}
{: #sat-storage-vpc-csi-params-cli}

| Parameter | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `g2_api_key` | Required | IAM api key. | N/A |
| `g2_resource_group_id` | Required | Resource group ID. The list resource groups, run `ibmcloud resource groups`. | N/A |
| `g2_riass_endpoint_url` | Required | PRIASS endpoint url. | N/A |
| `g2_token_exchange_endpoint_url` | Required | IAM token exchange endpoint url. | N/A |
{: caption="Table 1. Parameter reference for IBM VPC block storage" caption-side="top"}

## Storage class reference for {{site.data.keyword.block_storage_is_short}}
{: #sat-storage-vpc-ref}


Review the {{site.data.keyword.satelliteshort}} storage classes for IBM VPC block storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

 Storage class name | Default Read IOPS per GB | Default Write IOPS per GB | Size range (per disk) | Hard disk | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-vpc-block-gold-metro` **Default** | 10 | 10 | 10 GB - 4 TB | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-5iops-tier`  | 5 | 5 | 10 GB - 9600 GB | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-custom` | Custom | Custom | Based on IOPS | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-general-purpose` | 3 | 3 | 10 GB - 16 TB | SSD | Delete | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-10iops-tier`  | 10 | 10 | 10 GB - 4 TB | SSD | Retain | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-5iops-tier` | 5 | 5 | 10 GB - 9600 GB | SSD | Retain | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-custom`  | Custom | Custom | Based on IOPS | SSD | Retain | WaitForFirstConsumer |
| `ibmc-vpc-block-metro-retain-general-purpose` | 3 | 3 | 10 GiB - 16 TB | SSD | Retain | WaitForFirstConsumer |
{: caption="Table 2. Storage class reference for IBM Block Storage for VPC" caption-side="top"}

## Getting help and support for {{site.data.keyword.block_storage_is_short}}
{: #sat-vpc-csi-support}

If you run into an issue with using {{site.data.keyword.block_storage_is_short}} you can submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.
