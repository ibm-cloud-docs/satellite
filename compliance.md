---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Security and compliance
{: #compliance}

You can use built-in security features in {{site.data.keyword.satellitelong}} for risk analysis and security protection. These features help you to protect your {{site.data.keyword.satelliteshort}} workloads that run on your location infrastructure and network communication.
{: shortdesc}


## Data security
{: #secure-data}

Learn more about options to secure the data that you use for workloads in {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

### What data is stored when I use {{site.data.keyword.satelliteshort}}? How can I use my own keys to encrypt my data?
{: #secure-data-store-encrypt}

See [Securing your data in {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-data-security).

### How do I make my data secure over {{site.data.keyword.satelliteshort}} Link?
{: #secure-data-link}

Your {{site.data.keyword.satelliteshort}} location infrastructure is a part of your local network (on-prem hosts) or the network of your cloud provider, but is managed remotely by using secure {{site.data.keyword.satelliteshort}} Link access from {{site.data.keyword.cloud_notm}}. All communication over {{site.data.keyword.satelliteshort}} Link is encrypted by {{site.data.keyword.IBM_notm}}. When you create an endpoint, you can optionally specify an additional data encryption protocol for the endpoint connection between the client source and destination resource. For example, even if the traffic is not encrypted on the source side, you can specify your own additional TLS encryption for the connection that goes over the internet.

To review information about how {{site.data.keyword.satelliteshort}} Link handles each type of connection protocol, see [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). To review other frequently asked questions about {{site.data.keyword.satelliteshort}} Link network security, see [External network requirements and security](/docs/satellite?topic=satellite-link-location-cloud#link-security).

### What measures can I take to secure user access to data in my location?
{: #secure-data-access}

Review the following ways that you can secure access to your location.

- Consider the types of [user access to resources that run in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-service-connection#user-access)
- [Manage access for {{site.data.keyword.satelliteshort}} by using {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)](/docs/satellite?topic=satellite-iam)
- Monitor user-initiated activities by [Setting up {{site.data.keyword.at_short}} for {{site.data.keyword.satelliteshort}} location events](/docs/satellite?topic=satellite-health#setup-at)
- In the case of a potential security incident, [reset the key that the control plane uses to communicate with all the hosts in the Satellite location](/docs/satellite?topic=satellite-host-update-location#host-key-reset)

## {{site.data.keyword.IBM_notm}} operational access
{: #operational-access}

Learn more about {{site.data.keyword.IBM_notm}} operational access to your {{site.data.keyword.satelliteshort}} location and how to monitor {{site.data.keyword.IBM_notm}}-initiated activities.
{: shortdesc}

### What automated access does {{site.data.keyword.IBM_notm}} have to my location?
{: #operational-access-automated}

Operations such as host attachment, host assignment, and major and minor version updates for the {{site.data.keyword.satelliteshort}} location control plane hosts are controlled by automation through the {{site.data.keyword.satellitelong_notm}} API server in {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.satelliteshort}} API server communicates with the control plane master, which also exists in {{site.data.keyword.cloud_notm}}, to make these changes in your location.

Regular maintenance and automation tooling accesses the masters of {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters in your location through the default `openshift-api-<cluster_ID>` Link endpoint. This endpoint allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. For example, if you create a {{site.data.keyword.redhat_openshift_notm}} cluster to run applications in your location, all version updates for that cluster's master are automatically applied through the default `openshift-api-<cluster_ID>` Link endpoint for that cluster.

Updates to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that are running on hosts in your location are initiated by the {{site.data.keyword.cloud_notm}} team for that service. When a change is ready to be rolled out to a service, the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service team uses [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig) to upload a new version of the service to the subscription that the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster is included in. To apply the service update through {{site.data.keyword.satelliteshort}} Config, the control plane automation for the {{site.data.keyword.satellitelong_notm}} API server, which exists in {{site.data.keyword.cloud_notm}}, deploys the update to the master of the {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster in your location through the default `openshift-api-<cluster_ID>` Link endpoint. The cluster master then applies the updates to the worker nodes in the cluster.

### What access do {{site.data.keyword.IBM_notm}} SREs have to my location control plane, including the masters of {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters?
{: #operational-access-control-plane}

The default `satellite-healthcheck-<location_ID>` Link endpoint allows the {{site.data.keyword.satelliteshort}} control plane master to check the health of your location's control plane cluster, and alerts {{site.data.keyword.IBM_notm}} Site Reliability Engineers (SREs) when manual intervention is required.

* To manually resolve issues with your {{site.data.keyword.satelliteshort}} location infrastructure management, such as host assignment or attachment, {{site.data.keyword.IBM_notm}} SREs use tooling to access the {{site.data.keyword.satellitelong_notm}} API server. The {{site.data.keyword.satelliteshort}} API server communicates with the {{site.data.keyword.satelliteshort}} control plane master in {{site.data.keyword.cloud_notm}}.
* To manually resolve issues with masters of {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters in your location, such as if the master fails to correctly deploy, {{site.data.keyword.IBM_notm}} SREs use tooling to access the default `openshift-api-<cluster_ID>` Link endpoint for the master of the service cluster.

Note that tooling is controlled through the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.

### What access do {{site.data.keyword.IBM_notm}} SREs have to my data and workloads that run in my {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters?
{: #operational-access-workloads}

{{site.data.keyword.satelliteshort}} Link uses a zero-trust model: {{site.data.keyword.cloud_notm}} has no access to your workloads by default. Any management of infrastructure in your location that is initiated by {{site.data.keyword.IBM_notm}} SREs occurs through the {{site.data.keyword.satelliteshort}} API server, which exists in {{site.data.keyword.cloud_notm}}, and any management of {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster masters occurs through the default `openshift-api-<cluster_ID>` Link endpoint for that cluster.

The access through the `openshift-api-<cluster_ID>` endpoint is isolated from your workloads and the network connections, such as the Link endpoints, that your workloads use. The `openshift-api-<cluster_ID>` provides access only to the {{site.data.keyword.satelliteshort}} location control plane. {{site.data.keyword.IBM_notm}} SREs have no access to the hosts that are assigned as worker nodes of your {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service clusters, where you run workloads in your location, because no default Link endpoints are created for workloads that run on these worker nodes, and all SSH access is disabled as part of the host bootstrapping process.

### How can I monitor and manage {{site.data.keyword.IBM_notm}} access into my location?
{: #operational-access-monitor}

Default {{site.data.keyword.satelliteshort}} Link endpoints are created for your location's control plane cluster and for any other [{{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-managed-services) that you run in your location. These default {{site.data.keyword.satelliteshort}} Link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network. To review these endpoints, see [Default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-default-link-endpoints). You have full control over these default endpoints, including the ability to disable them. However, disabling these endpoints prevents your location from being fully managed and updated, and the endpoints cannot be removed.

{{site.data.keyword.satelliteshort}} Link provides built-in controls to help you restrict which clients can access endpoints, ensuring that there are no back door access points on your hosts. For more information, see [Access and audit controls](/docs/satellite?topic=satellite-link-location-cloud#link-audit-about).

Additionally, you can configure auditing to monitor user-initiated events for Link endpoints. {{site.data.keyword.satellitelong_notm}} integrates with {{site.data.keyword.at_full}} to collect and send audit events for all link endpoints in your location to your {{site.data.keyword.at_short}} instance. For example, after you set up event auditing, you can review all events that relate to the masters for the clusters that run in your location, including events that are initiated by {{site.data.keyword.cloud_notm}}. To get started with auditing, see [Auditing events for endpoint actions](/docs/satellite?topic=satellite-link-cloud-monitor#link-audit).

### What happens if {{site.data.keyword.satelliteshort}} Link becomes unavailable? Can {{site.data.keyword.IBM_notm}} still maintain my {{site.data.keyword.satelliteshort}} location?
{: #operational-access-availability}

{{site.data.keyword.satelliteshort}} Link depends on the underlying connectivity of your hosts' local network to monitor and maintain the managed services for your {{site.data.keyword.satelliteshort}} location. If {{site.data.keyword.satelliteshort}} Link become unavailable, any requested changes to your {{site.data.keyword.satelliteshort}} location, such as adding hosts or access control requests to {{site.data.keyword.IBM_notm}} services through {{site.data.keyword.iamshort}}, are disrupted. After connectivity is restored, logs and events are sent to your [{{site.data.keyword.la_full_notm}} and {{site.data.keyword.at_full_notm}} instances](/docs/satellite?topic=satellite-health).

Note that your on-location workloads continue to run independently even if the location's connectivity to {{site.data.keyword.cloud_notm}} is unavailable. However, if any applications use a Link endpoint to communicate with {{site.data.keyword.cloud_notm}}, communication between those apps and {{site.data.keyword.cloud_notm}} is disrupted.

For more information about making your {{site.data.keyword.satelliteshort}} location highly available, see [High availability and disaster recovery for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).


## Digital certificates for {{site.data.keyword.satelliteshort}} hosts and domains
{: #certs-hosts-domains}

Let's Encrypt certificates are automatically generated for several {{site.data.keyword.satellitelong_notm}} domains and hosts. Regular renewal of these certificates helps keep your {{site.data.keyword.satelliteshort}} components secure. To see the expiry dates for each certificate and understand whether you or {{site.data.keyword.IBM_notm}} is responsible for regenerating the certificate, review the following table.
{: shortdesc}

| Component | Example domain | Certificate expiry | Who regenerates | How to regenerate |
|---|---|---|---|---|
| Default {{site.data.keyword.openshiftlong_notm}} API endpoint (`openshift-api-<cluster_ID>`) for each {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster | `c-04.private.us-east.link.satellite.cloud.ibm.com` | 19800 hours (~2.26 years) | You | Regenerated during a [cluster master refresh](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_apiserver_refresh) or [cluster master update](/docs/containers?topic=containers-update#master). |
| Default endpoints for {{site.data.keyword.cloud_notm}} services (IAM, {{site.data.keyword.cos_short}}, {{site.data.keyword.mon_short}}, {{site.data.keyword.la_short}}) | `m65f0b26d6c5f695647f5-6b64a6ccc9c596bf59a86625d8fa2202-c000.us-east.satellite.appdomain.cloud` | 90 days | {{site.data.keyword.IBM_notm}} | The Link tunnel server regenerates the certificate, and the Link Connector automatically reboots to reflect the rotated certificate. |
| `c000`, `c001`, `c002`, and `c003` subdomains for each location zone | `s7033baaa45e1ae1a1060-d603ff82e51c94176a53d44566df9d79-c000.us-south.satellite.appdomain.cloud` | 19800 hours (~2.26 years) | You | Regenerated during a [cluster master refresh](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_apiserver_refresh) or [cluster master update](/docs/containers?topic=containers-update#master). |
| `ce00` Ingress subdomains | `s7033baaa45e1ae1a1060-d603ff82e51c94176a53d44566df9d79-ce00.us-south.satellite.appdomain.cloud` | 90 days | {{site.data.keyword.IBM_notm}} | [Automatically regenerated 37 days before expiration](/docs/openshift?topic=openshift-ingress-roks4#manage_certs_ibm). |
| Worker node connection to the API server | 10.240.128.09 | 3 years | You | [Update hosts that are assigned as worker nodes](/docs/satellite?topic=satellite-host-update-workers). |
| {{site.data.keyword.satelliteshort}} control plane master API endpoint | `http://c103-1.containers.cloud.ibm.com/` | 19800 hours (~2.26 years) | {{site.data.keyword.IBM_notm}} | Regenerated during automated rollouts for major and minor version updates for the {{site.data.keyword.satelliteshort}} location control plane master hosts. |
| {{site.data.keyword.satelliteshort}} control plane master hosts | - | 19800 hours (~2.26 years) | {{site.data.keyword.IBM_notm}} | [Update control plane hosts](/docs/satellite?topic=satellite-host-update-location). |
{: caption="Certificates for Satellite domains and hosts" caption-side="top"}


## Platform compliance and certification
{: #platform-compliance}

Learn more about compliance standards for the {{site.data.keyword.satellitelong_notm}} service.
{: shortdesc}

### What compliance standards does the service meet?
{: #compliance-standards}

See the [{{site.data.keyword.satelliteshort}} FAQ for compliance standards](/docs/satellite?topic=satellite-faqs#standards).

### Which areas of security compliance am I responsible for?
{: #compliance-responsibilities}

For an overview of who is responsible for particular cloud resources when using {{site.data.keyword.satellitelong_notm}}, see the table in [Overview of shared responsibilities](/docs/satellite?topic=satellite-responsibilities#overview-by-resource). For a detailed description of areas in which responsibilities are shared between you and {{site.data.keyword.IBM_notm}} for security and compliance, see [Tasks for shared responsibilities: Security and regulation compliance](/docs/satellite?topic=satellite-responsibilities#security-compliance). If you use {{site.data.keyword.satelliteshort}} Infrastructure Service as your infrastructure provider, [review the {{site.data.keyword.satelliteshort}} Infrastructure Service responsibilities instead](/docs/satellite?topic=satellite-satis-responsibilities).

### What are the security compliance responsibilities of {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services?
{: #compliance-services}

To see the security options and compliance standards of a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service, review the documentation for each {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service. For example, for {{site.data.keyword.redhat_openshift_notm}} clusters, see [What compliance standards does the service meet?](/docs/openshift?topic=openshift-faqs#standards) in the {{site.data.keyword.openshiftlong_notm}} documentation.
