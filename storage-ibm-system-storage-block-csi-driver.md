---

copyright:
  years: 2020, 2023
lastupdated: "2023-01-03"

keywords: satellite storage, satellite config, satellite configurations, 

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.IBM_notm}} Systems Block Storage CSI driver
{: #storage-ibm-system-storage-block-csi-driver}

The {{site.data.keyword.blockstorageshort}} CSI driver for {{site.data.keyword.satellitelong}} is based on an {{site.data.keyword.IBM_notm}} open-source project, and integrated into the {{site.data.keyword.IBM_notm}} Storage orchestration for containers. {{site.data.keyword.IBM_notm}} Storage orchestration for containers enables enterprises to implement a modern container-driven hybrid multicloud environment that can reduce IT costs and enhance business agility, while continuing to derive value from existing systems.

For full release notes, compatibility, installation, and user information, see the [{{site.data.keyword.blockstorageshort}} CSI driver documentation](https://www.ibm.com/docs/en/stg-block-csi-driver){: external}.

Supported {{site.data.keyword.IBM_notm}} storage systems for Satellite include,

- {{site.data.keyword.IBM_notm}} Spectrum Virtualize Family including {{site.data.keyword.IBM_notm}} SAN Volume Controller (SVC) and {{site.data.keyword.IBM_notm}} FlashSystem® family members built with {{site.data.keyword.IBM_notm}} Spectrum® Virtualize (FlashSystem 5010, 5030, 5100, 5200, 7200, 9100, 9200, 9200R)
- {{site.data.keyword.IBM_notm}} FlashSystem A9000 and A9000R
- {{site.data.keyword.IBM_notm}} DS8000 Family

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}
  
## Prerequisites for using {{site.data.keyword.blockstorageshort}}
{: #sat-storage-block-csi-prereq}

Be sure to complete all prerequisite and installation steps before assigning hosts to your location. Do not create a Kubernetes cluster.
{: important}

1. Review the [compatibility and requirements documentation](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=installation-compatibility-requirements){: external}.

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).




Before you begin, review the [parameter reference](#sat-storage-ibm-block-csi-params-cli) for the template version that you want to use.
{: important}

## Creating and assigning a configuration in the console
{: #ibm-system-storage-block-csi-driver-config-create-console}
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

## Creating a configuration in the CLI
{: #ibm-system-storage-block-csi-driver-config-create-cli}
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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 1.4.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.4.0 [--param "namespace=NAMESPACE"] 
    ```
    {: pre}

    Example command to create a version 1.5.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.5.0 [--param "namespace=NAMESPACE"] 
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
{: #ibm-system-storage-block-csi-driver-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.4.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.4.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\",\"user-secret-parameters\": { \"entry.name\": \"NAMESPACE\",
    ```
    {: pre}

    Example request to create a version 1.5.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.5.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\",\"user-secret-parameters\": { \"entry.name\": \"NAMESPACE\",
    ```
    {: pre}










{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}


## Deploying an app that uses {{site.data.keyword.blockstorageshort}}
{: #storage-block-csi-app-deploy}

You can use the `ibm-system-storage-block-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a Kubernetes secret configuration file that contains your {{site.data.keyword.blockstorageshort}} credentials. For more information, see [Creating a Kubernetes secret](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-secret){: external}.
    ```yaml
    kind: Secret
    apiVersion: v1
    metadata:
      name:  demo-secret
      namespace: default
    type: Opaque
    stringData:
      management_address: demo-management-address # Example: baremetal-cluster.xiv.ibm.com
      username: demo-username                     
    data:
      password: AAAA1AAA 
    ```
    {: codeblock}

1. Create the secret in your cluster.
    ```sh
    oc apply -f <secret.yaml>
    ```
    {: pre}

1. [Create a storage class that uses the {{site.data.keyword.blockstorageshort}} driver](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-storageclass){: external}.
    ```yaml
    kind: StorageClass
    apiVersion: storage.k8s.io/v1
    metadata:
      name: demo-storageclass
    provisioner: block.csi.ibm.com
    parameters:
      SpaceEfficiency: deduplicated   # Optional.
      pool: demo-pool
      csi.storage.k8s.io/provisioner-secret-name: demo-secret
      csi.storage.k8s.io/provisioner-secret-namespace: default
      csi.storage.k8s.io/controller-publish-secret-name: demo-secret
      csi.storage.k8s.io/controller-publish-secret-namespace: default
      csi.storage.k8s.io/controller-expand-secret-name: demo-secret
      csi.storage.k8s.io/controller-expand-secret-namespace: default
      csi.storage.k8s.io/fstype: xfs   # Optional. Values ext4\xfs. The default is ext4.
      volume_name_prefix: demoPVC      # Optional
    allowVolumeExpansion: true
    ```
    {: codeblock}

1. Create the storage class in your cluster.
    ```sh
    oc apply -f sc.yaml
    ```
    {: pre}

1. Verify that the storage class is created.
    ```sh
    oc get sc
    ```
    {: pre}

1. [Create a PVC](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-persistentvolumeclaim-pvc){: external} that references the storage class that you created earlier.
    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      name: demo-pvc-file
    spec:
      volumeMode: Filesystem
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi
      storageClassName: demo-storageclass
    ```
    {: codeblock}

1. Create the PVC in your cluster.
    ```sh
    oc apply -f pvc.yaml
    ```
    {: pre}

1. Verify that the PVC is created and the status is `Bound`.
    ```sh
    oc get pvc
    ```
    {: pre}

1. Create a YAML configuration file for a stateful set that mounts the PVC that you created.
    ```yaml
    kind: StatefulSet
    apiVersion: apps/v1
    metadata:
      name: demo-statefulset-file-system
    spec:
      selector:
        matchLabels:
          app: demo-statefulset
      serviceName: demo-statefulset
      replicas: 1
      template:
        metadata:
          labels:
            app: demo-statefulset
        spec:
          containers:
          - name: demo-container
            image: registry.access.redhat.com/ubi8/ubi:latest
            command: [ "/bin/sh", "-c", "--" ]
            args: [ "while true; do sleep 30; done;" ]
            volumeMounts:
              - name: demo-volume-file-system
                mountPath: "/data"
          volumes:
          - name: demo-volume-file-system
            persistentVolumeClaim:
              claimName: demo-pvc-file
    ```
    {: codeblock}

1. Create the pod in your cluster.
    ```sh
    oc apply -f <stateful-set>.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.
    ```sh
    oc get pods
    ```
    {: pre}

    Example output
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    demo-statefulset-file-system-0       1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your {{site.data.keyword.IBM_notm}} Block Storage instance.
    1. Log in to your pod.
        ```sh
        oc exec demo-statefulset-file-system-0 -it bash
        ```
        {: pre}

    1. Change to the `/data` directory, write a `test.txt` file, and view the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
        ```sh
        cd data && echo "Testing" >> test.txt && cat test.txt
        ```
        {: pre}

        Example output
        ```sh
        Testing
        ```
        {: screen}

    1. Exit the pod.
        ```sh
        exit
        ```
        {: pre}




## Parameter reference
{: #sat-storage-ibm-block-csi-params-cli}


| Parameter | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `namespace` | Optional | The namespace where you want to create the deployment. | `default` |
| `sc-name` | Required | The name of the storage class name that is created. | N/A |
| `space-efficiency` | Optional | The space efficiency of the volume that is created. | N/A |
| `pool` | Required | The name of an existing pool on the storage system where you want to create the volume. | N/A |
| `secret-name` | Required | The name of your existing Kubernetes secret. | N/A |
| `secret-namespace` | Required | The namespace that your Kubernetes secret is in. Your secret can be in a separate namespace from your deployment. | N/A |
| `fstype` | Optional | The file system type. | `ext4` |
| `prefix` | Optional | The prefix name of the volume that is created. | N/A |
| `VolumeExpansion` | Optional | Specify `true` to allow volume expansion or `false` to disallow volume expansion. | `false` |
{: caption="Table 1. {{site.data.keyword.IBM_notm}} {{site.data.keyword.blockstorageshort}} parameter reference." caption-side="bottom"}





