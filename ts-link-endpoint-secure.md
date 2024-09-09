---


copyright:
  years: 2022, 2024
lastupdated: "2024-09-09"

keywords: satellite, hybrid, multicloud, endpoint, link, endpoint secure

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I connect to my {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint?
{: #ts-link-endpoint-secure}

I can't connect to my {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint. I was able to before, what changed?  
{: tsSymptoms}

By default, your {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoints are protected. To access your link endpoints, you must configure an access control list (ACL) for your endpoint.
{: tsCauses}

To configure an access control list for your endpoint from the console, follow these steps.
{: tsResolve}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
1. Select the **Access control list** tab, then click **Create rule**.
1. On the ACL rule page, complete the following steps.
    1. Enter a **Rule name**.
    1. Enter the **IP addresses** of the clients that are allowed to connect to the endpoint. This value can be a single IP address, a CIDR block, or a comma-separated list. The value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.
    1. Select the endpoint (or multiple endpoints) this rule should control access to in your location. Network traffic to the destination through the endpoint is permitted only from clients that use an IP address in the range that you specified in the rule. Network traffic from other clients that is sent to the destination resource through the endpoint is blocked.
1. Click **Create**

Alternatively, you can configure an access control list for your endpoint from the CLI using a command similar to the following:

```sh
ibmcloud sat acl create --name NAME --location LOCATION --endpoint ENDPOINT --subnet SUBNET [--subnet SUBNET ...]
```
{: pre}

You can find the {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint by looking in the {{site.data.keyword.loganalysislong_notm}} logs for your {{site.data.keyword.satelliteshort}} location.
{: tip}
