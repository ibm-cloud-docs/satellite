---
copyright:
  years: 2020, 2022
lastupdated: "2022-07-22"

keywords: satellite storage, VMware, satellite config, satellite configurations, vsphere

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# VMware Block Container Storage Interface (CSI) Driver
{: #config-storage-vmware-csi}

The VMware Container Storage Interface (CSI) [Driver](https://github.com/kubernetes-sigs/ibm-vmware-csi-driver){: external} allows you to manage the lifecycle of your VMware Block Data volumes.

The template is currently in beta. Do not use it for production workloads. 
{: beta}
 
Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}


## Creating the VMware configuration from the console
{: #sat-storage-vmware-create-config-ui}
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

## Creating the VMware configuration in the command line
{: #sat-storage-vmware-create-config-cli}
{: cli}

Create a storage configuration in the command line by using the VMware configuration template.
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
    

1. Review the [template parameters](#sat-storage-vmware-csi-params-cli).

1. Create storage configuration. You can pass parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Note that Kubernetes resources can't contain capital letters or special characters. Enter a name for your config that uses only lowercase letters, numbers, hyphens or periods.

    ```sh
    ibmcloud sat storage config create --name vmware --template-name vsphere-csi-driver --template-version 2.5.0 -p "vcenter-username=<username>" -p 'vcenter-password=<password>' -p "insecure-flag=true/false" -p "host=<host address>" -p "datacenters=IBMCloud" --location <location>
    ```
    {: pre}

1. Verify that your storage configuration is created.

    ```sh
    ibmcloud sat storage config get --config <CONFIG>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-vmware-csi)

### Assigning a VMWare storage configuration from the console
{: #assign-storage-vmware-csi-ui}
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


### Assigning a VMWare storage configuration in the command line
{: #assign-storage-vmware-csi-cli}
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
    kubectl get all -n vmware-system-csi
    NAME                                         READY   STATUS    RESTARTS   AGE
    pod/vsphere-csi-controller-b4665c8dc-29kqd   7/7     Running   0          5m47s
    pod/vsphere-csi-controller-b4665c8dc-57cm8   7/7     Running   0          5m47s
    pod/vsphere-csi-controller-b4665c8dc-lngc4   7/7     Running   0          5m47s
    pod/vsphere-csi-node-dgqnq                   3/3     Running   0          40s
    pod/vsphere-csi-node-kgbll                   3/3     Running   0          40s
    pod/vsphere-csi-node-l7cxs                   3/3     Running   0          40s
    ```
    {: screen}

1. List the storage classes.

    ```sh
    $ kubectl get sc
    NAME                                     PROVISIONER              RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
    sat-vsphere-vsan-block                   csi.vsphere.vmware.com   Delete          Immediate              false                  2m59s
    sat-vsphere-vsan-block-metro (default)   csi.vsphere.vmware.com   Delete          WaitForFirstConsumer   false                  2m59s
    ```
    {: screen}

1. [Deploy an app that uses your VMware](#sat-storage-vmware-deploy-app)

## Deploying an app that uses VMware
{: #sat-storage-vmware-deploy-app}
{: cli}

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
        - name: web-server
            image: nginx
            command:
                - "/bin/sh"
                - "-c"
                - while true; do echo $(date) >> /mnt/vmwaredisk/outfile; sleep 1; done
            volumeMounts:
            - mountPath: /mnt/vmwaredisk
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

Removing the storage configuration uninstalls the driver from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
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


## Parameter reference for VMWare
{: #sat-storage-vmware-csi-params-cli}

| Parameter | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `cluster-id` | Required | The unique cluster identifier. This is the vcenter cluster ID. | N/A |
| `vcenter-username` | Required | The vCenter Server username. You must specify the username along with the domain name. For example, user = `Administrator@vsphere.local`.| N/A |
| `vcenter-password` | Required | Password for a vCenter Server user. | N/A |
| `insecure-flag` | Required | Set this option to `true` if you want to use self-signed certificate for login or to `false` if you want to use secure connection. | N/A |
| `vCenter host` | Required | The vCenter server IP address. | N/A |
| `vCenter datacenters` | Required | List all datacenter paths where host VMs are present, separated by commas. Provide the name of the datacenter when it is located at the root. When it is placed in the folder, you need to specify the path as folder/datacenter-name. | IBMCloud | 
| `Ca file path` | Optional | The path to a CA certificate in xxxx.pem format. | N/A | 
| `thumbprint` | Optional | The certificate thumbprint. This parameter is ignored when using an unsecured setup or when you provide `ca-file` | N/A | 
| `vcenter port` | Optional | vCenter server port. | 443 |
{: caption="Table 1. Parameter reference for VMware storage" caption-side="top"}




## Getting help and support for VMWare
{: #sat-vmware-csi-support}

If you run into an issue with using VMware you can submit a support request with [VMware Support](https://customerconnect.vmware.com/home){: external}.




