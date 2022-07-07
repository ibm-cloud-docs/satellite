---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: spectrum scale, satellite storage, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Deprecated {{site.data.keyword.IBM_notm}} Spectrum Scale driver
{: #config-storage-spectrum-scale}

The {{site.data.keyword.IBM_notm}} Spectrum Scale driver is deprecated as of April 04, 2022. Please reach out to scale@us.ibm.com for further compatibility details and options. 

Set up [{{site.data.keyword.IBM_notm}} Spectrum Scale](https://www.ibm.com/docs/en/spectrum-scale-csi){: external} storage for {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}


You can use the {{site.data.keyword.IBM_notm}} Spectrum Scale Container Storage Interface (CSI) driver to create persistent storage for stateful apps. that run in your {{site.data.keyword.satelliteshort}} clusters.

The {{site.data.keyword.IBM_notm}} Spectrum Scale CSI driver supports the following features.

Static provisioning
:   You can use your existing directories and file sets as persistent volumes.

Lightweight dynamic provisioning
:   You can dynamically create directory-based volumes.

Fileset-based dynamic provisioning
:   You can dynamically create fileset-based volumes.

Multiple file systems
:   You can create volumes across multiple file systems.

Remote volumes
:   You can create volumes on a remotely mounted file system.

Operator deployment
:   You can use the {{site.data.keyword.IBM_notm}} Spectrum Scale operator for easier deployment, upgrade, and cleanup.

Multiple volume access modes
:   You can create volumes with ReadWriteMany (RWX) and ReadWriteOnce (RWO) access modes.


## Prerequisites
{: #sat-storage-spectrum-scale-prereq}

Complete the following steps, but do not create an {{site.data.keyword.cloud_notm}} Spectrum Scale cluster.
{: shortdesc}


1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Follow the steps to install Spectrum Scale as root](https://www.ibm.com/docs/en/spectrum-scale/5.1.0?topic=isslndp-manually-installing-spectrum-scale-software-packages-linux-nodes){: external} and make sure that you install all the required packages by running the following command.
    ```sh
    yum install -y kernel-devel cpp gcc gcc-c++ binutils python3
    ```
    {: pre}

1. [Configure `sudo`](https://www.ibm.com/docs/en/spectrum-scale/5.1.0?topic=login-configuring-sudo){: external}.
1. Follow the steps to [install {{site.data.keyword.IBM_notm}} Spectrum Scale packages on Linux systems](https://www.ibm.com/docs/en/spectrum-scale/5.1.0?topic=nodes-installing-spectrum-scale-packages-linux-systems), but make sure that you do not create the cluster in step 4.

1. Follow the steps to [run {{site.data.keyword.IBM_notm}} Spectrum Scale commands without remote root login](https://www.ibm.com/docs/en/spectrum-scale/5.1.0?topic=cgc-running-spectrum-scale-commands-without-remote-root-login){: external}, but do not create a Spectrum Scale cluster.

1. [Switch to the `sudo` user and create a Spectrum Scale cluster on the worker nodes](https://www.ibm.com/docs/en/spectrum-scale/5.1.0?topic=login-configuring-cluster-use-sudo-wrapper-scripts){: external}.
1. Verify that the cluster is using sudo wrappers.
1. Verify Spectrum Scale starts up on all worker nodes by running the following commands.
    ```sh
    sudo /usr/lpp/mmfs/bin/mmstartup -a -- sudo /usr/lpp/mmfs/bin/mmgetstate -a
    ```
    {: pre}

1. Enable autostart for Spectrum Scale.

    ```sh
    sudo /usr/lpp/mmfs/bin/mmchconfig autoload=yes
    ```
    {: pre}

1. [Attach your {{site.data.keyword.IBM_notm}} Spectrum Scale Nodes to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-attach-hosts)
    * Make sure your system is configured for the desired default route if you have more than one clustering network.
    * Make sure that default route has a path to the public network, possibly via NAT or VPN.

1. Start {{site.data.keyword.IBM_notm}} Spectrum Scale nodes and verify that they are running. If there is an issue with the portability layer, you can [rebuild the portability layer](#ess-ts-rebuilding).

1. [Mount your file system remotely and verify that it is running on all worker nodes](https://www.ibm.com/docs/en/spectrum-scale/5.1.0?topic=system-mounting-remote-gpfs-file){: external}. Run the `mmcluster` command on both the local and remote {{site.data.keyword.IBM_notm}} Spectrum Scale cluster.

1. [Initialize the {{site.data.keyword.IBM_notm}} Spectrum Scale GUI](https://www.ibm.com/docs/en/spectrum-scale-csi?topic=i-performing-pre-installation-tasks){: external}.

1. Label the worker nodes where the {{site.data.keyword.IBM_notm}} Spectrum Scale client is installed and where the {{site.data.keyword.IBM_notm}} Spectrum Scale Container Storage Interface (CSI) driver is running.
    ```sh
    oc label nodes <node> <node> <node> scale=true --overwrite=true
    ```
    {: pre}

## Mapping {{site.data.keyword.IBM_notm}} Spectrum Scale hosts to worker node names
{: #sat-storage-spectrum-scale-ts-mapping}

In some environments, your worker node names might be different from your {{site.data.keyword.IBM_notm}} Spectrum Scale node names which results in the pods not mounting. You must map your worker node names to your Spectrum Scale nodes. For more information, see [Kubernetes to {{site.data.keyword.IBM_notm}} Spectrum Scale node mapping](https://www.ibm.com/docs/en/spectrum-scale-csi?topic=o-kubernetes-spectrum-scale-node-mapping){: external}.


## Creating an Spectrum Scale storage configuration in the command line
{: #sat-storage-spectrum-scale-cli}

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
    
1. Review the [template parameters](#sat-storage-spectrum-scale-params-cli).
1. Copy the following command and replace the variables with the parameters for your storage configuration. You can pass parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name ess --template-version <template_version> -p "scale-host-path=<scale-host-path>" -p "cluster-id=<cluseter-id>" -p "primary-fs=<primary-fs>" -p "gui-host=<gui-host>" -p "secret-name=<secret-name>" -p "gui-api-user=<gui-api-user>" -p "gui-api-password=<gui-api-password>" -p "k8-n1-ip=<k8-n1-ip>" -p "sc-n1-host=<sc-n1-host>" -p "k8-n2-ip=<k8-n2-ip>" -p "sc-n2-host=<sc-n2-host>" -p "k8-n3-ip=<k8-n3-ip>" -p "sc-n3-host=<sc-n3-host>" -p "storage-class-name=<storage-class-name>" -p "vol-backend-fs=<vol-backend-fs>" -p "vol-dir-base-path=<vol-dir-base>"
    ```
    {: pre}

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-spectrum-scale).







## Assigning your Spectrum Scale storage configuration to a cluster
{: #assign-storage-spectrum-scale}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-spectrum-scale), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.



### Assigning a storage configuration in the command line
{: #assign-storage-spectrum-scale-cli}

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
    oc get all -n ibm-spectrum-scale-csi-driver
    ```
    {: pre}

    Example output
    ```sh
    NAME                                                   READY   STATUS             RESTARTS   AGE
    pod/ibm-spectrum-scale-csi-attacher-0                  1/1     Running            2          173m
    pod/ibm-spectrum-scale-csi-k2978                       2/2     Running            0          152m
    pod/ibm-spectrum-scale-csi-operator-56955949c4-j4ljf   1/1     Running            0          178m
    pod/ibm-spectrum-scale-csi-provisioner-0               1/1     Running            2          173m
    pod/ibm-spectrum-scale-csi-t6hfz                       2/2     Running            0          152m
    pod/ibm-spectrum-scale-csi-x9h5g                       2/2     Running            0          152m

    NAME                                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)             AGE
    service/ibm-spectrum-scale-csi-operator-metrics   ClusterIP   172.XX.XX.XXX   <none>        8383/TCP,8686/TCP   178m

    NAME                                    DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    daemonset.apps/ibm-spectrum-scale-csi   3         3         3       3            3           scale=true      173m

    NAME                                              READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/ibm-spectrum-scale-csi-operator   1/1     1            1           178m

    NAME                                                         DESIRED   CURRENT   READY   AGE
    replicaset.apps/ibm-spectrum-scale-csi-operator-56955949c4   1         1         1       178m

    NAME                                                  READY   AGE
    statefulset.apps/ibm-spectrum-scale-csi-attacher      1/1     173m
    statefulset.apps/ibm-spectrum-scale-csi-provisioner   1/1     173m
    ```
    {: screen}

### Changing the Spectrum Scale CSI driver mount point
{: #ess-change-mount-point}

After you deploy the `ess` template, you must change the Spectrum Scale CSI driver mount point to `/var/data/kubelet`.
{: shortdesc}

1. Edit the daemonset by using the `oc edit` command or get the daemonset YAML file configuration and save it on your local machine.
    * Edit in the command line
        ```sh
        oc edit ds ibm-spectrum-scale-csi -n ibm-spectrum-scale-csi-driver
        ```
        {: pre}

    * Get the YAML configuration file
        ```sh
        oc get ds ibm-spectrum-scale-csi -n ibm-spectrum-scale-csi-driver -o yaml
        ```
        {: pre}

1. Search for the string: `/var\/lib\/kubelet$`. Note that `kubelet` is the end of the string. There are 2 instances in the daemonset configuration that must be replaced.
    ```sh
    /var\/lib\/kubelet$
    ```
    {: codeblock}

1. Replace the `/var\/lib\/kubelet$` string with `/var/data/kubelet`.
    ```yaml
    volumeMounts:
    ....
    -mountPath: /var/data/kubelet
      name: pods-mount-dir
        ....
    ```
    {: screen}

## Deploying an app that uses your Spectrum Scale storage
{: #storage-spectrum-app-deploy}

You can use the `ibm-spectrum-scale-csi-driver` to create PVCs that you can use in your apps.
{: shortdesc}

1. Create a storage class that uses the `spectrumscale.csi.ibm.com` driver.
    ```yaml
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: ess-sc
    provisioner: spectrumscale.csi.ibm.com
    parameters:
      volBackendFs: <vol-backend-fs> # The name of the file system on which the fileset is created.
    reclaimPolicy: Delete
    ```
    {: codeblock}

1. Create the storage class in your cluster.
    ```sh
    oc create -f sc.yaml
    ```
    {: pre}

1. Create a persistent volume (PVC) that references the storage class you created earlier.
    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      name: ess-pvc
    spec:
      accessModes:
       - ReadWriteMany
      resources:
        requests:
        storage: 5Gi
      storageClassName: ess-sc
    ```
    {: codeblock}

1. Create the PVC in your cluster.
    ```sh
    oc create -f pvc.yaml
    ```
    {: pre}

1. Verify that your PVC is `Bound`.
    ```sh
    oc get pvc
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you created. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file on your volume mount path.
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
          claimName: ess-pvc
    ```
    {: codeblock}

1. Create the pod in your cluster.
    ```sh
    oc create -f pod.yaml
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
    app                                 1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your {{site.data.keyword.IBM_notm}} Spectrum Scale instance.
    1. Log in to your pod.
        ```sh
        oc exec <app-pod-name> -it bash
        ```
        {: pre}

    1. Display the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
        ```sh
        cat /test/test.txt
        ```
        {: pre}

        Example output
        ```sh
        Wed May 5 20:37:04 UTC 2021
        Wed May 5 20:37:10 UTC 2021
        Wed May 5 20:37:15 UTC 2021
        Wed May 5 20:37:20 UTC 2021
        Wed May 5 20:37:26 UTC 2021
        Wed May 5 20:37:31 UTC 2021
        Wed May 5 20:37:36 UTC 2021
        Wed May 5 20:37:41 UTC 2021
        Wed May 5 20:37:46 UTC 2021
        Wed May 5 20:37:51 UTC 2021
        ```
        {: screen}

    1. Exit the pod.
        ```sh
        exit
        ```
        {: pre}


## Removing the Spectrum Scale storage configuration from the CLI
{: #sat-storage-spectrum-rm-cli}

Use the CLI to remove a storage configuration.
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the driver pods and default storage classes are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. Verify that the driver and storage classes are removed from your cluster.
    1. List the storage classes in your cluster and verify that the storage classes are removed.
        ```sh
        oc get sc
        ```
        {: pre}

    2. Optional: If you created custom storage classes, remove them.
        ```sh
        oc delete sc <storage-class-name>
        ```
        {: pre}

    3. List the pods in the `ibm-spectrum-scale-csi-driver` namespace and verify that the storage driver pods are removed.
        ```sh
        oc get pods -n ibm-spectrum-scale-csi-driver
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

5. [Clean up your Spectrum Scale deployment](https://www.ibm.com/docs/en/spectrum-scale-csi?topic=installation-cleaning-up-spectrum-scale-container-storage-interface-driver-operator-by-using-clis){: external}.

## Troubleshooting for Spectrum Scale
{: #ess-ts}

### Rebuilding the portability layer
{: #ess-ts-rebuilding}

Run the following commands to rebuild the portability layer on each affected worker. Note that you might need to replace some files and subdirectories.
{: shortdesc}

```sh
sudo yum install -y kernel-devel cpp gcc gcc-c++ binutils python3
sudo mkdir /usr/include/asm
sudo cp /usr/src/kernels/3.10.0-1160.11.1.el7.x86_64/arch/x86/include/uapi/asm/*.h
/usr/include/asm
sudo mkdir /usr/include/asm-generic
sudo cp /usr/src/kernels/3.10.0-1160.11.1.el7.x86_64/include/uapi/asm-generic/*.h
/usr/include/asm-generic
sudo mkdir /usr/include/linux
sudo cp /usr/src/kernels/3.10.0-1160.15.2.el7.x86_64/include/uapi/linux/*.h /usr/include/linux
```
{: codeblock}

## Additional references for Spectrum Scale
{: #sat-storage-spectrum-scale-ref}

* [{{site.data.keyword.IBM_notm}} Spectrum Scale CSI driver documentation](https://www.ibm.com/docs/en/spectrum-scale-csi){: external}.
* [Cleaning up your Spectrum Scale deployment](https://www.ibm.com/docs/en/spectrum-scale-csi?topic=installation-cleaning-up-spectrum-scale-container-storage-interface-driver-operator-by-using-clis){: external}.

## Limitations for Spectrum Scale
{: #sat-storage-spectrum-scale-limits}

Do not install the {{site.data.keyword.IBM_notm}} Spectrum Scale management API GUI on worker nodes that are managed by {{site.data.keyword.satelliteshort}}.
{: important}

* If the Spectrum Scale file system mount path is the exact same on the owning and primary Spectrum Scale cluster, then only one Spectrum Scale GUI is required.
* If the Spectrum Scale file system mount paths are different, then two GUIs are required. One of the GUIs must run on a node that has network connectivity to the worker nodes that managed by {{site.data.keyword.satelliteshort}}, but note that the GUI node itself, must not be managed by {{site.data.keyword.satelliteshort}}.

### Spectrum Scale configuration parameter reference
{: #sat-storage-spectrum-scale-params-cli}

| Parameter | Required? | Description |
| --- | --- | --- |
| `scale-host-path` | Required | The mount path of the primary file system. You can retrieve this value by running the `mmlsfs` command from a worker node in the primary Spectrum Scale cluster. |
| `cluster-id` | Required | The cluster ID of the primary {{site.data.keyword.IBM_notm}} Spectrum Scale cluster. You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `primary-fs` | Required | The primary file system name. You can retrieve this value by running the `mmlsfs` command from a worker node in the primary Spectrum Scale cluster. |
| `gui-host` | Required | Enter the FQDN or IP address that corresponds with the ID that is specified in `gui-api-user` parameter. You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `secret-name` | Required | The name of the secret that contains the `username` and `password` to connect to the primary Spectrum Scale cluster GUI server. This parameter is user-specified. |
| `gui-api-user` | Required | The username to connect to the primary Spectrum Scale cluster GUI. To retrieve this value, log in to the Spectrum Scale GUI node and run the `lsuser` command. The `gui-api-user` value is in the `CsiAdmin` group. |
| `gui-api-password` | Required | The password to connect to the primary Spectrum Scale cluster GUI. This parameter is user-specified.|
| `k8-n1-ip` | Required | The IP address of your worker node 1 running {{site.data.keyword.IBM_notm}} Spectrum Scale. You can retrieve this value by running the `oc get nodes` command. |
| `sc-n1-host` | Required | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 1.  You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n2-ip` | Optional | The IP address of your worker node 2 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command. |
| `sc-n2-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 2. You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n3-ip` | Optional | The IP address of your worker node 3 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command.|
| `sc-n3-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 3.  You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n4-ip` | Optional | The IP address of your worker node 3 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command.|
| `sc-n4-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 4.  You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n5-ip` | Optional | The IP address of your worker node 4 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command.|
| `sc-n5-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 5.  You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n6-ip` | Optional | The IP address of your worker node 5 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command.|
| `sc-n6-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 6.  You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n7-ip` | Optional | The IP address of your worker node 6 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command.|
| `sc-n7-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 7.  You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `k8-n8-ip` | Optional | The IP address of your worker node 7 running {{site.data.keyword.IBM_notm}} Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command.|
| `sc-n8-host` | Optional | the hostname of the {{site.data.keyword.IBM_notm}} Spectrum Scale Node 8. You can retrieve this value by running the `mmlscluster` command from a worker node in the primary Spectrum Scale cluster. |
| `storage-class-name` | Optional | The name of the {{site.data.keyword.IBM_notm}} Spectrum Scale storage class.  |
| `vol-backend-fs` | Optional | The name of the file system on which the fileset is created. |
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}



## Storage class reference for spectrum scale
{: #sat-storage-spectrum-scale-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for {{site.data.keyword.IBM_notm}} Spectrum Scale. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}


| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `ibm-spectrum-scale-csi-lt` | Light weight volumes | Delete  |
{: caption="Table 1. IBM Spectrum Scale storage class reference." caption-side="top"}


