---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-21"

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



# Setting up {{site.data.keyword.satelliteshort}} locations
{: #locations}

Set up an {{site.data.keyword.satellitelong}} location to represent a data center that you fill with your own infrastructure resources, and start running {{site.data.keyword.cloud_notm}} services on your own infrastructure.
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

**Steps to set up your first location.**
1. [Create a location](#location-create).
2. [Add 3 hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
3. [Assign at least 3 hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
4. [Add more hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
5. [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) and assign the hosts to the cluster, so that you can run OpenShift workloads in your location.

<br />


## Understanding {{site.data.keyword.satelliteshort}} locations
{: #location-concept}

A {{site.data.keyword.satelliteshort}} location represents a data center that you can fill with your own infrastructure resources to run and extend {{site.data.keyword.cloud_notm}} services in your location.
{: shortdesc}

The following diagram presents the concept of setting up your own {{site.data.keyword.satelliteshort}} locations in the Asia Pacific metro, and running the same application in your location as well as in {{site.data.keyword.cloud_notm}}.

![Concept overview of Satellite locations in Asia Pacific](/images/location-ov.png){: caption="Figure 1. A conceptual overview of setting up {{site.data.keyword.satelliteshort}} locations." caption-side="bottom"}

**{{site.data.keyword.satelliteshort}} control plane master**: When you create a location, a highly available control plane master is automatically created in one of the {{site.data.keyword.cloud_notm}} multizone metros that you selected during location creation. The control plane master securely connects your location to {{site.data.keyword.cloud_notm}} and stores the configuration of your location. If service updates are available, the control plane master automatically makes these updates available to the control plane worker nodes. All location metadata is automatically backed up to an {{site.data.keyword.cos_full_notm}} instance in your account.

**{{site.data.keyword.satelliteshort}} control plane worker**: After you create the location, you must add at least 3 hosts to the location control plane as worker nodes. Then, the location control plane worker is ready to manage your location resources. Other components are also set up, such as IBM monitoring components to monitor the health of your location and hosts and automatically resolve issues. For more information, see the [service architecture](/docs/satellite?topic=satellite-service-architecture).

**{{site.data.keyword.satelliteshort}} Link**: A {{site.data.keyword.satelliteshort}} Link connector component is automatically created in the location control plane worker to securely connect the location back to the control plane master in {{site.data.keyword.cloud_notm}}. You can use the Link component to control and audit network traffic in and out of the location.

**Hosts**: To fill the {{site.data.keyword.satelliteshort}} location with your own infrastructure, you attach hosts such as on-prem bare metal, virtual machines in other cloud providers, edge computing devices, and more. All hosts must the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host). The hosts become the compute capacity in your location. You must assign at least three of these hosts as worker nodes to the {{site.data.keyword.satelliteshort}} control plane. All other hosts can be used to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters on your own infrastructure.

**{{site.data.keyword.openshiftlong_notm}} clusters**: You can use host capacity in your location to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.openshiftlong_notm}} clusters. Your hosts become the worker nodes of your cluster in a `upi` (or user-provided infrastructure) worker pool. The cluster master runs on and is managed from your {{site.data.keyword.satelliteshort}} control plane worker nodes. {{site.data.keyword.openshiftshort}} is automatically set up for these clusters, and you can run any Kubernetes workload that you want on the clusters.

**{{site.data.keyword.satelliteshort}} configurations**: To help you deploy and manage workloads across {{site.data.keyword.openshiftshort}} clusters in your location and in {{site.data.keyword.cloud_notm}}, you can use {{site.data.keyword.satelliteshort}} configurations. Simply upload a Kubernetes resource YAML file as a version to your configuration and subscribe the clusters where you want to deploy the resource. Your Kubernetes resources and subsequent version updates are automatically deployed to the subscribed clusters, and you also can view an inventory of all the Kubernetes resources that are managed by a {{site.data.keyword.satelliteshort}} configuration.

**{{site.data.keyword.cloud_notm}} services**: You can use host capacity in your location to run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services on your own infrastructure. Simply select the service that you want to run in your location from the {{site.data.keyword.cloud_notm}} catalog, and start creating service instances in your location. Additionally, you use the same {{site.data.keyword.cloud_notm}} platform tools to manage identity and access, key management, certificate management, logging and monitoring, and other security and compliance controls for these services.

<br />


## Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you add to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

1. Set up a highly available location control plane worker with enough compute capacity to manage the resources in your {{site.data.keyword.satelliteshort}} location.
   *  **Minimum size**: To get started, you must add and assign at least 3 hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host) of 4 CPU, 16 GB memory, and 100 GB storage. You assign these hosts to 3 separate zones.
   *  **High availability**: When you assign hosts to the location control plane, you must assign at least 1 host to each of the 3 available zones of your {{site.data.keyword.cloud_notm}} multizone metro that you selected during location creation. To make the location control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might use 3 hosts that run in separate availability zones in your cloud provider, or that run in three separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable. For more information, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).
   *  **Compute capacity**: {{site.data.keyword.satelliteshort}} monitors your location for capacity. When the location reaches 70% capacity, you see a warning status to notify you to add more hosts to the location. If the location reaches 80% capacity, the state changes to **critical** and you see another warning that tells you to add more hosts to the location.
2. Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then IBM can assign the hosts to your location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.
3. To decide on the size and number of hosts to add to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations:
   * How many resources does my app require?
   * What else besides my app might use resources in the cluster?
   * What type of availability do I want my workload to have?
   * How many worker nodes (hosts) do I need to handle my workload?
   * How do I monitor resource usage and capacity in my cluster?

<br />


## Creating {{site.data.keyword.satelliteshort}} locations
{: #location-create}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north and south of the country. A {{site.data.keyword.satelliteshort}} location represents a data center that you fill with your own infrastructure resources to run your own workloads and {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

### Creating locations from the console
{: #location-create-console}

Use the {{site.data.keyword.satelliteshort}} console to create your location.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click **Create location**.
2. Enter a name and an optional description for your location. Do not reuse the name of a previously deleted location.
3. Select the {{site.data.keyword.cloud_notm}} multizone metro that you want to use to manage your location. For more information about why you must select a {{site.data.keyword.cloud_notm}} multizone metro, see [Understanding supported {{site.data.keyword.cloud_notm}} multizone metros in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the metro that is closest to where your host machines physically reside that you plan to add to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.
4. Click **Create location**. When you create the location, a location control plane master is deployed to one of the zones that are located in the {{site.data.keyword.cloud_notm}} multizone metro that you selected. That process might take a few minutes to complete.
5. Wait for the master to be fully deployed and the location **State** to change to `Action required`.
6. Continue with [adding hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts) to finish the setup of your  {{site.data.keyword.satelliteshort}} control plane.

### Creating locations from the CLI
{: #locations-create-cli}

Use the {{site.data.keyword.satelliteshort}} CLI to create your location.
{: shortdesc}

Before you begin, [install the {{site.data.keyword.satelliteshort}} CLI](/docs/satellite?topic=satellite-setup-cli).

1.  Log in to your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` flag, or create an API key to log in.
    ```
    ibmcloud login [-sso]
    ```
    {: pre}

2.  Create a {{site.data.keyword.satelliteshort}} location. When you create the location, an {{site.data.keyword.cos_full_notm}} service instance and a bucket are created on your behalf to back up the {{site.data.keyword.satelliteshort}} control plane data. You can create your own {{site.data.keyword.cos_full_notm}} service instance and bucket, and provide this information during location creation. For more information, see the [`ibmcloud sat location create`](/docs/satellite?topic=satellite-satellite-cli-reference#location-create) command.
    ```
    ibmcloud sat location create --managed-from <satellite-metro> --name <location_name>
    ```
    {: pre}

    <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component.">
    <caption>Understanding this command's components</caption>
      <thead>
      <th>Component</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
      <td><code>--managed-from us-east</code></td>
        <td>The {{site.data.keyword.cloud_notm}} multizone metro that your location is managed from. You can use any metro, but to reduce latency between {{site.data.keyword.cloud_notm}} and your location, choose the metro that is closest to the compute hosts that you plan to add to your location later. For a list of supported {{site.data.keyword.cloud_notm}} multizone metros, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions). </td>
      </tr>
      <tr>
      <td><code>--name &lt;location_name&gt;</code></td>
      <td>Enter a name for your {{site.data.keyword.satelliteshort}} location. Do not reuse the name of a previously deleted location.</td>
      </tr>
      </tbody>
    </table>

3. Verify that your location is created and wait for the location **Status** to change to `action required`. When you create the location, a location control plane master is deployed to the metro that you selected during location creation. During this process, the **Status** of the location shows `deploying`. When the master is fully deployed and you can now add compute capacity to your location to complete the setup of the {{site.data.keyword.satelliteshort}} control plane, the **Status** changes to `action required`.
   ```
   ibmcloud sat location ls
   ```
   {: pre}

   Example output:
   ```
   Name         ID                     Status            Ready   Created        Hosts (used/total)   Managed From   
   mylocation   brhtfum2015a6mgqj16g   action required   no      1 minute ago   0 / 3                Washington DC   
   ```
   {: screen}

4. To finish the setup of your location:
   1. [Add at least 3 compute hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts).
   2. Assign these hosts as worker nodes to the [{{site.data.keyword.satelliteshort}} control plane](#setup-control-plane).

<br />


## Setting up the {{site.data.keyword.satelliteshort}} control plane for the location
{: #setup-control-plane}

The location control plane runs resources that are managed by {{site.data.keyword.satelliteshort}} to help manage the hosts, clusters, and other resources that you add to the location. To create the control plane, you must add at least 3 compute hosts to your location that meet the [minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host).
{: shortdesc}

### Setting up the control plane from the console
{: #control-plane-ui}

Use the {{site.data.keyword.satelliteshort}} console to set up a control plane for your location.
{: shortdesc}

**Before you begin**:
- Make sure that you [added at least 3 hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane.
- Verify that your location is in an **Action required** state.

**To add hosts as worker nodes to the control plane**:

1. From the {{site.data.keyword.satelliteshort}} [**Locations** dashboard](https://cloud.ibm.com/satellite/locations), select the location where you want to finish the setup of your control plane.
2. From the **Hosts** tab, identify at least 3 hosts that you can assign as worker nodes to your control plane. All hosts must be in an **Unassigned** status.
3. From the actions menu of each host, click **Assign host**.
4. Select **Control plane** as your cluster and choose one of the available zones. Make sure that you assign each host to a different zone so that you spread all 3 hosts across all 3 zones in US East (`us-east-1`, `us-east-2`, and `us-east-3`). When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
5. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} control plane. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.
6. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
   {: note}

7. Refer to step 7 in [Setting up the control plane from the CLI](#control-plane-cli) to verify that your DNS records were successfully created.
   
   If your hosts are from Amazon Web Services or Google Cloud Platform, you must manually register the DNS for the location control plane. For more information, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-control-plane) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-control-plane) provider topics.
   {: note}

8. **Google Cloud Platform only**: If you use GCP hosts for your {{site.data.keyword.satellitelong_notm}} location control plane, you must request modified maximum transmission unit (MTU) settings. [Open a support case](/docs/satellite?topic=satellite-get-help#help-support).

### Setting up the control plane from the CLI
{: #control-plane-cli}

Use the {{site.data.keyword.satelliteshort}} command line to set up a control plane for your location.
{: shortdesc}

**Before you begin**:
- Make sure that you [added at least 3 hosts to your location](/docs/satellite?topic=satellite-hosts#add-hosts-cli) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane.
- Verify that your location is in an **Action required** state.

**To create the control plane**:

1.  Identify at least 3 machines that you want to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. All hosts must be in an `unassigned` state.
    ```
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Name             ID                     State        Status   Cluster   Worker ID   Worker IP   
    machine-name-1   aaaaa1a11aaaaaa111aa   unassigned   -        -         -           -   
    machine-name-2   bbbbbbb22bb2bbb222b2   unassigned   -        -         -           -   
    machine-name-3   ccccc3c33ccccc3333cc   unassigned   -        -         -           -  
    ```
    {: screen}

2.  Optional: If you want to assign hosts to your control plane by using a host label, retrieve the details of your host. Available labels that you can use are listed in the **Labels** section of your CLI output.
    ```
    ibmcloud sat host get --location <location_name_or_ID> --host <host_ID>
    ```
    {: pre}

    Example output:
    ```
    Retrieving host details...

    Name:     mymachine1   
    ID:       brjrgp920bg4u254brr0   
    State:    unassigned  
    Status:   -   

    Labels      
    cpu      4   
    memory   32774980  
    use      satloc

    Assignment        
    Cluster:       -  
    Worker Pool:   -  
    Zone:          -  
    Worker ID:     -
    Worker IP:     -   
    Date:          -   
    OK
    ```
    {: screen}

3.  Assign your host machine to the {{site.data.keyword.satelliteshort}} control plane. When you assign the host to the control plane, IBM bootstraps your machine. This process takes a few minutes to complete. You can choose to assign a host by using the host ID, or you can also define the label that the host must have to be assigned to the location.

    **Example for assigning a host by using the host ID:**
    ```
    ibmcloud sat host assign --location <location_name_or_ID>  --cluster <location_ID> --host <host_ID>  --zone <zone>
    ```
    {: pre}

    **Example for assigning a host by using the `use:satloc` label:**
    ```
    ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --label "use:satloc" --zone <zone>
    ```
    {: pre}

    <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component".>
    <caption>Understanding this command's components</caption>
      <thead>
      <th>Component</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
        <td><code>--location <em>&lt;location_name_or_ID&gt;</em></code></td>
      <td>Enter the name or ID of your {{site.data.keyword.satelliteshort}} location. To retrieve the location name or ID, run <code>ibmcloud sat location ls</code>.</td>
      </tr>
      <tr>
      <td><code>--cluster <em>&lt;location_ID&gt;</em></code></td>
      <td>Enter the ID of the {{site.data.keyword.satelliteshort}} location where you want to assign the hosts to run the {{site.data.keyword.satelliteshort}} control plane. To view your location ID, run <code>ibmcloud sat location ls</code>.</td>
      </tr>
      <tr>
      <td><code>--host <em>&lt;host_ID&gt;</em></code></td>
      <td>Enter the host ID to assign to the location control plane. To view the host ID, run <code>ibmcloud sat host ls --location &lt;location_name&gt;</code>. You can use the <code>--label</code> option to identify the host that you want to assign to your control plane.</td>
      </tr>
      <tr>
      <td><code>--label <em>&lt;label&gt;</em></code></td>
      <td>Enter the label that you want to use to identify the host that you want to assign. The label must be a key-value pair, and must exist on the host machine. When you run this command with the `label` option, the first host that is in an `unassigned` state and matches the label is assigned to your control plane. </td>
      </tr>
      <tr>
      <td><code>--zone <em>&lt;zone&gt;</em></code></td>
      <td>Enter the zone to assign the host in. When you repeat this command, change the zone so that you include all three zones in US East (`us-east-1`, `us-east-2`, and `us-east-3`).</td>
      </tr>
      </tbody>
    </table>

4.  Repeat the previous step for the other two hosts that you want to add to your {{site.data.keyword.satelliteshort}} control plane. Make sure that you assign your hosts to a different zone so that you spread all 3 hosts across all 3 zones in US East (`us-east-1`, `us-east-2`, and `us-east-3`).

5. Verify that your hosts are successfully assigned to your location. The assignment is successful when all hosts show an **assigned** state and a **Ready** status, and a public IP address is assigned to the host. If the **Status** of your machines shows `-`, the bootstrapping process is not yet completed and the health status could not be retrieved. Wait a few minutes, and then try again.
   ```
   ibmcloud sat host ls --location <location_name>
   ```
   {: pre}

   Example output:
   ```
   Retrieving hosts...
   OK
   Name              ID                     State      Status   Cluster          Worker ID                                                 Worker IP   
   machine-name-1    aaaaa1a11aaaaaa111aa   assigned   Ready    infrastructure   sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.xx.xxx.xxx   
   machine-name-2    bbbbbbb22bb2bbb222b2   assigned   Ready    infrastructure   sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.xx.xxx.xxx   
   machine-name-3    ccccc3c33ccccc3333cc   assigned   Ready    infrastructure   sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.xx.xxx.xxx   
   ```
   {: screen}

6. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show **action required**, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
   {: note}

   ```
   ibmcloud sat location ls
   ```
   {: pre}

   Example output:
   ```
   OK
   Name         ID                     Status      Ready   Created      Hosts (used/total)   Managed From   
   mylocation   brhtfum2015a6mgqj16g   normal      yes     4 days ago   3 / 3                Washington DC   
   ```
   {: screen}

7. Verify that the public IP addresses of all of your hosts were registered and added to the DNS record of your location. Check that the cert status is **created** and that the records are populated with the subdomains.

   If your hosts are from Amazon Web Services or Google Cloud Platform, you must manually register the DNS for the location control plane. For more information, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-control-plane) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-control-plane) provider topics.
   {: note}

   ```
   ibmcloud sat location dns ls --location <location_ID_or_name>
   ```
   {: pre}

   Example output:
   ```
   Retrieving location subdomains...
   OK
   Hostname                                                                                              Records                                                                                               Health Monitor   SSL Cert Status   SSL Cert Secret Name                                          Secret Namespace   
   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud   169.62.196.20,169.62.196.23,169.62.196.30                                                             None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000   default   
   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001.us-east.satellite.appdomain.cloud   169.62.196.30                                                                                         None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001   default   
   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002.us-east.satellite.appdomain.cloud   169.62.196.20                                                                                         None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002   default   
   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003.us-east.satellite.appdomain.cloud   169.62.196.23                                                                                         None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003   default   
   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00.us-east.satellite.appdomain.cloud   ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud        None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00   default  
   ```
   {: screen}

8. **Google Cloud Platform only**: If you use GCP hosts for your {{site.data.keyword.satellitelong_notm}} location control plane, you must request modified maximum transmission unit (MTU) settings. [Open a support case](/docs/satellite?topic=satellite-get-help#help-support).

### What's next?
{: #location-control-plane-next}

Now that your location control plane is set up, you can choose among the following options:
- [Add more compute capacity to your location to run {{site.data.keyword.openshiftlong_notm}} clusters](/docs/satellite?topic=satellite-hosts#add-hosts) on your own infrastructure.
- [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
- [Attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters) and start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui) with {{site.data.keyword.satelliteshort}} configurations.
- [Learn more about the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

<br />


## Removing locations
{: #location-remove}

You can remove a {{site.data.keyword.satelliteshort}} location if you no longer need the clusters and other resources that run in the location.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts and clusters that run in the location. Note that the underlying host infrastructure is not automatically deleted when you delete a location because you manage the infrastructure yourself.
{: important}

### Removing locations from the console
{: #location-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your locations.
{: shortdesc}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.
2. [Remove all hosts](/docs/satellite?topic=satellite-hosts#host-remove) from your location.
3. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/){: external} **Locations** table, hover over the location that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
4. Click **Remove location**, enter the location name to confirm the deletion, and click **Remove**.

### Removing locations from the CLI
{: #location-remove-cli}

Use the {{site.data.keyword.satelliteshort}} CLI to remove your locations.
{: shortdesc}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.
2. [Remove all hosts](/docs/satellite?topic=satellite-hosts#host-remove-cli) from your location.
3. Remove the location.
   ```
   ibmcloud sat location rm --location <location_name_or_ID>
   ```
   {: pre}

4. Confirm that your location is removed. The location no longer is displayed in the output of the following command.
   ```
   ibmcloud sat location ls
   ```
   {: pre}
