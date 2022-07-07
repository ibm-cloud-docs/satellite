---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite storage, google, csi, gcp, satellite configurations, google storage, gce, compute engine

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Google Compute Engine persistent disk Container Storage Interface (CSI) Driver
{: #config-storage-gcp-csi}

The Compute Engine persistent disk Container Storage Interface (CSI) [Driver](https://github.com/kubernetes-sigs/gcp-compute-persistent-disk-csi-driver/tree/v1.0.4){: external} is a CSI compliant driver that you can use to manage the lifecycle of your Google Compute Engine persistent disks.
{: shortdesc}

 The template is currently in beta. Do not use it for production workloads. 
 {: beta}
 
Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for Compute Engine
{: #sat-storage-gcp-csi-prereq}

1. [Create a Compute Engine service account](https://cloud.google.com/compute/docs/access/service-accounts){: external}.
1. [Create a JSON web key](https://cloud.google.com/iam/docs/service-accounts#key-types){: external}.
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).


## Creating the Google Compute Engine persistent disk configuration in the command line
{: #sat-storage-gcp-create-config}
{: cli}

Create a storage configuration in the command line by using the Google Compute Engine persistent disk template.
{: shortdesc}

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
    

1. Review the [template parameters](#sat-storage-gcp-csi-params-cli).

1. Create storage configuration. You can pass parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Note that Kubernetes resources can't contain capital letters or special characters. Enter a name for your config that uses only lowercase letters, numbers, hyphens or periods.

    ```sh
    ibmcloud sat storage config create --name <config_name> --template-name gcp-compute-persistent-disk-csi-driver --template-version 1.0.4 --location <location_name> -p "project_id=" -p "private_key_id= " -p "private_key=" -p "client_email=" -p "client_id=" -p "auth_uri=" -p "token_uri=" -p "auth_provider_x509_cert_url=" -p "client_x509_cert_url="
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

Before you begin, review and complete the [prerequisites](#sat-storage-gcp-csi-prereq) and review the [parameter reference](#sat-storage-gcp-csi-params-cli).

{sat-storage-config-create-console.md}

### Assigning a Compute Engine storage configuration in the command line
{: #assign-storage-gcp-csi}
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
    kubectl get pods -n gce-pd-csi-driver | grep gce
    csi-gce-pd-controller-7d89bf967d-92mc8                    5/5     Running             0          14m
    csi-gce-pd-node-4d66j                                     2/2     Running             0          14m
    csi-gce-pd-node-4mcvr                                     2/2     Running             0          14m
    csi-gce-pd-node-8vsmb                                     2/2     Running             0          14m
    ```
    {: screen}

1. List the storage classes.

    ```sh
    kubectl get sc -n gce-pd-csi-driver
    sat-gce-block-bronze           pd.csi.storage.gke.io   Delete          Immediate              false                  20s
    sat-gce-block-bronze-metro     pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   false                  21s
    sat-gce-block-gold             pd.csi.storage.gke.io   Delete          Immediate              false                  20s
    sat-gce-block-gold-metro       pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   false                  21s
    sat-gce-block-platinum         pd.csi.storage.gke.io   Delete          Immediate              false                  19s
    sat-gce-block-platinum-metro   pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   false                  20s
    sat-gce-block-silver           pd.csi.storage.gke.io   Delete          Immediate              false                  20s
    sat-gce-block-silver-metro     pd.csi.storage.gke.io   Delete          WaitForFirstConsumer   false                  21s
    ```
    {: screen}

1. [Deploy an app that uses your persistent disk storage](#sat-storage-gcp-deploy-app)

## Deploying an app that uses Google Compute Engine persistent disk
{: #sat-storage-gcp-deploy-app}
{: cli}

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

## Upgrading a Compute Engine storage configuration
{: #gcp-upgrade-config}
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

## Upgrading a Compute Engine storage assignment
{: #gcp-upgrade-assignment}
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

## Updating a Compute Engine storage assignment
{: #gcp-update-assignment}
{: cli}

## Removing Compute Engine storage from your apps
{: #gcp-rm-apps}
{: cli}

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


## Parameter reference for Google Compute Engine
{: #sat-storage-gcp-csi-params-cli}

| Parameter | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `project_id` | Required | Google Cloud Provider Project ID. You can find your Project ID from the Google cloud dashboard. | N/A |
| `private_key_id` | Required | Google Cloud Provider Private Key ID. This can be found in the JSON service account key file. | N/A |
| `private_key` | Required | Private Key of the Service Account. You can find the service account key on the **Service Account section** of the project dashboard. | N/A |
| `client_email` | Required | Client Email. The email of the service account can be found in the **IAM & Admin** section of the project dashboard. | N/A |
| `client_id` | Required | Client ID. You can find the Client ID in the **APIs & Services** section of the project dashboard. | N/A |
| `auth_uri` | Required | Auth URI for the Service Account. This can be found in the JSON service account key file. | N/A |
| `token_uri` | Required | Token URI for the Service Account. This can be found in the JSON service account key file. | N/A |
| `auth_provider_x509_cert_url` | Required | URL for the Auth Provider Certificate. This can be found in the JSON service account key file. | N/A |
| `client_x509_cert_url` | Required | URL for the Client Certificate. This can be found in the JSON service account key file. | N/A |
{: caption="Table 1. Parameter reference for Google compute engine persistent disk storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates required parameters. The third column is a brief description of the parameter."}

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
{: caption="Table 2. Storage class reference for Google compute engine persistent disk storage" caption-side="top"}


## Getting help and support for Google Compute Engine
{: #sat-gcp-csi-support}

If you run into an issue with using Google Persistent disk storage, you can open an issue in the [Google Cloud Console](https://cloud.google.com/support-hub){: external}.
