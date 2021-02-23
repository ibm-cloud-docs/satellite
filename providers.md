---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-23"

keywords: satellite, hybrid, multicloud

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



# Provider requirements
{: #providers}

Review the following requirements that are specific to providers when you attach hosts to your {{site.data.keyword.satellitelong}} location. For general requirements that are common across providers, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and subject to change. To register for the beta, see [{{site.data.keyword.satellitelong_notm}} beta](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

Provider requirements are subject to change. You are responsible for your infrastructure in your provider environment.
{: note}

## Amazon Web Services
{: #aws-reqs}

Review the following host requirements that are specific to hosts that are in the Amazon Web Services (AWS) cloud.
{: shortdesc}

Your hosts also must meet the general requirements that are common across providers, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

1. Retrieve the host registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
   ```
   ibmcloud sat host attach --location <location_name_or_ID>
   ```
   {: pre}
   
2. Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
   ```
   API_URL="https://containers.cloud.ibm.com/"
   # Enable AWS RHEL package updates
   yum update -y
   yum-config-manager --enable '*'
   yum repolist all
   yum install container-selinux -y
   echo "repos enabled"
   ```
   {: codeblock}
   
3. From the AWS EC2 dashboard, select **Instances** > **Launch Templates**. 
4. Click **Create Launch template** and enter the template details as follows. 
   1. Enter a name for your launch template. 
   2. In the **Amazon machine image (AMI)** section, make sure to select a supported Red Hat Enterprise Linux 7 operating system, such as `ami-0170fc126935d44c3`. 
   3. From the **Instance type** section, select one of the [supported AWS instance types](#aws-instance-types). 
   4. From the **Key pair (login)** section, select the pem key that you want to use to log in to your machines later. If you do not have a pem key, create one. 
   5. In the **Network settings**, select **Virtual Private Cloud (VPC)** and an existing subnet and a security group that allows network traffic as defined in [Security group settings](#aws-reqs-secgroup). If you do not have a subnet or security group that you want to use, create one. 
   6. In the **Storage (volumes)** section, expand the default root volume and update the size of the boot volume to a minimum of 100 GB. 
   7. Expand the **Advanced details** and go to the **User Data** field. 
   8. Enter the host registration script that you modified earlier. 
   9. Click **Create launch template**. 
   
5. From the **Launch Templates** dashboard, find the template that you created. 
6. From the **Actions** menu, select **Launch instance from template**. 
7. Enter the number of instances that you want to create and click **Launch instance from template**. 
8. Wait for the instance to launch. During the launch of your instance, the registration script runs automatically. This process takes a few minutes to complete. 
9. Monitor the progress of the registration script. 
   1. From the EC2 **Instances** dashboard, retrieve the public IP address of your instance. 
   2. Log in to your instance. 
      ```
      ssh -i nadine.pem ec2-user@<public_IP_address>
      ```
      {: pre}
      
   3. Review the status of the registration script. The 
      ```
      systemctl status ibm-host-attach.service
      ```
      {: pre}
      
10. Check that your hosts are shown in the **Hosts** tab of your {{site.data.keyword.satelliteshort}} location dashboard. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.
     
11. Assign your GCP hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign).
   

### Supported AWS instance types
{: #aws-instance-types}

Review the following [AWS EC2 instance types](https://aws.amazon.com/ec2/instance-types/){: external} that you can use as hosts in {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

| Instance | vCPU | Memory (GiB) | Storage disk (GiB) | Network bandwidth (Gbps) |
| -------- | ---- | ------------ | ------------------ | ------------------------ |
| m5d.xlarge | 4 | 16 | 1 x 150 NVMe SSD | Up to 10 |
| m5d.2xlarge | 8 | 32 | 1 x 300 NVMe SSD | Up to 10 |
| m5d.4xlarge | 16 | 64 | 2 x 300 NVMe SSD | Up to 10 |
{: caption="Supported AWS instance types" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the supported instance. The second column is the number of vCPUs. The third column is the memory in gibibytes (GiB). THe fourth column is the number of storage disks and their size in gibibytes (GiB). The fifth column is the network bandwidth in gigabits per second (Gbps)."}

### Security group settings
{: #aws-reqs-secgroup}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your AWS hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. If you use hosts in a virtual private cloud (VPC), you can create a security group similar to the following example. You can get the owner, group, user, and VPC IDs from your AWS provider resources.
{: shortdesc}

**Example security group for AWS**
```
{
	"Description": "Security group for IBM Cloud Satellite hosts",
	"GroupName": "Satellite",
	"IpPermissions": [{
			"FromPort": 80,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [],
			"PrefixListIds": [],
			"ToPort": 80,
			"UserIdGroupPairs": []
		},
		{
			"FromPort": 30000,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [{
				"CidrIpv6": "::/0"
			}],
			"PrefixListIds": [],
			"ToPort": 32767,
			"UserIdGroupPairs": []
		},
		{
			"IpProtocol": "-1",
			"IpRanges": [],
			"Ipv6Ranges": [],
			"PrefixListIds": [],
			"UserIdGroupPairs": [{
				"GroupId": "<group_ID>",
				"UserId": "<user_ID>"
			}]
		},
		{
			"FromPort": 22,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [],
			"PrefixListIds": [],
			"ToPort": 22,
			"UserIdGroupPairs": []
		},
		{
			"FromPort": 30000,
			"IpProtocol": "udp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [{
				"CidrIpv6": "::/0"
			}],
			"PrefixListIds": [],
			"ToPort": 32767,
			"UserIdGroupPairs": []
		},
		{
			"FromPort": 443,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [{
				"CidrIpv6": "::/0"
			}],
			"PrefixListIds": [],
			"ToPort": 443,
			"UserIdGroupPairs": []
		}
	],
	"OwnerId": "<owner_ID>",
	"GroupId": "<group_ID>",
	"IpPermissionsEgress": [{
		"IpProtocol": "-1",
		"IpRanges": [{
			"CidrIp": "0.0.0.0/0"
		}],
		"Ipv6Ranges": [],
		"PrefixListIds": [],
		"UserIdGroupPairs": []
	}],
	"VpcId": "<vpc_ID>"
}
```
{: screen}

### Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console
{: #aws-reqs-console-access}

The private IP addresses of your instances are automatically registered and added to your location's DNS record and the cluster's subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console. You must be connected to your hosts' private network, such as through VPN access, to [connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat_se). Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

<br />

## Google Cloud Platform
{: #gcp-reqs}

Review the following host requirements that are specific to hosts that are in the Google Cloud Platform (GCP) cloud.
{: shortdesc}

Your hosts also must meet the general requirements that are common across providers, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

1. Retrieve the host registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
   ```
   ibmcloud sat host attach --location <location_name_or_ID>
   ```
   {: pre}
   
2. Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
   ```
   ...
   API_URL="https://containers.cloud.ibm.com/"

   # Enable GCP RHEL package updates
   yum install subscription-manager -y
   ```
   {: codeblock}
   
3. From the Google Cloud Platform **Compute Engine** dashboard, select **Instance templates**. 
4. Click **Create instance template**. 
5. Enter the details for your instance template as follows: 
   1. Enter a name for your instance template. 
   2. In the **Machine configuration** section, select the **Series** and **Machine type** that you want to use. You can select any series that you want, but make sure that the machine type meets the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) for CPU and memmory. 
   3. In the **Boot disk** section, click **Change** to change the default operating system and boot disk size. Make sure to select Red Hat Enterprise Linux version 7 as your operating system and to change your boot disk size to a minimum of 100 GB. 
   4. Optional: If you want your machines to allow http and https traffic, select **Allow HTTP traffic** and **Allow HTTPS traffic** from the **Firewall** section of your instance template. 
   5. Click **Management, security, disks, networking, sole tenancy** to view additional networking and security settings. 
   6. In the **Management** tab, locate the **Startup script** field and enter the registration script that you modified earlier. 
   7. In the **Networking** tab, choose the network that you want your instances to be connected to. This network must allow access to {{site.data.keyword.satellitelong_notm}} as described in [Firewall settings](#gcp-reqs-firewall).
   8. Click **Create** to save your instance template. 
6. Optional: Update the firewall settings for the network that you assigned to your instance template. 
   1. From the Google Cloud Platform **VPC Network** dashboard, select **Firewall**. 
   2. Verify that your network allows access as describde in the [Network firewall settings](#gcp-reqs-firewall). Make changes as necessary. 
7. From the Google Cloud Platform **Compute Engine** dashboard, select **Instance templates** and find the instance template that you created. 
8. From the actions menu, click **Create VM** to create an instance from your template. You can alternatively create an instance group to add multiple instances at the same time. Make sure that you spread your instances across multiple zones for higher availability. 
9. Wait for the instance to create. During the creation of your instance, the registration script runs automatically. This process takes a few minutes to complete. You can monitor the progress of the script by reviewing the logs for your instance. Check that your hosts are shown in the **Hosts** tab of your {{site.data.keyword.satelliteshort}} location dashboard. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.openshiftlong_notm}} cluster. 
10. Assign your GCP hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign).

### Network firewall settings
{: #gcp-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your GCP hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your firewall settings in GCP, similar to the following example.
{: shortdesc}

**Example firewall settings**
```
Network default

Priority 1000

Direction Ingress

Action on match Allow

Targets
Target tags
satellite

Source filters
IP ranges
0.0.0.0/0

Protocols and ports
tcp:80
tcp:443
tcp:30000-32767
udp:30000-32767
```
{: screen}

### Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console
{: #gcp-reqs-console-access}

The private IP addresses of your instances are automatically registered and added to your location's DNS record and the cluster's subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console. You must be connected to your hosts' private network, such as through VPN access, to [connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat_se). Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

<br />

## Microsoft Azure
{: #azure-reqs}

Review the following host requirements that are specific to hosts that are in the Microsoft Azure cloud.
{: shortdesc}

Your hosts also must meet the general requirements that are common across providers, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

### RHEL package updates
{: #azure-reqs-rhel-packages}

To [attach Azure hosts to your {{site.data.keyword.satellitelong_notm}} location](/docs/satellite?topic=satellite-hosts#attach-hosts), you must update the RHEL packages on the host machines as in the following example steps.
{: shortdesc}

1.  Get a registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
    ```
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}
2.  Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
    ```
    ...
    API_URL="https://containers.cloud.ibm.com/"

    # Enable Azure RHEL package updates
    yum install subscription-manager -y
    ```
    {: codeblock}
3.  Continue with the [steps to copy and run the script on your hosts](/docs/satellite?topic=satellite-hosts#attach-hosts).

### Firewall settings
{: #azure-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your Azure hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your firewall settings in Azure, similar to the following example.
{: shortdesc}

**Example firewall settings**
```
Network default

Priority 1000

Direction Ingress

Action on match Allow

Targets
Target tags
satellite

Source filters
IP ranges
0.0.0.0/0

Protocols and ports
tcp:80
tcp:443
tcp:30000-32767
udp:30000-32767
```
{: screen}

### Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console
{: #azure-reqs-console-access}

The private IP addresses of your instances are automatically registered and added to your location's DNS record and the cluster's subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console. You must be connected to your hosts' private network, such as through VPN access, to [connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat_se). Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

<br />

## {{site.data.keyword.cloud_notm}} infrastructure
{: #ibm-cloud-reqs}

Review the following host requirements that are specific to hosts that are in {{site.data.keyword.cloud_notm}} classic or VPC infrastructure.
{: shortdesc}

Your hosts also must meet the general requirements that are common across providers, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Using {{site.data.keyword.cloud_notm}} infrastructure hosts in {{site.data.keyword.satelliteshort}} is supported only for testing or proof of concept purposes. For production workloads in your {{site.data.keyword.satelliteshort}} location, use on-premises, edge, or other cloud provider hosts. You can also create {{site.data.keyword.openshiftlong_notm}} clusters and add them to a {{site.data.keyword.satelliteshort}} Config cluster group to deploy the same app across your {{site.data.keyword.satelliteshort}} and {{site.data.keyword.cloud_notm}} clusters.
{: important}

### RHEL package updates
{: #ibm-cloud-reqs-rhel-packages}

To [attach {{site.data.keyword.cloud_notm}} infrastructure hosts to your {{site.data.keyword.satellitelong_notm}} location](/docs/satellite?topic=satellite-hosts#attach-hosts), you must update the RHEL packages on the host machines as in the following example steps.
{: shortdesc}

1. Get a registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
   ```
   ibmcloud sat host attach --location <location_name_or_ID>
   ```
   {: pre}
2. Retrieve the IP address and ID of your machine.
    * Classic:
       ```
       ibmcloud sl vs list
       ```
       {: pre}
    * VPC:
       ```
       ibmcloud is instances
       ```
       {: pre}

3. Retrieve the credentials to log in to your virtual machine.
    * Classic:
      ```
      ibmcloud sl vs credentials <vm_ID>
      ```
      {: pre}
    * VPC:
       ```
       ibmcloud is instance-initialization-values <instance_ID>
       ```
       {: pre}

4. Copy the script from your local machine to the virtual server instance.
   ```
   scp <path_to_attachHost.sh> root@<ip_address>:/tmp/attach.sh
   ```
   {: pre}
5. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.
   ```
   ssh root@<ip_address>
   ```
   {: pre}
6. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
   ```
   subscription-manager refresh
   ```
   {: pre}
   ```
   subscription-manager repos --enable=*
   ```
   {: pre}
7. Run the script on your machine.
   ```
   nohup bash /tmp/attach.sh &
   ```
   {: pre}
8. Exit the SSH session.  	
   ```
   exit
   ```
   {: pre}
