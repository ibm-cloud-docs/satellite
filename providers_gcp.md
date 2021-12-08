---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-08"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Google Cloud Platform (GCP)
{: #gcp}

Learn how you can set up an {{site.data.keyword.satellitelong_notm}} location with virtual instances that you created in Google Cloud Platform (GCP).
{: shortdesc}

## Adding GCP hosts to {{site.data.keyword.satelliteshort}}
{: #gcp-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Google Cloud Platform (GCP).
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add GCP hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Enter any host labels that are used later to [automatically assign hosts to {{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-hosts#host-autoassign-ov) in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu` label, but you might want to add more to control the autoassignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local machine.
3. Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
    ```sh
    # Enable GCP RHEL package updates
    yum update --disablerepo=* --enablerepo="*" -y
    yum repolist all
    yum install container-selinux -y
    yum install subscription-manager -y
    ```
    {: codeblock}  

4. From the [GCP **Compute Engine** dashboard](https://console.cloud.google.com/compute){: external}, select **Instance templates**.
5. Click **Create instance template**.
6. Enter the details for your instance template as follows.

    For an overview of available options when creating an instance template, see the [GCP documentation](https://cloud.google.com/compute/docs/instance-templates/create-instance-templates){: external}.
    {: tip}

    1. Enter a name for your instance template.
    2. In the **Machine configuration** section, select the **Series** and **Machine type** that you want to use. You can select any series that you want, but make sure that the machine type meets the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) for CPU and memory.
    3. In the **Boot disk** section, click **Change** to change the default operating system and boot disk size. Make sure to select Red Hat Enterprise Linux 7 as your operating system and to change your boot disk size to a minimum of 100 GB.
    4. Optional: If you want your machines to allow HTTP and HTTPS traffic, select **Allow HTTP traffic** and **Allow HTTPS traffic** from the **Firewall** section of your instance template.
    5. Click **Management, security, disks, networking, sole tenancy** to view additional networking and security settings.
    6. In the **Management** tab, locate the **Startup script** field and enter the registration script that you modified earlier.
    7. In the **Networking** tab, choose the network that you want your instances to be connected to. This network must allow access to {{site.data.keyword.satellitelong_notm}} as described in [Firewall settings](#gcp-reqs-firewall). You can check and change the firewall settings for your network in the next step.
    8. Click **Create** to save your instance template.
7. Optional: Update the firewall settings for the network that you assigned to your instance template.
    9. From the [GCP **VPC Network** dashboard](https://console.cloud.google.com/networking/networks){: external}, select **Firewall**.
    10. Verify that your network allows access as describe in the [Network firewall settings](#gcp-reqs-firewall). Make changes as necessary.
8. From the [GCP **Compute Engine** dashboard](https://console.cloud.google.com/compute){: external}, select **Instance templates** and find the instance template that you created.
9. From the actions menu, click **Create VM** to create an instance from your template. You can alternatively click **Create Instance Group** to create an instance group to add multiple instances at the same time. Make sure that you spread your instances across multiple zones for higher availability.
10. Wait for the instance to create. During the creation of your instance, the registration script runs automatically. This process takes a few minutes to complete. You can monitor the progress of the script by reviewing the logs for your instance. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.
11. Assign your GCP hosts to the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign-manual).

## Network firewall settings
{: #gcp-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your GCP hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your firewall settings in GCP, similar to the following example.
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

## Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console
{: #gcp-reqs-console-access}

The private IP addresses of your instances are automatically registered and added to your location's DNS record and the cluster's subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console. You must be connected to your hosts' private network, such as through VPN access, to [connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat_se). Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).
