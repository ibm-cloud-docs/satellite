---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud, aws, amazon web services, satellite location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Amazon Web Services (AWS)
{: #aws}

Add Amazon Web Services (AWS) cloud hosts to {{site.data.keyword.satellitelong}}. Review the following host requirements that are specific to hosts that are in the Amazon Web Services cloud. For required access in AWS cloud, see [AWS permissions](/docs/satellite?topic=satellite-iam#permissions-aws).
{: shortdesc}

If your hosts are running Red Hat CoreOS (RHCOS), you must [manually add](#aws-host-attach) them to your location.
{: note}


## Automating your AWS location setup with a {{site.data.keyword.bpshort}} template
{: #aws-template}

Automate your AWS setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your AWS account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. Or, you can [manually attach AWS hosts to a {{site.data.keyword.satelliteshort}} location](#aws-host-attach).
{: tip}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

1. In your AWS cloud provider, [set up your account credentials](#infra-creds-aws).
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
3. In the **Setup** section, click **Amazon Web Services**.
4. In the **AWS credentials** section, enter the **AWS access key ID** and **AWS secret access key** values that you previously created.
5. Click **Fetch options from AWS**.
6. Review the **Satellite location** details. If you edited the AWS EC2 instances, you might want to click the **Edit** pencil icon to change details such as the description, API key, or {{site.data.keyword.cloud_notm}} multizone region that the location is managed from.
7. In the **Summary** pane, review the cost estimate.
8. Click **Create location**. Your location might take about 30 minutes to finish provisioning.
9. Optional: To review the provisioning progress, review the logs in the {{site.data.keyword.bpshort}} workspace that is automatically created for you.
    1. Click **Manage in Schematics**. If you see an error, navigate to the [{{site.data.keyword.bpshort}} workspaces console](https://cloud.ibm.com/schematics/workspaces){: external} and click the name of your workspace, such as `us.east.cartOrder...`.
    2. From the **Activity** tab, find the current activity row and click **View log** to review the log details.
    3. Wait for the {{site.data.keyword.bpshort}} action to finish and the workspace to enter an **Active** state.

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.

The following resources are created by the template in your AWS cloud account.

- 1 virtual private cloud (VPC).
- 1 subnet for each of the 3 zones in the region.
- 1 security group to meet the host networking requirements for {{site.data.keyword.satelliteshort}}.
- 6 RHEL 7 EC2 instances spread evenly across zones, or the number of hosts that you specified.

The following resources are created by the template in your {{site.data.keyword.cloud_notm}} account.

- 1 {{site.data.keyword.satelliteshort}} location.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the EC2 instances in AWS, attached to the location and assigned to the {{site.data.keyword.satelliteshort}} location control plane.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the EC2 instances in AWS, attached to the location, unassigned, and available to use for services like a {{site.data.keyword.redhat_openshift_notm}} cluster. If you added more than 6 hosts, the number of hosts equals the number that you specified minus the 3 that are assigned to the control plane.

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about-locations).
{: note}

What's next?

The {{site.data.keyword.bpshort}} template helped with the initial creation, but you are in control for subsequent location management actions, such as [attaching more hosts](/docs/satellite?topic=satellite-attach-hosts), [creating {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters), or [scaling the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-location-sizing). If you [remove](/docs/satellite?topic=satellite-host-remove#location-remove-console) your {{site.data.keyword.satelliteshort}} location, make sure to [remove your workspace in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-workspace-setup#del-workspace), too.


## Adding AWS hosts to {{site.data.keyword.satelliteshort}}
{: #aws-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Amazon Web Services (AWS) cloud.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}


If you want to use Red Hat CoreOS (RHCOS) hosts in your location, provide your Red Hat CoreOS image file to your Amazon account. For more information, see [Importing a VM as an image using](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html){: external}. To find RHCOS images, see the list of [available images](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/). Note that you must use at least version 4.9.
{: important}


Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add AWS hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Enter any host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov)) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu`, `os`, and `memory` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
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
    2. In the **Amazon machine image (AMI)** section, make sure to select a supported Red Hat Enterprise Linux 7 operating system, such as RHEL 7.7 that you can find by entering the AMI ID `ami-030e754805234517e`. If you are creating an Red Hat CoreOS host, you must provide the image to AWS. For more information, see [Importing a VM as an image using](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html){: external}.
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
12. Assign your AWS hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).

## Manually running AWS instances with the CLI
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
{: caption="AWS instance types" caption-side="top"}


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

For more information, see [Control traffic to resources using security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html){: external} in the AWS documentation.

## AWS credentials
{: #infra-creds-aws}

Retrieve the Amazon Web Services (AWS) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your AWS cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your AWS account](/docs/satellite?topic=satellite-iam#permissions-aws) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. [Create a separate IAM user that is scoped to EC2 access](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html)){: external}.
3. [Retrieve the access key ID and secret access key credentials for the IAM user](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: external}.
4. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. The `client_id` is the ID of the access key and the `client_secret` is the secret access key that you created for the IAM user in AWS.
    ```json
    {
        "client_id":"string",
        "client_secret": "string"
    }
    ```
    {: screen}


## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #aws-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.




