---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-26"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Hosts
{: #ts-hosts}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your hosts and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the hosts by reviewing the provided health information.
{: shortdesc}

## Debugging host health
{: #ts-hosts-debug}

Review the health of your hosts, and use the error messages to resolve any issues that you might have.
{: shortdesc}

1.  Review the health and status of your hosts. From the CLI, list your hosts in a location. From the console, click your location, and then click the **Hosts** tab.
    ```
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Name               ID                     State        Status   Cluster          Worker ID   Worker IP   
    satellite-test-1   aaaaa1a11aaaaaa111aa   assigned     -        infrastructure   -           -   
    ```
    {: screen}
2.  Review the states and the steps to resolve the issue in the following table.
3.  Check that your hosts still meet the [minimum requirements](/docs/satellite?topic=satellite-requirements#reqs-host), such as for network connectivity. For example, if you change the underlying infrastructure environment where the host machine is to block access on the public network, you might make the host unsupported.
4.  If your host still has issues, try to [remove, update, and re-add the host](/docs/satellite?topic=satellite-hosts#host-update).

| Health state | Description
| --- | --- |
| `assigned` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster. View the status for more information. If the status is `-`, the hosts did not complete the bootstrapping process to the {{site.data.keyword.satelliteshort}} resource. For hosts that you just assigned, wait an hour or so for the process to complete. If you still see the status, [log in to the host to continue debugging](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-login).| 
| `provisioning` | The host is attached to the {{site.data.keyword.satelliteshort}} location and is in the process of bootstrapping to become part of a {{site.data.keyword.satelliteshort}} resource, such as the worker node of a {{site.data.keyword.openshiftlong_notm}} cluster.|
| `ready` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign).|
| `normal` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster, and ready for usage. |
| `reload-required` | The host is attached to the {{site.data.keyword.satelliteshort}} location, but requires a reload before it can be assigned to a {{site.data.keyword.satelliteshort}} resource. For example, you might have deleted a {{site.data.keyword.satelliteshort}} cluster, and now all of the hosts from the cluster require a reload. To reload a host, you must [remove the host from the location](/docs/satellite?topic=satellite-hosts#host-remove), reload the operating system in the underlying infrastructure provider, and [add the host](/docs/satellite?topic=satellite-hosts#add-hosts) back to the location. |
| `unassigned` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-hosts#host-assign). If you tried to assign the host unsuccessfully, see [Cannot assign hosts to a cluster](/docs/satellite?topic=satellite-ts-hosts#assign-fails).|
| `unknown` | The health of the host is unknown. If the host is unassigned, try [assigning the host](/docs/satellite?topic=satellite-hosts#host-assign) to a {{site.data.keyword.satelliteshort}} resource, such as a cluster. If the host is assigned, try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts). |
| `unresponsive` | The host did not check in with the {{site.data.keyword.satelliteshort}} location control plane within the past 5 minutes. The host cannot be assigned when it is unresponsive. Try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts), particularly the network connectivity. |
{: caption="Host health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the host. The second column describes what the health state means."}

<br />


## Logging in to a host machine to debug
{: #ts-hosts-login}

You might need to log in to your host machine to debug a host issue further.
{: shortdesc}

You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-hosts#host-remove) and reload the operating system to restore the ability to SSH into the machine.
{: note}

1.  Log in to the machine.
    ```
    ssh root@<IP_address>
    ```
    {: pre}
2.  Check the various log output files from the host registration and host bootstrapping processes. Replace `<filepath>` with the following files to check in order.
    ```
    tail <filepath>
    ```
    {: pre}
    1.  The `nohup.out` logs from the host registration attempt.
    2.  The `/var/log/firstboot.log` for the first bootstrapping attempt. If the host registration failed, you do not have this file.
    3.  The `/tmp/bootstrap/bootstrap_base.log` for the base bootstrapping process, if the first boot was unsuccessful. If the host registration failed, you do not have this file.
3.  Review the logs for errors. Some common errors include the following.
    <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
    <caption>Common host machine registration and bootstrapping errors</caption>
    <thead>
    <th>Message</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><pre class="screen"><code>export HOME=/root
    HOME=/root
    '[' '!' -f /var/log/firstboot.flag ']'
    ~</code></pre></td>
    <td>The first boot did not complete successfully. Check the <code>/tmp/bootstrap/bootstrap_base.log</code> file and continue looking for errors.</td>
    </tr>
    <tr>
    <td><code>No package matching '\''container-selinux'\'' found available, installed or updated</code>.<br><br><code>No package rh-python36 available. Error: Nothing to do</code>.<br><br>(Note that the package name might be replaced with another package name.)</td>
    <td>See [Host registration script fails](#host-registration-script-fails).</td>
    </tr>
    <tr>
    <td><pre class="screen"><code>curl: (6) Could not resolve host: <URL>.com; Unknown error
    tar -xvf bootstrap.tar
    tar: This does not look like a tar archive
    tar: Exiting with failure status due to previous errors
    [[ -n ‘’ ]]
    echo ‘Failed to untar bootstrap.tar’
    Failed to untar bootstrap.tar
    + rm -rf /tmp/bootstrap</code></pre></td>
    <td>The machine cannot be reached on the network. Check that your machine meets the [minimum requirements for network connectivity](/docs/satellite?topic=satellite-requirements#reqs-host), [remove the host](/docs/satellite?topic=satellite-hosts#host-remove), and try to [add](/docs/satellite?topic=satellite-hosts#add-hosts) and [assign](/docs/satellite?topic=satellite-hosts#host-assign) the host again. Alternatively, the infrastructure provider network might have issues, such as a failed connection. Consult the infrastructure provider documentation for further debugging steps.</td>
    </tr>
    </tbody>
    </table>

<br />


## Cannot SSH into host machine
{: #ssh-login-denied}

{: tsSymptoms}
When you try to SSH into a host machine that is assigned in {{site.data.keyword.satelliteshort}}, you see a message similar to the following.
```
Permission denied, please try again.
```
{: screen}

{: tsCauses}
After the host is successfully assigned to a {{site.data.keyword.satelliteshort}} location control plane or cluster, {{site.data.keyword.satelliteshort}} disables the ability to SSH into the host for security purposes.

{: tsResolve}
You cannot SSH into the host. If you need to modify settings on host machines in a cluster, try deploying a daemon set, such as in the [Tuning performance](/docs/containers?topic=containers-kernel) example.

If you remove a host from your location or remove the entire location, you must reload the machine in your host infrastructure provider to SSH into the host again.

<br />


## Host registration script fails
{: #host-registration-script-fails}

{: tsSymptoms}
When you SSH into your own infrastructure machine that you want to add as a {{site.data.keyword.satelliteshort}} host and run the host registration script, you see a message similar to the following.
```
No package rh-python36 available.
Error: Nothing to do
```
{: screen}

{: tsCauses}
Your machine does not meet the minimum requirements to become a {{site.data.keyword.satelliteshort}} host. In particular, you must have the following packages installed on your RHEL 7 machine.
```
Repository 'rhel-ha-for-rhel-7-server-eus-rpms' is enabled for this system.
Repository 'rhel-server-rhscl-7-rpms' is enabled for this system.
Repository 'rhel-7-server-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-eus-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-rh-common-rpms' is enabled for this system.
Repository 'rhel-7-server-eus-rpms' is enabled for this system.
Repository 'rhel-ha-for-rhel-7-server-rpms' is enabled for this system.
Repository 'rhel-rs-for-rhel-7-server-eus-rpms' is enabled for this system.
Repository 'rhel-rs-for-rhel-7-server-rpms' is enabled for this system.
Repository 'rhel-7-server-rpms' is enabled for this system.
Repository 'rhel-7-server-supplementary-rpms' is enabled for this system.
Repository 'rhel-7-server-extras-rpms' is enabled for this system.
Repository 'rhel-7-server-eus-supplementary-rpms' is enabled for this system.
```
{: screen}

{: tsResolve}
1.  Add the required packages to your machine. For example, in IBM Cloud infrastructure you can run the following commands to add the required packages.
    1.  Refresh the Red Hat packages on your machine.
        ```
        subscription-manager refresh
        ```
        {: pre}

        If you see an error such as `Network error, unable to connect to server. Please see /var/log/rhsm/rhsm.log for more information.`, check the security group and other network settings for your machine to make sure that you have connectivity to the internet.
        {: tip}

    2.  Enable the package repositories on your machine.
        ```
        subscription-manager repos --enable=*
        ```
        {: pre}
2.  Make sure that your machine meets the other [host minimum requirements](/docs/satellite?topic=satellite-requirements#reqs-host), such as minimum CPU and memory sizes and public network connectivity.
3.  [Run the registration script](/docs/satellite?topic=satellite-hosts#add-hosts) on your machine again.

<br />


## Cannot assign hosts to a cluster
{: #assign-fails}

{: tsSymptoms}
You try to assign a host to {{site.data.keyword.satelliteshort}} resource such as a cluster, but the assignment does not succeed. When you check your host, the health state might be `unresponsive`, `unknown`, or `reload-required`.

{: tsCauses}
Your host might have encountered an issue during the bootstrapping process. For example, the underlying infrastructure of the host machine changed and no longer meets the [minimum requirements](/docs/satellite?topic=satellite-requirements#reqs-host), such as for network connectivity. You might have set up a firewall or other change that prevents access to a dependency.

In particular, the bootstrapping process depends upon the following access.
*   Access to RHEL Satellite servers and the required packages installed on the host machine.
*   Access to {{site.data.keyword.registrylong_notm}} endpoints to pull down required images.
*   Access to the Kubernetes master of the {{site.data.keyword.satelliteshort}} cluster that you want to assign the host to. Access might be blocked because the host cannot communicate with the service endpoint of the cluster, or because a Kubernetes resource within the cluster such as a webhook intercepts and blocks communication with the Kubernetes API server.

{: tsResolve}
If you want, you can debug the connectivity issues for your host. Otherwise, remove the host, reload the operating system, and add the host back.

1.  Get the location ID where your host is attached, and note the {{site.data.keyword.cloud_notm}} multizone metro that the location is managed from. From the console, click your location, and then click the **Overview** tab. From the CLI, run the following command.
    ```
    ibmcloud sat location ls
    ```
    {: pre}
2.  Confirm that your host meets the [minimum requirements](/docs/satellite?topic=satellite-requirements#reqs-host).
2.  Check your host for connectivity issues.
    1.  Log in to your host machine, such as via SSH.
    2.  Check access to RHEL Satellite servers. If the following command succeeds, your host can access the servers. If the command fails, your host might not have access to the public network, such as blocked by a security group or firewall.
        ```
        yum install rh-python36 -y
        ```
        {: pre}
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
3.  After you resolve any connectivity issues, [check the health of your host](#ts-hosts-debug) for further information.
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
    4.  Confirm that your host meets the [minimum requirements](/docs/satellite?topic=satellite-requirements#reqs-host).
    5.  [Add the host](/docs/satellite?topic=satellite-hosts#add-hosts) back to your {{site.data.keyword.satelliteshort}} location.
    6.  Check that the host is added to your location and **unassigned**. From the console, click your location, and then click the **Hosts** tab. From the CLI, run the following command.
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

### Endpoints to verify connectivity by {{site.data.keyword.cloud_notm}} multizone metro
{: #endpoints-to-verify}

Review the following table to help troubleshoot network connectivity issues to {{site.data.keyword.cloud_notm}} endpoints that are required for the bootstrapping process of a {{site.data.keyword.satelliteshort}} host.
{: shortdesc}



| Command to check endpoint |
| ------------------------- |
| `nslookup origin.eu-gb.containers.cloud.ibm.com` |
| `curl -v https://origin.eu-gb.containers.cloud.ibm.com/bootstrap/firstboot` |
| `curl -v https://private.eu-gb.containers.cloud.ibm.com/bootstrap/firstboot` |
| `curl -v https://uk.icr.io` |
{: summary="Each row contains a command that you can run to check connectivity to a required endpoint in the {{site.data.keyword.cloud_notm}} multizone metro."}
{: class="simple-tab-table"}
{: caption="Endpoints to test when your {{site.data.keyword.satelliteshort}} location is managed from London." caption-side="top"}
{: #check-ep-london}
{: tab-title="London"}
{: tab-group="check-ep"}

| Command to check endpoint |
| ------------------------- |
| `nslookup origin.us-east.containers.cloud.ibm.com` |
| `curl -v https://origin.us-east.containers.cloud.ibm.com/bootstrap/firstboot` |
| `curl -v https://private.us-east.containers.cloud.ibm.com/bootstrap/firstboot` |
| `curl -v https://us.icr.io` |
{: summary="Each row contains a command that you can run to check connectivity to a required endpoint in the {{site.data.keyword.cloud_notm}} multizone metro."}
{: class="simple-tab-table"}
{: caption="Endpoints to test when your {{site.data.keyword.satelliteshort}} location is managed from Washington DC." caption-side="top"}
{: #check-ep-dc}
{: tab-title="Washington, DC"}
{: tab-group="check-ep"}
