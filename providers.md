---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-20"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# Provider requirements
{: #providers}

Review the following requirements that are specific to providers when you add hosts to your {{site.data.keyword.satellitelong}} location. For general requirements that are common across providers, see [Host requirements](/docs/satellite?topic=satellite-limitations#limits-host).
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and subject to change. To register for the beta, see [{{site.data.keyword.satellitelong_notm}} beta](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

Provider requirements are subject to change. You are responsible for your infrastructure in your provider environment.
{: note}

## Amazon Web Services
{: #aws-reqs}

Review the following host requirements that are specific to hosts that are in the Amazon Web Services (AWS) cloud.
{: shortdesc}

### DNS for location control plane
{: #aws-reqs-dns-control-plane}

If you use AWS hosts for your [{{site.data.keyword.satellitelong_notm}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane), the location subdomain is not automatically registered for you. Instead, you must manually set up the DNS entries.
{: shortdesc}

1.  [Add your hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
2.  Get the public floating IP addresses for your hosts from the AWS provider.
3.  Register the floating IP addresses for the location DNS. Include the `--ip` flag for each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_ID> --ip <aws_host_floating_IP>
    ```
    {: pre}

### DNS for cluster load balancing
{: #aws-reqs-dns-cluster-nlb}

By default, the private IP addresses for your AWS hosts are used for the DNS registration of {{site.data.keyword.openshift_short}} clusters that you create in your {{site.data.keyword.satellite_short}} location. However, you must register the public IP addresses of the hosts instead.
{: shortdesc}

1.  [Add AWS hosts](/docs/satellite?topic=satellite-hosts#add-hosts) to your {{site.data.keyword.satellitelong_notm}} location.
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

As described in the [host networking requirements](/docs/satellite?topic=satellite-limitations#limits-host-network), your AWS hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. If you use hosts in a virtual private cloud (VPC), you can create a security group similar to the following example. You can get the owner, group, user, and VPC IDs from your AWS provider resources.
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

### Example launch template
{: #aws-reqs-launch-template}

You can use [AWS launch templates](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html){: external} to add hosts to your {{site.data.keyword.satellitelong_notm}} location.
{: shortdesc}

1.  Get a registration script to add hosts to your {{site.data.keyword.satellitelong_notm}} location.
    ```
    ibmcloud sat host attach --location <location_name_or_ID>
    ```
    {: pre}
2.  Open the registration script. After the `API_URL` section, add a section to pull the required RHEL packages with the subscription manager.
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

<br />


## Google Cloud Platform
{: #gcp-reqs}

Review the following host requirements that are specific to hosts that are in the Google Cloud Platform (GCP) cloud.
{: shortdesc}

### DNS for location control plane
{: #gcp-reqs-dns-control-plane}

If you use GCP hosts for your [{{site.data.keyword.satellitelong_notm}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane), the location subdomain is not automatically registered for you. Instead, you must manually set up the DNS entries.
{: shortdesc}

1.  [Add your hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
2.  Get the public floating IP addresses for your hosts from the GCP provider.
3.  Register the floating IP addresses for the location DNS. Include the `--ip` flag for each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_ID> --ip <gcp_host_floating_IP>
    ```
    {: pre}

### DNS for cluster load balancing
{: #gcp-reqs-dns-cluster-nlb}

By default, the private IP addresses for your AWS hosts are used for the DNS registration of {{site.data.keyword.openshift_short}} clusters that you create in your {{site.data.keyword.satellite_short}} location. However, you must register the public IP addresses of the hosts instead.
{: shortdesc}

1.  [Add AWS hosts](/docs/satellite?topic=satellite-hosts#add-hosts) to your {{site.data.keyword.satellitelong_notm}} location.
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

As described in the [host networking requirements](/docs/satellite?topic=satellite-limitations#limits-host-network), your GCP hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your firewall settings in GCP, similar to the following example.
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

### RHEL package updates
{: #ibm-cloud-reqs-rhel-packages}

To [add {{site.data.keyword.cloud_notm}} infrastructure hosts to your {{site.data.keyword.satellitelong_notm}} location](/docs/satellite?topic=satellite-hosts#add-hosts), you must update the RHEL packages on the host machines as in the following example steps.
{: shortdesc}

1.  Get a registration script to add hosts to your {{site.data.keyword.satellitelong_notm}} location.
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
   scp <path_to_addHost.sh> root@<public_IP_address>:/tmp/attach.sh	
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
