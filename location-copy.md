---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Copying a location
{: #location-copy}

To copy your {{site.data.keyword.satelliteshort}} location to a new location, you can review the details of the resources in your location and re-create the resources in a new location. You might also want copies of your location details for archival purposes.
{: shortdesc}

1. Get your location details and optionally save the details to a local file.

    ```sh
    ibmcloud sat location get --location <location_ID> [> mylocation.txt]
    ```
    {: pre}

2. List the hosts in your location.

    ```sh
    ibmcloud sat host ls --location <location_ID>
    ```
    {: pre}

3. Get the details of a host and optionally save the details to a local file. In particular, note the labels for **CPU** and **memory** so that you know your host configuration. Repeat this step for each host in your location.

    ```sh
    ibmcloud sat host get --location <location_ID> --host <host_ID> [> host1.txt]
    ```
    {: pre}

4. List your {{site.data.keyword.satelliteshort}} Link endpoints. A certain set of endpoints are created [by default](/docs/satellite?topic=satellite-default-link-endpoints), but note your other endpoints.

    ```sh
    ibmcloud sat endpoint ls --location <location_ID>
    ```
    {: pre}

5. Get the details of an endpoint and optionally save the details to a local file. Repeat this step for each endpoint in your location.

    ```sh
    ibmcloud sat endpoint get --location <location_ID> --endpoint <endpoint_ID> [> endpoint1.txt]
    ```
    {: pre}

6. List the clusters that are created in your {{site.data.keyword.satelliteshort}} location.

    ```sh
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}

7. Re-create your location.
    - For options to create the location and hosts, see [Deciding how to create your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
    - To create {{site.data.keyword.satelliteshort}} Link endpoints, see [cloud](/docs/satellite?topic=satellite-link-cloud-create#link-cloud) or [location](/docs/satellite?topic=satellite-link-cloud-create#link-location) endpoint instructions.
    - For clusters, see [Creating {{site.data.keyword.redhat_openshift_notm}} in {{site.data.keyword.satelliteshort}}](/docs/openshift?topic=openshift-satellite-clusters). If you manually deployed Kubernetes configurations instead of using {{site.data.keyword.satelliteshort}} Config, also see [Copying deployments to another cluster](/docs/openshift?topic=openshift-update_app#copy_apps_cluster).

8. Redeploy your apps to your cluster. Because {{site.data.keyword.satelliteshort}} Config works across {{site.data.keyword.satelliteshort}} locations, you can [add your new clusters to existing cluster groups](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups) that are already [subscribed to the configurations](/docs/satellite?topic=satellite-satcon-create) that you want to deploy.
9. Similar to {{site.data.keyword.satelliteshort}} Config, if you used {{site.data.keyword.satelliteshort}} storage configurations, you can assign these configurations to your new clusters to install the storage drivers.

When you are done and your new location is healthy, you can [remove the previous location](/docs/satellite?topic=satellite-host-remove#location-remove-console).
