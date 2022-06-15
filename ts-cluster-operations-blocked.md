---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I update or complete other actions with my cluster?
{: #ts-cluster-operations-blocked}

When you try to manage your cluster, such as to perform a cluster update, the operation fails with a message similar to the following.
{: tsSymptoms}

```sh
Cluster management is not currently available for this Satellite location. The location requires action to enable cluster operations. Please see logs for more detailed information.
```
{: screen}

Your {{site.data.keyword.satelliteshort}} location is currently in an unhealthy state, so cluster operations are blocked to prevent the operation from failing. 
{: tsCauses}

[Debug your location](/docs/satellite?topic=satellite-ts-locations-debug), such as reviewing the platform logs and common error messages to find the reason for the unhealthy state.
{: tsResolve}

When the location is healthy, you can resume managing your cluster.


