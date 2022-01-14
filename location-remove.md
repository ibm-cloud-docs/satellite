---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-14"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Removing locations
{: #location-remove}

You can remove a {{site.data.keyword.satelliteshort}} location if you no longer need the clusters and other resources that run in the location.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts and clusters that run in the location. Note that the underlying host infrastructure is not automatically deleted when you delete a location because you manage the infrastructure yourself.
{: important}

## Removing locations from the console
{: #location-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your locations.
{: shortdesc}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.
2. [Remove all hosts](/docs/satellite?topic=satellite-host-remove) from your location.
3. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} **Locations** table, hover over the location that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
4. Click **Remove location**, enter the location name to confirm the deletion, and click **Remove**.

Now that the location is removed, check the hosts in your underlying infrastructure provider. To reuse the hosts for other purposes, you must reload the operating system. If you no longer need the hosts, delete them from your infrastructure provider.

## Removing locations from the CLI
{: #location-remove-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to remove your locations.
{: shortdesc}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.

2. [Remove all hosts](/docs/satellite?topic=satellite-host-remove-cli) from your location.

3. Remove the location.

    ```sh
    ibmcloud sat location rm --location <location_name_or_ID>
    ```
    {: pre}

4. Confirm that your location is removed. The location no longer is displayed in the output of the following command.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

Now that the location is removed, check the hosts in your underlying infrastructure provider. To reuse the hosts for other purposes, you must reload the operating system. If you no longer need the hosts, delete them from your infrastructure provider.

