---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Limitations, default settings, and usage requirements
{: #requirements}

{{site.data.keyword.satellitelong}} comes with usage requirements, default service settings, and limitations to ensure security, convenience, and basic functionality.
{: shortdesc}



## Locations
{: #reqs-locations}

You can create up to 20 {{site.data.keyword.satelliteshort}} locations per {{site.data.keyword.cloud_notm}} multizone metro that the location is managed from.
{: shortdesc}

Name
:    The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.

Latency
:    As you select your infrastructure provider, consider the following latency requirements. Environments that do not meet the latency requirements experience degraded performance.
     - **Between {{site.data.keyword.cloud_notm}} and the location**: The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).
     - **Between hosts in your location**: Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.


## Hosts
{: #reqs-host}

See [Host requirements](/docs/satellite?topic=satellite-host-reqs#host-reqs).
{: shortdesc}

For cloud provider-specific configurations, see the following topics.

- [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
- [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
- [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
- [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) (for testing and demonstration purposes only)
- [Microsoft Azure](/docs/satellite?topic=satellite-azure).



## Clusters
{: #reqs-clusters}

See [{{site.data.keyword.satelliteshort}} cluster limitations](/docs/openshift?topic=openshift-openshift_limitations#satellite_limits) in the {{site.data.keyword.openshiftlong_notm}} documentation. The limitations include information related to the following components.
{: shortdesc}

- {{site.data.keyword.openshiftlong_notm}} clusters that you create in your {{site.data.keyword.satelliteshort}} location.
- Storing data in Kubernetes persistent volumes for apps that run in your clusters.
- Cluster networking, such as Kubernetes load balancers.
- Using your hosts as the worker nodes in the cluster.

## Link and endpoints
{: #reqs-link}

Link connector instances
:    The {{site.data.keyword.satelliteshort}} Link connector instances that run in your [{{site.data.keyword.satelliteshort}} location control plane worker nodes](/docs/satellite?topic=satellite-service-architecture) are limited to three instances, one per host. Even if you attach hosts to the location control plane, network traffic that is routed through the {{site.data.keyword.satelliteshort}} Link connector is sent only over three hosts.

Cloud and location endpoints
:    Review the maximum number of each type of Link endpoint that you can create for one {{site.data.keyword.satelliteshort}} location.
     - `cloud` endpoints: 1000 total. For example, you might create up to 650 TLS endpoints and 350 HTTP endpoints through which clients in your location can connect to resources outside of the location network.
     - `location` endpoints: 25 total. For example, you might create up to 20 TLS endpoints and 5 HTTP endpoints through which clients outside of your location network can connect to resources inside the location.


## Config
{: #reqs-config}

Review the following application configuration requirements for {{site.data.keyword.satelliteshort}} Config.
{: shortdesc}


{{site.data.keyword.satelliteshort}} Config is not supported on locations that are enabled for Red Hat CoreOS.
{: note}


{{site.data.keyword.satelliteshort}} Config access to modify Kubernetes resources within a cluster
:    By default, {{site.data.keyword.satelliteshort}} Config is limited to what Kubernetes resources it can read and modify in your clusters. You must grant {{site.data.keyword.satelliteshort}} Config access in each cluster where you want to use {{site.data.keyword.satelliteshort}} Config to manage your Kubernetes resources.
:    Choose from the following options.
     - Opt in to cluster admin access when you create the cluster in the [console](/docs/openshift?topic=openshift-satellite-clusters#satcluster-create-console) or [CLI](/docs/openshift?topic=openshift-satellite-clusters#satcluster-create-cli) with the `--enable-admin-agent` option. Note that you must perform a one-time `oc login` in each cluster to synchronize the admin permissions.
     - To opt in after creating a cluster or to scope the access, see [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-access).

{{site.data.keyword.satelliteshort}} Config and {{site.data.keyword.cloud_notm}} IAM
:    You cannot scope access policies for {{site.data.keyword.satelliteshort}} Config resources (configuration, subscription, cluster, or cluster group) to an {{site.data.keyword.cloud_notm}} resource group. {{site.data.keyword.satelliteshort}} Config uses the open source Razee project, which authenticates users by using the organization. The organization supports only the account ID, not resource groups.
:    You cannot scope access policies to particular configuration or subscription resources. When you assign a policy in the {{site.data.keyword.cloud_notm}} IAM console, leave the **Resource** field blank for configurations or subscriptions. Instead, you can scope the access policy to a cluster group for more control of how your {{site.data.keyword.satelliteshort}} Config resources are deployed.
:    To let users view the Kubernetes resources that run in clusters with {{site.data.keyword.satelliteshort}} Config, you must assign an access policy with the appropriate role (Administrator, Manager, or Reader) to {{site.data.keyword.satellitelong_notm}} (and not scoped to a particular resource or resource type).
:    After you enable {{site.data.keyword.satelliteshort}} Config permissions when you create a {{site.data.keyword.satelliteshort}} cluster in the console or in the CLI with the `--enable-admin-agent` option for the `ibmcloud oc cluster create satellite` command, you must set the context of the cluster to synchronize permissions. You can set the cluster context by launching the {{site.data.keyword.redhat_openshift_notm}} web console or by running the `ibmcloud oc cluster config` command in the CLI. **Note**: If you registered a {{site.data.keyword.openshiftlong_notm}} cluster in the public cloud to use with {{site.data.keyword.satelliteshort}} Config, you do not need to set the cluster context to synchronize permissions.

Configuration files in {{site.data.keyword.satelliteshort}} Config
:    - You can upload only an individual configuration file of Kubernetes resources per release version. You cannot upload a directory or several different configuration files.
     - Configuration files are subject to Kubernetes requirements, such as that the manifest must be expressed in YAML format.

## {{site.data.keyword.cloud_notm}} services
{: #reqs-services}

You can have up to 40 instances of a supported {{site.data.keyword.cloud_notm}} service per {{site.data.keyword.satelliteshort}} location. For example, you might have a {{site.data.keyword.satelliteshort}} location that has 40 {{site.data.keyword.openshiftlong_notm}} clusters in it.
{: shortdesc}

Each supported service might have its own limitations to run in {{site.data.keyword.satelliteshort}}.
- {{site.data.keyword.openshiftlong_notm}}: See [{{site.data.keyword.satelliteshort}} cluster limitations](/docs/openshift?topic=openshift-openshift_limitations#satellite_limits) in the {{site.data.keyword.openshiftlong_notm}} documentation.
- {{site.data.keyword.cloud_notm}} Databases (ICD): See See [{{site.data.keyword.cloud_notm}} Databases (ICD) enabled by {{site.data.keyword.satellitelong_notm}}](/docs/cloud-databases?topic=cloud-databases-satellite-get-started).

