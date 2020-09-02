---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-02"

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



# Your responsibilities
{: #responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.satellitelong}}. For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between you as the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{:shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.satellitelong_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

<br />


## Incident and operations management
{: #incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | <ul><li>Provide 24x7 customer support for {{site.data.keyword.satelliteshort}} locations.</li><li>Provide [customer support plans](/docs/get-support?topic=get-support-support-plans) to help resolve problems that you might encounter.</li></ul> | <ul><li>Procure the underlying compute, network, and storage infrastructure in your own environments that {{site.data.keyword.satelliteshort}} uses to extend {{site.data.keyword.cloud_notm}} to these environments.</li></ul> |
|{{site.data.keyword.satelliteshort}} Location | <ul><li>Provide a suite of API, CLI, and UI tools to initiate operational activities, such as to create and delete locations.</li><li>Set up a highly available location control plane that is fully managed for you in an {{site.data.keyword.cloud_notm}} multizone metro.</li><li>Monitor the health of the location, automatically resolve issues when possible, and alert IBM site reliability engineers when manual intervention is required.</li><li>Automatically forward location events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Use the provided tools to create a location, attach hosts to the location, assign hosts with sufficient compute resources for the control plane worker components, and debug any issues that might happen in the location.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Provide a suite of API, CLI, and UI tools to initiate operational activities, such as to attach and remove hosts to a location.</li><li>Set up a location-specific control plane that runs on your user-provided hosts with three replicas for high availability.</li><li>Generate a script that users can run on select hosts to attach them to a {{site.data.keyword.satelliteshort}} location.</li><li>Assign hosts as worker nodes to {{site.data.keyword.openshiftshort}} clusters that you designate.</li><li>Monitor the health of hosts that are attached to a location, automatically resolve issues when possible, and alert IBM site reliability engineers when manual intervention is required.</li><li>Automatically forward host events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Attach hosts that meet the [basic system requirements](/docs/satellite?topic=satellite-host-reqs) to {{site.data.keyword.satelliteshort}} locations as needed.</li><li>Assign hosts with enough compute resources to support the location control plane, at least three hosts with at least 4 vCPU and 16 GB memory.</li><li>Assign hosts with enough compute resources to support your application workloads.</li><li>Establish initial network configuration so that hosts can connect to {{site.data.keyword.satelliteshort}} locations, such as allowing connectivity through firewalls or virtual private networks (VPNs).</li><li>In the on-prem or other user-provided environment, set up hosts in whatever highly available architecture that you want, as long as the hosts meet the basic system requirements.</li><li>Use the provided tools to manage hosts and debug any issues that might happen in the hosts.</li></ul> |
|{{site.data.keyword.satelliteshort}} Configuration| <ul><li>Provide a highly available configuration management service that you can use to manage the deployment of Kubernetes resources across clusters that are registered with the location.</li><li>Provide a suite of API, CLI, and UI tools to initiate operational activities, such as to create and delete configurations.</li><li>Provide a `kubectl` command that users can run in an {{site.data.keyword.openshiftshort}} cluster to register the cluster to {{site.data.keyword.satelliteshort}} Configuration.</li><li>Provide the ability to create Kubernetes resource configurations, upload new versions, and subscribe a subset of cluster to a version, including to a previous version.</li><li>Store app configuration files in a highly available, back-end data store (`etcd`).</li><li>Automatically forward configuration events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Use the provided tools to upload your Kubernetes configuration file content as versions in the configuration and to subscribe your clusters to the configuration. You are responsible for any applications that run in your clusters.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Set up the {{site.data.keyword.satelliteshort}} link connector in the {{site.data.keyword.satelliteshort}} location to connect the control plane worker nodes to the control plane master.</li><li>Provide a suite of API, CLI, and UI tools to allow connections between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} or any publicly accessible endpoint.</li><li>Provide the ability to enable and disable connections between your location and an endpoint.</li><li>Automatically collect incoming and outgoing network traffic for an endpoint.</li><li>Provide a dashboard to review endpoint metrics, and automatically send endpoint logs to your {{site.data.keyword.la_full_notm}} instance.</li><li>Automatically forward link events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Use the provided tools to create and manage {{site.data.keyword.satelliteshort}} location endpoints.</li><li>Ensure that the {{site.data.keyword.satelliteshort}} link connector in the {{site.data.keyword.satelliteshort}} control plane is enabled to allow network traffic between your location and endpoints outside your location.</li><li>Enable all connections that are required to successfully run the apps in your location and debug any connection issues for your endpoints.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Provide the ability to deploy a select group of {{site.data.keyword.cloud_notm}} services such as {{site.data.keyword.openshiftshort}} clusters to a {{site.data.keyword.satelliteshort}} location.</li><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Use the provided tools to set up additional services as needed.</li><li>Provide enough hosts for the services to use as compute capacity, per the service documentation.</li><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Table 1. Responsibilities for incident and operations." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />


## Change management
{: #change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|{{site.data.keyword.satelliteshort}} Location| <ul><li>Provide a suite of API, CLI, and UI tools to initiate change management activities, such as to delete locations.</li></ul> | <ul><li>Update the hosts that are assigned to the location control plane, and ensure the control plane has enough compute resources to run.</li><li>Before you delete any locations, remove all associated hosts and clusters. Save any backup information about the location before you delete the location.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Provide a suite of API, CLI, and UI tools to initiate change management activities, such as to update hosts.</li><li>Monitor the health of hosts and report back status with actions that you must complete, such as reloading a host operating system.</li><li>Disable the ability to SSH into hosts after you attach the hosts to a location, to enhance security.</li><li>Provide minor patch version changes for the operating system that you choose when to apply.</li></ul> | <ul><li>Review the status of your hosts and take any actions required to resolve host infrastructure issues, such as operating system reloads or updates.</li><li>Before you update or delete any hosts, make sure that you have enough additional hosts in the cluster or location control plane to continue running any components that you must run. Save any backup information about the hosts before you update or delete.</li><li>If you want to use the host again, perform a complete operating system reload. After you update a host, to use the host in the same cluster again, you must reattach the host to the location and reassign the host to the cluster.</li><li>Initiate patch update changes. Perform any major or minor updates.</li></ul> |
|{{site.data.keyword.satelliteshort}} Configuration| <ul><li>Provide a suite of API, CLI, and UI tools to initiate change management activities, such as to update configurations or subscriptions.</li><li>Automatically deploy any changes to a configuration to subscribed clusters.</li><li>Automatically delete Kubernetes resources that run in subscribed clusters when you delete a configuration.</li></ul> | <ul><li>Use the provided {{site.data.keyword.satelliteshort}} configuration and {{site.data.keyword.openshiftshort}} tools to manage all changes to your apps. You are completely responsible for your app lifecycle, including any downtime that might occur when you update an app version, depending on your update rollout strategy.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Maintain {{site.data.keyword.satelliteshort}} link connector versions.</li></ul> | <ul><li>Use the provided tools to create, update, or delete the endpoints that you need.</li><li>Enable and disable endpoints to allow or block network traffic between your location and the endpoint.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains. For example, with {{site.data.keyword.openshiftlong_notm}} clusters, IBM provides patch version updates for the masters automatically and for the worker nodes that you initiate.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Table 2. Responsibilities for change management." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />


## Identity and access management
{: #iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access. For more information, see [Managing access for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-iam).
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|{{site.data.keyword.satelliteshort}} Location | <ul><li>Provide a suite of API, CLI, and UI tools to assign access control to locations via IAM.</li></ul> | <ul><li>Use the provided tools to manage authentication, authorization, and access control policies.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Disable the ability to SSH into hosts after you assign the hosts to a location control plane or cluster, to enhance security.</li></ul> | <ul><li>SSH into the host to attach the host to a location. After attaching the host, assign the host to a cluster. After assigning the host, access to the host is controlled via IAM access to the cluster as a worker node in the cluster. SSH is disabled after the host is attached to a location.</li></ul> |
|{{site.data.keyword.satelliteshort}} Configuration| <ul><li>Provide a suite of API, CLI, and UI tools to assign access control to configurations via IAM.</li></ul> | <ul><li>Use the provided tools to manage authentication, authorization, and access control policies.</li><li>With configurations, you can create, update, and delete Kubernetes resources that run in your cluster. However, keep in mind that access in IAM to a configuration does not give users access to the clusters, nor the ability to log in and manage the Kubernetes resources from the cluster. Users with access to a cluster might log in and manually change the Kubernetes resources.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Provide a suite of API, CLI, and UI tools to assign access control to endpoints via IAM.</li></ul> | <ul><li>Use the provided tools to manage authentication, authorization, and access control policies.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Table 3. Responsibilities for identity and access management." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />


## Security and regulation compliance
{: #security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | <ul><li>Provide platform-level compliance to certain standards. For more information, see [{{site.data.keyword.cloud_notm}} compliance](/docs/overview?topic=overview-compliance).</li><li>Provide tools to manage billing, usage, and identity and access control (IAM).</li><li>Set default security settings for {{site.data.keyword.satelliteshort}} components. These settings do not guarantee security, and might be modified by the user.</li></ul> | <ul><li>Identify government, industry, and proprietary corporate standards that are required for the environment.</li><li>Review the physical premises that host the underlying infrastructure for security controls to protect the data center.</li></ul>  |
|{{site.data.keyword.satelliteshort}} Location| <ul><li>Maintain security and regulation compliance for the {{site.data.keyword.cloud_notm}}-managed location control plane.</li><li>Update the managed master components.</li><li>Provide patch updates for the control plane components that run in the location worker nodes.</li><li>Provide the ability to control access to locations via {{site.data.keyword.cloud_notm}} IAM.</li></ul> | <ul><li>You are responsible for keeping your host infrastructure secure and compliant, including applying worker node patch updates.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Provide patch updates for the hosts that run as worker nodes in {{site.data.keyword.satelliteshort}} clusters.</li><li>Disable the ability to SSH into hosts after you assign the hosts to a location control plane or clusters, to enhance security.</li></ul> | <ul><li>You are responsible for keeping your host infrastructure secure and compliant, including applying worker node patch updates.</li></ul> |
|{{site.data.keyword.satelliteshort}} Configuration | <ul><li>Deploy apps consistently across clusters and locations.</li><li>Provide the ability to control access to configurations via {{site.data.keyword.cloud_notm}} IAM.</li></ul> | <ul><li>Create your Kubernetes configuration files by following the security standards that you want to comply to, such as by using security context constraints. You are responsible for the security and compliance of your apps.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Establish a secure connection between {{site.data.keyword.cloud_notm}} and {{site.data.keyword.satelliteshort}} locations by using the {{site.data.keyword.satelliteshort}} link connector.</li><li>Provide the ability to control access to endpoints via {{site.data.keyword.cloud_notm}} IAM.</li><li>Provide the ability to monitor network traffic between your location and endpoints outside of your location.</li></ul> | <ul><li>Set up and audit link endpoints across locations.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Table 4. Responsibilities for security and regulation compliance." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />


## Disaster recovery
{: #disaster-recovery}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|{{site.data.keyword.satelliteshort}} Location| <ul><li>Back up location information to recover {{site.data.keyword.satelliteshort}} location control plane components to an IBM-managed {{site.data.keyword.cos_full_notm}} instance.</li><li>Monitor the health of the location, automatically resolve issues when possible, and alert IBM site reliability engineers when manual intervention is required.</li></ul> | <ul><li>Define the disaster recovery requirements for the environment.</li><li>Provide access to the backup in your {{site.data.keyword.cos_full_notm}} instance if you need to recover a location.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Back up information for hosts that are assigned to the location control plane to an IBM-managed {{site.data.keyword.cos_full_notm}} instance.</li><li>Back up information for hosts that are assigned to {{site.data.keyword.satelliteshort}} clusters to an {{site.data.keyword.cos_full_notm}} instance in your account.</li></ul> | <ul><li>Maintain, repair, or replace hardware as needed.</ul> |
|{{site.data.keyword.satelliteshort}} Configuration| <ul><li>Back up information about saved {{site.data.keyword.satelliteshort}} configurations in etcd.</li><li>When service is restored, automatically deploy configuration files to available clusters.</li></ul> | <ul><li>You are responsible for your apps and data, which includes designing your apps for data storage, failover, disaster recovery, and other highly available situations.</ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>N/A</li></ul> | <ul><li>Reinstate any necessary endpoints to your resources after recovering from a disaster.</ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Table 5. Responsibilities for disaster recovery." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}
