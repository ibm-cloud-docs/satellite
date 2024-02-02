---


copyright:
  years: 2022, 2024
lastupdated: "2024-02-02"

keywords: satellite, hybrid, multicloud, endpoint, link, endpoint secure

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Accessing your {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoints
{: #link-endpoint-secure}

By default, your {{site.data.keyword.openshiftlong_notm}} API {{site.data.keyword.satelliteshort}} link endpoints are protected to accept traffic from only the {{site.data.keyword.cloud_notm}} control plane. To access them from other sources, you must configure a source list for your endpoint. You can configure a source list from the console only.
{: shortdesc}
 
1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
2. From the **User endpoints** tab, click **Link endpoints**, and then click your endpoint.
3. In the **Access control list** section, click **Create rule**.
4. Enter a rule name and IP addresses of the clients that will be allowed to connect to the endpoint, and click **Add**.


    The value for **IP addresses** can be a single IP address, a CIDR block, or a comma-separated list. The value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.
    {: note}

5. Use the toggle to enable or disable the rule. After you enable a rule, network traffic to the destination through the endpoint is permitted only from clients that use an IP address in the range that you specified in the rule. Network traffic from other clients that is sent to the destination resource through the endpoint is blocked.
6. Repeat these steps for any clients that you want to grant access to.

You can find the {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint by looking in the {{site.data.keyword.loganalysislong_notm}} logs for your {{site.data.keyword.satelliteshort}} location. To open these logs, click **Open Dashboard** under **Logging for Link**. You can set up a filter in the monitoring instance to filter out the value you need. For example, search for `flowlog: rejected by` in the log and you will see an IP. Add a filter with a subnet matching that IP for your endpoint. This IP is logged when you use `oc` commands via link endpoint on the {{site.data.keyword.redhat_openshift_notm}} API. For more information, see [Logging for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-health).
{: tip}




