---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-20"

keywords: satellite storage, google, csi, gcp, satellite configurations, google storage, gce, compute engine

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Google Compute Engine persistent disk Container Storage Interface (CSI) Driver
{: #storage-gcp-compute-persistent-disk-csi-driver}

The Compute Engine persistent disk Container Storage Interface (CSI) [Driver](https://github.com/kubernetes-sigs/gcp-compute-persistent-disk-csi-driver/tree/v1.0.4){: external} is a CSI compliant driver that you can use to manage the lifecycle of your Google Compute Engine persistent disks.
{: shortdesc}
 
Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for Compute Engine
{: #sat-storage-gcp-csi-prereq}

1. [Create a Compute Engine service account](https://cloud.google.com/compute/docs/access/service-accounts){: external}.
1. [Create a JSON web key](https://cloud.google.com/iam/docs/creating-managing-service-account-keys#creating){: external}.
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).





## Creating a configuration
{: #gcp-compute-persistent-disk-csi-driver-config-create}

Before you begin, review the [parameter reference](#gcp-compute-persistent-disk-csi-driver-parameter-reference) for the template version that you want to use.
{: important}

### Creating and assigning a configuration in the console
{: gcp-compute-persistent-disk-csi-driver-config-create-console}
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
{: #gcp-compute-persistent-disk-csi-driver-config-create-cli}
{: cli}

1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 1.0.4 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name gcp-compute-persistent-disk-csi-driver --template-version 1.0.4 --param "project_id=PROJECT_ID"  --param "private_key_id=PRIVATE_KEY_ID"  --param "private_key=PRIVATE_KEY"  --param "client_email=CLIENT_EMAIL"  --param "client_id=CLIENT_ID"  --param "auth_uri=AUTH_URI"  --param "token_uri=TOKEN_URI"  --param "auth_provider_x509_cert_url=AUTH_PROVIDER_X509_CERT_URL"  --param "client_x509_cert_url=CLIENT_X509_CERT_URL" 
    ```
    {: pre}

    Example command to create a version 1.7.1 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name gcp-compute-persistent-disk-csi-driver --template-version 1.7.1 --param "project_id=PROJECT_ID"  --param "private_key_id=PRIVATE_KEY_ID"  --param "private_key=PRIVATE_KEY"  --param "client_email=CLIENT_EMAIL"  --param "client_id=CLIENT_ID"  --param "auth_uri=AUTH_URI"  --param "token_uri=TOKEN_URI"  --param "auth_provider_x509_cert_url=AUTH_PROVIDER_X509_CERT_URL"  --param "client_x509_cert_url=CLIENT_X509_CERT_URL" 
    ```
    {: pre}

    Example command to create a version 1.8.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name gcp-compute-persistent-disk-csi-driver --template-version 1.8.0 --param "project_id=PROJECT_ID"  --param "private_key_id=PRIVATE_KEY_ID"  --param "private_key=PRIVATE_KEY"  --param "client_email=CLIENT_EMAIL"  --param "client_id=CLIENT_ID"  --param "auth_uri=AUTH_URI"  --param "token_uri=TOKEN_URI"  --param "auth_provider_x509_cert_url=AUTH_PROVIDER_X509_CERT_URL"  --param "client_x509_cert_url=CLIENT_X509_CERT_URL" 
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
{: #gcp-compute-persistent-disk-csi-driver-assignment-create}

After you create a storage configuration, you can assign that configuration to clusters, cluster groups, or service clusters to automatically deploy storage resources across clusters in your Location.

If you haven't yet created a storage configuration, see [Creating a configuration](#gcp-compute-persistent-disk-csi-driver-config-create).

### Creating an assignment in the console
{: #gcp-compute-persistent-disk-csi-driver-assignment-create-console}
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
{: #gcp-compute-persistent-disk-csi-driver-assignment-create-cli}
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
{: #gcp-compute-persistent-disk-csi-driver-assignment-create-console}
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
{: #gcp-compute-persistent-disk-csi-driver-assignment-update}

Update your assignments to rollout your configurations to new or different clusters, cluster groups, or service.


### Updating an assignment in the console
{: #gcp-compute-persistent-disk-csi-driver-assignment-update-console}
{: ui}

Updating assignments in the console is currently not available.
{: note}

However, you can use the [CLI](#gcp-compute-persistent-disk-csi-driver-assignment-update-cli) or [API](#gcp-compute-persistent-disk-csi-driver-assignment-update-api) to update your assignemnts.


### Updating an assignment in the CLI
{: #gcp-compute-persistent-disk-csi-driver-assignment-update-cli}
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
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name gcp-compute-persistent-disk-csi-driver --template-version 1.8.0 --param "project_id=PROJECT_ID"  --param "private_key_id=PRIVATE_KEY_ID"  --param "private_key=PRIVATE_KEY"  --param "client_email=CLIENT_EMAIL"  --param "client_id=CLIENT_ID"  --param "auth_uri=AUTH_URI"  --param "token_uri=TOKEN_URI"  --param "auth_provider_x509_cert_url=AUTH_PROVIDER_X509_CERT_URL"  --param "client_x509_cert_url=CLIENT_X509_CERT_URL" 
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
{: #gcp-compute-persistent-disk-csi-driver-config-create-api}

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.0.4 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"gcp-compute-persistent-disk-csi-driver\", \"storage-template-version\": \"1.0.4\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"PROJECT_ID\",\"user-secret-parameters\": { \"entry.name\": \"PROJECT_ID\",  { \"entry.name\": \"PRIVATE_KEY_ID\",\"user-secret-parameters\": { \"entry.name\": \"PRIVATE_KEY_ID\",  { \"entry.name\": \"PRIVATE_KEY\",\"user-secret-parameters\": { \"entry.name\": \"PRIVATE_KEY\",  { \"entry.name\": \"CLIENT_EMAIL\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_EMAIL\",  { \"entry.name\": \"CLIENT_ID\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_ID\",  { \"entry.name\": \"AUTH_URI\",\"user-secret-parameters\": { \"entry.name\": \"AUTH_URI\",  { \"entry.name\": \"TOKEN_URI\",\"user-secret-parameters\": { \"entry.name\": \"TOKEN_URI\",  { \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",\"user-secret-parameters\": { \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",  { \"entry.name\": \"CLIENT_X509_CERT_URL\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_X509_CERT_URL\", 
    ```
    {: pre}

    Example request to create a version 1.7.1 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"gcp-compute-persistent-disk-csi-driver\", \"storage-template-version\": \"1.7.1\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"PROJECT_ID\",\"user-secret-parameters\": { \"entry.name\": \"PROJECT_ID\",  { \"entry.name\": \"PRIVATE_KEY_ID\",\"user-secret-parameters\": { \"entry.name\": \"PRIVATE_KEY_ID\",  { \"entry.name\": \"PRIVATE_KEY\",\"user-secret-parameters\": { \"entry.name\": \"PRIVATE_KEY\",  { \"entry.name\": \"CLIENT_EMAIL\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_EMAIL\",  { \"entry.name\": \"CLIENT_ID\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_ID\",  { \"entry.name\": \"AUTH_URI\",\"user-secret-parameters\": { \"entry.name\": \"AUTH_URI\",  { \"entry.name\": \"TOKEN_URI\",\"user-secret-parameters\": { \"entry.name\": \"TOKEN_URI\",  { \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",\"user-secret-parameters\": { \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",  { \"entry.name\": \"CLIENT_X509_CERT_URL\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_X509_CERT_URL\", 
    ```
    {: pre}

    Example request to create a version 1.8.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"gcp-compute-persistent-disk-csi-driver\", \"storage-template-version\": \"1.8.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"PROJECT_ID\",\"user-secret-parameters\": { \"entry.name\": \"PROJECT_ID\",  { \"entry.name\": \"PRIVATE_KEY_ID\",\"user-secret-parameters\": { \"entry.name\": \"PRIVATE_KEY_ID\",  { \"entry.name\": \"PRIVATE_KEY\",\"user-secret-parameters\": { \"entry.name\": \"PRIVATE_KEY\",  { \"entry.name\": \"CLIENT_EMAIL\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_EMAIL\",  { \"entry.name\": \"CLIENT_ID\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_ID\",  { \"entry.name\": \"AUTH_URI\",\"user-secret-parameters\": { \"entry.name\": \"AUTH_URI\",  { \"entry.name\": \"TOKEN_URI\",\"user-secret-parameters\": { \"entry.name\": \"TOKEN_URI\",  { \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",\"user-secret-parameters\": { \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",  { \"entry.name\": \"CLIENT_X509_CERT_URL\",\"user-secret-parameters\": { \"entry.name\": \"CLIENT_X509_CERT_URL\", 
    ```
    {: pre}





1. Verify that your storage configuration is created.

    ```sh
    ibmcloud sat storage config get --config <CONFIG>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-gcp-csi)

### Creating the Google Compute Engine persistent disk storage configuration from the console
{: #sat-storage-gcp-ui}
{: ui}


Use the console to create a Google Compute Engine persistent disk storage configuration for your location.
{: shortdesc}

Before you begin, review and complete the [prerequisites](#sat-storage-gcp-csi-prereq) and review the [parameter reference](#gcp-compute-persistent-disk-csi-driver-parameter-reference).

{sat-storage-config-create-console.md}



## Deploying an app that uses Google Compute Engine persistent disk
{: #sat-storage-gcp-deploy-app}

You can use the `gce-pd-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references a GCP storage class that you created earlier.

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: pvc-gce
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 10Gi
      storageClassName: sat-gce-block-silver
    ```
    {: codeblock}
        
1. Create the PVC in your cluster. 

    ```sh
    oc apply -f pvc-gce.yaml
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you created. 

    ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: statefulset-gce
      labels:
        app: nginx
    spec:
      podManagementPolicy: Parallel  # default is OrderedReady
      serviceName: statefulset-gce
      replicas: 1
      template:
        metadata:
          labels:
            app: nginx
        spec:
          nodeSelector:
            "kubernetes.io/os": linux
          containers:
            - name: statefulset-gce
              image: mcr.microsoft.com/oss/nginx/nginx:1.19.5
              command:
                - "/bin/bash"
                - "-c"
                - set -euo pipefail; while true; do echo $(date) >> /mnt/gce/outfile; sleep 1; done
              volumeMounts:
                - name: persistent-storage
                  mountPath: /mnt/gce
      updateStrategy:
        type: RollingUpdate
      selector:
        matchLabels:
          app: nginx
      volumeClaimTemplates:
        - metadata:
            name: persistent-storage
            annotations:
              volume.beta.kubernetes.io/storage-class: sat-gce-block-silver
          spec:
            accessModes: ["ReadWriteOnce"]
            resources:
              requests:
                storage: 10Gi
    ```
    {: codeblock}

1. Create the pod in your cluster.

    ```sh
    oc apply -f statefulset-gce.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}
    
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    statefulset-gce                          1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your persistent disk by logging in to your pod.

    ```sh
    oc exec web-server -it bash
    ```
    {: pre}

1. View the contents of the `outfile` file to confirm that your app can write data to your persistent storage.

    ```sh
    cat /mnt/gce/outfile
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



## Removing Compute Engine storage from your apps
{: #gcp-rm-apps}


If you no longer need your Google Compute Engine configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters.
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
        app    sat-gce-block-platinum
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

## Removing the Compute Engine storage configuration from your cluster
{: #gcp-template-rm}


If you no longer plan on using your persistent disk storage in your cluster, you can use the CLI unassign your cluster from the storage configuration.
{: shortdesc}

Note that if you remove the storage configuration, the driver is then uninstalled from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}

### Removing the Google Compute Engine storage configuration from the console
{: #gcp-template-rm-ui}
{: ui}

Use the console to remove a storage configuration.
{: shortdesc}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.


### Removing the Google Compute Engine storage configuration from the CLI
{: #gcp-template-rm-cli}
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
        oc get pods -n kube-system | grep gce
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
{: #gcp-compute-persistent-disk-csi-driver-parameter-reference}

### 1.0.4 parameter reference
{: #1.0.4-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| Google Cloud project ID. | `project_id` | Google Cloud project ID. You can find your Project ID from the Google Cloud dashboard. | true | 
| Google Cloud private key ID | `private_key_id` | Google Cloud private key ID. You can find this in the JSON service account key file. | true | 
| Private key of the service account. | `private_key` | Private key of the service account. You can find the service account key on the Service Account section of the project dashboard. | true | 
| Client email | `client_email` | Client email. The email of ther service account can be found in the IAM & Admin section of the project dashboard. | true | 
| Client ID | `client_id` | Client ID. You can find the Client ID in the APIs & Services section of the project dashboard. | true | 
| Authorization URI | `auth_uri` | Authorization URI for the service account. You can find this in the JSON service account key file. | true | 
| Token URI | `token_uri` | Token URI for the service account. You can find this in the JSON service account key file. | true | 
| URL for the authorization provider certificate | `auth_provider_x509_cert_url` | URL for the authorization provider certificate. You can find this in the JSON service account key file. | true | 
| URL for the client certificate | `client_x509_cert_url` | URL for the client certificate. You can find this in the JSON service account key file. | true | 
{: caption="Table 1. 1.0.4 parameter reference" caption-side="bottom"}


### 1.7.1 parameter reference
{: #1.7.1-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| Google Cloud project ID | `project_id` | Google Cloud project ID. You can find your Project ID from the Google Cloud dashboard. | true | 
| Google Cloud private key ID | `private_key_id` | Google Cloud private key ID. You can find this in the JSON service account key file. | true | 
| Private key of the service account | `private_key` | Private key of the service account. You can find the service account key on the Service Account section of the project dashboard. | true | 
| Client email | `client_email` | Client email. The email of ther service account can be found in the IAM & Admin section of the project dashboard. | true | 
| Client ID | `client_id` | Client ID. You can find the Client ID in the APIs & Services section of the project dashboard. | true | 
| Authorization URI | `auth_uri` | Authorization URI for the service account. You can find this in the JSON service account key file. | true | 
| Token URI | `token_uri` | Token URI for the service account. You can find this in the JSON service account key file. | true | 
| URL for the authorization provider certificate | `auth_provider_x509_cert_url` | URL for the authorization provider certificate. You can find this in the JSON service account key file. | true | 
| URL for the client certificate | `client_x509_cert_url` | URL for the client certificate. You can find this in the JSON service account key file. | true | 
{: caption="Table 2. 1.7.1 parameter reference" caption-side="bottom"}


### 1.8.0 parameter reference
{: #1.8.0-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| Google Cloud project ID | `project_id` | Google Cloud project ID. You can find your Project ID from the Google Cloud dashboard. | true | 
| Google Cloud private key ID | `private_key_id` | Google Cloud private key ID. You can find this in the JSON service account key file. | true | 
| Private key of the service account | `private_key` | PrPrivate key of the service account. You can find the service account key on the Service Account section of the project dashboard. | true | 
| Client email | `client_email` | Client email. The email of ther service account can be found in the IAM & Admin section of the project dashboard. | true | 
| Client ID | `client_id` | Client ID. You can find the Client ID in the APIs & Services section of the project dashboard. | true | 
| Authorization URI | `auth_uri` | Authorization URI for the service account. You can find this in the JSON service account key file. | true | 
| Token URI | `token_uri` | Token URI for the service account. You can find this in the JSON service account key file. | true | 
| URL for the authorization provider certificate | `auth_provider_x509_cert_url` | URL for the authorization provider certificate. You can find this in the JSON service account key file. | true | 
| URL for the client certificate | `client_x509_cert_url` | URL for the client certificate. You can find this in the JSON service account key file. | true | 
{: caption="Table 3. 1.8.0 parameter reference" caption-side="bottom"}


## Storage class reference for Compute Engine
{: #sat-storage-gcp-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for Google compute engine persistent disk storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

 Storage class name | Default Read IOPS per GB | Default Write IOPS per GB | Size range (per disk) | Hard disk | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-gce-block-platinum` | NA | NA | 500 GB - 64 TB | SSD | Delete | Immediate |
| `sat-gce-block-platinum-metro`  | NA | NA | 500 GB - 64 TB | SSD | Delete | WaitForFirstConsumer |
| `sat-gce-block-gold` | 30 | 30 | 10 GB - 64 TB | SSD | Delete | Immediate |
| `sat-gce-block-gold-metro` **Default** | 30 | 30 | 10 GB - 64 TB | SSD | Delete | WaitForFirstConsumer |
| `sat-gce-block-silver`  | 6 | 30 | 10 GB - 64 GB | SSD | Delete | Immediate |
| `sat-gce-block-silver-metro` | 6 | 6 | 10 GB - 64 GB | SSD | Delete | WaitForFirstConsumer |
| `sat-gce-block-bronze`  | 0.75 | 1.5 | 10 GiB - 64 TiB | HDD | Delete | Immediate |
| `sat-gce-block-bronze-metro` | 0.75 | 1.5 | 10 GiB - 64 TiB | HDD | Delete | WaitForFirstConsumer |
{: caption="Table 2. Storage class reference for Google compute engine persistent disk storage" caption-side="bottom"}


## Getting help and support for Google Compute Engine
{: #sat-gcp-csi-support}

If you run into an issue with using Google Persistent disk storage, you can open an issue in the [Google Cloud Console](https://cloud.google.com/support-hub){: external}.
