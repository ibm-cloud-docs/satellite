---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-22"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I assign hosts to a cluster?
{: #assign-fails}


You try to assign a host to {{site.data.keyword.satellitelong_notm}} resource such as a cluster, but the assignment does not succeed.
{: tsSymptoms}

When you check your host, the health state might be `unresponsive`, `unknown`, or `reload-required`.

Your host might have encountered an issue during the bootstrapping process. For example, the underlying infrastructure of the host machine changed and no longer meets the [minimum requirements](/docs/satellite?topic=satellite-host-reqs), such as for network connectivity.
{: tsCauses}

You might have set up a firewall or other change that prevents access to a dependency.

In particular, the bootstrapping process depends upon the following access.
- Access to RHEL Satellite servers and the required packages installed on the host machine.
- Access to {{site.data.keyword.registrylong_notm}} endpoints to pull down required images.
- Access to the Kubernetes master of the {{site.data.keyword.satelliteshort}} cluster that you want to assign the host to. Access might be blocked because the host cannot communicate with the service endpoint of the cluster, or because a Kubernetes resource within the cluster such as a webhook intercepts and blocks communication with the Kubernetes API server.

## Debugging hosts for connectivity issues
{: #debug-host-connectivity}

If you want, you can debug the connectivity issues for your host.
{: tsResolve}

Otherwise, remove the host, reload the operating system, and attach the host back.

1. Get the location ID where your host is attached, and note the {{site.data.keyword.cloud_notm}} multizone metro that the location is managed from. From the console, click your location, and then click the **Overview** tab. From the CLI, run the following command.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

2. Confirm that your host meets the [minimum requirements](/docs/satellite?topic=satellite-host-reqs) and verify that the hostname contains only lowercase alphanumeric characters, `-`, or `.`.
3. Check your host for connectivity issues.
    1. Log in to your host machine, such as via SSH.
    2. Check your [host network settings](/docs/satellite?topic=satellite-reqs-host-network) to ensure that your host can access the required ports and IP addresses, which might be blocked by a security group or firewall.
    3. Check access to the required [{{site.data.keyword.cloud_notm}} multizone metro endpoints](#endpoints-to-verify).
    4. For hosts that are assigned to clusters, get the details of the cluster master endpoint.
        ```sh
        ibmcloud ks cluster get -c <cluster_name_or_ID> | grep "Master URL"
        ```
        {: pre}

    5. Check connectivity to the cluster master. If the curl request fails, your host might not have access to the endpoint, such as blocked by a security group, firewall, or private network.
        ```sh
        curl -k <master_URL>
        ```
        {: pre}

    6. If you think you might have a webhook in the cluster that block access to the API server, see [Cluster cannot update because of broken webhook](/docs/openshift?topic=openshift-webhooks_update). Webhooks are often components for additional capabilities in your cluster, such as Cloud Paks, Istio, or container image security enforcement.
4. After you resolve any connectivity issues, [check the health of your host](/docs/satellite?topic=satellite-ts-hosts-debug) for further information.
5. Reassign your hosts if you continue to have issues.
    1. [Remove the host](/docs/satellite?topic=satellite-host-remove) from your {{site.data.keyword.satelliteshort}} location.
    2. Reload the operating system of your host by following the procedure of the underlying infrastructure provider.
    3. Verify that you reloaded the host machine by logging in to the machine and checking for the following file.
        ```sh
        file /etc/satelittemachineidgeneration/machineidgenerated
        ```
        {: pre}

        If the file does not exist, you see a message similar to the following. Your host was reloaded and you can continue to the next step.
        ```sh
        /etc/satelittemachineidgeneration/machineidgenerated: cannot open (No such file or directory)
        ```
        {: screen}

        If the file exists, you see a message similar to the following. You must reload your host machine operating system before continuing to the next step.
        ```sh
        /etc/satelittemachineidgeneration/machineidgenerated: empty
        ```
        {: screen}

    4. Confirm that your host meets the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
    5. [Attach the host](/docs/satellite?topic=satellite-attach-hosts) back to your {{site.data.keyword.satelliteshort}} location.
    6. Check that the host is attached to your location and **unassigned**. From the console, click your location, and then click the **Hosts** tab. From the CLI, run the following command.
        ```sh
        ibmcloud sat host ls --location <location_name_or_ID>
        ```
        {: pre}

    7. [Assign the host](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) to your {{site.data.keyword.satelliteshort}} resource, such as a cluster.
    8. Check that the host is **assigned** to your cluster. The process might take an hour to complete. From the console, click your location, and then click the **Hosts** tab. From the CLI, run the following command.
        ```sh
        ibmcloud sat host ls --location <location_name_or_ID>
        ```
        {: pre}

## Endpoints to verify connectivity by {{site.data.keyword.cloud_notm}} multizone metro
{: #endpoints-to-verify}

Review the following table to help troubleshoot network connectivity issues to {{site.data.keyword.cloud_notm}} endpoints that are required for the bootstrapping process of a {{site.data.keyword.satelliteshort}} host.
{: shortdesc}



| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint | `nslookup origin.us-south.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.us-south.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint| `curl -v https://private.us-south.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://us.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from Dallas." caption-side="bottom"}
{: #check-ep-dallas}
{: tab-title="Dallas"}
{: tab-group="check-ep"}

| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint| `nslookup origin.eu-de.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.eu-de.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint| `curl -v https://private.eu-de.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://de.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from Frankfurt." caption-side="bottom"}
{: #check-ep-frankfurt}
{: tab-title="Frankfurt"}
{: tab-group="check-ep"}



| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint| `nslookup origin.br-sao.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.br-sao.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint| `curl -v https://private.br-sao.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://br-sao.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from Sao Paulo." caption-side="bottom"}
{: #check-ep-saopaulo}
{: tab-title="Sao Paulo"}
{: tab-group="check-ep"}





| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint | `nslookup origin.eu-gb.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.eu-gb.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint | `curl -v https://private.eu-gb.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://uk.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from London." caption-side="bottom"}
{: #check-ep-london}
{: tab-title="London"}
{: tab-group="check-ep"}

| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint| `nslookup origin.jp-tok.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.jp-tok.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint | `curl -v https://private.jp-tok.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://jp.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from Tokyo." caption-side="bottom"}
{: #check-ep-tokyo}
{: tab-title="Tokyo"}
{: tab-group="check-ep"}

| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint| `nslookup origin.ca-tor.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.ca-tor.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint | `curl -v https://private.ca-tor.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://jp.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from Toronto." caption-side="bottom"}
{: #check-ep-toronto}
{: tab-title="Toronto"}
{: tab-group="check-ep"}

| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint | `nslookup origin.us-east.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.us-east.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint | `curl -v https://private.us-east.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://us.icr.io` |
{: class="simple-tab-table"}
{: caption="Endpoints to test when your location is managed from Washington DC." caption-side="bottom"}
{: #check-ep-dc}
{: tab-title="Washington, DC"}
{: tab-group="check-ep"}
