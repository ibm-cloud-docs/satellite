---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-09"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Why can't I assign hosts to a cluster?
{: #assign-fails}

{: tsSymptoms}
You try to assign a host to {{site.data.keyword.satelliteshort}} resource such as a cluster, but the assignment does not succeed. When you check your host, the health state might be `unresponsive`, `unknown`, or `reload-required`.

{: tsCauses}
Your host might have encountered an issue during the bootstrapping process. For example, the underlying infrastructure of the host machine changed and no longer meets the [minimum requirements](/docs/satellite?topic=satellite-host-reqs), such as for network connectivity. You might have set up a firewall or other change that prevents access to a dependency.

In particular, the bootstrapping process depends upon the following access.
*   Access to RHEL Satellite servers and the required packages installed on the host machine.
*   Access to {{site.data.keyword.registrylong_notm}} endpoints to pull down required images.
*   Access to the Kubernetes master of the {{site.data.keyword.satelliteshort}} cluster that you want to assign the host to. Access might be blocked because the host cannot communicate with the service endpoint of the cluster, or because a Kubernetes resource within the cluster such as a webhook intercepts and blocks communication with the Kubernetes API server.

## Debugging hosts for connectivity issues
{: #debug-host-connectivity}

{: tsResolve}
If you want, you can debug the connectivity issues for your host. Otherwise, remove the host, reload the operating system, and attach the host back.

1.  Get the location ID where your host is attached, and note the {{site.data.keyword.cloud_notm}} multizone metro that the location is managed from. From the console, click your location, and then click the **Overview** tab. From the CLI, run the following command.
    ```
    ibmcloud sat location ls
    ```
    {: pre}
2.  Confirm that your host meets the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
2.  Check your host for connectivity issues.
    1.  Log in to your host machine, such as via SSH.
    2.  Check your [host network settings](/docs/satellite?topic=satellite-host-reqs#reqs-host-network) to ensure that your host can access the required ports and IP addresses, which might be blocked by a security group or firewall.
    3.  Check access to the required [{{site.data.keyword.cloud_notm}} multizone metro endpoints](#endpoints-to-verify).
    4.  For hosts that are assigned to clusters, get the details of the cluster master endpoint.
        ```
        ibmcloud ks cluster get -c <cluster_name_or_ID> | grep "Master URL"
        ```
        {: pre}
    5.  Check connectivity to the cluster master. If the curl request fails, your host might not have access to the endpoint, such as blocked by a security group, firewall, or private network.
        ```
        curl -k <master_URL>
        ```
        {: pre}
    6.  If you think you might have a webhook in the cluster that block access to the API server, see [Cluster cannot update because of broken webhook](/docs/openshift?topic=openshift-cs_troubleshoot#webhooks_update). Webhooks are often components for additional capabilities in your cluster, such as Cloud Paks, Istio, or container image security enforcement.
3.  After you resolve any connectivity issues, [check the health of your host](/docs/satellite?topic=satellite-ts-hosts-debug) for further information.
4.  Reassign your hosts if you continue to have issues.
    1.  [Remove the host](/docs/satellite?topic=satellite-hosts#host-remove) from your {{site.data.keyword.satelliteshort}} location.
    2.  Reload the operating system of your host by following the procedure of the underlying infrastructure provider.
    3.  Verify that you reloaded the host machine by logging in to the machine and checking for the following file.
        ```
        file /etc/satelittemachineidgeneration/machineidgenerated
        ```
        {: pre}

        If the file does not exist, you see a message similar to the following. Your host was reloaded and you can continue to the next step.
        ```
        /etc/satelittemachineidgeneration/machineidgenerated: cannot open (No such file or directory)
        ```
        {: screen}

        If the file exists, you see a message similar to the following. You must reload your host machine operating system before continuing to the next step.
        ```
        /etc/satelittemachineidgeneration/machineidgenerated: empty
        ```
        {: screen}
    4.  Confirm that your host meets the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
    5.  [Attach the host](/docs/satellite?topic=satellite-hosts#attach-hosts) back to your {{site.data.keyword.satelliteshort}} location.
    6.  Check that the host is attached to your location and **unassigned**. From the console, click your location, and then click the **Hosts** tab. From the CLI, run the following command.
        ```
        ibmcloud sat host ls --location <location_name_or_ID>
        ```
        {: pre}
    7.  [Assign the host](/docs/satellite?topic=satellite-hosts#host-assign) to your {{site.data.keyword.satelliteshort}} resource, such as a cluster.
    8.  Check that the host is **assigned** to your cluster. The process might take an hour to complete. From the console, click your location, and then click the **Hosts** tab. From the CLI, run the following command.
        ```
        ibmcloud sat host ls --location <location_name_or_ID>
        ```
        {: pre}

## Endpoints to verify connectivity by {{site.data.keyword.cloud_notm}} multizone metro
{: #endpoints-to-verify}

Review the following table to help troubleshoot network connectivity issues to {{site.data.keyword.cloud_notm}} endpoints that are required for the bootstrapping process of a {{site.data.keyword.satelliteshort}} host.
{: shortdesc}



| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint| `nslookup origin.eu-de.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.eu-de.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint| `curl -v https://private.eu-de.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://de.icr.io` |
{: summary="The rows are read from left to right. The first row contains an endpoint to check. The second row contains a command that you can run to check connectivity to a required endpoint in the {{site.data.keyword.cloud_notm}} multizone metro."}
{: class="simple-tab-table"}
{: caption="Endpoints to test when your {{site.data.keyword.satelliteshort}} location is managed from Frankfurt." caption-side="top"}
{: #check-ep-frankfurt}
{: tab-title="Frankfurt"}
{: tab-group="check-ep"}

| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint | `nslookup origin.eu-gb.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.eu-gb.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint | `curl -v https://private.eu-gb.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://uk.icr.io` |
{: summary="The rows are read from left to right. The first row contains an endpoint to check. The second row contains a command that you can run to check connectivity to a required endpoint in the {{site.data.keyword.cloud_notm}} multizone metro."}
{: class="simple-tab-table"}
{: caption="Endpoints to test when your {{site.data.keyword.satelliteshort}} location is managed from London." caption-side="top"}
{: #check-ep-london}
{: tab-title="London"}
{: tab-group="check-ep"}

| Endpoint | Command to check endpoint |
| --- | ------------------------- |
| Public regional endpoint | `nslookup origin.us-east.containers.cloud.ibm.com` |
| Public regional bootstrap endpoint | `curl -v https://origin.us-east.containers.cloud.ibm.com/bootstrap/firstboot` |
| Private regional bootstrap endpoint | `curl -v https://private.us-east.containers.cloud.ibm.com/bootstrap/firstboot` |
|{{site.data.keyword.registrylong_notm}} region | `curl -v https://us.icr.io` |
{: summary="The rows are read from left to right. The first row contains an endpoint to check. The second row contains a command that you can run to check connectivity to a required endpoint in the {{site.data.keyword.cloud_notm}} multizone metro."}
{: class="simple-tab-table"}
{: caption="Endpoints to test when your {{site.data.keyword.satelliteshort}} location is managed from Washington DC." caption-side="top"}
{: #check-ep-dc}
{: tab-title="Washington, DC"}
{: tab-group="check-ep"}
