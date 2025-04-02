---


copyright:
  years: 2022, 2025
lastupdated: "2025-04-02"

keywords: satellite, hybrid, multicloud, bare metal, coreos, rhcos, virtualization

subcollection: satellite

content-type: tutorial
services: satellite
account-plan: paid
completion-time: 2hr

---

{{site.data.keyword.attribute-definition-list}}


# Setting up virtualization on a {{site.data.keyword.satelliteshort}} location
{: #virtualization-location}
{: toc-content-type="tutorial"}
{: toc-services="satellite"}
{: toc-completion-time="2hr"}

You can set up your {{site.data.keyword.baremetal_short}} to use {{site.data.keyword.redhat_openshift_notm}} virtualization in your {{site.data.keyword.satelliteshort}} location. By using virtualization, you can provision Windows or other virtual machines on your {{site.data.keyword.baremetal_short}} in a managed {{site.data.keyword.redhat_openshift_notm}} space. 
{: shortdesc}

Supported host operating systems
:   Red Hat CoreOS (RHCOS)


## Prerequisites
{: #virt-bare-metal-prereq}

- Create a RHCOS-enabled location. To check whether your location is RHCOS-enabled, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). If your location is not enabled, [create a new one with RHCOS](/docs/satellite?topic=satellite-locations).
- Attach hosts to your location and set up your [location control plane](/docs/satellite?topic=satellite-setup-control-plane).
- Find and record your bare metal host name. 
- Find your bare metal server network information. Record the CIDR and gateway information for the public and private interfaces for your system.
- If you want to use {{site.data.keyword.cos_full_notm}} to store your ignition file, create or identify a bucket.
- Create or identify a cluster within the {{site.data.keyword.satelliteshort}} location that runs a supported operating system; for example, this tutorial uses a {{site.data.keyword.redhat_openshift_notm}} cluster that is running 4.11.
- If you want to use OpenShift Data Foundation as your storage solution, add 2 storage disks to each of your {{site.data.keyword.baremetal_short}} when you provision them.

## {{site.data.keyword.baremetal_short_sing}} requirements for {{site.data.keyword.satelliteshort}}
{: #virt-setup-bare-metal}

To set up virtualization, your {{site.data.keyword.baremetal_short_sing}} must meet the following requirements. 

- Must support virtualization technology.
    - For Intel CPUs, support for virtualization is referred to as `Intel VT` or `VT-x`.
    - For AMD CPUs, support for virtualization is referred to as `AMD Virtualization` or `AMD-V`.
- Must have a minimum of 8 cores and 32 GB RAM, plus any additional cores that you need for your vCPU overhead. For more information, see [CPU overhead](https://docs.openshift.com/container-platform/4.11/virt/install/preparing-cluster-for-virt.html#CPU-overhead_preparing-cluster-for-virt){: external} in the {{site.data.keyword.redhat_openshift_notm}} docs.
- Must include enough memory for your workload needs. For example: `360 MiB + (1.002 * requested memory) + 146 MiB + 8 MiB * (number of vCPUs) + 16 MiB * (number of graphics devices)`. For more information, see [Memory overhead](https://docs.openshift.com/container-platform/4.11/virt/install/preparing-cluster-for-virt.html#memory-overhead_preparing-cluster-for-virt){: external} in the {{site.data.keyword.redhat_openshift_notm}} docs.
- No operating system installed. The Red Hat CoreOS operating system is installed later in this process.
- If you want to use OpenShift Data Foundation as your storage solution, add 2 storage disks to each of your {{site.data.keyword.baremetal_short}} when you provision them.

If your servers do not meet these requirements, you can [create a {{site.data.keyword.baremetal_short_sing}}](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server). For a list of bare metal options, see [Available options for a bare metal server](/docs/bare-metal?topic=bare-metal-about-bm#options-for-bare-metal-servers).
{: tip}

## Attaching bare metal servers to your location
{: #assign-bm-loc-virt}
{: step}

Follow these general steps to attach your bare metal servers to your location. These steps might vary, depending on your specific hardware. For a complete tutorial, see [Attaching {{site.data.keyword.cloud_notm}} Bare Metal Servers for Classic hosts](/docs/satellite?topic=satellite-assign-bare-metal). 

1. [Download a Red Hat CoreOS ISO](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external}. Find the corresponding ISO version that matches the {{site.data.keyword.redhat_openshift_notm}} version that you want to use. For example, if you want to use version 4.11, [download a version of RHCOS for 4.11](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/4.11/){: external}, such as `rhcos-4.11.9-x86_64-live.x86_64.iso`.
2. Log in to your bare metal server.
3. In the BIOS settings, ensure that virtualization is enabled.
4. Set up your boot order to boot the Red Hat CoreOS ISO file that you downloaded in step 1.
5. Boot your system to install the ISO.
6. After Red Hat CoreOS is booted into memory, set up network connections so you can provide the location ignition file and attach the server to your location.
7. Download the ignition script for your {{site.data.keyword.satelliteshort}} location.
    
    ```sh
    ibmcloud sat host attach --location <location_name> --operating-system RHCOS
    ```
    {: pre}
    
9. Edit your ignition file to include the bare metal host name and network information. For more information about adding these details, see [Configuring your ignition file](/docs/satellite?topic=satellite-assign-bare-metal#config-ignition). Note that you must edit the ignition file for each bare metal server that you want to attach to your location.
10. Put your ignition file in a location that your bare metal server can reach. For example, you can upload it to an {{site.data.keyword.cos_full_notm}} bucket.
11. Download the ignition script to your bare metal server.

    ```sh
    curl <ignition_file_location > ignition.ign
    ```
    {: pre}
    
12. Run the following `install` command to start the ignition file. Replace `<diskName>` with the full path of the disk location where you want Red Hat  CoreOS installed and replace `<filename>` with the path of the ignition file.

    ```sh
    sudo coreos-installer install <diskName> --ignition-file <filename>
    ```
    {: pre}
    
    The installation process can take an hour or two to complete. 
    {: note}

13. After the installation completes, unplug your RHCOS ISO file and reboot from your hard disk. 
14. Check your {{site.data.keyword.satelliteshort}} location to confirm that your bare metal server is attached.
15. Repeat these steps for each bare metal host that you want to attach to your location.


## Assigning a {{site.data.keyword.baremetal_short_sing}} host to your {{site.data.keyword.redhat_openshift_notm}} cluster
{: #assign-bare-metal-cluster-virt}
{: step}

After your {{site.data.keyword.baremetal_short_sing}} is attached to your location, you can assign it to a {{site.data.keyword.redhat_openshift_notm}} cluster worker pool. 

1. Find the hosts to add to your {{site.data.keyword.redhat_openshift_notm}} cluster worker pool.

    ```sh
    ibmcloud sat hosts --location <locationID>
    ```
    {: pre}

2. Assign the {{site.data.keyword.baremetal_short_sing}} to the {{site.data.keyword.redhat_openshift_notm}} cluster worker pool.

    ```sh
    ibmcloud sat host assign --location <locationID> --cluster <clusterID> --host <hostID> --worker-pool default --zone <zone>
    ```
    {: pre}

Repeat these steps to assign more {{site.data.keyword.baremetal_short}} to your cluster.
{: tip}

Now that your {{site.data.keyword.baremetal_short_sing}} is assigned to a worker pool, you can set up {{site.data.keyword.redhat_openshift_notm}} virtualization.


## Setting up storage for your cluster
{: #virt-cluster-storage}
{: step}

In this example scenario, you deploy OpenShift Data Foundation across 3 nodes in the cluster by automatically discovering the available storage disks on your {{site.data.keyword.baremetal_short}}.

After you attach at least 3 {{site.data.keyword.baremetal_short}} to your location and assigned them as worker nodes in your cluster, you can deploy OpenShift Data Foundation by using the `odf-local` {{site.data.keyword.satelliteshort}} storage template.

1. From the [{{site.data.keyword.satelliteshort}} locations console](https://cloud.ibm.com/satellite/locations){: external}, click your location, then click **Storage > Create storage configuration**.
1. Give your configuration a name.
1. Select **OpenShift Data Foundation for local devices** and select version **4.10**.
1. For this example, leave the rest of the default settings and click **Next**.
1. Wait for ODF to deploy. Then, verify that the pods are ready by listing the pods in the `openshift-storage` namespace.
    ```sh
    oc get pods -n openshift-storage
    ```
    {: pre}
    
    Example output
    ```sh
    NAME                                                              READY   STATUS      RESTARTS   AGE
    ocs-metrics-exporter-5b85d48d66-lwzfn                             1/1     Running     0          2d1h
    ocs-operator-86498bf74c-qcgvh                                     1/1     Running     0          2d1h
    odf-console-68bcd54c7c-5fvkq                                      1/1     Running     0          2d1h
    rook-ceph-mgr-a-758845d77c-xjqkg                                  2/2     Running     0          2d1h
    rook-ceph-mon-a-85d65d9f66-crrhb                                  2/2     Running     0          2d1h
    rook-ceph-mon-b-74fd78856d-s2pdf                                  2/2     Running     0          2d1h
    rook-ceph-mon-c-76f9b8b5f9-gqcm4                                  2/2     Running     0          2d1h
    rook-ceph-operator-5d659cb494-ctkx6                               1/1     Running     0          2d1h
    rook-ceph-osd-0-846cf86f79-z97mc                                  2/2     Running     0          2d1h
    rook-ceph-osd-1-7f79ccf77d-8g4cn                                  2/2     Running     0          2d1h
    rook-ceph-osd-2-549cc486b4-7wf5k                                  2/2     Running     0          2d1h
    rook-ceph-osd-prepare-ocs-deviceset-0-data-0z6pn9-6fwqr           0/1     Completed   0          10d
    rook-ceph-osd-prepare-ocs-deviceset-1-data-0kkxrw-cppk9           0/1     Completed   0          10d
    rook-ceph-osd-prepare-ocs-deviceset-2-data-0pxktc-xm2rc           0/1     Completed   0          10d
    rook-ceph-rgw-ocs-storagecluster-cephobjectstore-a-54c58859nc8j   2/2     Running     0          2d1h
    ...
    ...
    ```
    {: screen}
    
    
## Installing the virtualization operator
{: #virt-operator-install}
{: step}

Follow the steps to [Install {{site.data.keyword.redhat_openshift_notm}} Virtualization using the CLI](https://docs.openshift.com/container-platform/4.11/virt/install/installing-virt-cli.html){: external}.

## Setting up the `virtctl` CLI
{: #virt-tools}
{: step}

Follow the steps to [download and install the `virtctl` CLI tool](https://docs.openshift.com/container-platform/4.11/virt/install/virt-enabling-virtctl.html){: external}.

## Creating a data volume for your virtual machine
{: #virt-vm-storage}
{: step}

After you deploy OpenShift Data Foundation, you can use the `sat-ocs-ceprbd-gold` storage class to create a data volume to use storage for your virtual machine.

1. Copy the following example data volume and save it to a file called `datavol.yaml`.
    ```yaml
    apiVersion: cdi.kubevirt.io/v1beta1
    kind: DataVolume
    metadata:
      name: fedora-1
      namespace: default
    spec:
      source:
        registry:
          pullMethod: node
          url: docker://quay.io/containerdisks/fedora@sha256:29b80ef738f9b09c19efc245aac3921deab9acd542c886cf5295c94ab847dfb5
      pvc:
        accessModes:
          - ReadWriteMany
        resources:
          requests:
            storage: 10Gi
        volumeMode: Block
        storageClassName: sat-ocs-cephrbd-gold
    ```
    {: codeblock}
    
1. Create the data volume.
    ```sh
    oc apply -f datavol.yaml
    ```
    {: pre}
    
1. Verify that the data volume and corresponding PVC were created.
    ```sh
    oc get dv,pvc -n default
    ```
    {: pre}
    
    Example output.
    ```sh
    NAME                                  PHASE       PROGRESS   RESTARTS   AGE
    datavolume.cdi.kubevirt.io/fedora-1   Succeeded   100.0%                16h
    NAME                             STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS           AGE
    persistentvolumeclaim/fedora-1   Bound    pvc-fd8b1a5b-cc32-42bd-95d0-4ccf2e40bca7   10Gi       RWX            sat-ocs-cephrbd-gold   16h
    ```
    {: screen}

## Creating a virtual machine
{: #virt-vm-create}
{: step}

1. Copy the following `VirtualMachine` configuration and save it to a file called `vm.yaml`. Note that you can also create virtual machines through the OpenShift web console.
    ```sh
    apiVersion: kubevirt.io/v1
    kind: VirtualMachine
    metadata:
      labels:
        app: fedora-1
      name: fedora-1
      namespace: default
    spec:
      running: false
      template:
        metadata:
          labels:
            kubevirt.io/domain: fedora-1
        spec:
          domain:
            cpu:
              cores: 1
              sockets: 2
              threads: 1
            devices:
              disks:
              - disk:
                  bus: virtio
                name: rootdisk
              - disk:
                  bus: virtio
                name: cloudinitdisk
              interfaces:
              - masquerade: {}
                name: default
              rng: {}
            features:
              smm:
                enabled: true
            firmware:
              bootloader:
                efi: {}
            resources:
              requests:
                memory: 8Gi
          evictionStrategy: LiveMigrate
          networks:
          - name: default
            pod: {}
          volumes:
          - dataVolume:
              name: fedora-1
            name: rootdisk
          - cloudInitNoCloud:
              userData: |-
                #cloud-config
                user: cloud-user
                password: 'fedora-1-password' 
                chpasswd: { expire: False }
            name: cloudinitdisk
    ```
    {: codeblock}

1. Create the virtual machine in your cluster.
    ```sh
    oc apply -f vm.yaml
    ```
    {: pre}
    
1. Start the virtual machine.
    ```sh
    virtctl start fedora-1 -n default
    ```
    {: pre}
    
1. Verify that the virtual machine is running.
    ```sh
    oc get vm -n default
    ```
    {: pre}
    
    Example output.
    ```sh
    NAME                                  AGE   STATUS    READY
    virtualmachine.kubevirt.io/fedora-1   16h   Running   True
    ```
    {: screen}
    
    
1. From the OpenShift web console, log in to your VM by using the username and password you specified in the VirtualMachine config. For example, `user: cloud-user` and `password: 'fedora-1-password'`.


Congratulations! You just deployed a Fedora virtual machine on your {{site.data.keyword.satelliteshort}} cluster. 

You can find more information about what to do next in the [OpenShift Virtualization Hands-on Lab](https://github.com/rdoxenham/openshift-virt-labs){: external}.
    

## Additional resources
{: #sat-virt-additional}

- [Running a Windows 2019 Server VM in IBM Cloud Satellite with OpenShift Virtualization](https://lisowski0925.medium.com/running-a-windows-2019-server-vm-in-ibm-cloud-satellite-with-openshift-virtualization-234aa9a01def){: external}.
- [Using the virtualization CLI tools](https://docs.redhat.com/en/documentation/openshift_container_platform/4.11/html/virtualization/virt-using-the-cli-tools){: external}
- [Creating VMs](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-create-vms.html){: external}
- [Creating a VM using the OpenShift Web Console](https://www.redhat.com/en/blog/creating-a-vm-using-the-openshift-web-console){: external}
- [Editing VMs](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-edit-vms.html){: external}
- [Deleting VMs](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-delete-vms.html){: external}
- [Accessing VM consoles](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-accessing-vm-consoles.html){: external}
- [OpenShift Virtualization Hands-on Lab](https://github.com/rdoxenham/openshift-virt-labs){: external}
