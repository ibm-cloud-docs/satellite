---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why doesn't my cluster add-on work?
{: #addon-errors}

When you try to use a cluster add-on in {{site.data.keyword.satellitelong}}, the cluster add-on does not work. For example, you installed the Kubernetes web terminal add-on but cannot open the add-on.
{: tsSymptoms}

Add-ons might not work for several reason.
{: tsCauses}

- The add-on is not supported for clusters in a {{site.data.keyword.satelliteshort}} location.
- The add-on is in an unhealthy state.
- The add-on settings are misconfigured, such as renaming or editing a ConfigMap.
- The add-on cannot be backed up in {{site.data.keyword.cos_full_notm}} due to conflicting service instance and bucket endpoints.

Take the following steps to troubleshoot the add-on.
{: tsResolve}

1. Check that the [add-on is supported](/docs/openshift?topic=openshift-managed-addons#addons-satellite). If not, uninstall the add-on.
2. If the add-on is misconfigured, refresh the cluster master to restore the add-on to the default settings. 
    ```sh
    ibmcloud oc cluster master refresh -c <cluster_name_or_ID>
    ```
    {: pre}

3. If the add-on is in critical state, review the {{site.data.keyword.cos_full_notm}} instance and bucket that backs up the cluster data. The instance and endpoint must have matching endpoints, such as a **Global** instance with a **Cross Region** bucket (`us-geo` endpoint), or a **Regional** instance with a **Regional** bucket (`us-east` endpoint). If the endpoints do not match, you must re-create the {{site.data.keyword.satelliteshort}} location with matching {{site.data.keyword.cos_full_notm}} instance and bucket endpoints.
4. For more information, [review the add-on state and statuses](/docs/containers?topic=containers-debug_addons).


