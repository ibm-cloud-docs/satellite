---
copyright:
  years: 2020, 2021
lastupdated: "2021-07-12"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations, 

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# NetApp ONTAP-NAS
{: #config-storage-netapp-nas}

Set up [NetApp ONTAP-NAS storage](https://netapp-trident.readthedocs.io/en/stable-v20.07/){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp ONTAP-NAS template](/docs/satellite?topic=satellite-config-storage-netapp-trident) which installs the required operator.
{: important}

<br />



## Creating a NetApp ONTAP-NAS storage configuration in the command line
{: #sat-storage-netapp-cli-nas}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.
    ```sh
    ibmcloud login
    ```
    {: pre}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.
    ```
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
3. Review the [NetApp ONTAP-NAS storage configuration parameters](#sat-storage-netapp-params-cli-nas).
4. Copy the following command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <config_name> --location <location> --template-name netapp-ontap-nas --template-version <template_version> --param "managementLIF=<managementLIF>" --param "dataLIF=<dataLIF>" --param "svm=<svm>" --param "export-policy=<export-policy>" --param "username=<username>" --param "password=<password>"
  ```
  {: pre}
5. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}
## Assigning your NetApp ONTAP-NAS storage configuration to a cluster
{: #assign-storage-netapp-nas}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-netapp-nas), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />



### Assigning a storage configuration in the command line
{: #assign-storage-netapp-cli-nas}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig).
  * **Group**
    ```sh
    ibmcloud sat group ls
    ```
    {: pre}

  * **Cluster**
    ```sh
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}

  * **{{site.data.keyword.satelliteshort}}-enabled service cluster**
    ```sh
    ibmcloud sat service ls --location <location>
    ```
    {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. Replace `<group>` with the ID of your cluster group or `<cluster>` with the ID of your cluster. Replace `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

  * **Group**
    ```sh
    ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
    ```
    {: pre}

  * **Cluster**
    ```sh
    ibmcloud sat storage assignment create --cluster <cluster> --config <config> --name <name>
    ```
    {: pre}

  * **{{site.data.keyword.satelliteshort}}-enabled service cluster**
    ```sh
    ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
    ```
    {: pre}

1. Verify that your assignment is created.
  ```sh
  ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>) | grep <storage-assignment-name>
  ```
  {: pre}
5. Verify that the `trident-kubectl-nas` pod is deployed in the `trident` namespace.
  ```sh
  oc get pods -n trident | grep trident-kubectl-nas
  ```
  {: pre}

  **Example output**
  ```sh
  trident-kubectl-nas                 1/1     Running   0          2m32s
  ```
  {: screen}

6. Verify that the `sat-netapp` storage classes are deployed.
  ```sh
  oc get sc | grep netapp
  ```
  {: pre}

7. Verify that all resources in the `trident` namespace are `Running` or `Ready`.
  ```sh
  oc get all -n trident
  ```
  {: pre}

  **Example output**
  ```sh
  NAME                                    READY   STATUS    RESTARTS   AGE
  pod/trident-csi-2nrt4                   2/2     Running   0          14m
  pod/trident-csi-7f999bfb96-z4dr5        6/6     Running   0          14m
  pod/trident-csi-cd5mx                   2/2     Running   0          14m
  pod/trident-csi-zlwwn                   2/2     Running   0          14m
  pod/trident-kubectl-nas                 1/1     Running   0          4m14s
  pod/trident-operator-794f74cd4b-zpnt4   1/1     Running   0          14m

  NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)              AGE
  service/trident-csi   ClusterIP   172.21.106.252   <none>        34571/TCP,9220/TCP   14m

  NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                                     AGE
  daemonset.apps/trident-csi   3         3         3       3            3           kubernetes.io/arch=amd64,kubernetes.io/os=linux   14m

  NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
  deployment.apps/trident-csi        1/1     1            1           14m
  deployment.apps/trident-operator   1/1     1            1           14m

  NAME                                          DESIRED   CURRENT   READY   AGE
  replicaset.apps/trident-csi-7f999bfb96        1         1         1       14m
  replicaset.apps/trident-operator-794f74cd4b   1         1         1       14m
  ```
  {: screen}

<br />

## Deploying an app that uses ONTAP-NAS storage
{: #sat-storage-netapp-nas-deploy}

You can use the `trident-kubectl-nas` driver to deploy apps that use your NetApp ONTAP-NAS in your clusters.
{: shortdesc}

1. Create a PVC configuration file that uses one of the `sat-netapp` storage classes. 
  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: netapp-pvc
  spec:
    accessModes:
      - ReadWriteMany
    storageClassName: sat-netapp-file-gold
    resources:
      requests:
        storage: 10Gi
  ```
  {: pre}

1. Create the PVC in your cluster.
  ```sh
  oc apply -f pvc.yaml
  ```
  {: pre}

3. Verify that the PVC is created. Make sure that the PVC is in a `Bound` status.
  ```sh
  oc get pvc
  ```
  {: pre}

  Example output:
  ```
  NAME         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS           AGE
  netapp-pvc   Bound    pvc-acd9e5b4-0b24-4e20-ac00-69a05148c799   10Gi       RWX            sat-netapp-file-gold   39s
  ```
  {: screen}

4. Create a YAML configuration file for a pod that mounts the PVC that you created. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file on your ONTAP-NAS volume mount path.
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: app
  spec:
    containers:
    - name: app
      image: nginx
      command: ["/bin/sh"]
      args: ["-c", "while true; do echo $(date -u) >> /test/test.txt; sleep 5; done"]
      volumeMounts:
      - name: persistent-storage
        mountPath: /test
    volumes:
    - name: persistent-storage
      persistentVolumeClaim:
        claimName: netapp-pvc
  ```
  {: codeblock}

5. Create the pod in your cluster.
   ```sh
   oc apply -f pod.yaml
   ```
   {: pre}

6. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.
   ```sh
   oc get pods
   ```
   {: pre}

   Example output:
   ```
   NAME                                READY   STATUS    RESTARTS   AGE
   app   1/1     Running   0          50s
   ```
   {: screen}

7. Verify that the app can write to your ONTAP-NAS instance.
   1. Log in to your pod.
      ```sh
      oc exec app -it bash
      ```
      {: pre}

   2. Display the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
      ```sh
      cat /test/test.txt
      ```
      {: pre}

      **Example output**:
      ```sh
      Wed May 19 13:28:31 UTC 2021
      Wed May 19 13:28:37 UTC 2021
      Wed May 19 13:28:42 UTC 2021
      Wed May 19 13:28:47 UTC 2021
      ```
      {: screen}

   3. Exit the pod.
      ```sh
      exit
      ```
      {: pre}

<br />

## Removing NetApp ONTAP-NAS storage from your apps
{: #netapp-nas-rm}

Before you remove your storage configuration, remove the app pods and PVCs that are using your NetApp storage.
{: shortdesc}

1. List your PVCs and note the name of the PVC and the corresponding PV that you want to remove.
   ```sh
   oc get pvc
   ```
   {: pre}

2. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC.
  ```sh
  oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
  ```
  {: pre}

  **Example output**:
  ```
  app    sat-netapp-file-gold
  ```
  {: screen}

3. If the pod is part of a deployment, delete the deployment.
  ```sh
  oc delete deployment <deployment_name>
  ```
  {: pre}

4. Verify that the pod or the deployment is removed.
  ```sh
  oc get pods
  ```
  {: pre}

  ```sh
  oc get deployments
  ```
  {: pre}

5. Delete the PVC.
  ```sh
  oc delete pvc <pvc_name>
  ```
  {: pre}

6. Delete the corresponding PV.
  ```sh
  oc delete pv <pv_name>
  ```
  {: pre}
7. [Remove your NetApp ONTAP-NAS storage configuration from your cluster](#netapp-nas-template-rm-cli)

<br />


### Removing the NetApp ONTAP-NAS storage assignment and configuration from the CLI
{: #netapp-nas-template-rm-cli}

Use the CLI to remove a storage assignment and storage configuration.
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.
   ```sh
   ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>)
   ```
   {: pre}

2. Remove the assignment. After the assignment is removed, the NetApp ONTAP-NAS driver pods and storage class are removed from all clusters that were part of the storage assignment.
   ```sh
   ibmcloud sat storage assignment rm --assignment <assignment_name>
   ```
   {: pre}

3. Verify that the NetApp ONTAP-NAS driver is removed from your cluster. List the storage classess in your cluster and verify that the NetApp ONTAP-NAS storage class is removed.
  ```sh
  oc get sc
  ```
  {: pre}

4. List the pods in the `trident` namespace and verify that the NetApp ONTAP-NAS storage driver pods are removed.
  ```sh
  oc get pods -n trident
  ```
  {: pre}

5. **Optional**: List your storage configurations and remove your NetApp configuration.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

  ```sh
  ibmcloud sat storage config rm --config <config_name>
  ```
  {: pre}

7. **Next steps**: [Remove the NetApp Trident operator from your cluster](/docs/satellite?topic=satellite-config-storage-netapp-trident).

  

## NetApp ONTAP-NAS storage configuration parameter reference
{: #sat-storage-netapp-params-cli-nas}

For more information about the NetApp ONTAP-NAS configuration parameters, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v20.07/docker/install/ndvp_ontap_config.html#configuration-file-options){: external}.

| Parameter name | Required? | Description | Default if not provided |
| --- | --- | --- | 
| `--name` | Required | Enter a name for your storage configuration. | N/A |
| `--template-name` | Required | Enter `netapp-ontap-nas` | N/A |
| `--template-version` | Required | Enter the template version number. To get a list of templates, run `ibmcloud sat storage template ls`. | N/A |
| `namespace` | Required | Enter the namespace where you want to install the NetApp ONTAP-NAS storage drivers. | `trident` |
| `managementLIF` | Required | Enter the IP address of management LIF. Example: `10.0.0.1`. | N/A |
| `dataLIF` | Required | Enter the IP address of data LIF. Example: `10.0.0.2`. | N/A | 
| `svm` | Required | Enter the name of the storage virtual machine. Example: `svm-nfs`. | N/A | 
| `export-policy` | Required | Enter the NFS export policy to use. Example: `default`. | N/A |
| `username` | Required | Enter your username. | N/A |
| `password` | Required | Enter your user password. | N/A |
| `limitVolumeSize` | Optional | Maximum volume size that can be requested and qtree parent volume size. | `50Gi` |
| `limitAggregateUsage` | Optional | Limit provisioning of volumes if the parent volume usage exceeds this value. For example, if a volume is requested that causes the parent volume usage to exceed this value, the volume provisioning fails.  | `80%` |
| `nfsMountOptions` | Optional | Specify the NFS mount version. Example: `nfsvers=4` | `nfsvers=4` |
{: caption="Table 1. NetApp ONTAP-NAS storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}

<br />
## Storage class reference
{: #netapp-sc-reference-nas}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-NAS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the NetApp Trident storage classes in [GitHub](https://github.com/IBM/ibm-satellite-storage/tree/develop/config-templates/netapp/netapp-ontap-nas/20.07).
{: shortdesc}

| Storage class name | Type | File system | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-netapp-file-gold` | ONTAP-NAS | File | Delete |
| `sat-netapp-file-silver` | ONTAP-NAS | File | Delete |
| `sat-netapp-file-bronze` | ONTAP-NAS | File | Delete |
{: caption="Table 2. NetApp ONTAP-NAS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}
