<hidden storage>---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-25"

keywords: spectrum scale, satellite storage, satellite config, satellite configurations, 

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


# Setting up IBM Spectrum Scale storage
{: #config-storage-spectrum-scale}

Set up [IBM Spectrum Scale](https://www.ibm.com/support/knowledgecenter/STXKQY_CSI_SHR/ibmspectrumscalecsi_welcome.html){: external} storage for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. After you create a storage configuration, you can assign it to your clusters or cluster groups. When you assign a storage configuration to your clusters, the storage drivers for the provider that you used to create your configuration are installed on your cluster.
{: shortdesc}


You can use the IBM Spectrum Scale Container Storage Interface (CSI) driver to create persistent storage for stateful applications running in Kubernetes clusters. You can use the IBM Spectrum Scale CSI driver to create persistent volumes (PVs) from IBM Spectrum Scale.

The following features are available with IBM Spectrum Scale Container Storage Interface driver:

- **Static provisioning**: You can use your existing directories and filesets as persistent volumes.
- **Lightweight dynamic provisioning**: You can dynamically create directory-based volumes.
- **Fileset-based dynamic provisioning**: You can dynamically create fileset-based volumes.
- **Multiple file systems**: You can create volumes across multiple file systems.
- **Remote volumes**: You can create volumes on a remotely mounted file system.
- **Operator deployment**: You can use the IBM Spectrum Scale operator for easier deployment, upgrade, and cleanup.
- **Multiple volume access modes** You can create volumes with ReadWriteMany (RWX) and ReadWriteOnce (RWO) access modes.

<br />

## Prerequisites
{: #sat-storage-spectrum-scale-prereq}

1. Before you can create a storage configuration, set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
1. [Set up your IBM Spectrum Scale worker nodes](#sat-storage-spectrum-scale-setup-workers).

### Setting up your IBM Spectrum Scale worker nodes
{: ##sat-storage-spectrum-scale-setup-workers}

  - **Setup IBM Spectrum Scale Nodes for IBM Cloud Satellite**
	  -  **Setup IBM Spectrum Scale ( as root )**
		  - Follow the instructions in https://www.ibm.com/support/knowledgecenter/STXKQY_5.1.0/com.ibm.spectrum.scale.v5r10.doc/bl1ins_linsoft.htmto make sure all required packages are installed
			  - For example:
		  ```yum install -y kernel-devel cpp gcc gcc-c++ binutils python3```
			- Follow instructions 1-3 ( **Do NOT create the cluster** ) https://www.ibm.com/support/knowledgecenter/STXKQY_5.1.0/com.ibm.spectrum.scale.v5r10.doc/bl1ins_manuallyinstallingonlinux_packages.htm

	- **Setup sudo user -as root**
		- adduser, passwd
		- Follow initial instructions in https://www.ibm.com/support/knowledgecenter/STXKQY_5.1.0/com.ibm.spectrum.scale.v5r10.doc/bl1adm_sudowrapper.htm
		up to but not including creating a Spectrum Scale cluster
		
	- **Switch to sudo user**
		- Using the sudowrapper.htm instructions create a Spectrum Scale cluster on the worker nodes
		- verify the cluster is using sudo wrappers
		- verify Spectrum Scale starts up on all worker nodes
			```sudo /usr/lpp/mmfs/bin/mmstartup -a```
			```sudo /usr/lpp/mmfs/bin/mmgetstate -a```
		- make Spectrum Scale autostart
		```sudo /usr/lpp/mmfs/bin/mmchconfig autoload=yes```

- **Attach IBM Spectrum Scale Nodes to IBM Cloud Satellite**
	- Make sure your system is configured for the desired default route if you have more than one clustering network
	- Make sure that default route has a path to the public network, possibly via NAT or VPN

- **Start IBM Spectrum Scale Nodes**
	-	Check Spectrum Scale to see if it is running -it is likely that the IBM Cloud Openshift clustering process removed the portability layer
		-	Rebuild the portability layer if needed on each affected worker. (You may need to replace some files and subdirectories)
		```sudo yum install -y kernel-devel cpp gcc gcc-c++ binutils python3
		
		sudo mkdir /usr/include/asm
		sudo cp /usr/src/kernels/3.10.0-1160.11.1.el7.x86_64/arch/x86/include/uapi/asm/*.h
		/usr/include/asm
		
		sudo mkdir /usr/include/asm-generic
		sudo cp /usr/src/kernels/3.10.0-1160.11.1.el7.x86_64/include/uapi/asm-generic/*.h
		/usr/include/asm-generic
		
		sudo mkdir /usr/include/linux
		sudo cp /usr/src/kernels/3.10.0-1160.15.2.el7.x86_64/include/uapi/linux/*.h /usr/include/linux```

	- Start Spectrum Scale and verify it runs on all nodes
		- Follow the instructions in https://www.ibm.com/support/knowledgecenter/STXKQY_5.1.0/com.ibm.spectrum.scale.v5r10.doc/bl1adv_admrmsec.htm mount your desired filesystem remotely
			- mount the remote filesystem on all nodes and verify the mount
			- Record the information from mmcluster on both the local and remote IBM Spectrum Scale cluster(s)
- **Initialize the IBM Spectrum Scale GUI**
https://www.ibm.com/support/knowledgecenter/STXKQY_CSI_SHR/com.ibm.spectrum.scale.csi.v2r10.doc/bl1csi_instal_prereq.html
  
- **Label the Kubernetes worker nodes** where IBM Spectrum Scale client is installed and where IBM Spectrum Scale Container Storage Interface driver runs:

```

	kubectl label node node1 scale=true --overwrite=true

```




<br />

## Creating an Spectrum Scale storage configuration in the command line
{: #sat-storage-spectrum-scale-cli}

1. Before you can create a storage configuration, [review the prerequisites](#sat-storage-spectrum-scale-prereq).
1. Review the [template parameters](#sat-storage-spectrum-scale-params-cli).
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name ess --template-version 1.1 -p "scale-host-path=<scale-host-path>" -p "cluster-id=<cluseter-id>" -p "primary-fs=<primary-fs>" -p "gui-host=<gui-host>" -p "secret-name=<secret-name>" -p "gui-api-user=<gui-api-user>" -p "gui-api-password=<gui-api-password>" -p "k8-n1-ip=<k8-n1-ip>" -p "sc-n1-host=<sc-n1-host>" -p "k8-n2-ip=<k8-n2-ip>" -p "sc-n2-host=<sc-n2-host>" -p "k8-n3-ip=<k8-n3-ip>" -p "sc-n3-host=<sc-n3-host>" -p "storage-class-name=<storage-class-name>" -p "vol-backend-fs=<vol-backend-fs>" -p "vol-dir-base-path=<vol-dir-base>"
  ```
  {: pre}

1. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-spectrum-scale).

<br />
### Spectrum Scale configuration parameter reference
{: #sat-storage-spectrum-scale-params-cli}


| Parameter | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `scale-host-path` | Required | The mount path of the primary file system. You can retrieve this value by running the `mmlsfs` command. | N/A |
| `cluster-id` | Required | The cluster ID of the primary IBM Spectrum Scale cluster. You can retrieve this value by running the `mmlscluster` command from a node within the primary cluster. | N/A | |
| `primary-fs` | Required | The primary file system name. You can retrieve this value by running the `mmlsfs` command. | N/A |
| `gui-host` | Required | The FQDN or IP address of the GUI node of the scale cluster that is specified against the id parameter. You can retrieve this value by running the `mmlscluster` command from a node within the primary cluster. | N/A |
| `secret-name` | Required | The name of secret that contains the `username` and `password` to connect to primary cluster GUI server.  This parameter is user-specified. | N/A |
| `gui-api-user` | Required | The username to connect to primary cluster GUI. To retrieve this value, log-in to the GUI node and run the `lsuser` command. The `gui-api-user` value is in the `CsiAdmin` group. | N/A |
| `gui-api-password` | Required | The password to connect to primary cluster GUI. This parameter is user-specified.| N/A |
| `k8-n1-ip` | Required | The IP address of Kubernetes Node#1 running IBM Spectrum Scale. You can retrieve this value by running the `oc get nodes` commamd. | N/A |
| `sc-n1-host` | Required | Hostname of IBM Spectrum Scale Node#1.  You can retrieve this value by running the `mmlscluster` command from a node within the primary cluster. | N/A |
| `k8-n2-ip` | Required | IP address of Kubernetes Node#2 running IBM Spectrum Scale.  You can retrieve this value by running the `oc get nodes` command. | N/A |
| `sc-n2-host` | Required | Hostname of IBM Spectrum Scale Node#2. You can retrieve this value by running the `mmlscluster` command from a node within the primary cluster. | N/A |
| `k8-n3-ip` | Required | IP address of Kubernetes Node#3 running IBM Spectrum Scale.  Run: `oc get nodes`| N/A |
| `sc-n3-host` | Required | Hostname of IBM Spectrum Scale Node#3.  You can retrieve this value by running the `mmlscluster` command from a node within the primary cluster. | N/A |
| `storage-class-name` | Required | Name of IBM Spectrum Scale Storage Class.  | N/A |
| `vol-backend-fs` | Required | The name of the file system on which the directory-based volume is created. | N/A |
| `vol-dir-base-path` | Required | The base path under which all volumes with this storage class is created. | N/A |
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}


<br />
## Assigning your Spectrum Scale storage configuration to a cluster
{: #assign-storage-spectrum-scale}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-spectrum-scale), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />



### Assigning a storage configuraton in the command line
{: #assign-storage-spectrum-scale-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

2. List your {{site.data.keyword.satelliteshort}} cluster groups and make a note of the group that you want to assign storage.
  ```sh
  ibmcloud sat group ls
  ```
  {: pre}

3. Assign storage to the clusters that you retrieved in step 2. Replace `<group>` with the name of your cluster group, `<configuration>` with the name of your storage configuration, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).
  ```sh
  ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
  ```
  {: pre}

4. Verify that your assignment is created.
  ```sh
  ibmcloud sat storage assignment ls | grep <storage-assignment-name>
  ```
  {: pre}
5. Verify that the storage configuration resources are deployed. For more information about the resources that are deployed with the configuration that you assigned to your clusters, review the `deployment.yaml` file for your [configuration template](https://github.com/IBM/ibm-satellite-storage/tree/master/config-templates){:external}.



<br />
## Storage class reference
{: #sat-storage-spectrum-scale-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for IBM Spectrum Scale. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the Spectrum Scale deployment in [GitHub](https://github.com/IBM/ibm-satellite-storage/tree/ess-latest/config-templates/ibm/ess/1.1).
{: shortdesc}


| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `ibm-spectrum-scale-csi-lt` | Light weight volumes | Delete  | 

## Verifying your IBM Spectrum Scale CSI Driver storage configuration is assigned to your clusters

To verify that your configuration is assigned to your cluster. Verify that the driver pods are running, and list the Satellite storage classes that are installed.


**Example output**


## Troubleshooting
{: #sat-storage-spectrum-scale-ts}
Review the following troubleshooting topics.
### Changing the mount of you IBM Specturm Scale storage
{: #sat-storage-spectrum-scale-ts-mount}

1. Edit the daemonset.
    ```sh
    oc edit ds ibm-spectrum-scale-csi -n ibm-spectrum-scale-csi-driver
    ```
    {: pre}

2. Search for the `/var\/lib\/kubelet$` string and replace it with `/var/data/kubelet`
    
    **Example mount path with updated `/var\/lib\/kubelet`:
    ```yaml
    volumeMounts:
    ....
    -mountPath: /var/data/kubelet
        name: pods-mount-dir
    ....
    ```
    {: screen}

3. Save and exit the daemonset to reapply it.

### Mapping IBM Spectrum Scale hosts to worker node names
{: #sat-storage-spectrum-scale-ts-mapping}
In some environments, Kubernetes node names might be different from the IBM Spectrum Scale node names. This results in failure during mounting of pods. Kubernetes node to IBM Spectrum Scale node mapping must be configured to address this condition during the Operator configuration. For more information, see [Kubernetes to IBM Spectrum Scale node mapping](https://www.ibm.com/support/knowledgecenter/STXKQY_CSI_SHR/com.ibm.spectrum.scale.csi.v2r10.doc/bl1csi_config_kubernet_SS_mapping_procedure_final.html)

## Storage class reference
{: #sat-storage-spectrum-scale-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for IBM Spectrum Scale. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the Spectrum Scale deployment in [GitHub](https://github.com/IBM/ibm-satellite-storage/tree/ess-latest/config-templates/ibm/ess/1.1).
{: shortdesc}


| Storage class name | Type | Reclaim policy |
| --- | --- | --- |
| `ibm-spectrum-scale-csi-lt` | Light weight volumes | Delete  | 

## Additional references
{: #sat-storage-spectrum-scale-ref}

- [IBM Spectrum Scale CSI Driver documentation](https://www.ibm.com/support/knowledgecenter/STXKQY_CSI_SHR/ibmspectrumscalecsi_welcome.html)
- [Cleaning up your Spectrum Scale deployment](https://www.ibm.com/support/knowledgecenter/STXKQY_CSI_SHR/com.ibm.spectrum.scale.csi.v2r10.doc/bl1csi_operator.html)





<hidden storage>
