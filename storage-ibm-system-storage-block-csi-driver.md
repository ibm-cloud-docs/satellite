---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-13"

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

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}
  
## Prerequisites for using {{site.data.keyword.blockstorageshort}}
{: #sat-storage-block-csi-prereq}

Be sure to complete all prerequisite and installation steps before assigning hosts to your location. Do not create a Kubernetes cluster.
{: important}

1. Review the [compatibility and requirements documentation](https://www.ibm.com/docs/en/stg-block-csi-driver){: external}.

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).






## Creating and assigning a configuration in the console
{: #ibm-system-storage-block-csi-driver-config-create-console}
{: ui}


1. Review the [parameter reference](#ibm-system-storage-block-csi-driver-parameter-reference).


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


1. Review the [parameter reference](#ibm-system-storage-block-csi-driver-parameter-reference) for the template version that you want to use.


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
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.4.0 --param "namespace=NAMESPACE" 
    ```
    {: pre}


    Example command to create a version 1.5.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.5.0 --param "namespace=NAMESPACE" 
    ```
    {: pre}


    Example command to create a version 1.10.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.10.0 --param "namespace=NAMESPACE" 
    ```
    {: pre}


    Example command to create a version 1.11.1 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.11.1 --param "namespace=NAMESPACE" 
    ```
    {: pre}


    Example command to create a version 1.11.2 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-system-storage-block-csi-driver --template-version 1.11.2 --param "namespace=NAMESPACE"  --param "secret-name=SECRET-NAME"  --param "secret-management-address=SECRET-MANAGEMENT-ADDRESS"  --param "secret-username=SECRET-USERNAME"  --param "secret-password=SECRET-PASSWORD" 
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

1. Review the [parameter reference](#ibm-system-storage-block-csi-driver-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.4.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.4.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 1.5.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.5.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 1.10.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.10.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 1.11.1 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.11.1\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\",\"user-secret-parameters\": }
    ```
    {: pre}


    Example request to create a version 1.11.2 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-system-storage-block-csi-driver\", \"storage-template-version\": \"1.11.2\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"NAMESPACE\", { \"entry.name\": \"SECRET-NAME\",\"user-secret-parameters\": { \"entry.name\": \"SECRET-MANAGEMENT-ADDRESS\",{ \"entry.name\": \"SECRET-USERNAME\",{ \"entry.name\": \"SECRET-PASSWORD\",}
    ```
    {: pre}













{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}


## Deploying an app that uses {{site.data.keyword.blockstorageshort}}
{: #storage-block-csi-app-deploy}

You can use the `ibm-system-storage-block-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a Kubernetes secret configuration file that contains your {{site.data.keyword.blockstorageshort}} credentials.
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

1. Create a storage class that uses the {{site.data.keyword.blockstorageshort}} driver.
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

1. Create a PVC that references the storage class that you created earlier.
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


{{site.data.content.configuration-upgrade-cli}}
{{site.data.content.configuration-upgrade-console}}



## Parameter reference
{: #ibm-system-storage-block-csi-driver-parameter-reference}

### 1.4.0 parameter reference
{: #ibm-system-storage-block-csi-driver-1.4.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Namespace | `namespace` | Config | The namespace where you want to create the deployment. | true | `default` |
{: caption="Table 1. 1.4.0 parameter reference" caption-side="bottom"}


### 1.5.0 parameter reference
{: #ibm-system-storage-block-csi-driver-1.5.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Namespace | `namespace` | Config | The namespace where you want to create the deployment. | true | `default` |
{: caption="Table 2. 1.5.0 parameter reference" caption-side="bottom"}


### 1.10.0 parameter reference
{: #ibm-system-storage-block-csi-driver-1.10.0-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Namespace | `namespace` | Config | The namespace where you want to create the deployment. | true | `default` |
{: caption="Table 3. 1.10.0 parameter reference" caption-side="bottom"}


### 1.11.1 parameter reference
{: #ibm-system-storage-block-csi-driver-1.11.1-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Namespace | `namespace` | Config | The namespace where you want to create the deployment. | true | `default` |
{: caption="Table 4. 1.11.1 parameter reference" caption-side="bottom"}


### 1.11.2 parameter reference
{: #ibm-system-storage-block-csi-driver-1.11.2-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Namespace | `namespace` | Config | The namespace where you want to create the deployment. | true | `default` |
| Secret Name | `secret-name` | Config | The name of the secret to create. | true | N/A |
| Secret Management Address | `secret-management-address` | Secret | The address of the management server. This could be an IP address or a URL. For example: `example-cluster.xiv.ibm.com`. | true | N/A |
| Secret Username | `secret-username` | Secret | The username to use to authenticate to the management server. | true | N/A |
| Secret Password | `secret-password` | Secret | The password to use to authenticate to the management server. | true | N/A |
{: caption="Table 5. 1.11.2 parameter reference" caption-side="bottom"}






## Getting help and support for {{site.data.keyword.blockstorageshort}}
{: #sat-storage-block-csi-support}



1. Review the FAQs in the [{{site.data.keyword.blockstorageshort}} docs](/docs/cloud-object-storage?topic=cloud-object-storage-faq){: external}.
1. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-storage-must-gather) to troubleshoot and resolve common issues.
1. Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
1. Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. Tag any questions with `ibm-cloud`.
1. If you run into an issue with {{site.data.keyword.blockstorageshort}} submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.



