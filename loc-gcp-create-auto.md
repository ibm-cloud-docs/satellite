---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-15"

keywords: satellite, hybrid, multicloud, gcp, google cloud platform

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Automating your GCP location setup with a {{site.data.keyword.bpshort}} template
{: #loc-gcp-create-auto}

Automate your GCP setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your GCP account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. Or, you can [manually attach GCP hosts to a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-gcp).
{: tip}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide [credentials](#infra-creds-gcp) to the cloud provider. The credentials that you provide are stored and encrypted in etcd of the {{site.data.keyword.satelliteshort}} location management plane. For more information, see [Securing your data](/docs/satellite?topic=satellite-data-security).

## Creating your location with a {{site.data.keyword.bpshort}} template
{: #create-auto-gcp}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

Do not reuse the same name for multiple locations, even after the other location is deleted. If you use the same name 5 times or more within 7 days, you might reach the Let's Encrypt [Duplicate Certificate rate limit](/docs/openshift?topic=openshift-cs_rate_limit).
{: note}

1. In your GCP cloud provider, [set up your account credentials](#infra-creds-gcp).
1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
1. In the **Get started** section, click **GCP Quick Start**.
1. Upload your GCP credentials file.
1. Review the **GCP environment** details that are automatically populated. By default, enough VMs are created to provide hosts for 1 small location that can run about 2 demo clusters. To change the subscription, region, instance type, or number of VMs for the hosts, click the **Edit** pencil icon.
1. Review the **Satellite location** details. If you edited the GCP environment details, you might want to click the **Edit** pencil icon to change details such as the description, API key, or {{site.data.keyword.cloud_notm}} multizone region that the location is managed from.
1. In the **Summary** pane, review the cost estimate.
1. Click **Create location**. Your location might take about 30 minutes to finish provisioning.
1. Optional: To review the provisioning progress, review the logs in the {{site.data.keyword.bpshort}} workspace that is automatically created for you.
    1. Click **Manage in Schematics**. If you see an error, navigate to the [{{site.data.keyword.bpshort}} workspaces console](https://cloud.ibm.com/schematics/workspaces){: external} and click the name of your workspace, such as `us.east.cartOrder...`.
    1. From the **Activity** tab, find the current activity row and click **View log** to review the log details.
    1. Wait for the {{site.data.keyword.bpshort}} action to finish and the workspace to enter an **Active** state.
1. Optional: If you need to setup SSH access to your hosts in GCP, see [Choose your access method](https://cloud.google.com/compute/docs/instances/access-overview){: external} in the Google documentation. GCP recommends using the OS Login technology. You can also use the `gcloud` CLI or the built-in SSH client in the web UI to access your VMs.

The GCP VPC created by the Quick Start template does not have port 22 open externally for SSH and so you might need to add a firewall rule before you can use SSH. If you add a firewall rule to open port 22 externally, you must remove this firewall rule before you specify the **Destroy resources** option in {{site.data.keyword.bpshort}} to clean up your location.
{: note}
     

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.

## What does this template create?
{: #template-gcp}

The following resources are created by the template in the resource group of your GCP cloud subscription.

- 1 virtual network that spans the region.
- 1 network security group to meet the host networking requirements for {{site.data.keyword.satelliteshort}}.
- 1 virtual machine running RHEL 8 for each host that you specified, spread evenly across the region. By default, 6 virtual machines are created.
- 1 network interface for each virtual machine.
- 1 disk for each virtual machine.

The following resources are created by the template in your {{site.data.keyword.cloud_notm}} account.

- 1 {{site.data.keyword.satelliteshort}} location.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the virtual machines in GCP, attached to the location and assigned to the {{site.data.keyword.satelliteshort}} location control plane.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the virtual machines in GCP, attached to the location, unassigned, and available to use for services, such as a {{site.data.keyword.redhat_openshift_notm}} cluster. If you added more than 6 hosts, the additional hosts are unassigned and available for use in the control plane or by services. 

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-location-host).
{: note}

## Google Cloud Platform credentials
{: #infra-creds-gcp}

Retrieve the Google Cloud Platform (GCP) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your GCP cloud on your behalf.
{: shortdesc}

1. [Create a service account and service account key](https://cloud.google.com/docs/authentication/client-libraries#creating_a_service_account){: external} with at least the required [GCP permissions](/docs/satellite?topic=satellite-iam-common#permissions-gcp). As part of creating the service account, a JSON key file is downloaded to your local machine.
2. Open the JSON key file on your local machine, and verify that the format matches the following example. You can provide this JSON key file as your GCP credentials for actions such as creating a {{site.data.keyword.satelliteshort}} location.
    ```json
    {
        "type":"string",
        "project_id":"string",
        "private_key_id": "string",
        "private_key": "string",
        "client_email": "string",
        "client_id": "string",
        "auth_uri": "string",
        "token_uri": "string",
        "auth_provider_x509_cert_url": "string",
        "client_x509_cert_url": "string"
    }
    ```
    {: screen}

## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #gcpauto-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.
