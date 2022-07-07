---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: block storage, satellite storage, satellite config, satellite configurations, 

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.IBM_notm}} Systems block storage CSI driver
{: #config-storage-block-csi}

The block storage CSI driver for {{site.data.keyword.satellitelong}} is based on an {{site.data.keyword.IBM_notm}} open-source project, and integrated into the {{site.data.keyword.IBM_notm}} Storage orchestration for containers. {{site.data.keyword.IBM_notm}} Storage orchestration for containers enables enterprises to implement a modern container-driven hybrid multicloud environment that can reduce IT costs and enhance business agility, while continuing to derive value from existing systems.

For full release notes, compatibility, installation, and user information, see the [block storage CSI driver documentation](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0){: external}.

Supported {{site.data.keyword.IBM_notm}} storage systems for Satellite include,

- {{site.data.keyword.IBM_notm}} Spectrum Virtualize Family including {{site.data.keyword.IBM_notm}} SAN Volume Controller (SVC) and {{site.data.keyword.IBM_notm}} FlashSystem® family members built with {{site.data.keyword.IBM_notm}} Spectrum® Virtualize (FlashSystem 5010, 5030, 5100, 5200, 7200, 9100, 9200, 9200R)
- {{site.data.keyword.IBM_notm}} FlashSystem A9000 and A9000R
- {{site.data.keyword.IBM_notm}} DS8000 Family

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}
  
## Prerequisites for using block storage
{: #sat-storage-block-csi-prereq}

Be sure to complete all prerequisite and installation steps before assigning hosts to your location. Do not create a Kubernetes cluster.
{: important}

1. Review the [compatibility and requirements documentation](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=installation-compatibility-requirements){: external}.

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).


## Creating a block storage configuration in the command line
{: #sat-storage-block-csi-cli}

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
    
1. List the available templates and versions and review the output. Make a note of the template and version that you want to use.

    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
    
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Note that Kubernetes resources can't contain capital letters or special characters. Enter a name for your config that uses only lowercase letters, numbers, hyphens or periods.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name ibm-system-storage-block-csi-driver --template-version <template_version> -p "namespace=<namespace>" 
    ```
    {: pre}

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your storage configuration to your clusters](#assign-storage-block-csi).





## Assigning your {{site.data.keyword.IBM_notm}} block storage configuration to a cluster
{: #assign-storage-block-csi}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-block-csi), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.





### Assigning a block storage configuration in the command line
{: #assign-storage-block-csi-cli}

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
    oc get all -A | grep block
    ```
    {: pre}

## Deploying an app that uses your {{site.data.keyword.IBM_notm}} block storage
{: #storage-block-csi-app-deploy}

You can use the `ibm-system-storage-block-csi-driver` to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a Kubernetes secret configuration file that contains your block storage credentials. For more information, see [Creating a Kubernetes secret](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-secret){: external}.
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

1. [Create a storage class that uses the block storage driver](https://www.ibm.com/docs/en/stg-block-csi-driver/1.4.0?topic=configuration-creating-storageclass){: external}.
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





