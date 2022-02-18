---

copyright:
  years: 2021, 2022
lastupdated: "2022-02-18"

keywords: satellite, host, location

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I reuse a host in a different {{site.data.keyword.satelliteshort}} location?
{: #sat-host-reattach-fail}


When you try to reuse an existing host by assigning it to a new location, you see a message similar to the following error. 
{: tsSymptoms}

```sh
Unable to assign host. Could not find a host to assign to your Satellite cluster. 
```
{: screen}

If the host is not removed from the cluster before it is removed from the original location, the cluster master retains the information about the host. When the host is later added to a new location, a duplicate hostname is recognized and the assignment fails.
{: tsCauses}


If the cluster master still recognizes the host, you must rename and reload the host before you can attach it to a new location.
{: tsResolve}

1. Follow your host infrastructure provider's instructions to rename the host. 

    **For hosts provisioned through {{site.data.keyword.cloud_notm}}**:

    1. Log in to the {{site.data.keyword.cloud_notm}} console and navigate to **Classic infrastructure**>**Device list**.
    2. In the **Devices** table, locate the host you want to rename. Click the **Action** menu and select **Rename**.
    3. Rename the host. 

2. Follow your host infrastructure provider's instructions to reload the host.

    **For hosts provisioned through {{site.data.keyword.cloud_notm}}**:

    - **In the {{site.data.keyword.cloud_notm}} console**:
        1. Navigate to **Classic infrastructure**>**Device list**.
        2. In the **Devices** table, click on the host you want to reload. 
        3. From the **Actions** drop down menu, select **OS Reload**.
        4. Click **Reload**. Note that it may take several minutes for the reload process to complete.

    - **In the CLI:**
        1. List your hosts and note the **ID** of the host you want to reload.

            ```sh
            ibmcloud sl vs list
            ```
            {: pre}

        2. Run the `ibmcloud sl vs reload` command. Note that it may take several minutes for the reload process to complete.

            ```sh
            ibmcloud sl vs reload <host_id>
            ```
            {: pre}

        3. Check if the host is ready.

            ```sh
            ibmcloud sl vs ready <host_id>
            ```
            {: pre}

3. [Attach the host to the location](/docs/satellite?topic=satellite-attach-hosts). 

