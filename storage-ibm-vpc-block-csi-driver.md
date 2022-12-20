---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-20"

keywords: satellite storage, csi, satellite configurations, block storage,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.block_storage_is_short}} Container Storage Interface (CSI) Driver
{: #storage-ibm-vpc-block-csi-driver}

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

There is currently an issue with autocomplete in some browsers. If you don't see the IAM API Key field on the **Secrets** tab, try clearing the search field or a different web browser. 
{: note}





## Creating a configuration
{: #ibm-vpc-block-csi-driver-config-create}

Before you begin, review the [parameter reference](#ibm-vpc-block-csi-driver-parameter-reference) for the template version that you want to use.
{: important}

### Creating and assigning a configuration in the console
{: ibm-vpc-block-csi-driver-config-create-console}
{: ui}

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

### Creating a configuration in the CLI
{: #ibm-vpc-block-csi-driver-config-create-cli}
{: cli}

1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 4.3.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-vpc-block-csi-driver --template-version 4.3.0 --param "g2_token_exchange_endpoint_url=G2_TOKEN_EXCHANGE_ENDPOINT_URL"  --param "g2_riaas_endpoint_url=G2_RIAAS_ENDPOINT_URL"  --param "g2_resource_group_id=G2_RESOURCE_GROUP_ID"  --param "g2_api_key=G2_API_KEY" 
    ```
    {: pre}

    Example command to create a version 5.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-vpc-block-csi-driver --template-version 5.0 --param "g2_token_exchange_endpoint_url=G2_TOKEN_EXCHANGE_ENDPOINT_URL"  --param "g2_riaas_endpoint_url=G2_RIAAS_ENDPOINT_URL"  --param "g2_resource_group_id=G2_RESOURCE_GROUP_ID"  --param "g2_api_key=G2_API_KEY" 
    ```
    {: pre}


1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}



## Assigning configurations to clusters
{: #ibm-vpc-block-csi-driver-assignment-create}

After you create a storage configuration, you can assign that configuration to clusters, cluster groups, or service clusters to automatically deploy storage resources across clusters in your Location.

If you haven't yet created a storage configuration, see [Creating a configuration](#ibm-vpc-block-csi-driver-config-create).

### Creating an assignment in the console
{: #ibm-vpc-block-csi-driver-assignment-create-console}
{: ui}

If you didn't assign your configuration to a cluster or service when you created it, you can create an assignment by completing the following steps.

1. Open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} in your browser.
1. Select the location where you created your storage configuration.
1. Click the **Locations** tab, then click **Storage**.
1. Click the storage configuration that you want to assign to a cluster group.
1. On the **Configuration details** page, click **Create storage assignment**.
1. In the **Create an assignment** pane, enter a name for your assignment.
1. From the **Version** drop-down list, select the storage configuration version that you want to assign.
1. From the **Cluster group** drop-down list, select the cluster group that you want to assign to the storage configuration. Note that the clusters in your cluster group where you want to assign storage must all be in the same  location.
1. Click **Create** to create the assignment.
1. Verify that your storage configuration is deployed to your cluster. 
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, navigate to your Location and select **Storage**
    1. Click the storage configuration that you created and review the **Assignments** tab.
    1. Click the **Assignment** that you created and review the **Rollout status** for your configuration.


### Creating an assignment in the CLI
{: #ibm-vpc-block-csi-driver-assignment-create-cli}
{: ui}

1. List your storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster, cluster group, or service that you want to assign storage to. 

    To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    {: tip}
    
    List cluster groups.
    
    ```sh
    ibmcloud sat group ls
    ```
    {: pre}

    List clusters.
    
    ```sh
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}
    
    List services.
    
    ```sh
    ibmcloud sat service ls --location <location>
    ```
    {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    Assign a configuration to a cluster group.
    ```sh
    ibmcloud sat storage assignment create --group GROUP --config CONFIG --name NAME
    ```
    {: pre}

    Assign a configuration to a cluster.
    ```sh
    ibmcloud sat storage assignment create --cluster CLUSTER --config CONFIG --name NAME
    ```
    {: pre}

    Assign a configuration to a service cluster.
    ```sh
    ibmcloud sat storage assignment create --service-cluster-id CLUSTER --config CONFIG --name NAME
    ```
    {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER) | grep <storage-assignment-name>
    ```
    {: pre}


### Creating an assignment in the API
{: #ibm-vpc-block-csi-driver-assignment-create-console}
{: api}

1. Copy one of the following example requests. 

    Example request to assign a [configuration to a cluster](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite/createAssignmentByCluster){: external}.
    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createAssignmentByCluster" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"channelName\": \"CONFIGURATION-NAME\", \"cluster\": \"CLUSTER-ID\", \"controller\": \"LOCATION-ID\", \"name\": \"ASSIGNMENT-NAME\"}"
    ```
    {: pre}
    
    Example request to [assign configuration to a cluster group](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite/createAssignment){: external}.
    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createAssignment" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"channelName\": \"CONFIGURATION-NAME\", \"cluster\": \"string\", \"groups\": [ \"CLUSTER-GROUP\" ], \"name\": \"ASSIGNMENT-NAME\"}"
    ```
    {: pre}
    
1. Replace the variables with your details and run the request.

1. Verify the assignment was created by listing your assignments.

    ```sh
    curl -X GET "https://containers.cloud.ibm.com/global/v2/storage/satellite/getAssignments" -H "accept: application/json" -H "Authorization: Bearer TOKEN"
    ```
    {: pre}
    
## Updating assignments
{: #ibm-vpc-block-csi-driver-assignment-update}

Update your assignments to rollout your configurations to new or different clusters, cluster groups, or service.


### Updating an assignment in the console
{: #ibm-vpc-block-csi-driver-assignment-update-console}
{: ui}

Updating assignments in the console is currently not available.
{: note}

However, you can use the [CLI](#ibm-vpc-block-csi-driver-assignment-update-cli) or [API](#ibm-vpc-block-csi-driver-assignment-update-api) to update your assignemnts.


### Updating an assignment in the CLI
{: #ibm-vpc-block-csi-driver-assignment-update-cli}
{: cli}

You can use the `storage assignment update` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-upgrade) to rename your assignment or assign it to a new cluster or cluster group.


1. List your assignments, make a note of the  assignment you want to update and the clusters or cluster groups included in the assignment.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Update the assignment.
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT [--group GROUP ...] [--name NAME]
    ```
    {: pre}

    Example command to update assignment name and assign different cluster groups.
    
    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-vpc-block-csi-driver --template-version 5.0 --param "g2_token_exchange_endpoint_url=G2_TOKEN_EXCHANGE_ENDPOINT_URL"  --param "g2_riaas_endpoint_url=G2_RIAAS_ENDPOINT_URL"  --param "g2_resource_group_id=G2_RESOURCE_GROUP_ID"  --param "g2_api_key=G2_API_KEY" 
    ```
    {: pre}


1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}

### Creating a configuration in the API
{: #ibm-vpc-block-csi-driver-config-create-api}

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 4.3.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-vpc-block-csi-driver\", \"storage-template-version\": \"4.3.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"G2_TOKEN_EXCHANGE_ENDPOINT_URL\",\"user-secret-parameters\": { \"entry.name\": \"G2_TOKEN_EXCHANGE_ENDPOINT_URL\",  { \"entry.name\": \"G2_RIAAS_ENDPOINT_URL\",\"user-secret-parameters\": { \"entry.name\": \"G2_RIAAS_ENDPOINT_URL\",  { \"entry.name\": \"G2_RESOURCE_GROUP_ID\",\"user-secret-parameters\": { \"entry.name\": \"G2_RESOURCE_GROUP_ID\",  { \"entry.name\": \"G2_API_KEY\",\"user-secret-parameters\": { \"entry.name\": \"G2_API_KEY\", 
    ```
    {: pre}

    Example request to create a version 5.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-vpc-block-csi-driver\", \"storage-template-version\": \"5.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"G2_TOKEN_EXCHANGE_ENDPOINT_URL\",\"user-secret-parameters\": { \"entry.name\": \"G2_TOKEN_EXCHANGE_ENDPOINT_URL\",  { \"entry.name\": \"G2_RIAAS_ENDPOINT_URL\",\"user-secret-parameters\": { \"entry.name\": \"G2_RIAAS_ENDPOINT_URL\",  { \"entry.name\": \"G2_RESOURCE_GROUP_ID\",\"user-secret-parameters\": { \"entry.name\": \"G2_RESOURCE_GROUP_ID\",  { \"entry.name\": \"G2_API_KEY\",\"user-secret-parameters\": { \"entry.name\": \"G2_API_KEY\", 
    ```
    {: pre}






## Deploying an app that uses {{site.data.keyword.block_storage_is_short}}
{: #sat-storage-vpc-deploy-app}


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
    oc exec my-deployment
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



## Parameter reference
{: #ibm-vpc-block-csi-driver-parameter-reference}

### 4.3.0 parameter reference
{: #4.3.0-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| IAM endpoint | `g2_token_exchange_endpoint_url` | The IAM endpoint. For example `https://iam.cloud.ibm.com`. | true | 
| VPC IaaS endpoint | `g2_riaas_endpoint_url` | The VPC regional endpoint of your VPC cluster in the format `https://region.iaas.cloud.ibm.com`. Example: `https://eu-de.iaas.cloud.ibm.com`. For more information, see https://ibm.biz/vpc-endpoints | true | 
| Resource group ID | `g2_resource_group_id` | The ID of the resource group where your VPC is located. To retrieve this value, run the `ibmcloud is vpc VPC-ID` command and note the Resource group field. | true | 
| IAM API key | `g2_api_key` | The IAM API key of account where your VPC is located. You can use your existing API key or you can create an API key by running the `ibmcloud iam api-key-create NAME` command. | true | 
{: caption="Table 1. 4.3.0 parameter reference" caption-side="bottom"}


### 5.0 parameter reference
{: #5.0-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| IAM endpoint | `g2_token_exchange_endpoint_url` | The IAM endpoint. For example `https://iam.cloud.ibm.com`. | true | 
| VPC IaaS endpoint | `g2_riaas_endpoint_url` | The VPC regional endpoint of your VPC cluster in the format `https://region.iaas.cloud.ibm.com`. Example: `https://eu-de.iaas.cloud.ibm.com`. For more information, see https://ibm.biz/vpc-endpoints | true | 
| Resource group ID | `g2_resource_group_id` | The ID of the resource group where your VPC is located. To retrieve this value, run the `ibmcloud is vpc VPC-ID` command and note the Resource group field. | true | 
| IAM API key | `g2_api_key` | The IAM API key of account where your VPC is located. You can use your existing API key or you can create an API key by running the `ibmcloud iam api-key-create NAME` command. | true | 
{: caption="Table 2. 5.0 parameter reference" caption-side="bottom"}


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
{: caption="Table 2. Storage class reference for IBM Block Storage for VPC" caption-side="bottom"}

## Getting help and support for {{site.data.keyword.block_storage_is_short}}
{: #sat-vpc-csi-support}

If you run into an issue with using {{site.data.keyword.block_storage_is_short}} you can submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.
