---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-25"

keywords: satellite, hybrid, multicloud, aws, amazon web services, satellite location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Attaching AWS hosts to {{site.data.keyword.satelliteshort}}
{: #aws}

Add Amazon Web Services (AWS) cloud hosts to {{site.data.keyword.satellitelong}}. Review the following host requirements that are specific to hosts that are in the Amazon Web Services cloud. For required access in AWS cloud, see [AWS permissions](/docs/satellite?topic=satellite-iam-common#permissions-aws).
{: shortdesc}

To attach Red Hat CoreOS (RHCOS) hosts, your location must be enabled for Red Hat CoreOS. For more information, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). Note that you can still attach Red Hat Enterprise Linux hosts to a location that is enabled for Red Hat CoreOS.

Before you begin, make sure that you create host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers.

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
{: tip}


## Adding AWS hosts to {{site.data.keyword.satelliteshort}}
{: #aws-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Amazon Web Services (AWS) cloud.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 or 8 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}


If you want to use Red Hat CoreOS (RHCOS) hosts in your location, provide your Red Hat CoreOS image file to your Amazon account. For more information, see [Importing a VM as an image using](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html){: external}. To find RHCOS images, see the list of [available images](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/). Note that you must use at least version 4.9.
{: important}


Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add AWS hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Enter any host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-host-autoassign-ov) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu`, `os`, and `memory` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which should be treated and protected as sensitive information.
3. **RHEL only** Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
    ```sh
    # Enable AWS RHEL package updates
    yum update -y
    yum-config-manager --enable '*'
    yum repolist all
    yum install container-selinux -y
    echo "repos enabled"
    ```
    {: codeblock}
    
4. From the [AWS EC2 dashboard](https://console.aws.amazon.com/ec2/v2/home){: external}, go to **Instances** > **Launch Templates**.
5. Click **Create Launch template** and enter the template details as follows.

    For an overview of available options that you can specify in your launch template, see the [AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html#create-launch-template-define-parameters){: external}
    {: tip}

    1. Enter a name for your launch template.
    2. In the **Amazon machine image (AMI)** section, make sure to select a supported Red Hat Enterprise Linux 7 or 8 operating system that you can find by entering the AMI ID. You can match AMI IDs and the proper Red Hat Enterprise Linux version by referring to the [Red Hat Enterprise Linux AMI Available on Amazon Web Services documentation](https://access.redhat.com/solutions/15356){: external}. If you are creating an Red Hat CoreOS host, you must provide the image to AWS. For more information, see [Importing a VM as an image using](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html){: external}.
    3. From the **Instance type** section, select one of the [supported AWS instance types](#aws-instance-types).
    4. From the **Key pair (login)** section, select the `.pem` key that you want to use to log in to your machines later. If you do not have a `.pem` key, create one.
    5. In the **Network settings**, select **Virtual Private Cloud (VPC)** and an existing subnet and a security group that allows network traffic as defined in [Security group settings](#aws-reqs-secgroup). If you do not have a subnet or security group that you want to use, create one.
    6. In the **Storage (volumes)** section, expand the default root volume and update the size of the boot volume to a minimum of 100 GB. Add a second disk with at least 100 GB capacity. For more information about storage requirements, see [Host storage and attached devices](/docs/satellite?topic=satellite-reqs-host-storage).
    7. Expand the **Advanced details** and go to the **User Data** field.
    8. Enter the host registration script that you modified earlier. If you are adding an RHCOS host, add the ignition script.
    9. Click **Create launch template**.
6. From the **Launch Templates** dashboard, find the template that you created.
7. From the **Actions** menu, select **Launch instance from template**.
8. Enter the number of instances that you want to create and click **Launch instance from template**.
9. Wait for the instance to launch. During the launch of your instance, the registration script runs automatically. This process takes a few minutes to complete.
10. Monitor the progress of the registration script.
    1. From the EC2 **Instances** dashboard, retrieve the public IP address of your instance.
    2. Log in to your instance.
        ```sh
        ssh -i <key>.pem ec2-user@<public_IP_address>
        ```
        {: pre}

    3. Review the status of the registration script.
        ```sh
        journalctl -f -u ibm-host-attach
        ```
        {: pre}  

11. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.   
12. Assign your AWS hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts).

## Manually attaching AWS instances with the CLI
{: #aws-hosts-cli}

You can use the AWS ClI to run your EC2 instances and attach them to your {{site.data.keyword.satelliteshort}} location. For more information, see the `aws ec2 run-instances` [command reference](https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html){: external}.

Example command to run AWS EC2 instances.

```sh
aws ec2 run-instances --count COUNT --instance-type INSTANCE-TYPE --launch-template LaunchTemplateName=AWS-LAUNCH-TEMPLATE --user-data file://ATTACH-SCRIPT-LOCATION 
```
{: pre}

## AWS instance types
{: #aws-instance-types}

Review the following suggested [AWS EC2 instance types](https://aws.amazon.com/ec2/instance-types/){: external} that you can use as hosts in {{site.data.keyword.satellitelong_notm}}. You can use other AWS instance types as long as they meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}.
{: shortdesc}

| Instance | vCPU | Memory (GiB) | Storage disk (GiB) | Network bandwidth (Gbps) |
| -------- | ---- | ------------ | ------------------ | ------------------------ |
| `m5d.xlarge` | 4 | 16 | At least 100 GB SSD attached | Up to 10 |
| `m5d.2xlarge` | 8 | 32 | At least 100 GB SSD attached | Up to 10 |
| `m5d.4xlarge` | 16 | 64 | At least 100 GB SSD attached | Up to 10 |
{: caption="AWS instance types" caption-side="bottom"}


## Security group settings for AWS
{: #aws-reqs-secgroup}

As described in the [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network), your AWS hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. If you use hosts in a virtual private cloud (VPC), you can create a security group similar to the following example. You can get the owner, group, user, and VPC IDs from your AWS provider resources.
{: shortdesc}

The following example is a security group that you might create for AWS.

```json
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
{: codeblock}

For more information, see [Control traffic to resources using security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html){: external} in the AWS documentation.


	
## I added hosts to my location, what's next?
{: #aws-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.




