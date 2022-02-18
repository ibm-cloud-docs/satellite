---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring location health
{: #location-monitor-health}

When you set up a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your location healthy. For more information, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default). For troubleshooting help, see [Debugging location health](/docs/satellite?topic=satellite-ts-locations-debug).
{: shortdesc}

You can review the host health from the **Locations** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat location ls`.

| Health state | Description
| --- | --- |
| `action required` | The location needs your attention. Check the status and message for more information, and try [debugging your location](/docs/satellite?topic=satellite-ts-locations-debug). |
| `completed` | {{site.data.keyword.satelliteshort}} completed the setup of the location control plane components on the hosts that you assigned to the control plane. The location is soon ready to be used.|
| `completing` | {{site.data.keyword.satelliteshort}} is setting up the location control plane components on the hosts that you assigned to the control plane. Check back in a little while.|
| `critical` | The {{site.data.keyword.satelliteshort}} location control plane needs your attention. Check the status and message for more information, and try [debugging your location control plane](/docs/satellite?topic=satellite-ts-locations-control-plane).|
| `failed` | {{site.data.keyword.satelliteshort}} did not successfully resolve issues in your location. For more information, see the status message. |
| `host required` | The {{site.data.keyword.satelliteshort}} location is created, but you must [assign hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane). Assign hosts in multiples of 3, such as 6, 9, or 12. |
| `normal` | The {{site.data.keyword.satelliteshort}} location is ready to use. |
| `provisioning` | The control plane for the {{site.data.keyword.satelliteshort}} is provisioning. You cannot assign hosts to other {{site.data.keyword.satelliteshort}} resources, such as clusters, in the location until the control plane is ready.|
| `resolving` | {{site.data.keyword.satelliteshort}} is trying to resolve issues for you, such as by assigning available hosts to the control plane to relieve capacity issues. For more information, see the status message. |
{: caption="Location health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the location. The second column describes what the health state means."}
