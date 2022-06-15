---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Why is {{site.data.keyword.cloud_notm}} unable to check my location's health?
{: #ts-location-healthcheck}


After you assign hosts to your {{site.data.keyword.satelliteshort}} location control plane, you see a message similar to the following.
{: tsSymptoms}

```sh
R0047 IBM Cloud is unable to use the health check Link endpoint to check the location's health.
```
{: screen}


The location control plane is not accessible from {{site.data.keyword.cloud_notm}} through {{site.data.keyword.satelliteshort}} Link due to one of the following reasons:
{: tsCauses}

- The automated health check endpoint was disabled.
- The hosts are behind a firewall that blocks traffic to or from {{site.data.keyword.cloud_notm}}.


To troubleshoot the health check for the Link endpoint:
{: tsResolve}

1. Verify that the automated health check endpoint for your location is enabled.
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
    2. From the **Link endpoints** tab, verify that the **Status** for the endpoint in the format `satellite-healthcheck-<location_ID>` is toggled to **Enabled**.

2. Verify that the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable.
    1. [Set up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-health#setup-la).
    2. In the [**Logging** dashboard](https://cloud.ibm.com/observe/logging){: external}, click **Open Dashboard** for your {{site.data.keyword.la_short}} instance. The {{site.data.keyword.la_short}} dashboard opens.
    3. Check the `Endpoint health status` logs. These logs report the results of health checks for the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint.
        - If logs report `Successfully checked endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable. Continue to the next step.
        - If logs report `Failed to reach endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is unreachable.
3. Resolve the issue that prevents the tunnel from being reached.
    - If you have a firewall in your infrastructure provider, allow traffic from the hosts to the location control plane access through the firewall. For example, see [AWS Security group](/docs/satellite?topic=satellite-aws#aws-reqs-secgroup).
    - If you still see the `R0047` error message, [open a support case](/docs/satellite?topic=satellite-get-help#help-support).

