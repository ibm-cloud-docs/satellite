---


copyright:
  years: 2020, 2024
lastupdated: "2024-02-23"

keywords: satellite, hybrid, multicloud, gcp, google cloud platform

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Attaching Google Cloud Platform (GCP) hosts
{: #gcp}

Learn how attach Google Cloud Platform (GCP) virtual instances to your {{site.data.keyword.satellitelong_notm}} location.
{: shortdesc}

To attach Red Hat CoreOS (RHCOS) hosts, your location must be enabled for Red Hat CoreOS. For more information, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). Note that you can still attach Red Hat Enterprise Linux hosts to a location that is enabled for Red Hat CoreOS.

Before you begin, make sure that you create host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers.

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
{: tip}


## Manually adding hosts to {{site.data.keyword.satelliteshort}} in the GCP console
{: #gcp-host-attach}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from Google Cloud Platform (GCP).
{: shortdesc}

If you want to use Red Hat CoreOS (RHCOS) hosts in your location, provide your RHCOS image file to your Google account. For more information, see [Creating custom images](https://cloud.google.com/compute/docs/images/create-custom){: external}. To find RHCOS images, see the list of [available images](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/). Note that you must use at least version 4.9.
{: important}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add GCP hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Enter any host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-host-autoassign-ov) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
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
    3. In the **Boot disk** section, click **Change** to change the default operating system and boot disk size. Make sure to select Red Hat Enterprise Linux 8 as your operating system for Red Hat Enterprise Linux or the Red Hat CoreOS image that you provided earlier and to change your boot disk size to a minimum of 100 GB.
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
11. Assign your GCP hosts to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts).


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

For more information, see [VPC firewall rules overview](https://cloud.google.com/firewall/docs/firewalls){: external} in the Google Cloud Platform documentation.



## I added hosts to my location, what's next?
{: #gcp-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.





