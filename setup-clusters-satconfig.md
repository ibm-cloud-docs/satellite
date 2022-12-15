---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-15"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Automatically granting {{site.data.keyword.satelliteshort}} Config access to your clusters
{: #setup-clusters-satconfig}

By default, clusters that you create in a {{site.data.keyword.satelliteshort}} location have {{site.data.keyword.satelliteshort}} Config components automatically installed. However, you must grant the service accounts that {{site.data.keyword.satelliteshort}} Config uses the appropriate access to view and manage Kubernetes resources in each cluster. 
{: shortdesc}

If you do not grant {{site.data.keyword.satelliteshort}} Config access, you cannot later use the {{site.data.keyword.satelliteshort}} Config functionality to view or deploy Kubernetes resources for your clusters.

To give {{site.data.keyword.satelliteshort}} Config access to manage Kubernetes resources using the console, make sure you select **Enable Satellite Config** when you create the cluster. To accomplish this using the CLI, specify the `--enable-config-admin` option when you create the cluster.

If you didn't give {{site.data.keyword.satelliteshort}} Config access at cluster creation time, follow the steps in [Manually granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig-access).


  
