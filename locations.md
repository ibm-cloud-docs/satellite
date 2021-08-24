---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

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
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
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
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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



# Setting up {{site.data.keyword.satelliteshort}} locations
{: #locations}

Set up an {{site.data.keyword.satellitelong}} location to represent a data center that you fill with your own infrastructure resources, and start running {{site.data.keyword.cloud_notm}} services on your own infrastructure.
{: shortdesc}

Not sure if your infrastructure is ready to use for {{site.data.keyword.satelliteshort}} locations? See [Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-infrastructure-plan).

## Automating your location setup with a {{site.data.keyword.bpshort}} template
{: #satloc-template}

Automate your setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-about-schematics) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in a cloud provider, and set up the {{site.data.keyword.satelliteshort}} location control plane for you.
{: shortdesc}

**Setting up a {{site.data.keyword.satelliteshort}} location with a template**

You can set up {{site.data.keyword.satelliteshort}} locations with a {{site.data.keyword.bpshort}} template for the following cloud providers. 

* [AWS](/docs/satellite?topic=satellite-aws#aws-template)
* [Azure](/docs/satellite?topic=satellite-azure#azure-template)


**What's next?**

The {{site.data.keyword.bpshort}} template helped with the initial creation, but you are in control for subsequent location management actions, such as [attaching more hosts](/docs/satellite?topic=satellite-hosts#attach-hosts), [creating {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters), or [scaling the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale). If you [remove](/docs/satellite?topic=satellite-locations#location-remove) your {{site.data.keyword.satelliteshort}} location, make sure to [remove your workspace in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-workspace-setup#del-workspace), too.

<br />

## Manually creating {{site.data.keyword.satelliteshort}} locations
{: #location-create}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north and south of the country. A {{site.data.keyword.satelliteshort}} location represents a data center that you fill with your own infrastructure resources to run your own workloads and {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Don't have your own infrastructure or want a managed solution? [Check out {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: tip}



### Creating locations from the console
{: #location-create-console}

Use the {{site.data.keyword.satelliteshort}} console to create your location.
{: shortdesc}

**Before you begin**:
* Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
* You must have an existing {{site.data.keyword.cos_full_notm}} service instance so that control plane data for your {{site.data.keyword.satelliteshort}} location can be backed up to a bucket in that instance. For example, to set up a dedicated {{site.data.keyword.cos_short}} instance and bucket:
    1. [Set up a {{site.data.keyword.cos_full_notm}} instance](https://cloud.ibm.com/objectstorage/create){: external} that you plan to use for all of your {{site.data.keyword.satelliteshort}} locations in your account.
    2. Create a bucket in this service instance to back up your {{site.data.keyword.satelliteshort}} location control plane. The {{site.data.keyword.cos_full_notm}} bucket must be in the same region as your {{site.data.keyword.satelliteshort}} location. The bucket endpoint must match the instance endpoint, such as a **Cross Region** bucket for a **Global** instance. Use a name that can help you identify it later, such as `bucket-<satloc_name>-<region>`. Create a separate bucket for each {{site.data.keyword.satelliteshort}} location that you create in your account.
    3. Pass in the name of this bucket when you create the {{site.data.keyword.satelliteshort}} location.

    <p class="important">Do not delete your {{site.data.keyword.cos_short}} instance or this bucket. If the service instance or bucket is deleted, your {{site.data.keyword.satelliteshort}} location control plane data cannot be backed up.</p>

**To create a location from the console**:
1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
2. Click **Manual setup**.
3. In the **{{site.data.keyword.satelliteshort}} location** section, review the following details. To change any of the default values, click **Edit**.
    1. For **Name**: The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name.
    2. The **Description** and **Tags** fields are optional, and are metadata to help you organize your {{site.data.keyword.cloud_notm}} resources.
    3. The **Resource group** is set to `default` by default.
    4. For **Managed from**: Select the {{site.data.keyword.cloud_notm}} region that you want to use to manage your location. For more information about why you must select an {{site.data.keyword.cloud_notm}} region, see [About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the region that is closest to where your host machines physically reside that you plan to attach to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.
    5. For **Zones**: The names of the zones **must match exactly** the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider.
        * [AWS regions and zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html){: external}, such as `us-east-1a`, `us-east-1b`, `us-east-1c`.
        * [Azure `topology.kubernetes.io/zone` labels](https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones){: external}, such as `eastus-1`, `eastus-2`, and `eastus-3`. Do **not** use only the location name (`eastus`) or the zone number (`1`).
        * [GCP regions and zones](https://cloud.google.com/compute/docs/regions-zones){: external}, such as `us-west1-a`, `us-west1-b`, and `us-west1-c`.
4. In the **Object Storage** section, you can click **Edit** to optionally enter the exact name of an existing {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data. Otherwise, a new bucket is automatically created in an {{site.data.keyword.cos_short}} instance in your account.
5. In the **Summary** panel, review your order details, and then click **Create location**. When you create the location, a location control plane master is deployed to one of the zones that are located in the {{site.data.keyword.cloud_notm}} region that you selected.
6. Continue with [attaching hosts to your location](/docs/satellite?topic=satellite-hosts#attach-hosts) to finish the setup of your {{site.data.keyword.satelliteshort}} location control plane.

### Creating locations from the CLI
{: #locations-create-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to create your location.
{: shortdesc}

**Before you begin**:
* [Install the CLI plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli).
* Make sure that you have the [correct permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
* You must have an existing {{site.data.keyword.cos_full_notm}} service instance so that control plane data for your {{site.data.keyword.satelliteshort}} location can be backed up to a bucket in that instance. For example, to set up a dedicated {{site.data.keyword.cos_short}} instance and bucket:
    1. [Set up a {{site.data.keyword.cos_full_notm}} instance](https://cloud.ibm.com/objectstorage/create){: external} that you plan to use for all of your {{site.data.keyword.satelliteshort}} locations in your account.
    2. Create a bucket in this service instance to back up your {{site.data.keyword.satelliteshort}} location control plane. The {{site.data.keyword.cos_full_notm}} bucket must be in the same region as your {{site.data.keyword.satelliteshort}} location. The bucket endpoint must match the instance endpoint, such as a **Cross Region** bucket for a **Global** instance. Use a name that can help you identify it later, such as `bucket-<satloc_name>-<region>`. Create a separate bucket for each {{site.data.keyword.satelliteshort}} location that you create in your account.
    3. Pass in the name of this bucket when you create the {{site.data.keyword.satelliteshort}} location.

Don't delete your {{site.data.keyword.cos_short}} instance or this bucket. If you delete your service instance or bucket, you can't back up your {{site.data.keyword.satelliteshort}} location control plane data.
{: important}

**To create a {{site.data.keyword.satelliteshort}} location from the CLI**:

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` flag, or create an API key to log in.
    ```
    ibmcloud login [-sso]
    ```
    {: pre}

2. Create a {{site.data.keyword.satelliteshort}} location.
    ```
    ibmcloud sat location create --managed-from <region> --name <location_name> --ha-zone zone1_name --ha-zone zone2_name --ha-zone zone3_name [--cos-bucket cos_bucket_name]
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
        <td><code>--managed-from &lt;region&gt;</code></td>
        <td>The {{site.data.keyword.cloud_notm}} region, such as <code>wdc</code> or <code>lon</code>, that your {{site.data.keyword.satelliteshort}} location is managed from. You can use any region, but to reduce latency between {{site.data.keyword.cloud_notm}} and your location, choose the region that is closest to the compute hosts that you plan to attach to your location later. For a list of supported {{site.data.keyword.cloud_notm}} regions, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).</td>
        </tr>
        <tr>
        <td><code>--name &lt;location_name&gt;</code></td>
        <td>Enter a name for your {{site.data.keyword.satelliteshort}} location. The {{site.data.keyword.satelliteshort}} location name must start with a letter, can contain letters, numbers, periods (.), and hyphen (-), and must be 35 characters or fewer. Do not reuse the same name for multiple locations, even if you deleted another location with the same name..</td>
        </tr>
        <tr>
        <td><code>--cos-bucket &lt;cos_bucket_name&gt;</code></td>
        <td>Optional: Enter the name of the {{site.data.keyword.cos_full_notm}} bucket that you want to use to back up the control plane data. Otherwise, a new bucket is automatically created in a {{site.data.keyword.cos_short}} instance in your account.</td>
        </tr>
        <tr>
        <td><code>--ha-zone &lt;ZONE1_NAME&gt; --ha-zone &lt;ZONE2_NAME&gt; --ha-zone &lt;ZONE3_NAME&gt;</code></td>
        <td>Specify three names for high availability zones in your location. The names of the zones <strong>must match exactly</strong> the names of the corresponding zones in your infrastructure provider where you plan to create hosts, such as a cloud provider zone or on-prem rack. To retrieve the name of the zone, consult your infrastructure provider.
        <ul><li><a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html">AWS regions and zones</a>, such as <code>us-east-1a</code>, <code>us-east-1b</code>, <code>us-east-1c</code>.</li>
        <li><a href="https://docs.microsoft.com/en-us/azure/aks/availability-zones#verify-node-distribution-across-zones">Azure <code>topology.kubernetes.io/zone</code> labels</a>, such as <code>eastus-1</code>, <code>eastus-2</code>, and <code>eastus-3</code>. Do <strong>not</strong> use only the location name (<code>eastus</code>) or the zone number (<code>1</code>).</li>
        <li><a href="https://cloud.google.com/compute/docs/regions-zones">GCP regions and zones</a>, such as <code>us-west1-a</code>, <code>us-west1-b</code>, and <code>us-west1-c</code>.</li></ul>
        <p>Optional: If you use this flag, zone names must be specified in three repeated flags. If you do not use this flag, the zones in your location are assigned names such as <code>zone-1</code>.</p></td>
        </tr>
        </tbody>
    </table>

3. Verify that your location is created and wait for the location **Status** to change to `action required`. When you create the location, a location control plane master is deployed to the region that you selected during location creation. During this process, the **Status** of the location shows `deploying`. While the master deploys, you can now attach compute capacity to your location to complete the setup of the {{site.data.keyword.satelliteshort}} location control plane.
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
    1. [Attach compute hosts to your location](/docs/satellite?topic=satellite-hosts#attach-hosts).
    2. Assign these hosts as worker nodes to the [{{site.data.keyword.satelliteshort}} location control plane](#setup-control-plane).

<br />

## Setting up the {{site.data.keyword.satelliteshort}} location control plane
{: #setup-control-plane}

The location control plane runs resources that are managed by {{site.data.keyword.satelliteshort}} to help manage the hosts, clusters, and other resources that you attach to the location.
{: shortdesc}

When you set up the {{site.data.keyword.satelliteshort}} location control plane, keep in mind the following host considerations.
{: important}

* You must attach compute hosts in groups of 3 to your location that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs) and any provider-specific requirements. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
* The minimum of 3 hosts for the control plane is for demonstration purposes. To continue to use the location for production workloads, [attach more hosts to the {{site.data.keyword.satelliteshort}} location control plane](#control-plane-scale) in multiples of 3, such as 6, 9, or 12 hosts.
* Make sure that your hosts meet the [latency requirements](/docs/satellite?topic=satellite-about#architecture-latency).

### Setting up the control plane from the console
{: #control-plane-ui}

Use the {{site.data.keyword.satelliteshort}} console to set up a control plane for your location.
{: shortdesc}

**Before you begin**:
- [Attach at least 6 hosts (or 3 hosts for demonstration purposes only) to your location](/docs/satellite?topic=satellite-hosts#attach-hosts) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
- Verify that your location is in an **Action required** state.

**To attach hosts as worker nodes to the control plane**:

1. From the {{site.data.keyword.satelliteshort}} [**Locations** dashboard](https://cloud.ibm.com/satellite/locations), select the location where you want to finish the setup of your control plane.
2. From the **Hosts** tab, select the hosts to assign as worker nodes to your control plane. All hosts must be in an **Unassigned** status.
3. From the actions menu of each host, click **Assign host**.
4. Select **Control plane** as your cluster.
5. Assign hosts in groups of 3 evenly to the control plane cluster. For high availability, make sure that your hosts correspond to physically separate zones in your infrastructure provider. For example, if your infrastructure provider has `us-east-1a`, `us-east-1b`, and `us-east-1c`, enter these names for your {{site.data.keyword.satelliteshort}} zones. Then, assign 2 hosts from `us-east-1a` in your infrastructure provider to `us-east-1a` in your {{site.data.keyword.satelliteshort}} control plane, and so on. When you assign the hosts to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
6. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} location control plane. The assignment is successful when an IP address is added to your host and the **Health** status changes to **Normal**.
7. Verify that your location status changed to **Normal**. You might see a location message about the location not having enough hosts until the bootstrapping process completes.

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
    {: note}

8. Refer to step 7 in [Setting up the control plane from the CLI](#control-plane-cli) to verify that your DNS records were successfully created.

9. To continue to use the location for production workloads, repeat these steps to attach more hosts to the location control plane in multiples of 3, such as 6, 9, or 12 hosts. For more information, see [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](#control-plane-scale).

### Setting up the control plane from the CLI
{: #control-plane-cli}

Use the {{site.data.keyword.satelliteshort}} command line to set up a control plane for your location.
{: shortdesc}

**Before you begin**:
- [Attach at least 6 hosts (or 3 hosts for demonstration purposes only) to your location](/docs/satellite?topic=satellite-hosts#attach-hosts-cli) to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
- Verify that your location is in an **Action required** state.

**To create the control plane**:

1. Identify the hosts that you want to use as worker nodes for your {{site.data.keyword.satelliteshort}} control plane. All hosts must be in an `unassigned` state.
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

2. Optional: If you want to assign hosts to your control plane by using a host label, retrieve the details of your host. Available labels that you can use are listed in the **Labels** section of your CLI output.
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

3. Assign your host machine to the {{site.data.keyword.satelliteshort}} location control plane. When you assign the host to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process takes a few minutes to complete. You can choose to assign a host by using the host ID, or you can also define the label that the host must have to be assigned to the location.

    **Example for assigning a host by using the host ID:**
    ```
    ibmcloud sat host assign --location <location_name_or_ID>  --cluster <location_ID> --host <host_ID>  --zone <zone>
    ```
    {: pre}

    **Example for assigning a host by using the `use:satloc` label:**
    ```
    ibmcloud sat host assign --location <location_name_or_ID> --cluster <location_ID> --host-label "use:satloc" --zone <zone>
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
        <td>Enter the ID of the {{site.data.keyword.satelliteshort}} location where you want to assign the hosts to run the {{site.data.keyword.satelliteshort}} location control plane. To view your location ID, run <code>ibmcloud sat location ls</code>.</td>
        </tr>
        <tr>
        <td><code>--host <em>&lt;host_ID&gt;</em></code></td>
        <td>Enter the host ID to assign to the {{site.data.keyword.satelliteshort}} location control plane. To view the host ID, run <code>ibmcloud sat host ls --location &lt;location_name&gt;</code>. You can use the <code>--host-label</code> option to identify the host that you want to assign to your control plane.</td>
        </tr>
        <tr>
        <td><code>--host-label <em>&lt;label&gt;</em></code></td>
        <td>Enter the label that you want to use to identify the host that you want to assign. The label must be a key-value pair, and must exist on the host machine. When you run this command with the <code>label</code> option, the first host that is in an <code>unassigned</code> state and matches the label is assigned to your control plane. </td>
        </tr>
        <tr>
        <td><code>--zone <em>&lt;zone&gt;</em></code></td>
        <td>Enter the zone to assign the host in, which can correspond to a physically separate zone in your infrastructure provider. To see the zone names for your location, run <code>ibmcloud sat location get --location <location_name_or_ID></code> and look for the <strong>Host Zones</strong> field.</td>
        </tr>
        </tbody>
    </table>

4. Repeat the previous step for the other hosts that you want to attach to your {{site.data.keyword.satelliteshort}} location control plane. For high availability, make sure that you assign hosts evenly across zones that correspond to physically separate zones in your infrastructure provider. For example, if your infrastructure provider has `us-east-1a`, `us-east-1b`, and `us-east-1c`, you can enter these names for your {{site.data.keyword.satelliteshort}} zones. Then, assign 2 hosts from `us-east-1a` in your infrastructure provider to `us-east-1a` in your {{site.data.keyword.satelliteshort}} control plane, 2 hosts from `us-east-1b`, and 2 hosts from `us-east-1c`, for a total of 6 hosts in the control plane.

5. Verify that your hosts are successfully assigned to your location. The assignment is successful when all hosts show an **assigned** state and a **Ready** status, and an IP address is assigned to the host. If the **Status** of your machines shows `-`, the bootstrapping process is not yet completed and the health status could not be retrieved. Wait a few minutes, and then try again.
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

    After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until {{site.data.keyword.IBM_notm}} monitoring is properly set up for your location. In addition, a DNS record is created for your location and the IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show **action required**, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
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

7. Verify that the IP addresses of all of your hosts were registered and added to the DNS record of your location. Check that the cert status is **created** and that the records are populated with the subdomains.
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

8. To continue to use the location for production workloads, repeat these steps to attach more hosts to the location control plane in multiples of 3, such as 6, 9, or 12 hosts. For more information, see [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](#control-plane-scale).

### What's next?
{: #location-control-plane-next}

Now that your {{site.data.keyword.satelliteshort}} location control plane is set up, you can choose among the following options.
{: shortdesc}

- [Attach more compute capacity to your location to run {{site.data.keyword.openshiftlong_notm}} clusters](/docs/satellite?topic=satellite-hosts#attach-hosts) on your own infrastructure.
- [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
- [Attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) and start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} configurations.
- [Learn more about the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.
- When a host update becomes available, see [Updating {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-hosts#host-update-location).

<br />

## Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane
{: #control-plane-scale}

As you attach more resources to your {{site.data.keyword.satelliteshort}} location, such as {{site.data.keyword.openshiftlong_notm}} clusters, your location control plane needs more compute capacity to maintain the location. You can assign hosts to the control plane to increase the compute capacity.
{: shortdesc}

**How do I know when to attach capacity to the {{site.data.keyword.satelliteshort}} location control plane?**

When you list locations, such as with the `ibmcloud sat location ls` command or in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, the location enters an `Action required` health state. You see warning messages similar to the following.

```
Hosts in the location control plane are running out of disk space.

Hosts in the location control plane have critical CPU or memory usage issues.

The location control plane is running at max capacity and cannot support any more workloads.
```
{: screen}

**How many {{site.data.keyword.openshiftlong_notm}} clusters can I run before I need to attach capacity to the location control plane?**

The number of clusters depends on the size of your clusters and the size of the hosts that you use for the {{site.data.keyword.satelliteshort}} location control plane. You must scale up the control plane hosts in multiples of 3, such as 6, 9, or 12.

The following tables provide examples of the number of hosts that the control plane must have to run the masters for various combinations of clusters and worker nodes, for informational purposes only. 
* The size of the hosts that run the control plane, **4 vCPU and 16GB RAM** or **16 vCPU and 64GB RAM**, affect the numbers of clusters and worker nodes that are possible in the location. Keep in mind that actual performance requirements depend on many factors, such as the underlying CPU performance and control plane usage by the applications that run in the location.
* You can assign hosts to the control plane in groups of 3. The table presents examples up to 12 hosts as common configurations to give you an idea of how you might size the control plane for your host and application environment. Note that you can add more than 12 hosts to your control plane in groups of 3. For example you might create a control plane with 18 or 27 hosts.

| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 3 hosts | 1 cluster, for demonstration purposes | Up to 20 workers | 20 workers per cluster |
| 6 hosts | Up to 5 clusters  | 20 workers across 5 clusters, or 80 workers across 2 clusters | 60 workers per cluster |
| 9 hosts |  Up to 8 clusters | 40 workers across 8 clusters, or 140 workers across 3 clusters | 60 workers per cluster |
| 12 hosts |  Up to 11 clusters | 60 workers across 11 clusters, or 200 workers across 4 clusters | 60 workers per cluster |
{: caption="Sizing guidance for example numbers of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run the master components for various combinations of clusters and worker nodes in the location." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the number of clusters that you want to run in the location. The second column describes the number of hosts that the location control plane must have to run the masters for those clusters."}
{: class="simple-tab-table"}
{: #4cpu-16ram}
{: tab-title="4 vCPU, 16GB RAM"}
{: tab-group="loc-size"}

| Number of control plane hosts | Max clusters in location | Example of max worker nodes in location | Max cluster size |
| --- | --- | --- | --- |
| 3 hosts | 1 cluster, for demonstration purposes | Up to 100 workers | 100 workers per cluster |
| 6 hosts | Up to 20 clusters | 200 workers across 20 clusters, or 550 workers across 2 clusters | 300 workers per cluster |
| 9 hosts  | Up to 26 clusters | 400 workers across 26 clusters, or 850 workers across 3 clusters | 300 workers per cluster |
| 12 hosts  | Up to 36 clusters | 520 workers across 26 clusters, or 1150 workers across 4 clusters | 300 workers per cluster |
{: caption="Sizing guidance for example numbers of hosts that the {{site.data.keyword.satelliteshort}} location control plane requires to run the master components for various combinations of clusters and worker nodes in the location." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the number of clusters that you want to run in the location. The second column describes the number of hosts that the location control plane must have to run the masters for those clusters."}
{: class="simple-tab-table"}
{: #16cpu-64ram}
{: tab-title="16 vCPU, 64GB RAM"}
{: tab-group="loc-size"}

**How do I scale up my {{site.data.keyword.satelliteshort}} location control plane to be highly available?**

See [Highly available control plane worker setup](/docs/satellite?topic=satellite-ha#satellite-ha-setup). Make sure to attach hosts to the control plane location in each zone, in multiples of three. For example, you might have 6 hosts that are assigned to your control plane location that is managed from the {{site.data.keyword.cloud_notm}} `wdc` region, with 2 hosts each zone (`us-east-1`, `us-east-2`, and `us-east-3`).

**I am ready to scale up my {{site.data.keyword.satelliteshort}} location control plane. How do I start?**

You can follow the same steps to [Set up the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).

<br />

## Copying a location
{: #location-copy}

To copy your {{site.data.keyword.satelliteshort}} location to a new location, you can review the details of the resources in your location and re-create the resources in a new location. You might also want copies of your location details for archival purposes.
{: shortdesc}

1. Get your location details and optionally save the details to a local file.
    ```
    ibmcloud sat location get --location <location_ID> [> mylocation.txt]
    ```
    {: pre}

2. List the hosts in your location.
    ```
    ibmcloud sat host ls --location <location_ID>
    ```
    {: pre}

3. Get the details of a host and optionally save the details to a local file. In particular, note the labels for **cpu** and **memory** so that you know your host configuration. Repeat this step for each host in your location.
    ```
    ibmcloud sat host get --location <location_ID> --host <host_ID> [> host1.txt]
    ```
    {: pre}

4. List your {{site.data.keyword.satelliteshort}} Link endpoints. A certain set of endpoints are created [by default](/docs/satellite?topic=satellite-link-location-cloud#default-link-endpoints), but note your other endpoints.
    ```
    ibmcloud sat endpoint ls --location <location_ID>
    ```
    {: pre}

5. Get the details of an endpoint and optionally save the details to a local file. Repeat this step for each endpoint in your location.
    ```
    ibmcloud sat endpoint get --location <location_ID> --endpoint <endpoint_ID> [> endpoint1.txt]
    ```
    {: pre}

6. List the clusters that are created in your {{site.data.keyword.satelliteshort}} location.
    ```
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}

7. Re-create your location.
    *  For options to create the location and hosts, see [Deciding how to create your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-infrastructure-plan#create-options).
    *  To create {{site.data.keyword.satelliteshort}} Link endpoints, see [cloud](/docs/satellite?topic=satellite-link-location-cloud#link-cloud) or [location](/docs/satellite?topic=satellite-link-location-cloud#link-location) endpoint instructions.
    *  For clusters, see [Creating {{site.data.keyword.openshiftshort}} in {{site.data.keyword.satelliteshort}}](/docs/openshift?topic=openshift-satellite-clusters). If you manually deployed Kubernetes configurations instead of using {{site.data.keyword.satelliteshort}} Config, also see [Copying deployments to another cluster](/docs/openshift?topic=openshift-update_app#copy_apps_cluster).
8. Redeploy your apps to your cluster. Because {{site.data.keyword.satelliteshort}} Config works across {{site.data.keyword.satelliteshort}} locations, you can [add your new clusters to existing cluster groups](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups) that are already [subscribed to the configurations](/docs/satellite?topic=satellite-satcon-create) that you want to deploy.
9. Similar to {{site.data.keyword.satelliteshort}} Config, if you used {{site.data.keyword.satelliteshort}} storage configurations, you can assign these configurations to your new clusters to install the storage drivers.

When you are done and your new location is healthy, you can [remove the previous location](#location-remove).

<br />

## Monitoring location health
{: #location-monitor-health}

When you set up a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your location healthy. For more information, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default). For troubleshooting help, see [Debugging location health](/docs/satellite?topic=satellite-ts-locations-debug).
{: shortdesc}

You can review the host health from the **Locations** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat location ls`.

| Health state | Description
| --- | --- |
| `action required` | The location needs your attention. Check the status and message for more information, and try [debugging your location](/docs/satellite?topic=satellite-ts-locations-debug). |
| `completed` | {{site.data.keyword.satelliteshort}} completed the setup of the location control plane components on the hosts that you assigned to the control plane. The location is soon ready to be used.|
| `completing` | {{site.data.keyword.satelliteshort}} is setting up the location control plane components on the hosts that you assigned to the control plane. Check back in a little while.|
| `critical` | The {{site.data.keyword.satelliteshort}} location control plane needs your attention. Check the status and message for more information, and try [debugging your location control plane](/docs/satellite?topic=satellite-ts-locations-control-plane).|
| `failed` | {{site.data.keyword.satelliteshort}} did not successfully resolve issues in your location. For more information, see the status message. |
| `host required` | The {{site.data.keyword.satelliteshort}} location is created, but you must [assign hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane). Assign hosts in multiples of 3, such as 6, 9, or 12. |
| `normal` | The {{site.data.keyword.satelliteshort}} location is ready to use. |
| `provisioning` | The control plane for the {{site.data.keyword.satelliteshort}} is provisioning. You cannot assign hosts to other {{site.data.keyword.satelliteshort}} resources, such as clusters, in the location until the control plane is ready.|
| `resolving` | {{site.data.keyword.satelliteshort}} is trying to resolve issues for you, such as by assigning available hosts to the control plane to relieve capacity issues. For more information, see the status message. |
{: caption="Location health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the location. The second column describes what the health state means."}

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
3. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} **Locations** table, hover over the location that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
4. Click **Remove location**, enter the location name to confirm the deletion, and click **Remove**.

Now that the location is removed, check the hosts in your underlying infrastructure provider. To reuse the hosts for other purposes, you must reload the operating system. If you no longer need the hosts, delete them from your infrastructure provider.

### Removing locations from the CLI
{: #location-remove-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to remove your locations.
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

Now that the location is removed, check the hosts in your underlying infrastructure provider. To reuse the hosts for other purposes, you must reload the operating system. If you no longer need the hosts, delete them from your infrastructure provider.


