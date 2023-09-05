---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-05"

keywords: satellite, hybrid, multicloud, aws, amazon web services, satellite location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Automating your AWS location setup with a {{site.data.keyword.bpshort}} template
{: #loc-aws-create-auto}

Automate your AWS setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your AWS account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. Or, you can [manually attach AWS hosts to a {{site.data.keyword.satelliteshort}} location](#aws-host-attach).
{: tip}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide [credentials](#infra-creds-aws) to the cloud provider. The credentials that you provide are stored and encrypted in etcd of the {{site.data.keyword.satelliteshort}} location control plane master. For more information, see [Securing your data](/docs/satellite?topic=satellite-data-security).

## Creating your location with a {{site.data.keyword.bpshort}} template
{: #create-auto-aws}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

Do not reuse the same name for multiple locations, even after the other location is deleted. If you use the same name 5 times or more within 7 days, you might reach the Let's Encrypt [Duplicate Certificate rate limit](/docs/openshift?topic=openshift-cs_rate_limit).
{: note}

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

## What does this template create?
{: #template-aws}

The following resources are created by the template in your AWS cloud account.

- 1 virtual private cloud (VPC).
- 1 subnet for each of the 3 zones in the region.
- 1 security group to meet the host networking requirements for {{site.data.keyword.satelliteshort}}.
- 6 RHEL 8 EC2 instances, spread evenly across zones, or the number of hosts that you specified.

The following resources are created by the template in your {{site.data.keyword.cloud_notm}} account.

- 1 {{site.data.keyword.satelliteshort}} location.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the EC2 instances in AWS, attached to the location and assigned to the {{site.data.keyword.satelliteshort}} location control plane.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the EC2 instances in AWS, attached to the location, unassigned, and available to use for services such as a {{site.data.keyword.redhat_openshift_notm}} cluster. If you added more than 6 hosts, If you added more than 6 hosts, the additional hosts are unassigned and available for use in the control plane or by services. 

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-location-host).
{: note}


## AWS credentials
{: #infra-creds-aws}

Retrieve the Amazon Web Services (AWS) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your AWS cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your AWS account](/docs/satellite?topic=satellite-iam-common#permissions-aws) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. [Create a separate IAM user that is scoped to EC2 access](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html){: external}.
3. [Retrieve the access key ID and secret access key credentials for the IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html#access-keys-and-secret-access-keys){: external}.
4. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. The `client_id` is the ID of the access key and the `client_secret` is the secret access key that you created for the IAM user in AWS.
    ```json
    {
        "client_id":"string",
        "client_secret": "string"
    }
    ```
    {: screen}
    
## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #awsauto-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.


