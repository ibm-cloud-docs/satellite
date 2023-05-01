---

copyright:
  years: 2022, 2023
lastupdated: "2023-05-01"

keywords: satellite, hybrid, multicloud, endpoint, link, endpoint secure

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Accessing your {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoints
{: #link-endpoint-secure}

By default, your {{site.data.keyword.openshiftlong_notm}} API {{site.data.keyword.satelliteshort}} link endpoints are protected to accept traffic from only the {{site.data.keyword.cloud_notm}} control plane. To access them from other sources, you must configure a source list for your endpoint. You can configure a source list from the console only. 
{: shortdesc}
 
1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
2. From the **Link endpoints** tab, click the name of your endpoint.
3. In the **Source list** section, click **Add source**.
  Service source should be subset of the following CIDRs: `10.0.0.0/8`, `161.26.0.0/16`, `166.8.0.0/14`, and `172.16.0.0/12`. 
  {: note}
  
4. Click **Configure source** to enter a source name and the IP address or subnet CIDR for the client that you want to connect to the endpoint, and click **Add**.
5. Use the toggle to enable the source to connect to the destination resource. After you enable a source, network traffic to the destination through the endpoint is permitted only from clients that use an IP address in the range that you specified in the source. Network traffic from other clients that is sent to the destination resource through the endpoint is blocked.
6. Repeat these steps for any sources that you want to grant access to the destination resource through the endpoint.

You can find the {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint by looking in the {{site.data.keyword.loganalysislong_notm}} logs for your {{site.data.keyword.satelliteshort}} location. For more information, see [Logging for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-health).
{: tip}




