---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-10"

keywords: azure, azure storage, satellite storage, satellite, config, configurations, file

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Azure File CSI Driver [BETA]
{: #config-storage-azurefile-csi}

The Azure File CSI driver implements the CSI specification for container orchestrators to manage the lifecycle of Azure File volumes.
{: shortdesc}

For an overview of the available features of the Azure File CSI driver, see [Features](https://github.com/kubernetes-sigs/azurefile-csi-driver#features){: external}. 

The Azure File CSI driver template for {{site.data.keyword.satelliteshort}} is currently available for cluster versions X.X and later. The template is currently in beta and should not be used for production workloads. 
{: beta}


Supported features:
- Topology(Availability Zone)
- ZRS disk support(Preview)
- Volume Cloning
- Volume Expansion
- Raw Block Volume
- Shared Disk
- Volume Limits
- fsGroupPolicy

## Prerequisites
{: #sat-storage-azure-file-csi-prereq}

Set up [Azure File storage](https://docs.microsoft.com/en-us/azure/aks/azure-files-csi){: external} for {{site.data.keyword.satelliteshort}} clusters by creating a storage configuration in your location. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

To use the Azure File CSI driver storage template, complete the following tasks: 

1. Create an Azure location by using the [location template](/docs/satellite?topic=satellite-azure) or manually [adding Azure hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-azure#azure-host-attach). 
    If you choose to manually assign hosts, you must [label your worker nodes](#azure-disk-label-nodes) before creating your storage configuration.
    {: important}
    
2. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters) that runs on compute hosts in Azure.
3. [Create your configuration file](#azure-file-config-file).

### Optional: Labeling your worker nodes
{: azure-file-label-nodes}

Complete the following steps to add the required labels to your worker nodes for the Azure File CSI driver template.
{: shortdesc}

If you manually assigned your Azure hosts to your location and did not use an automated deployment such as deploying from the console or a Terraform template, then you must [label your worker nodes](#azure-file-label-nodes) before creating your storage configuration. 
{: important}

1. List your Azure worker nodes and make a note of the `name` of each node. 

    ```sh
    oc get nodes
    ```
    {: pre}

1. Get the details of each node and make a note of the `zone` that the node is in. For example: `eastus-1`. 

    ```sh
    oc get nodes NODE-NAME -o yaml | grep zone
    ```
    {: pre}

1. Label your Azure worker nodes with the `zone` value that you retrieved earlier. Replace `<node_name>` and `<zone>` with the node name and zone of your worker nodes. For example, if you have a worker node in zone: `eastus-1`, use the following commands to add `eastus-1` as a label to the worker node in the `eastus-1` zone.
    ```sh
    oc label node <node_name> topology.kubernetes.io/zone-
    oc label node <node_name> topology.kubernetes.io/zone=<zone> --overwrite
    ```
    {: pre}

1. Repeat the previous steps for each worker node.


### Gathering your Azure File configuration paramerters
{: azure-file-config-file}

Create a configuration file with your Azure File settings. 
{: shortdesc}

1. List the Azure File storage template parameters.
    ```sh
    ibmcloud sat storage template get --name azurefile-csi-driver --version <version>
    ```
    {: pre}
    
    Example output
    ```sh
    Name                Display Name        Description                                                                       Required   Type   Default   
    aadClientId         aadClientId         aadClientId                                                                       true       text   -   
    aadClientSecret     aadClientSecret     aadClientSecret                                                                   true       text   -   
    location            location            location                                                                          true       text   -   
    resourceGroup       resourceGroup       resourceGroup                                                                     true       text   -   
    securityGroupName   securityGroupName   securityGroupName                                                                 true       text   -   
    subscriptionId      subscriptionId      subscriptionId                                                                    true       text   -   
    tenantId            tenantId            tenantId : The base64 encoded string generated from the azure.json config file.   true       text   -   
    vmType              vmType              vmType                                                                            true       text   -   
    vnetName            vnetName            vnetName                                                                          true       text   -   
    ```
    {: screen}

1. [Sign in to your Azure account](https://azure.microsoft.com/en-us/account/){: external} and retrieve the required parameters. For more information about the parameters, see the [parameter reference](#sat-storage-azure-file-params-cli).


## Creating an Azure File configuration in the command line
{: #sat-storage-azure-file-csi-cli}

Create a storage configuration in the command line by using the Azure File template.
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
    
1. Review the [template parameters](#sat-storage-azure-file-params-cli).
1. Create a storage configuration. You can pass parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).

    ```sh
    ibmcloud sat storage config create --name <config-name> --template-name azurefile-csi-driver --template-version 1.9.0 --location <location> -p "tenantId=<tenantId>" -p "subscriptionId=<subscription_ID>" -p "aadClientId=<Azure_AD_ClientId>" -p "aadClientSecret=<Azure_AD_Client_Secret>" -p "resourceGroup=<resource_group>" -p "location=<location>" -p "vmType=<vm_type>" -p "securityGroupName=<security_group_name>" -p "vnetName=<vnet_name>"

    ```
    {: pre}

1. Verify that your storage configuration is created.

    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. **Optional** [Create a custom storage class](#azurefile-add-sc).
1. [Assign your storage configuration to your cluster or clusters](#assign-storage-azure).



## Assigning your Azure storage configuration to a cluster
{: #assign-storage-azurefile}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-azurefile-csi), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}

### Assigning a storage configuration in the console
{: #assign-storage-azurefile-csi-ui}

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


### Assigning a storage configuration in the command line 
{: assign-storage-azurefile-csi-cli}

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

    - [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) cluster
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

    - [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) cluster
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>) | grep <storage-assignment-name>
    ```
    {: pre}

1. Verify that the storage configuration resources are deployed.

    ```sh
    kubectl get pods -n kube-system | grep azure
    ```
    {: pre}

    Example output
    ```sh
    csi-azurefile-controller-849d854b96-6jbjg   5/5     Running   0          167m
    csi-azurefile-controller-849d854b96-lkplx   5/5     Running   0          167m
    csi-azurefile-node-7qwlj                    3/3     Running   6          167m
    csi-azurefile-node-8xm4c                    3/3     Running   6          167m
    csi-azurefile-node-snsdb                    3/3     Running   6          167m
    ```
    {: screen}

1. List the Azure File storage classes.

    ```sh
    oc get sc | grep azure
    ```
    {: pre}

    Example output

    ```sh
    sat-azure-file-bronze           disk.csi.azure.com   Delete          Immediate              true                   167m
    sat-azure-file-bronze-metro     disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   167m
    sat-azure-file-gold             disk.csi.azure.com   Delete          Immediate              true                   167m
    sat-azure-file-gold-metro       disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   167m
    sat-azure-file-platinum         disk.csi.azure.com   Delete          Immediate              true                   167m
    sat-azure-file-platinum-metro   disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   167m
    sat-azure-file-silver           disk.csi.azure.com   Delete          Immediate              true                   167m
    sat-azure-file-silver-metro     disk.csi.azure.com   Delete          WaitForFirstConsumer   true                   167m
    ```
    {: screen}

1. [Deploy an app that uses your Azure File storage](#storage-azure-file-csi-app-deploy).

## Deploying an app that uses your Azure File storage
{: storage-azure-file-csi-app-deploy}

You can use the Azure File driver to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references an Azure File storage class that you created earlier.

    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
        name: pvc-azurefile4
    spec:
    accessModes:
         - ReadWriteMany
    resources:
        requests:
            storage: 100Gi
    storageClassName: sat-azure-file-bronze
    ```
    {: codeblock}

1. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc-azurefile.yml
    ```
    {: pre}

1. Verify that the PVC is created and the status is `Bound`.

    ```sh
    oc get pvc
    ```
    {: pre}

1. Create a YAML configuration file for a stateful set that mounts the PVC that you created. This example deployment creates one app pod that writes the date.

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
        labels:
            app: nginx
    name: DEPLOYMENT-NAME
    spec:
        replicas: 1
        selector:
            matchLabels:
                app: nginx
        template:
            metadata:
                labels:
                    app: nginx
                name: DEPLOYMENT-NAME
            spec:
                nodeSelector:
                    "kubernetes.io/os": linux
                containers:
                - name: deployment-azurefile
                    image: mcr.microsoft.com/oss/nginx/nginx:1.19.5
                    command:
                        - "/bin/bash"
                        - "-c"
                        - set -euo pipefail; while true; do echo $(date) >> /mnt/azurefile/outfile; sleep 1; done
                    volumeMounts:
                        - name: azurefile
                            mountPath: "/mnt/azurefile"
                            readOnly: false
                volumes:
                    - name: azurefile
                    persistentVolumeClaim:
                    claimName: PVC-NAME #The name of the PVC you created earlier.
        strategy:
            rollingUpdate:
            maxSurge: 0
            maxUnavailable: 1
        type: RollingUpdate
    ```
    {: codeblock}

1. Create the pod in your cluster. 

    ```sh
    oc apply -f statefulset-azurefile.yml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}
    
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    statefulset-azurefile                1/1     Running     0       2m58s
    ```
    {: screen}

1. Verify that the app can write to your Azure Disk by logging in to your pod.

    ```sh
    oc exec statefulset-azurefile -it bash
    ```
    {: pre}

1. View the contents of the `outfile` file to confirm that your app can write data to your presistent storage.

    ```sh
    cat /mnt/azurefile/outfile
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

## Removing Azure file storage from your apps
{: #azure-file-rm}

If you no longer need your Azure File configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters.
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
        app    sat-azure-file-platinum
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

1. Delete the PVC. Because the Azure File storage classes have a `Delete` reclaim policy, the PV and the disks in your Azure account are automatically deleted when you delete the PVC.

    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

1. Verify that your PV is automatically removed.

    ```sh
    oc get pv
    ```
    {: pre}

## Removing the Azure File storage configuration from your cluster
{: #azure-file-template-rm}

If you no longer plan on using Azure File storage in your cluster, you can use the CLI unassign your cluster from the storage configuration.
{: shortdesc}

Removing the storage configuration uninstalls the driver from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}

1. List your storage assignments and find the one that you used for your cluster.

    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>)
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
        oc get pods -n kube-system | grep azure
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
{: #sat-storage-azure-file-params-cli}

For help finding these paramters, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest){: external}
{: tip}

| Parameter | Required? | Description | 
| --- | --- | --- |
| `tenantId` | Required | The Azure tenant ID that you want to use for your configuration. Follow the Azure documentation to find your [tenant ID](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-to-find-tenant){: external} or run the  |
| `subscriptionId` | Required | Your Azure subscription ID. From the Azure portal, search for `Subscription` to find a list of your subscriptions. You can also find this value by running the `az account subscription list` command. |
| `aadClientId` | Required | Your Azure Active Directory Client ID. You can find this value by running the `az identity list` command. |
| `aadClientSecret` | Required | Your Azure Active Directory Client Secret. |
| `resourceGroup` | Required | The name of your Azure resource group. You can find this value by running the `az group list` command. |
| `location` | Required | The location of your Azure hosts. For example `useast` |
| `vmType` | Required | The virtual machine type. You can find this value by running the `az vm list` command. For example: `standard` or `VMSS`. |
| `securityGroupName` | Required | The security group name. You can find this parameter by running the `az network nsg list` command. |
| `vnetName` | Required | The name of the virtual network. You can find this value by running the `az network vnet subnet list` command. |
{: caption="Table 1. Parameter reference for Azure Disk storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates required parameters. The third column is a brief description of the parameter."}


## Storage class reference
{: #azure-file-sc-ref}

| Storage class name | Reclaim policy | Volume Binding Mode |
| --- | --- | --- |
| `sat-azure-file-platinum`  | Delete | Immediate |
| `sat-azure-file-platinum-metro` | Delete | WaitForFirstConsumer |
| `sat-azure-file-gold` | Delete | Immediate |
| `sat-azure-file-gold-metro` | Delete | WaitForFirstConsumer |
| `sat-azure-file-silver` | Delete | Immediate |
| `sat-azure-file-silver-metro` | Delete | WaitForFirstConsumer |
| `sat-azure-file-bronze` | Delete | Immediate |
| `sat-azure-file-bronze-metro` | Delete | WaitForFirstConsumer |
{: caption="Table 2. Storage class reference for Azure File storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the reclaim policy. The third column is the volume binding mode."}


