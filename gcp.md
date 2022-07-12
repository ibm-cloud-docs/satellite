---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-12"

keywords: satellite, hybrid, multicloud, gcp, google cloud platform

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Google Cloud Platform (GCP)
{: #gcp}

Learn how you can set up an {{site.data.keyword.satellitelong_notm}} location with virtual instances that you created in Google Cloud Platform (GCP).
{: shortdesc}

If your hosts are running Red Hat CoreOS (RHCOS), you must manually attach them to your location.
{: note}

## Automating your GCP location setup with a {{site.data.keyword.bpshort}} template
{: #gcp-template}

Automate your GCP setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your GCP account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. Or, you can [manually attach GCP hosts to a {{site.data.keyword.satelliteshort}} location](#gcp-host-attach).
{: tip}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

1. In your GCP cloud provider, [set up your account credentials](#infra-creds-gcp).
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
3. In the **Get started** section, click **GCP Quick Start**.
4. Upload your GCP credentials file.
5. Review the **GCP environment** details that are automatically populated. By default, enough VMs are created to provide hosts for 1 small location that can run about 2 demo clusters. To change the subscription, region, instance type, or number of VMs for the hosts, click the **Edit** pencil icon.
6. Review the **Satellite location** details. If you edited the GCP environment details, you might want to click the **Edit** pencil icon to change details such as the description, API key, or {{site.data.keyword.cloud_notm}} multizone region that the location is managed from.
7. In the **Summary** pane, review the cost estimate.
8. Click **Create location**. Your location might take about 30 minutes to finish provisioning.
9. Optional: To review the provisioning progress, review the logs in the {{site.data.keyword.bpshort}} workspace that is automatically created for you.
    1. Click **Manage in Schematics**. If you see an error, navigate to the [{{site.data.keyword.bpshort}} workspaces console](https://cloud.ibm.com/schematics/workspaces){: external} and click the name of your workspace, such as `us.east.cartOrder...`.
    2. From the **Activity** tab, find the current activity row and click **View log** to review the log details.
    3. Wait for the {{site.data.keyword.bpshort}} action to finish and the workspace to enter an **Active** state.

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.

If you are using this template for demonstration purposes, do not assign all your hosts to your control plane. Hosts that are assigned to the control plane cannot be used for other purposes, such as worker nodes for your cluster. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about-locations).
{: note}

What's next?

The {{site.data.keyword.bpshort}} template helped with the initial creation, but you are in control for subsequent location management actions, such as [attaching more hosts](/docs/satellite?topic=satellite-attach-hosts), [creating {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters), or [scaling the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-location-sizing). If you [remove](/docs/satellite?topic=satellite-host-remove#location-remove-console) your {{site.data.keyword.satelliteshort}} location, make sure to [remove your workspace in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-workspace-setup#del-workspace), too.

## Manually adding hosts to {{site.data.keyword.satelliteshort}} in the GCP console
{: #gcp-host-attach}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from Google Cloud Platform (GCP).
{: shortdesc}


If you want to use Red Hat CoreOS (RHCOS) hosts in your location, provide your RHCOS image file to your Google account. For more information, see [Creating custom images](https://cloud.google.com/compute/docs/images/create-delete-deprecate-private-images){: external}. To find RHCOS images, see the list of [available images](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/). Note that you must use at least version 4.9.
{: important}


All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add GCP hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Enter any host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which should be treated and protected as sensitive information.
3. **RHEL hosts only** Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
    ```sh
    # Enable GCP RHEL package updates
    yum update --disablerepo=* --enablerepo="*" -y
    yum repolist all
    yum install container-selinux -y
    yum install subscription-manager -y
    ```
    {: codeblock}

4. From the [GCP main menu](https://console.cloud.google.com/getting-started){: external}, navigate to the **Compute Engine** dashboard and select **Instance templates**.
5. Click **Create instance template**.
6. Enter the details for your instance template as follows.

    For an overview of available options when creating an instance template, see the [GCP documentation](https://cloud.google.com/compute/docs/instance-templates/create-instance-templates){: external}.
    {: tip}

    1. Enter a name for your instance template.
    2. In the **Machine configuration** section, select the **Series** and **Machine type** that you want to use. You can select any series that you want, but make sure that the machine type meets the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for CPU and memory.
    3. In the **Boot disk** section, click **Change** to change the default operating system and boot disk size. Make sure to select Red Hat Enterprise Linux 7 as your operating system for Red Hat Enterprise Linux or the Red Hat CoreOS image that you provided earlier and to change your boot disk size to a minimum of 100 GB.
    4. Optional: If you want your machines to allow HTTP and HTTPS traffic, select **Allow HTTP traffic** and **Allow HTTPS traffic** from the **Firewall** section of your instance template.
    5. Click **Management, security, disks, networking, sole tenancy** to view additional networking and security settings.
    6. In the **Management** tab, locate the **Startup script** field and enter the registration script that you modified earlier.
    7. In the **Networking** tab, choose the network that you want your instances to be connected to. This network must allow access to {{site.data.keyword.satellitelong_notm}} as described in [Firewall settings](#gcp-reqs-firewall). You can check and change the firewall settings for your network in the next step.
    8. Click **Create** to save your instance template.
7. Optional: Update the firewall settings for the network that you assigned to your instance template.
    9. From the [GCP main menu](https://console.cloud.google.com/getting-started){: external}, navigate to the **VPC Network** dashboard and select **Firewall**.
    10. Verify that your network allows access as describe in the [Network firewall settings](#gcp-reqs-firewall). Make changes as necessary.
8. From the GCP **Compute Engine** dashboard, select **Instance templates** and find the instance template that you created.
9. From the actions menu, click **Create VM** to create an instance from your template. You can alternatively click **Create Instance Group** to create an instance group to add multiple instances at the same time. Make sure that you spread your instances across multiple zones for higher availability.
10. Wait for the instance to create. During the creation of your instance, the registration script runs automatically. This process takes a few minutes to complete. You can monitor the progress of the script by reviewing the logs for your instance. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.
11. Assign your GCP hosts to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).


## Manually ordering hosts with the `gcloud` CLI
{: #gcp-manual-cli}

When you order your instances, pass the `--metadata-from-file user-data` option and include your attach script. For more information, see the `gcloud compute instances create` [command reference](https://cloud.google.com/sdk/gcloud/reference/compute/instances/create){: external}. 

Example command to order and attach GCP VMs to your {{site.data.keyword.satelliteshort}} location.

```sh
gcloud compute instances create INSTANCE-1 INSTANCE-2 INSTANCE-3 --machine-type=n2-standard-8 --source-instance-template INSTANCE-TEMPLATE --metadata-from-file user-data=ATTACH-SCRIPT-LOCATION --zone ZONE --image-family=IMAGE-FAMILY --image-project=IMAGE-PROJECT
```
{: pre}


## Network firewall settings
{: #gcp-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network), your GCP hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your firewall settings in GCP, similar to the following example.
{: shortdesc}

```sh
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

For more information, see [VPC firewall rules overview](https://cloud.google.com/vpc/docs/firewalls){: external} in the Google Cloud Platform documentation.


## Google Cloud Platform credentials
{: #infra-creds-gcp}

Retrieve the Google Cloud Platform (GCP) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your GCP cloud on your behalf.
{: shortdesc}

1. [Create a service account and service account key](https://cloud.google.com/docs/authentication/getting-started#creating_a_service_account){: external} with at least the required [GCP permissions](/docs/satellite?topic=satellite-iam#permissions-gcp). As part of creating the service account, a JSON key file is downloaded to your local machine.
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
{: #gcp-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.





