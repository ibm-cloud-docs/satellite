---

copyright:
  years: 2023, 2023
lastupdated: "2023-12-06"

keywords: satellite, hybrid, multicloud, vmware, vmware host, satellite location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Automating your VMware location setup with a {{site.data.keyword.bpshort}} template
{: #loc-vmware-create-auto}

Automate your VMware Virtual Cloud Director setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your VMware vCloud Director (VCD) account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}


You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. 
{: tip}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide [credentials](#infra-creds-vmware) to the cloud provider. The credentials that you provide are stored and encrypted in etcd of the {{site.data.keyword.satelliteshort}} location management plane. For more information, see [Securing your data](/docs/satellite?topic=satellite-data-security).
  
## Before you begin
{: #vmware-bybegin}
  
Before you can run the template, you must gather information about your VMware Virtual Data Centers (VDC) instance.

### General information
{: #vmware-general}
  
To connect {{site.data.keyword.satelliteshort}} to your VMware Virtual Data Centers (VDC), you need the following values. To find these values, see [VMware credentials](#infra-creds-vmware).
  
| Value | Description | Example |
|--------|-----------------------------|--------------|
| VCD user ID | The VMware Cloud Director username. | `admin` |
| VCD password | The VMware Cloud Director password. | `<password>`|
| VCD org name | The VMware organization name. |` 0ff080abcdef123456789abcd12345678` |
| VCD URL | The VMware Cloud Director URL | `https://daldir01.vmware-solutions.cloud.ibm.com/api` |
| VCD name | The VMware Cloud Director virtual data center name | `vmware-satellite` |
{: caption="VMware values for {{site.data.keyword.satelliteshort}}" caption-side="bottom"}

### Networking information
{: #vmware-network}

Your VMware data center must include a [routed network](https://docs.vmware.com/en/VMware-Cloud-Director/10.4/VMware-Cloud-Director-Tenant-Portal-Guide/GUID-74C4D27F-9E2A-4EB2-BBE1-CDD45C80E270.html){: external}. You can also include an edge gateway that is configured and enabled with [distributed routing](https://docs.vmware.com/en/VMware-Cloud-Director/10.4/VMware-Cloud-Director-Service-Provider-Admin-Portal-Guide/GUID-19F4DF76-209C-4CFA-8EE7-E5264E170B5D.html){: external} and [DHCP](https://docs.vmware.com/en/VMware-Cloud-Director/10.5/VMware-Cloud-Director-Tenant-Guide/GUID-24AB3A28-F4F9-465A-8439-B2BDA1B23A1E.html){: external}. If you include the edge gateway name, then firewall rules are created for full outbound connectivity and a SNAT rule is created for mapping to an external IP address.
  
Include the following values when you run the template.
  
| Name | Description | Example |
|--------------|----------------------------|--------------|
| VCD DHCP network name | The name of the network pre-dconfigured for the environment. | `my-network` |
| VCD edge gateway name | The name of the edge network configured in the environment. This value is optional. | `edge-dal10-12345678` | 
{: caption="VMware network environment values" caption-side="bottom"} 
  
 
## Creating your location with an {{site.data.keyword.bpshort}} template
{: #create-auto-vmware}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

Do not reuse the same name for multiple locations, even after the other location is deleted. If you use the same name 5 times or more within 7 days, you might reach the Let's Encrypt [Duplicate Certificate rate limit](/docs/openshift?topic=openshift-cs_rate_limit).
{: note}

1. Gather all the [required values](#vmware-bybegin).
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
3. In the **Setup** section, click **VMware Cloud Director**.
4. Enter the values that you previously collected.
5. Review the **Satellite location** details.
6. In the **Summary** pane, review the cost estimate.
7. Click **Create location**. Your location might take about 30 minutes to finish provisioning.
8. Optional: To review the provisioning progress, review the logs in the {{site.data.keyword.bpshort}} workspace that is automatically created for you.
    1. Click **Manage in Schematics**. If you see an error, navigate to the [{{site.data.keyword.bpshort}} workspaces console](https://cloud.ibm.com/schematics/workspaces){: external} and click the name of your workspace, such as `us.east.cartOrder...`.
    2. From the **Activity** tab, find the current activity row and click **View log** to review the log details.
    3. Wait for the {{site.data.keyword.bpshort}} action to finish and the workspace to enter an **Active** state.

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.

## What does this template create?
{: #template-vmware}

The following resources are created by the template in your VMware data center.

- Control plane virtual machines (8 CPU, 32GB RAM, 100GB primary disk)
- Worker virtual machines (4 CPU, 16GB RAM, 25GB primary disk, 100GB secondary disk)
- Storage virtual machines (16 CPU, 64GB RAM, 25GB primary disk, 100GB secondary disk, 500GB tertiary disk). The specs for the storage VMs are configurable in the Terraform variables.

The following resources are created by the template in your {{site.data.keyword.cloud_notm}} account.

- 1 {{site.data.keyword.satelliteshort}} location.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the VMware virtual machines, attached to the location, and assigned to the {{site.data.keyword.satelliteshort}} location control plane.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the VMware virtual machines, attached to the location, unassigned, and available to use for services.  

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-location-host).
{: note}


## VMware credentials
{: #infra-creds-vmware}

Retrieve the VMWare credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your VMWare cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your VMWare account](/docs/satellite?topic=satellite-iam-common#permissions-vmware) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. Identify or [create a user](https://docs.vmware.com/en/VMware-Cloud-Director/10.4/VMware-Cloud-Director-Tenant-Portal-Guide/GUID-1CACBB2E-FE35-4662-A08D-D2BCB174A43C.html){: external} with **Administrator** role.
3. Find your [network information](/docs/satellite?topic=satellite-loc-vmware-create-auto#vmware-network).
4. Provide this information on the [VMware Cloud Director template](/docs/satellite?topic=satellite-loc-vmware-create-auto#create-auto-vmware).

    
## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #vmwareauto-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.








