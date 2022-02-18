---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud, satellite infrastructure service

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Getting started with {{site.data.keyword.satellitelong_notm}} Infrastructure Service
{: #infrastructure-service}

With {{site.data.keyword.satelliteshort}} Infrastructure Service, you can order managed infrastructure from {{site.data.keyword.IBM_notm}} to create an on-premises location in {{site.data.keyword.satellitelong}}.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Infrastructure Service comes in varying target sizes with elastic capacity that scales to your computing needs. As a managed infrastructure service, {{site.data.keyword.IBM_notm}} deploys {{site.data.keyword.satellitelong_notm}} for you in your data center, so you can focus on maximizing your application workloads with the capabilities of {{site.data.keyword.cloud_notm}}.

## Requesting a {{site.data.keyword.satelliteshort}} Infrastructure Service environment
{: #satis-getting-started}

You can request a {{site.data.keyword.satelliteshort}} Infrastructure Service environment by using the {{site.data.keyword.satellitelong_notm}} console.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2. From the Navigation Menu, click **Satellite > Locations > Create location**.
3. Click the **Satellite Infrastructure Service** tile.
4. Fill out the **Location details** form with information such as the country, city, and requested computing and storage capacity.

Your request is complete! Now, review the three-staged setup process to understand happens next.

## Stage 1: Planning your setup
{: #satis-gs-plan}

After you submit the {{site.data.keyword.satelliteshort}} Infrastructure Service location details form, the {{site.data.keyword.IBM_notm}} team begins to tailor your infrastructure to your request.
{: shortdesc}

1. The {{site.data.keyword.IBM_notm}} team contacts you to confirm the details of the order and contact information for your technical and facilities staff.
2. The {{site.data.keyword.IBM_notm}} team works with your technical staff to confirm location details and prepare for the onsite installation including:
    - Workload details to confirm the capacity of your {{site.data.keyword.satelliteshort}} Infrastructure Service location
    - Power, cooling, and physical environment details
    - Networking setup including ports and IP address ranges
    - Coordinating physical access
    - Installation setup dates
3. You and {{site.data.keyword.IBM_notm}} review the [shared responsibilities](/docs/satellite?topic=satellite-satis-responsibilities) that you have with {{site.data.keyword.satelliteshort}} Infrastructure Service.

## Stage 2: Installing your on-prem infrastructure
{: #satis-gs-install}

After you and {{site.data.keyword.IBM_notm}} agree to the installation plan, your {{site.data.keyword.satelliteshort}} Infrastructure Service location is set up.
{: shortdesc}

1. {{site.data.keyword.IBM_notm}} delivers the infrastructure to your physical location.
2. {{site.data.keyword.IBM_notm}} specialists work onsite to install the infrastructure, connect the power and network, validate the configuration, and establish remote access.
3. The {{site.data.keyword.IBM_notm}} operations team creates the {{site.data.keyword.satellitelong_notm}} location with the {{site.data.keyword.satelliteshort}} Infrastructure Service hardware that was set up.

Now that your {{site.data.keyword.satelliteshort}} location is created, you can view the location from the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.

## Stage 3: Transitioning to daily operations
{: #satis-gs-ops}

With your {{site.data.keyword.satelliteshort}} Infrastructure Service location up and running, you can use {{site.data.keyword.cloud_notm}} to deploy and manage your application workloads.
{: shortdesc}

### Application workloads
{: #satis-gs-ops-app}

1. As needed, [create clusters](/docs/satellite?topic=openshift-satellite-clusters) in your {{site.data.keyword.satelliteshort}} location to run your apps. The clusters use the available hosts as worker nodes.
2. Use tools such as [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig), [{{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-link-location-cloud), and [{{site.data.keyword.satelliteshort}} storage](/docs/satellite?topic=satellite-config-storage-local-block) to consistently deploy and manage your apps across the clusters in your {{site.data.keyword.satelliteshort}} location.

### Usage and capacity planning
{: #satis-gs-ops-capacity}

1. Monitor daily usage such as [platform metrics](/docs/satellite?topic=satellite-monitor#setup-mon) or the [health of your location, hosts, and clusters](/docs/satellite?topic=satellite-monitor#view-health).
2. Contact the {{site.data.keyword.IBM_notm}} team for exceptional changes to capacity needs in your {{site.data.keyword.satelliteshort}} location.

### Support requests
{: #satis-gs-ops-support}

See [Getting help](/docs/satellite?topic=satellite-get-help).
