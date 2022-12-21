---

copyright:
  years: 2022, 2022
lastupdated: "2022-12-21"

keywords: satellite storage, satellite config, satellite configurations, cos, object storage, storage configuration, cloud object storage

subcollection: satellite
---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cos_full_notm}} CSI Driver 
{: #storage-ibm-object-storage-plugin}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}


## Prerequisites
{: #storage-ibm-object-storage-plugin-prereqs}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create an {{site.data.keyword.cos_full_notm}} Secret](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials). 



## Creating a configuration
{: #ibm-object-storage-plugin-config-create}

Before you begin, review the [parameter reference](#ibm-object-storage-plugin-parameter-reference) for the template version that you want to use.
{: important}

### Creating and assigning a configuration in the console
{: ibm-object-storage-plugin-config-create-console}
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
{: #ibm-object-storage-plugin-config-create-cli}
{: cli}

1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 2.2 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-object-storage-plugin --template-version 2.2 [--param "helm-release-name=HELM-RELEASE-NAME"]  [--param "parameters=PARAMETERS"]  --param "license=LICENSE"  --param "cos-endpoint=COS-ENDPOINT"  --param "cos-storageclass=COS-STORAGECLASS" 
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
{: #ibm-object-storage-plugin-assignment-create}

After you create a storage configuration, you can assign that configuration to clusters, cluster groups, or service clusters to automatically deploy storage resources across clusters in your Location.

If you haven't yet created a storage configuration, see [Creating a configuration](#ibm-object-storage-plugin-config-create).

### Creating an assignment in the console
{: #ibm-object-storage-plugin-assignment-create-console}
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
{: #ibm-object-storage-plugin-assignment-create-cli}
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
{: #ibm-object-storage-plugin-assignment-create-console}
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
{: #ibm-object-storage-plugin-assignment-update}

Update your assignments to rollout your configurations to new or different clusters, cluster groups, or service.


### Updating an assignment in the console
{: #ibm-object-storage-plugin-assignment-update-console}
{: ui}

Updating assignments in the console is currently not available.
{: note}

However, you can use the [CLI](#ibm-object-storage-plugin-assignment-update-cli) or [API](#ibm-object-storage-plugin-assignment-update-api) to update your assignemnts.


### Updating an assignment in the CLI
{: #ibm-object-storage-plugin-assignment-update-cli}
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
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-object-storage-plugin --template-version 2.2 [--param "helm-release-name=HELM-RELEASE-NAME"]  [--param "parameters=PARAMETERS"]  --param "license=LICENSE"  --param "cos-endpoint=COS-ENDPOINT"  --param "cos-storageclass=COS-STORAGECLASS" 
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
{: #ibm-object-storage-plugin-config-create-api}

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 2.2 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-object-storage-plugin\", \"storage-template-version\": \"2.2\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"HELM-RELEASE-NAME\",\"user-secret-parameters\": { \"entry.name\": \"HELM-RELEASE-NAME\",  { \"entry.name\": \"PARAMETERS\",\"user-secret-parameters\": { \"entry.name\": \"PARAMETERS\",  { \"entry.name\": \"LICENSE\",\"user-secret-parameters\": { \"entry.name\": \"LICENSE\",  { \"entry.name\": \"COS-ENDPOINT\",\"user-secret-parameters\": { \"entry.name\": \"COS-ENDPOINT\",  { \"entry.name\": \"COS-STORAGECLASS\",\"user-secret-parameters\": { \"entry.name\": \"COS-STORAGECLASS\", 
    ```
    {: pre}






{{site.data.content.managing-configurations-and-assignments}}

## Deploying an app that uses {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-app}


You can use the `ibm-object-s3fs` driver to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references your object storage configuration.

    ```yaml 
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata: 
        name: pvc-cos #Enter a name for your PVC.
        namespace: default
        annotations: 
        ibm.io/auto-create-bucket: "false"
        ibm.io/auto-delete-bucket: "false" 
        ibm.io/bucket: <BUCKET-NAME> #Enter the name of your object storage bucket.
        ibm.io/secret-name: <SECRET-NAME> #Enter the name of the secret you created earlier.
        ibm.io/secret-namespace: <default> #Enter the namespace where you want to create the PVC.
    spec: 
        accessModes:
        - ReadWriteOnce
        resources:
            requests:
                storage: 10gi
        storageClassName: ibmc-s3fs-cos #The storage class that you want to use.
    ```
    {: codeblock}

1. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc-cos.yaml
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you create. 

    ```yaml
    apiVersion: v1
    kind: List
    metadata:
      name: ibm-object-storage-plugin
      namespace: kube-system
      annotations:
        revision: 1  
    items:
      - apiVersion: satstorage.ibm.com/v1
        kind: SatStorageHelmChart
        metadata:
          name: ibm-object-storage-plugin
          namespace: ibm-satellite-storage
        spec:
          helm-repo-url: "raw.githubusercontent.com/IBM/charts/master/repo/ibm-helm"
          helm-chart-name: "ibm-object-storage-plugin"
          helm-chart-namespace: "ibm-object-s3fs"
          helm-chart-version: "2.2.1"  
          release-name: "{{{ helm-release-name }}}"
          storage-template-revision: "1" 
          params: 
            - license={{{ license }}}
            - cos.endpoint={{{ cos-endpoint }}}
            - cos.storageClass={{{ cos-storageclass }}}
            {{#parameters }}
            - "{{{ item }}}"
            {{/parameters }}
    ```
    {: codeblock}

1. Create the pod in your cluster. 

    ```sh
    oc apply -f ibm-object-storage-plugin
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state. 

    ```sh
    oc get pods
    ```
    {: pre}

    ```sh
        NAME                                READY   STATUS    RESTARTS   AGE
        ibm-object-storage-plugin           1/1     Running   0          2m58s
        ```
        {: screen}

1. Verify that the app can write to your block storage volume by logging in to your pod.

    ```sh
    oc exec ibm-object-storage-plugin
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



## Removing the {{site.data.keyword.cos_full_notm}} storage configuration using the console
{: #config-storage-cos-rm-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

## Removing the {{site.data.keyword.cos_full_notm}} storage configuration using the command line
{: #config-storage-cos-rm-cli}
{: cli}

If you no longer need your {{site.data.keyword.cos_full_notm}} configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters. 
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.

    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

1. Remove the assignment. After the assignment is removed, the driver pods and storage classes are removed from all clusters that were part of the storage assignment.

    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

1. Verify that the driver is removed from your cluster.

    1. List of the storage classes in your cluster and verify that the storage classes are removed.
    
        ```sh
        oc get sc
        ```
        {: pre}

    1. List the pods in the `kube-system` namespace and verify that the storage driver pods are removed.
    
        ```sh
        oc get pods -n kube-system | grep cos
        ```
        {: pre}

1. Optional: Remove the storage configuration.

    1. List the storage configurations.
    
        ```sh
        ibmcloud sat storage config ls
        ```
        {: pre}

    1. Remove the storage configuration.
    
        ```sh
        ibmcloud sat storage config rm --config <config_name>
        ```
        {: pre}


## Parameter reference
{: #ibm-object-storage-plugin-parameter-reference}

### 2.2 parameter reference
{: #2.2-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| Helm Chart Release Name | `helm-release-name` | Release name of the chart | false | 
| Helm Chart Additional Parameters (Optional) | `parameters` | Helm Chart Additional Parameters (Optional) | false | 
| COS plug-in License: Apache License Version 2.0 | `license` | COS plug-in License: Apache License Version 2.0. Set to `true` to accept the license and install the plugin | true | 
| COS Endpoint | `cos-endpoint` | Enter COS Endpoint. For more information, refer to https://ibm.biz/cos-endpoints | true | 
| COS storageclass | `cos-storageclass` | Enter COS storageclass. For more info, refer to https://ibm.biz/cos-storage-classes | true | 
{: caption="Table 1. 2.2 parameter reference" caption-side="bottom"}


## Storage class reference for {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-sc-ref}

| Storage class name | Volume binding mode | Retain | 
| --- | --- | --- |
| `ibm-s3fs-cos` | Immediate | False |
| `ibm-s3fs-cos-perf` | Immediate | False |
{: caption="Table 2. Cloud Object storage class reference" caption-side="bottom"}

## Getting help and support for {{site.data.keyword.cos_full_notm}}
{: #sat-storage-cos-support}

If you run into an issue with using {{site.data.keyword.block_storage_is_short}} you can submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.


