---


copyright:
  years: 2022, 2025
lastupdated: "2025-04-02"

keywords: satellite, hybrid, multicloud, managed services, enabled service, satellite-enabled

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Supported Satellite-enabled IBM Cloud services
{: #managed-services}

Learn about what services are supported by {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

| Service | Description of support | Supported by RHCOS hosts |
| ------- | -------------- | -- |
| {{site.data.keyword.openshiftlong_notm}} | You can create {{site.data.keyword.openshiftlong_notm}} clusters in a {{site.data.keyword.satelliteshort}} location, and use the hosts of your own infrastructure that you added to your location as the worker nodes for the cluster. See [Creating {{site.data.keyword.redhat_openshift_notm}} clusters in {{site.data.keyword.satelliteshort}}](/docs/openshift?topic=openshift-satellite-clusters). | Yes, for {{site.data.keyword.redhat_openshift_notm}} version 4.9 and later. |
| {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.cos_full_notm}} offers users the flexibility to run a managed {{site.data.keyword.cos_short}} service on client-owned on-premises infrastructure, edge locations or third-party public cloud infrastructure. See [About {{site.data.keyword.cos_short}} for {{site.data.keyword.satelliteshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cos-satellite). | No |
| {{site.data.keyword.cloud_notm}} Paks | Cloud Paks provide AI-powered software designed to accelerate application modernization with pre-integrated data, automation and security capabilities. Our software delivers a comprehensive and unified hybrid cloud platform experience, enabling business and IT teams to build and modernize applications faster across any cloud or IT infrastructure. | Depends on the Cloud Pak |
{: caption="Supported managed services for Satellite" caption-side="bottom"}


## Setting up access for Satellite-enabled services
{: #managed-services-iam}

For most Satellite-enabled services, you must set up service-to-service access through IAM, with **Satellite** as your target service and the managed service as the source service. 

1. Open [Manage authorizations](https://cloud.ibm.com/iam/authorizations){: external} in the IBM Cloud console.
2. Click **Create**.
3. Select the account for the authorization.
4. For **Source service**, select the service that you want to authorize. You can further scope the access by selecting **Resources based on selected attributes** and then adding attributes.
5. For **Target service**, select `{{site.data.keyword.satelliteshort}}`. You can further scope the access by selecting **Resources based on selected attributes** and then adding attributes.
6. Select your options as the **Service access**.
7. Click **Authorize**. 

For more information, see [Managing access overview](/docs/satellite?topic=satellite-iam) or consult the service documentation.
