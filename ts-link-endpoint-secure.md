---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-17"

keywords: satellite, hybrid, multicloud, endpoint, link, endpoint secure

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I connect to my {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint?
{: #ts-link-endpoint-secure}

I can't connect to my {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint. I was able to before, what changed?  
{: tsSymptoms}

By default, your {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoints are protected. To access your link endpoints, you must configure a source list for your endpoint.
{: tsCauses}

To configure a source list for your endpoint, follow these steps.
{: tsResolve}
 
1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
2. From the **Link endpoints** tab, click the name of your endpoint.
3. In the **Source list** section, click **Add source**.
4. Click **Configure source** to enter a source name and the IP address or subnet CIDR for the client that you want to connect to the endpoint, and click **Add**.
5. Use the toggle to enable the source to connect to the destination resource. After you enable a source, network traffic to the destination through the endpoint is permitted only from clients that use an IP address in the range that you specified in the source. Network traffic from other clients that is sent to the destination resource through the endpoint is blocked.
6. Repeat these steps for any sources that you want to grant access to the destination resource through the endpoint.

You can find the {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint by looking in the {{site.data.keyword.loganalysislong_notm}} logs for your {{site.data.keyword.satelliteshort}} location.
{: tip}




