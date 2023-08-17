---
copyright:
  years: 2020, 2023
lastupdated: "2023-08-17"

keywords: satellite storage, VMware, satellite config, satellite configurations, vsphere

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# VMware Block Container Storage Interface (CSI) Driver
{: #storage-vsphere-csi-driver}

The VMware Container Storage Interface (CSI) [Driver](https://github.com/kubernetes-sigs/vsphere-csi-driver/){: external} allows you to manage the lifecycle of your VMware Block Data volumes.

## Prerequisites
{: #prereq-vmware-csi}

Before you can create a VMware block storage configuration, you must satisfy the following prerequisites. 

1. Verify that you are running vSphere version 6.7U3 or later.
1. Verify that your virtual machine hardware version is version 15 or later.
1. Verify that master nodes can communicate with the vCenter management interface.
1. Disable Swap on all nodes.
1. Enable Disk UUID on all node virtual machines. 
1. Deploy vSAN in your vSphere environment. For more information, see the [vSAN documentation](https://docs.vmware.com/en/VMware-vSAN/index.html){: external}.

 
Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}







## Creating and assigning a configuration in the console
{: #vsphere-csi-driver-config-create-console}
{: ui}


1. Review the [parameter reference](#vsphere-csi-driver-parameter-reference).


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
{: #vsphere-csi-driver-config-create-cli}
{: cli}


1. Review the [parameter reference](#vsphere-csi-driver-parameter-reference) for the template version that you want to use.


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


    Example command to create a version 2.5.1 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name vsphere-csi-driver --template-version 2.5.1 --param "vcenter-username=VCENTER-USERNAME"  --param "vcenter-password=VCENTER-PASSWORD"  --param "insecure-flag=INSECURE-FLAG"  --param "host=HOST"  --param "datacenters=DATACENTERS"  [--param "thumbprint=THUMBPRINT"] 
    ```
    {: pre}


    Example command to create a version 2.7.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name vsphere-csi-driver --template-version 2.7.0 --param "vcenter-username=VCENTER-USERNAME"  --param "vcenter-password=VCENTER-PASSWORD"  --param "insecure-flag=INSECURE-FLAG"  --param "host=HOST"  --param "datacenters=DATACENTERS"  [--param "thumbprint=THUMBPRINT"] 
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
{: #vsphere-csi-driver-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#vsphere-csi-driver-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 2.5.1 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"vsphere-csi-driver\", \"storage-template-version\": \"2.5.1\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"INSECURE-FLAG\", { \"entry.name\": \"HOST\", { \"entry.name\": \"DATACENTERS\",\"user-secret-parameters\": { \"entry.name\": \"VCENTER-USERNAME\",{ \"entry.name\": \"VCENTER-PASSWORD\",{ \"entry.name\": \"THUMBPRINT\",}
    ```
    {: pre}


    Example request to create a version 2.7.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"vsphere-csi-driver\", \"storage-template-version\": \"2.7.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"INSECURE-FLAG\", { \"entry.name\": \"HOST\", { \"entry.name\": \"DATACENTERS\",\"user-secret-parameters\": { \"entry.name\": \"VCENTER-USERNAME\",{ \"entry.name\": \"VCENTER-PASSWORD\",{ \"entry.name\": \"THUMBPRINT\",}
    ```
    {: pre}








{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}


## Deploying an app that uses VMware
{: #sat-storage-vmware-deploy-app}

You can use the `vmware-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references the storage class that you created earlier.

    ```yaml
        kind: PersistentVolumeClaim
        apiVersion: v1
        metadata:
            name: podpvc
        spec:
            accessModes:
             - ReadWriteOnce
            storageClassName: sat-vsphere-vsan-block-metro
            resources:
                requests:
                storage: 10Gi
    ```
    {: codeblock}
        
1. Create the PVC in your cluster. 

    ```sh
    oc apply -f podpvc.yaml
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you created. 

    ```yaml
        apiVersion: v1
        kind: Pod
        metadata:
        name: web-server
        spec:
        containers:
        - name: #web-server
            image: nginx
            command:
                - "/bin/sh"
                - "-c"
                - while true; do echo $(date) >> /mnt/vmwaredisk/outfile; sleep 1; done
            volumeMounts:
             mountPath:  /mnt/vmwaredisk
                name: mypvc
        volumes:
        - name: mypvc
            persistentVolumeClaim:
            claimName: podpvc
            readOnly: false

    ```
    {: codeblock}

1. Create the pod in your cluster.

    ```sh
    oc apply -f mypvc.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}
    
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    my-pvc                              1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your block storage volume by logging in to your pod.

    ```sh
    oc exec -it web-server /bin/bash
    ```
    {: pre}

1. View the contents of the `outfile` file to confirm that your app can write data to your persistent storage.

    ```sh
    cat outfile
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

{{site.data.content.configuration-upgrade-cli}}

## Removing VMWare storage from your apps
{: #vmware-csi-rm-apps}

If you no longer need your VMware configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters.
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
        NAME         READY   STATUS    RESTARTS   AGE
        web-server   1/1     Running   0          55s
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

## Removing the VMware storage configuration from your cluster
{: #vmware-csi-template-rm}

If you no longer plan on using VMware in your cluster, you can use the CLI unassign your cluster from the storage configuration.
{: shortdesc}

Removing the storage configuration removes the driver from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}

### Removing the VMWare storage configuration using the console
{: #vmware-csi-rm-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Removing the VMWare storage configuration using the command line
{: #vmware-csi-rm-cli}
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
        oc get pods -n kube-system | grep vsphere
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
{: #vsphere-csi-driver-parameter-reference}

### 2.5.1 parameter reference
{: #vsphere-csi-driver-2.5.1-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| vCenter username | `vcenter-username` | Secret | The vCenter username. You must specify the username along with the domain name. For example: `Administrator@vsphere.local`. | true | N/A |
| vCenter password | `vcenter-password` | Secret | The vCenter server user password. | true | N/A |
| Insecure connection | `insecure-flag` | Config | Include the `insecure-flag`. `true` indicates that you want to include the flag, which uses self-signed certificate for login. `false` indicates that you use a secure connection. If you select `false`, you must provide an SSL thumbprint. | true | `false` |
| vCenter host | `host` | Config | The vCenter server IP address. | true | N/A |
| vCenter data centers | `datacenters` | Config | List all data center paths where host VMs are present, separated by commas. Provide the name of the data center when it is located at the root. When it is placed in the folder, you need to specify the path as folder/data-center-name | true | N/A |
| SSL certificate thumbprint | `thumbprint` | Secret | The SSL thumbprint to be used to establish a secure connection to VC.  | false | N/A |
{: caption="Table 1. 2.5.1 parameter reference" caption-side="bottom"}


### 2.7.0 parameter reference
{: #vsphere-csi-driver-2.7.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| vCenter username | `vcenter-username` | Secret | The vCenter username. You must specify the username along with the domain name. For example: `Administrator@vsphere.local`. | true | N/A |
| vCenter password | `vcenter-password` | Secret | The vCenter server user password. | true | N/A |
| Insecure connection | `insecure-flag` | Config | Include the `insecure-flag`. `true` indicates that you want to include the flag, which uses self-signed certificate for login. `false` indicates that you use a secure connection. If you select `false`, you must provide an SSL thumbprint. | true | `false` |
| vCenter host | `host` | Config | The vCenter server IP address. | true | N/A |
| vCenter data centers | `datacenters` | Config | List all data center paths where host VMs are present, separated by commas. Provide the name of the data center when it is located at the root. When it is placed in the folder, you need to specify the path as folder/data-center-name | true | N/A |
| SSL certificate thumbprint | `thumbprint` | Secret | The SSL thumbprint to be used to establish a secure connection to VC.  | false | N/A |
{: caption="Table 2. 2.7.0 parameter reference" caption-side="bottom"}





## Getting help and support for VMWare
{: #sat-vmware-csi-support}

If you run into an issue with VMware submit a support request with [VMware Support](https://www.vmware.com/support/contacts.html){: external}.



