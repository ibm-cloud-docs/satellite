---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, spectrum, spectrum fusion

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Attaching IBM Storage Fusion HCI hosts
{: #host-storage-fusion}

IBM Storage Fusion is a container-native data platform for {{site.data.keyword.redhat_openshift_full}} with enterprise-grade data storage and protection services. It offers an agile way to manage, recover and access your mission-critical data as needed.
{: shortdesc}

Before you can create a Location and add IBM Storage Fusion HCI virtual machines as hosts, you must first order the required hardware. Then, you can deploy Storage Fusion to your {{site.data.keyword.satelliteshort}} Location. 

Get started by reviewing the benefits and solution brief of [IBM Storage Fusion HCI](https://www.ibm.com/products/storage-fusion){: external}.


Ready to order?
:   [Contact IBM Sales](https://www.ibm.com/products/storage-fusion){: external}

Ready to add IBM Storage Fusion HCI virtual machines as hosts?
:   See the [IBM Storage Fusion documentation](https://www.ibm.com/docs/storage-fusion/2.6?topic=cloud-satellite-storage-fusion-hci-system){: external} for deployment steps.

## I added hosts to my location, what's next?
{: #fusion-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.


