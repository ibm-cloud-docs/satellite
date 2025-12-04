---


copyright:
  years: 2020, 2025
lastupdated: "2025-12-04"

keywords: satellite storage, google, csi, gcp, satellite configurations, google storage, gce, compute engine

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Google Compute Engine persistent disk Container Storage Interface (CSI) Driver
{: #storage-gcp-compute-persistent-disk-csi-driver}

The Compute Engine persistent disk Container Storage Interface (CSI) [Driver](https://github.com/kubernetes-sigs/gcp-compute-persistent-disk-csi-driver/tree/v1.0.4){: external} is a CSI compliant driver that you can use to manage the lifecycle of your Google Compute Engine persistent disks.
{: shortdesc}
 
Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

## Prerequisites
{: #sat-storage-gcp-csi-prereq}

1. [Create a Compute Engine service account](https://docs.cloud.google.com/compute/docs/access/service-accounts){: external}.
1. [Create a JSON web key](https://docs.cloud.google.com/iam/docs/keys-create-delete#creating){: external}.








## Creating and assigning a configuration in the console
{: #gcp-compute-persistent-disk-csi-driver-config-create-console}
{: ui}


1. Review the [parameter reference](#gcp-compute-persistent-disk-csi-driver-parameter-reference).


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
{: #gcp-compute-persistent-disk-csi-driver-config-create-cli}
{: cli}


1. Review the [parameter reference](#gcp-compute-persistent-disk-csi-driver-parameter-reference) for the template version that you want to use.


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

## Creating a configuration in the API
{: #gcp-compute-persistent-disk-csi-driver-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#gcp-compute-persistent-disk-csi-driver-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.0.4 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"gcp-compute-persistent-disk-csi-driver\", \"storage-template-version\": \"1.0.4\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"PROJECT_ID\",{ \"entry.name\": \"PRIVATE_KEY_ID\",{ \"entry.name\": \"PRIVATE_KEY\",{ \"entry.name\": \"CLIENT_EMAIL\",{ \"entry.name\": \"CLIENT_ID\",{ \"entry.name\": \"AUTH_URI\",{ \"entry.name\": \"TOKEN_URI\",{ \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",{ \"entry.name\": \"CLIENT_X509_CERT_URL\",}
    ```
    {: pre}


    Example request to create a version 1.7.1 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"gcp-compute-persistent-disk-csi-driver\", \"storage-template-version\": \"1.7.1\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"PROJECT_ID\",{ \"entry.name\": \"PRIVATE_KEY_ID\",{ \"entry.name\": \"PRIVATE_KEY\",{ \"entry.name\": \"CLIENT_EMAIL\",{ \"entry.name\": \"CLIENT_ID\",{ \"entry.name\": \"AUTH_URI\",{ \"entry.name\": \"TOKEN_URI\",{ \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",{ \"entry.name\": \"CLIENT_X509_CERT_URL\",}
    ```
    {: pre}


    Example request to create a version 1.8.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"gcp-compute-persistent-disk-csi-driver\", \"storage-template-version\": \"1.8.0\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"PROJECT_ID\",{ \"entry.name\": \"PRIVATE_KEY_ID\",{ \"entry.name\": \"PRIVATE_KEY\",{ \"entry.name\": \"CLIENT_EMAIL\",{ \"entry.name\": \"CLIENT_ID\",{ \"entry.name\": \"AUTH_URI\",{ \"entry.name\": \"TOKEN_URI\",{ \"entry.name\": \"AUTH_PROVIDER_X509_CERT_URL\",{ \"entry.name\": \"CLIENT_X509_CERT_URL\",}
    ```
    {: pre}









{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-autopatch-cli}}
{{site.data.content.assignment-upgrade-api}}
{{site.data.content.assignment-autopatch-api}}


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

1. Remove any pods that mount the PVC. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC.
    
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
{: #gcp-compute-persistent-disk-csi-driver-1.0.4-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Google Cloud project ID. | `project_id` | Secret | Google Cloud project ID. You can find your Project ID from the Google Cloud dashboard. | N/A |
| Google Cloud private key ID | `private_key_id` | Secret | Google Cloud private key ID. You can find this in the JSON service account key file. | N/A |
| Private key of the service account. | `private_key` | Secret | Private key of the service account. You can find the service account key on the Service Account section of the project dashboard. | N/A |
| Client email | `client_email` | Secret | The email of the service account can be found in the IAM & Admin section of the project dashboard. | N/A |
| Client ID | `client_id` | Secret | Client ID. You can find the Client ID in the APIs & Services section of the project dashboard. | N/A |
| Authorization URI | `auth_uri` | Secret | Authorization URI for the service account. You can find this in the JSON service account key file. | N/A |
| Token URI | `token_uri` | Secret | Token URI for the service account. You can find this in the JSON service account key file. | N/A |
| URL for the authorization provider certificate | `auth_provider_x509_cert_url` | Secret | URL for the authorization provider certificate. You can find this in the JSON service account key file. | N/A |
| URL for the client certificate | `client_x509_cert_url` | Secret | URL for the client certificate. You can find this in the JSON service account key file. | N/A |
{: caption="Table 1. 1.0.4 parameter reference" caption-side="bottom"}


### 1.7.1 parameter reference
{: #gcp-compute-persistent-disk-csi-driver-1.7.1-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Google Cloud project ID | `project_id` | Secret | Google Cloud project ID. You can find your Project ID from the Google Cloud dashboard. | N/A |
| Google Cloud private key ID | `private_key_id` | Secret | Google Cloud private key ID. You can find this in the JSON service account key file. | N/A |
| Private key of the service account | `private_key` | Secret | Private key of the service account. You can find the service account key on the Service Account section of the project dashboard. | N/A |
| Client email | `client_email` | Secret | The email of the service account can be found in the IAM & Admin section of the project dashboard. | N/A |
| Client ID | `client_id` | Secret | Client ID. You can find the Client ID in the APIs & Services section of the project dashboard. | N/A |
| Authorization URI | `auth_uri` | Secret | Authorization URI for the service account. You can find this in the JSON service account key file. | N/A |
| Token URI | `token_uri` | Secret | Token URI for the service account. You can find this in the JSON service account key file. | N/A |
| URL for the authorization provider certificate | `auth_provider_x509_cert_url` | Secret | URL for the authorization provider certificate. You can find this in the JSON service account key file. | N/A |
| URL for the client certificate | `client_x509_cert_url` | Secret | URL for the client certificate. You can find this in the JSON service account key file. | N/A |
{: caption="Table 2. 1.7.1 parameter reference" caption-side="bottom"}


### 1.8.0 parameter reference
{: #gcp-compute-persistent-disk-csi-driver-1.8.0-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| Google Cloud project ID | `project_id` | Secret | Google Cloud project ID. You can find your Project ID from the Google Cloud dashboard. | N/A |
| Google Cloud private key ID | `private_key_id` | Secret | Google Cloud private key ID. You can find this in the JSON service account key file. | N/A |
| Private key of the service account | `private_key` | Secret | PrPrivate key of the service account. You can find the service account key on the Service Account section of the project dashboard. | N/A |
| Client email | `client_email` | Secret | The email of the service account can be found in the IAM & Admin section of the project dashboard. | N/A |
| Client ID | `client_id` | Secret | Client ID. You can find the Client ID in the APIs & Services section of the project dashboard. | N/A |
| Authorization URI | `auth_uri` | Secret | Authorization URI for the service account. You can find this in the JSON service account key file. | N/A |
| Token URI | `token_uri` | Secret | Token URI for the service account. You can find this in the JSON service account key file. | N/A |
| URL for the authorization provider certificate | `auth_provider_x509_cert_url` | Secret | URL for the authorization provider certificate. You can find this in the JSON service account key file. | N/A |
| URL for the client certificate | `client_x509_cert_url` | Secret | URL for the client certificate. You can find this in the JSON service account key file. | N/A |
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
{: caption="Storage class reference for Google compute engine persistent disk storage" caption-side="bottom"}


## Getting help and support for Google Compute Engine
{: #sat-gcp-csi-support}

When you use Google Compute Engine, try the following resources before you open a support case. 
{: shortdesc}

1. Review the FAQs in the [Google Cloud docs](https://docs.cloud.google.com/storage/docs/faq){: external}.
1. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-storage-must-gather) to troubleshoot and resolve common issues.
1. Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
1. Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. Tag any questions with `ibm-cloud` and `Google-Cloud`.
1. Open an issue in the [Google Cloud Console](https://cloud.google.com/support-hub){: external}.
