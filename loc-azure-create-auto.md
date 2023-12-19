---

copyright:
  years: 2020, 2023
lastupdated: "2023-12-19"

keywords: satellite, hybrid, multicloud, microsoft azure, azure, azure host

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Automating your Azure location setup with a {{site.data.keyword.bpshort}} template
{: #loc-azure-create-auto}

Automate your Azure setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your Azure account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. Or, you can [manually attach Azure hosts](/docs/satellite?topic=satellite-azure) to a {{site.data.keyword.satelliteshort}} location. 
{: tip}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide [credentials](#infra-creds-azure) to the cloud provider. The credentials that you provide are stored and encrypted in etcd of the {{site.data.keyword.satelliteshort}} location management plane. For more information, see [Securing your data](/docs/satellite?topic=satellite-data-security).

## Creating your location with a {{site.data.keyword.bpshort}} template
{: #create-auto-azure}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

Do not reuse the same name for multiple locations, even after the other location is deleted. If you use the same name 5 times or more within 7 days, you might reach the Let's Encrypt [Duplicate Certificate rate limit](/docs/openshift?topic=openshift-cs_rate_limit).
{: note}

1. In your Azure cloud provider, [set up your account credentials](#infra-creds-azure).
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
3. In the **Setup** section, click **Azure**.
4. In the **Azure credentials** section, enter the **Azure client ID (app ID)**, **Azure tenant ID**, and **Azure secret key (password)** values that you previously created for the service principal.
5. Click **Fetch options from Azure**.
6. Review the **Azure environment** details that are automatically populated. By default, enough VMs are created to provide hosts for 1 small location that can run about 2 demo clusters. To change the subscription, region, instance type, or number of VMs for the hosts, click the **Edit** pencil icon.
7. Review the **Satellite location** details. If you edited the Azure environment details, you might want to click the **Edit** pencil icon to change details such as the description, API key, or {{site.data.keyword.cloud_notm}} multizone region that the location is managed from.
8. In the **Summary** pane, review the cost estimate.
9. Click **Create location**. Your location might take about 30 minutes to finish provisioning.
10. Optional: To review the provisioning progress, review the logs in the {{site.data.keyword.bpshort}} workspace that is automatically created for you.
    1. Click **Manage in Schematics**. If you see an error, navigate to the [{{site.data.keyword.bpshort}} workspaces console](https://cloud.ibm.com/schematics/workspaces){: external} and click the name of your workspace, such as `us.east.cartOrder...`.
    2. From the **Activity** tab, find the current activity row and click **View log** to review the log details.
    3. Wait for the {{site.data.keyword.bpshort}} action to finish and the workspace to enter an **Active** state.

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-location-host).
{: note}

## What does this template create?
{: #template-azure}

This {{site.data.keyword.bpshort}} template creates a resource group of your Azure cloud subscription and then creates the following resources in your Azure account.

- 1 virtual network that spans the region.
- 1 network security group to meet the host networking requirements for {{site.data.keyword.satelliteshort}}.
- 1 virtual machine running RHEL 8 for each host that you specified, spread evenly across the region. By default, 6 virtual machines are created.
- 1 network interface for each virtual machine.
- 1 disk for each virtual machine.

The following resources are created in your {{site.data.keyword.cloud_notm}} account.

- 1 {{site.data.keyword.satelliteshort}} location.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the virtual machines in Azure, attached to the location and assigned to the {{site.data.keyword.satelliteshort}} location control plane.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the virtual machines in Azure, attached to the location, unassigned, and available to use for services such as a {{site.data.keyword.redhat_openshift_notm}} cluster. If you added more than 6 hosts, the additional hosts are unassigned and available for use in the control plane or by services. 

This template does not include the required storage resources for some integrations, such as OpenShift Data Foundation or other services. If you need OpenShift Data Foundation or similar services, review the required resources and manually create hosts in your Azure account. Then, [attach the hosts to your location](/docs/satellite?topic=satellite-azure).
{: important}


## Microsoft Azure credentials
{: #infra-creds-azure}

Retrieve the Microsoft Azure credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your Azure cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your Azure account](/docs/satellite?topic=satellite-iam-common#permissions-azure) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. [Sign in to your Azure account](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli){: external} from the command line.
    ```sh
    az login
    ```
    {: pre}

3. List the available subscriptions in your account.
    ```sh
    az account list
    ```
    {: pre}

4. Set the subscription to create your Azure resources in.
    ```sh
    az account set --subscription="<subscription_ID>"
    ```
    {: pre}

5. Create a service principal identity with the Contributor role, scoped to your subscription. These credentials are used by {{site.data.keyword.satellitelong_notm}} to provision resources in your Azure account. For more information, see the [Azure documentation](https://learn.microsoft.com/en-us/cli/azure/azure-cli-sp-tutorial-1){: external}.
    ```sh
    az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_ID>" -n"<service_principal_name>"
    ```
    {: pre}

6. In the output, note the values of the `appID`, `password`, and `tenant` fields.
    ```json
    {
    "appId": "<azure-client-id>",
    "displayName": "<service_principal_name>",
    "name": "http://<service_principal_name>",
    "password": "<azure-secret-key>",
    "tenant": "<tenant-id>"
    }
    ```
    {: screen}

7. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. 
    ```json
    {
        "app_id":"string",
        "tenant_id":"string",
        "password": "string"
    }
    ```
    {: screen}
    



## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #azureauto-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.


