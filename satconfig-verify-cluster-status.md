---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}
  
# Verifying cluster status
{: #satconfig-verify-cluster-status}

After you set up your clusters to use with {{site.data.keyword.satelliteshort}} Config, verify the status of your clusters to make sure {{site.data.keyword.satelliteshort}} Config can deploy resources to them.
{: shortdesc}
  
You can view your cluster's status on the {{site.data.keyword.satelliteshort}} [**Clusters**](https://cloud.ibm.com/satellite/clusters){: external} dashboard.
 
| Cluster status | Description |
| --- | --- |
| `Registered` | The cluster is registered. You must apply the attach script and wait for the cluster status to change to `Active` before you can deploy resources to this cluster. |
| `Active` | You can start deploying resources to this cluster. |
| `Inactive` | The cluster cannot communicate with the backend API for more than one hour and is flagged as `Inactive`. You must fix the errors on the cluster before it can be active again. |
{: caption="Satellite Config cluster status" caption-side="bottom"}
  

