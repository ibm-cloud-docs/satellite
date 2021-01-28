---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-28"

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
{:swift-ios: .ph data-hd-programlang='iOS Swift'}
{:swift-server: .ph data-hd-programlang='server-side Swift'}
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

### RHEL package updates in example launch template
{: #aws-reqs-launch-template}

To [attach AWS hosts to your {{site.data.keyword.satellitelong_notm}} location](/docs/satellite?topic=satellite-hosts#attach-hosts), you must update the RHEL packages on the host machines. You can also use [AWS launch templates](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html){: external} to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
{: shortdesc}

1.  Get a registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
    ```
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}
2.  Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
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
3.  In the `UserData` section of your AWS launch template, add the registration script.
    ```
    ...
    "DisableApiTermination": false,
    "InstanceInitiatedShutdownBehavior": "stop",
    "UserData": "<registration_script>",
    "CapacityReservationSpecification": {
        "CapacityReservationPreference": "open"
    },
    ...
    ```
    {: screen}

### Security group
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

### VXLAN encapsulation for AWS hosts
{: #aws-reqs-vxlan}

When you use hosts from this provider to set up a {{site.data.keyword.satelliteshort}} location and create a {{site.data.keyword.openshiftlong_notm}} cluster in that location, the cluster's Calico network plug-in is created with IP in IP encapsulation. To access the {{site.data.keyword.openshiftshort}} web console for your cluster, you must change the IP in IP encapsulation protocol to VXLAN encapsulation instead. For more information, see step 8 in [Creating {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=openshift-satellite-clusters#satcluster-create-cli).
{: shortdesc}

<br />

## Google Cloud Platform
{: #gcp-reqs}

Review the following host requirements that are specific to hosts that are in the Google Cloud Platform (GCP) cloud.
{: shortdesc}

Your hosts also must meet the general requirements that are common across providers, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

### RHEL package updates
{: #gcp-reqs-rhel-packages}

To [attach GCP hosts to your {{site.data.keyword.satellitelong_notm}} location](/docs/satellite?topic=satellite-hosts#attach-hosts), you must update the RHEL packages on the host machines as in the following example steps.
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

    # Enable GCP RHEL package updates
    yum install subscription-manager -y
    ```
    {: codeblock}
3.  Continue with the [steps to copy and run the script on your hosts](/docs/satellite?topic=satellite-hosts#attach-hosts).

### Firewall settings
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

### VXLAN encapsulation for GCP hosts
{: #gcp-reqs-vxlan}

When you use hosts from this provider to set up a {{site.data.keyword.satelliteshort}} location and create a {{site.data.keyword.openshiftlong_notm}} cluster in that location, the cluster's Calico network plug-in is created with IP in IP encapsulation. To access the {{site.data.keyword.openshiftshort}} web console for your cluster, you must change the IP in IP encapsulation protocol to VXLAN encapsulation instead. For more information, see step 8 in [Creating {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=openshift-satellite-clusters#satcluster-create-cli).
{: shortdesc}

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


### VXLAN encapsulation for {{site.data.keyword.vpc_short}} hosts
{: #ibm-cloud-reqs-vxlan}

When you use hosts from this provider to set up a {{site.data.keyword.satelliteshort}} location and create a {{site.data.keyword.openshiftlong_notm}} cluster in that location, the cluster's Calico network plug-in is created with IP in IP encapsulation. To access the {{site.data.keyword.openshiftshort}} web console for your cluster, you must change the IP in IP encapsulation protocol to VXLAN encapsulation instead. For more information, see step 8 in [Creating {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=openshift-satellite-clusters#satcluster-create-cli).
{: shortdesc}
