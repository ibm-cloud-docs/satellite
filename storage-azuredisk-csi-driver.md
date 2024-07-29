---


copyright:
  years: 2020, 2024
lastupdated: "2024-07-26"

keywords: azure storage, satellite storage, satellite config, satellite configurations, azure disk csi, azure disk

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Azure Disk CSI driver
{: #storage-azuredisk-csi-driver}

The Azure Disk CSI driver for {{site.data.keyword.satellitelong}} implements the CSI specification so that container orchestration tools can manage the lifecycle of Azure Disk volumes.
{: shortdesc}

For an overview of the available features of the Azure Disk CSI driver, see [Features](https://github.com/kubernetes-sigs/azuredisk-csi-driver#features){: external}.

The Azure Disk CSI driver template for {{site.data.keyword.satelliteshort}} is currently available for cluster versions 4.7 and later.

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot customize your storage classes because {{site.data.keyword.satelliteshort}} Config overwrites your changes. 
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

## Prerequisites for using Azure Disk
{: #sat-storage-azure-csi-prereq}

Set up [Azure Disk storage](https://learn.microsoft.com/en-us/azure/aks/azure-disk-csi){: external} for {{site.data.keyword.satelliteshort}} clusters by creating a storage configuration in your location. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

To use the Azure Disk CSI driver storage template, complete the following tasks.

1. Create an Azure location by using the [location template](/docs/satellite?topic=satellite-azure) or manually [adding Azure hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-azure#azure-host-attach). 
    If you choose to manually assign hosts, you must [label your worker nodes](#azure-disk-label-nodes) before creating your storage configuration.
    {: important}
    

    
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters) that runs on compute hosts in Azure. Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage.
1. [Create your configuration file](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver&interface=ui#azuredisk-csi-driver-config-create-console).

### Optional: Labeling your worker nodes for Azure Disk
{: #azure-disk-label-nodes}

Complete the following steps to add the required labels to your worker nodes for the Azure Disk CSI driver template.
{: shortdesc}

If you manually assigned your Azure hosts to your Location and did not use the Schematics template in the console, you must [label your worker nodes](#azure-disk-label-nodes) before creating your storage configuration.
{: important}


1. List your Azure worker nodes and make a note of the `name` of each node.

    ```sh
    oc get nodes
    ```
    {: pre}

1. Get the details of each node and make a note of the `zone` that the node is in. For example: `eastus-1`.

    ```sh
    oc get nodes <node-name> -o yaml | grep zone
    ```
    {: pre}

1. Label your worker nodes with the `zone` value that you retrieved earlier. Replace `<node-name>` and `<zone>` with the node name and zone of your worker node. For example, if you have a worker node in zone: `eastus-1`, use the following commands to add `eastus-1` as a label to the worker node in the `eastus-1` zone.

    ```sh
    oc label node <node-name> topology.kubernetes.io/zone-
    oc label node <node-name> topology.kubernetes.io/zone=<zone> --overwrite
    ```
    {: pre}

1. Repeat the previous steps for each worker node


1. [Sign in to your Azure account](https://azure.microsoft.com/en-us/get-started/){: external} and retrieve the required parameters. For more information about the parameters, see [Cluster config](https://cloud-provider-azure.sigs.k8s.io/install/configs/#cluster-config).




## Creating and assigning a configuration in the console
{: #azuredisk-csi-driver-config-create-console}
{: ui}


1. Review the [parameter reference](#azuredisk-csi-driver-parameter-reference).


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
{: #azuredisk-csi-driver-config-create-cli}
{: cli}


1. Review the [parameter reference](#azuredisk-csi-driver-parameter-reference) for the template version that you want to use.


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


    Example command to create a version 1.4.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name azuredisk-csi-driver --template-version 1.4.0 --param "tenantId=TENANTID"  --param "subscriptionId=SUBSCRIPTIONID"  --param "aadClientId=AADCLIENTID"  --param "location=LOCATION"  --param "aadClientSecret=AADCLIENTSECRET"  --param "resourceGroup=RESOURCEGROUP"  --param "vmType=VMTYPE"  --param "securityGroupName=SECURITYGROUPNAME"  --param "vnetName=VNETNAME" 
    ```
    {: pre}


    Example command to create a version 1.18.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name azuredisk-csi-driver --template-version 1.18.0 --param "tenantId=TENANTID"  --param "subscriptionId=SUBSCRIPTIONID"  --param "aadClientId=AADCLIENTID"  --param "location=LOCATION"  --param "aadClientSecret=AADCLIENTSECRET"  --param "resourceGroup=RESOURCEGROUP"  --param "vmType=VMTYPE"  --param "securityGroupName=SECURITYGROUPNAME"  --param "vnetName=VNETNAME" 
    ```
    {: pre}


    Example command to create a version 1.23.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name azuredisk-csi-driver --template-version 1.23.0 --param "tenantId=TENANTID"  --param "subscriptionId=SUBSCRIPTIONID"  --param "aadClientId=AADCLIENTID"  --param "location=LOCATION"  --param "aadClientSecret=AADCLIENTSECRET"  --param "resourceGroup=RESOURCEGROUP"  --param "vmType=VMTYPE"  --param "securityGroupName=SECURITYGROUPNAME"  --param "vnetName=VNETNAME" 
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
{: #azuredisk-csi-driver-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#azuredisk-csi-driver-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.4.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"azuredisk-csi-driver\", \"storage-template-version\": \"1.4.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"LOCATION\", { \"entry.name\": \"RESOURCEGROUP\", { \"entry.name\": \"VMTYPE\", { \"entry.name\": \"SECURITYGROUPNAME\", { \"entry.name\": \"VNETNAME\",\"user-secret-parameters\": { \"entry.name\": \"TENANTID\",{ \"entry.name\": \"SUBSCRIPTIONID\",{ \"entry.name\": \"AADCLIENTID\",{ \"entry.name\": \"AADCLIENTSECRET\",}
    ```
    {: pre}


    Example request to create a version 1.18.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"azuredisk-csi-driver\", \"storage-template-version\": \"1.18.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"LOCATION\", { \"entry.name\": \"RESOURCEGROUP\", { \"entry.name\": \"VMTYPE\", { \"entry.name\": \"SECURITYGROUPNAME\", { \"entry.name\": \"VNETNAME\",\"user-secret-parameters\": { \"entry.name\": \"TENANTID\",{ \"entry.name\": \"SUBSCRIPTIONID\",{ \"entry.name\": \"AADCLIENTID\",{ \"entry.name\": \"AADCLIENTSECRET\",}
    ```
    {: pre}


    Example request to create a version 1.23.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"azuredisk-csi-driver\", \"storage-template-version\": \"1.23.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"LOCATION\", { \"entry.name\": \"RESOURCEGROUP\", { \"entry.name\": \"VMTYPE\", { \"entry.name\": \"SECURITYGROUPNAME\", { \"entry.name\": \"VNETNAME\",\"user-secret-parameters\": { \"entry.name\": \"TENANTID\",{ \"entry.name\": \"SUBSCRIPTIONID\",{ \"entry.name\": \"AADCLIENTID\",{ \"entry.name\": \"AADCLIENTSECRET\",}
    ```
    {: pre}









{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-autopatch-cli}}
{{site.data.content.configuration-upgrade-manual-cli}}
{{site.data.content.assignment-upgrade-api}}
{{site.data.content.assignment-autopatch-api}}




## Deploying an app that uses your Azure Disk storage
{: #storage-azure-csi-app-deploy}


You can use the Azure Disk driver to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references an Azure Disk storage class that you created earlier.

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: pvc-azuredisk
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 10Gi
      storageClassName: sat-azure-block-silver
    ```
    {: codeblock}

1. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc-azuredisk.yaml
    ```
    {: pre}

1. Verify that the PVC is created and the status is `Bound`.

    ```sh
    oc get pvc
    ```
    {: pre}

1. Create a YAML configuration file for a stateful set that mounts the PVC that you created.

    ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: statefulset-azuredisk
      labels:
        app: nginx
    spec:
      podManagementPolicy: Parallel  # default is OrderedReady
      serviceName: statefulset-azuredisk
      replicas: 1
      template:
        metadata:
          labels:
            app: nginx
        spec:
          nodeSelector:
            "kubernetes.io/os": linux
          containers:
            - name: statefulset-azuredisk
              image: mcr.microsoft.com/oss/nginx/nginx:1.19.5
              command:
                - "/bin/bash"
                - "-c"
                - set -euo pipefail; while true; do echo $(date) >> /mnt/azuredisk/outfile; sleep 1; done
              volumeMounts:
                - name: persistent-storage
                  mountPath: /mnt/azuredisk
      updateStrategy:
        type: RollingUpdate
      selector:
        matchLabels:
          app: nginx
      volumeClaimTemplates:
        - metadata:
            name: persistent-storage
            annotations:
              volume.beta.kubernetes.io/storage-class: sat-azure-block-silver
          spec:
            accessModes: ["ReadWriteOnce"]
            resources:
              requests:
                storage: 10Gi
    ```
    {: codeblock}

1. Create the pod in your cluster.

    ```sh
    oc apply -f statefulset-azuredisk.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}
    
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    statefulset-azuredisk       1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your Azure Disk by logging in to your pod.

    ```sh
    oc exec statefulset-azuredisk -it bash
    ```
    {: pre}

1. View the contents of the `outfile` file to confirm that your app can write data to your persistent storage.

    ```sh
    cat /mnt/azuredisk/outfile
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


## Removing Azure Disk storage from your apps
{: #azure-disk-rm}

If you no longer need your Azure Disk configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters.
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
        app    sat-azure-block-platinum
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

1. Delete the PVC. Because the Azure Disk storage classes have a `Delete` reclaim policy, the PV and the disks in your Azure account are automatically deleted when you delete the PVC.

    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

1. Verify that your PV is automatically removed.

    ```sh
    oc get pv
    ```
    {: pre}




## Removing the Azure Disk storage configuration from your cluster
{: #azure-disk-template-rm}

If you no longer plan on using Azure Disk storage in your cluster, you can use the CLI unassign your cluster from the storage configuration.
{: shortdesc}

Note that if you remove the storage configuration, the driver is then uninstalled from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}


### Removing the Azure Disk storage configuration from the console
{: #azure-disk-template-rm-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Removing the Azure Disk storage configuration from the cli
{: #azure-disk-template-rm-cli}
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
        oc get pods -n kube-system | grep azure
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
{: #azuredisk-csi-driver-parameter-reference}

### 1.4.0 parameter reference
{: #azuredisk-csi-driver-1.4.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Tenant ID | `tenantId` | Secret | Tenant ID : The Azure tenant ID that you want to use for your configuration. You can find your tenant ID in the Azure portal or by running the `az account tenant list` command. | true | N/A |
| Subscription ID | `subscriptionId` | Secret | Your Azure subscription ID. From the Azure portal, search for `Subscription` to find a list of your subscriptions. You can also find your subscription ID by running the `az account subscription list` command. | true | N/A |
| Azure Active Directory Client ID | `aadClientId` | Secret | Your Azure Active Directory Client ID. You can find your Client ID in the Azure portal or by running the `az ad sp list --display-name appDisplayName` command. | true | N/A |
| Location | `location` | Config | The location of your Azure hosts. You can find the location of your virtual machines in the Azure portal or by running the `az vm list` command. Example location: `useast`. | true | N/A |
| Azure Active Directory Client Secret | `aadClientSecret` | Secret | Your Azure Active Directory Client Secret. You can find your client secret in the Azure portal under the `App registrations` menu. | true | N/A |
| Resource Group | `resourceGroup` | Config | The name of your Azure resource group. You can find your resource group detail in the Azure portal or by running the `az group list` command. | true | N/A |
| Virtual Machine Type | `vmType` | Config | You can find your virtual machine type in the Azure portal or by running the `az vm list` command. Example types: `standard` or `VMSS`. | true | N/A |
| Network Security Group Name | `securityGroupName` | Config | The name of your security group. You can find your security group details in the Azure portal or by running the `az network nsg list` command. | true | N/A |
| Virtual Network Name | `vnetName` | Config | The name of the virtual network. You can find the name of your virtual network in the Azure portal or by running the `az network vnet list` command. | true | N/A |
{: caption="Table 1. 1.4.0 parameter reference" caption-side="bottom"}


### 1.18.0 parameter reference
{: #azuredisk-csi-driver-1.18.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Tenant ID | `tenantId` | Secret | Tenant ID : The Azure tenant ID that you want to use for your configuration. You can find your tenant ID in the Azure portal or by running the `az account tenant list` command. | true | N/A |
| Subscription ID | `subscriptionId` | Secret | Your Azure subscription ID. From the Azure portal, search for `Subscription` to find a list of your subscriptions. You can also find your subscription ID by running the `az account subscription list` command. | true | N/A |
| Azure Active Directory Client ID | `aadClientId` | Secret | Your Azure Active Directory Client ID. You can find your Client ID in the Azure portal or by running the `az ad sp list --display-name appDisplayName` command. | true | N/A |
| Location | `location` | Config | The location of your Azure hosts. You can find the location of your virtual machines in the Azure portal or by running the `az vm list` command. Example location: `useast`. | true | N/A |
| Azure Active Directory Client Secret | `aadClientSecret` | Secret | Your Azure Active Directory Client Secret. You can find your client secret in the Azure portal under the `App registrations` menu. | true | N/A |
| Resource Group | `resourceGroup` | Config | The name of your Azure resource group. You can find your resource group detail in the Azure portal or by running the `az group list` command. | true | N/A |
| Virtual Machine Type | `vmType` | Config | You can find your virtual machine type in the Azure portal or by running the `az vm list` command. Example types: `standard` or `VMSS`. | true | N/A |
| Network Security Group Name | `securityGroupName` | Config | The name of your security group. You can find your security group details in the Azure portal or by running the `az network nsg list` command. | true | N/A |
| Virtual Network Name | `vnetName` | Config | The name of the virtual network. You can also find the name of your virtual network in the Azure portal or by running the `az network vnet list` command. | true | N/A |
{: caption="Table 2. 1.18.0 parameter reference" caption-side="bottom"}


### 1.23.0 parameter reference
{: #azuredisk-csi-driver-1.23.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Tenant ID | `tenantId` | Secret | Tenant ID : The Azure tenant ID that you want to use for your configuration. You can find your tenant ID in the Azure portal or by running the `az account tenant list` command. | true | N/A |
| Subscription ID | `subscriptionId` | Secret | Your Azure subscription ID. From the Azure portal, search for `Subscription` to find a list of your subscriptions. You can also find your subscription ID by running the `az account subscription list` command. | true | N/A |
| Azure Active Directory Client ID | `aadClientId` | Secret | Your Azure Active Directory Client ID. You can find your Client ID in the Azure portal or by running the `az ad sp list --display-name appDisplayName` command. | true | N/A |
| Location | `location` | Config | The location of your Azure hosts. You can find the location of your virtual machines in the Azure portal or by running the `az vm list` command. Example location: `useast`. | true | N/A |
| Azure Active Directory Client Secret | `aadClientSecret` | Secret | Your Azure Active Directory Client Secret. You can find your client secret in the Azure portal under the `App registrations` menu. | true | N/A |
| Resource Group | `resourceGroup` | Config | The name of your Azure resource group. You can find your resource group detail in the Azure portal or by running the `az group list` command. | true | N/A |
| Virtual Machine Type | `vmType` | Config | You can find your virtual machine type in the Azure portal or by running the `az vm list` command. Example types: `standard` or `VMSS`. | true | N/A |
| Network Security Group Name | `securityGroupName` | Config | The name of your security group. You can find your security group details in the Azure portal or by running the `az network nsg list` command. | true | N/A |
| Virtual Network Name | `vnetName` | Config | The name of the virtual network. You can find the name of your virtual network in the Azure portal or by running the `az network vnet list` command. | true | N/A |
{: caption="Table 3. 1.23.0 parameter reference" caption-side="bottom"}




## Storage class reference for Azure Disk
{: #azure-disk-sc-ref}

| Storage class name | IOPS range per disk | Size range | Disk type | Reclaim policy | Volume Binding Mode |
| --- | --- | --- | --- | --- | --- |
| `sat-azure-block-platinum` |  1200 - 160000 | 4 GiB - 64 TiB | SSD | Delete | Immediate |
| `sat-azure-block-platinum-metro`  | 1200 - 160000 | 4 GiB - 64 TiB | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-gold` | 120 - 20000 | 32 GiB - 32 TiB | SSD | Delete | Immediate |
| `sat-azure-block-gold-metro` **Default** | 120 - 20000 | 32 GiB - 32 TiB | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-silver`  | 120 - 6000 | N/A | SSD | Delete | Immediate |
| `sat-azure-block-silver-metro` | 120 - 6000 | N/A | SSD | Delete | WaitForFirstConsumer |
| `sat-azure-block-bronze`  | 500 - 2000 | 32 GiB - 32 TiB | HDD | Delete | Immediate |
| `sat-azure-block-bronze-metro` | 500 - 2000 | 32 GiB - 32 TiB | HDD | Delete | WaitForFirstConsumer |
{: caption="Table 2. Storage class reference for Azure Disk storage" caption-side="bottom"}


## Getting help and support for Azure Disk storage
{: #sat-azure-disk-support}

When you use Azure Disk Storage, try the following resources before you open a support case. 
{: shortdesc}

1. Review the FAQs in the [Azure Knowledge Center](https://azure.microsoft.com/en-us/support/legal/faq){: external}.
1. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-storage-must-gather) to troubleshoot and resolve common issues.
1. Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
1. Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. Tag any questions with `ibm-cloud` and `Azure-Disk`.
1. Search the [Azure Service Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview){: external} for more information. 




