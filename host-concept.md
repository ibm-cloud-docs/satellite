---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud, os upgrade, operating system, security patch

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} hosts
{: #host-concept}

A {{site.data.keyword.satelliteshort}} host represents a compute machine of your own infrastructure, such as an on-premises data center or a public cloud provider. You can attach hosts to a {{site.data.keyword.satelliteshort}} location, and then assign the hosts to run {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters, {{site.data.keyword.messagehub}}, or {{site.data.keyword.cloud_notm}} databases.
{: shortdesc}

The following diagram presents the initial setup steps for hosts.

![Concept overview of Satellite host setup](/images/host-process.png){: caption="Figure 1. The initial setup process for Satellite hosts." caption-side="bottom"}

1. **Attach**: Your machine becomes a {{site.data.keyword.satelliteshort}} host after you successfully [attach the host](/docs/satellite?topic=satellite-attach-hosts) to a {{site.data.keyword.satelliteshort}} location by running a registration script on the machine. Your machine must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs). For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan). After the host is attached and a heartbeat can be detected, its health is **ready** and its status is **unassigned**. You can still log in to the machine with SSH to troubleshoot any issues.

2. **Assign**: The hosts in your {{site.data.keyword.satelliteshort}} location do not run any workloads until you assign them as compute capacity to the {{site.data.keyword.satelliteshort}} control plane or a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, a basic setup has 6 hosts that are assigned as worker nodes to the {{site.data.keyword.satelliteshort}} location control plane. Other hosts might be assigned to clusters as worker nodes for your Kubernetes workloads, or to other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services. After you assign a host, it enters a **provisioning** status.

3. **Bootstrap**: When you assign a host, the host is bootstrapped to become a worker node in a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) such as a managed {{site.data.keyword.cos_full_notm}} instance or your {{site.data.keyword.satelliteshort}} control plane. This bootstrap process consists of three phases, all of which must successfully complete. First, required images are downloaded to the host from {{site.data.keyword.registrylong_notm}}. Then, the host is rebooted to apply the imaging configuration. Finally, software packages for the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service, such as {{site.data.keyword.redhat_openshift_notm}}, are set up on the host. After successfully bootstrapping, the host enters a **normal** health state with an **assigned** status. You can no longer log in to the underlying machine via SSH to troubleshoot any issues. Instead, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).

Now, your hosts serve as worker nodes for your {{site.data.keyword.satelliteshort}} location control plane, {{site.data.keyword.openshiftlong_notm}} cluster, or as compute capacity for other [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). If you run a {{site.data.keyword.redhat_openshift_notm}} cluster, you can log in to the clusters and use Kubernetes or {{site.data.keyword.redhat_openshift_notm}} APIs to manage your containerized workloads, or use [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig) to manage your workloads across clusters.
