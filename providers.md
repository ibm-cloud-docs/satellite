---

copyright:
  years: 2020, 2020
lastupdated: "2020-10-19"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
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

### DNS for location control plane
{: #aws-reqs-dns-control-plane}

If you use AWS hosts for your [{{site.data.keyword.satellitelong_notm}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane), the location subdomain is not automatically registered for you. Instead, you must manually set up the DNS entries.
{: shortdesc}

1.  [Add your hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
2.  Wait for the hosts to provision as worker nodes in the control plane cluster. You see a message similar to the following for the location.
    ```
    R0014 Verify that the Satellite location has a DNS record for load balancing requests to the location control plane.
    ```
    {: screen}
3.  Get the public floating IP addresses for your hosts from the AWS provider.
4.  Register the floating IP addresses for the location DNS. Include the `--ip` flag for each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_ID> --ip <aws_host_floating_IP>
    ```
    {: pre}

### DNS for cluster load balancing
{: #aws-reqs-dns-cluster-nlb}

By default, the private IP addresses for your AWS hosts are used for the DNS registration of {{site.data.keyword.openshiftshort}} clusters that you create in your {{site.data.keyword.satelliteshort}} location. However, you must register the public IP addresses of the hosts instead.
{: shortdesc}

1.  [Attach AWS hosts](/docs/satellite?topic=satellite-hosts#attach-hosts) to your {{site.data.keyword.satellitelong_notm}} location.
2.  [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) in your {{site.data.keyword.satellitelong_notm}} location.
3.  [Assign your AWS hosts to your {{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign-cli).
4.  Get the **Hostname** and private **IP** addresses of the cluster's default Ingress subdomain.
    ```
    ibmcloud oc nlb-dns ls -c <cluster_name_or_ID>
    ```
    {: pre}
4.  In your AWS portal, find the host with the matching private IP address that you retrieved and note the public IP address. Repeat this step for each private IP address that you retrieved.
5.  Register the public IP address of your AWS host worker nodes for the cluster's Ingress subdomain.
    ```
    ibmcloud oc nlb-dns add -c <cluster_name_or_ID> --nlb-host <cluster_hostname> --ip <aws_public_IP>
    ```
    {: pre}
6.  Remove the private IP addresses from the cluster's Ingress subdomain.
    ```
    ibmcloud oc nlb-dns rm classic -c <cluster_name_or_ID> --nlb-host <cluster_hostname> --ip <aws_private_IP>
    ```
    {: pre}

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

### DNS for location control plane
{: #gcp-reqs-dns-control-plane}

If you use GCP hosts for your [{{site.data.keyword.satellitelong_notm}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane), the location subdomain is not automatically registered for you. Instead, you must manually set up the DNS entries.
{: shortdesc}

1.  [Attach your hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
2.  Wait for the hosts to provision as worker nodes in the control plane cluster. You see a message similar to the following for the location.
    ```
    R0014 Verify that the Satellite location has a DNS record for load balancing requests to the location control plane.
    ```
    {: screen}
3.  Get the public floating IP addresses for your hosts from the GCP provider.
4.  Register the floating IP addresses for the location DNS. Include the `--ip` flag for each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_ID> --ip <gcp_host_floating_IP>
    ```
    {: pre}

### DNS for cluster load balancing
{: #gcp-reqs-dns-cluster-nlb}

By default, the private IP addresses for your GCP hosts are used for the DNS registration of {{site.data.keyword.openshiftshort}} clusters that you create in your {{site.data.keyword.satelliteshort}} location. However, you must register the public IP addresses of the hosts instead.
{: shortdesc}

1.  [Attach GCP hosts](/docs/satellite?topic=satellite-hosts#attach-hosts) to your {{site.data.keyword.satellitelong_notm}} location.
2.  [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) in your {{site.data.keyword.satellitelong_notm}} location.
3.  [Assign your GCP hosts to your {{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign-cli).
4.  Get the **Hostname** and private **IP** addresses of the cluster's default Ingress subdomain.
    ```
    ibmcloud oc nlb-dns ls -c <cluster_name_or_ID>
    ```
    {: pre}
4.  In your GCP portal, find the host with the matching private IP address that you retrieved and note the public IP address. Repeat this step for each private IP address that you retrieved.
5.  Register the public IP address of your GCP host worker nodes for the cluster's Ingress subdomain.
    ```
    ibmcloud oc nlb-dns add -c <cluster_name_or_ID> --nlb-host <cluster_hostname> --ip <gcp_public_IP>
    ```
    {: pre}
6.  Remove the private IP addresses from the cluster's Ingress subdomain.
    ```
    ibmcloud oc nlb-dns rm classic -c <cluster_name_or_ID> --nlb-host <cluster_hostname> --ip <gcp_private_IP>
    ```
    {: pre}

### MTU limitation for location control plane
{: #gcp-reqs-control-plane}

If you use GCP hosts for your [{{site.data.keyword.satellitelong_notm}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane), you must request modified maximum transmission unit (MTU) settings. [Open a support case](/docs/satellite?topic=satellite-get-help#help-support).
{: shortdesc}

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

<br />


## {{site.data.keyword.cloud_notm}} infrastructure
{: #ibm-cloud-reqs}

Review the following host requirements that are specific to hosts that are in {{site.data.keyword.cloud_notm}} infrastructure.
{: shortdesc}

Your hosts also must meet the general requirements that are common across providers, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

### RHEL package updates
{: #ibm-cloud-reqs-rhel-packages}

To [attach {{site.data.keyword.cloud_notm}} infrastructure hosts to your {{site.data.keyword.satellitelong_notm}} location](/docs/satellite?topic=satellite-hosts#attach-hosts), you must update the RHEL packages on the host machines as in the following example steps.
{: shortdesc}

1.  Get a registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
    ```
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}
2. Retrieve the **public_ip** address and **id** of your machine.	
   ```	
   ibmcloud sl vs list	
   ```	
   {: pre}	
3. Retrieve the credentials to log in to your virtual machine.	
   ```	
   ibmcloud sl vs credentials <vm_ID>	
   ```	
   {: pre}	
4. Copy the script from your local machine to the virtual server instance.	
   ```	
   scp <path_to_attachHost.sh> root@<public_IP_address>:/tmp/attach.sh	
   ```	
   {: pre}	
5. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.	
   ```	
   ssh root@<public_IP_address>	
   ```	
   {: pre}	
6. Refresh the Red Hat packages on your machine.	
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
