---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-14"

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



# Getting started with {{site.data.keyword.satellitelong_notm}}
{: #getting-started}

With {{site.data.keyword.satellitelong_notm}}, you can bring your own compute infrastructure that is in your on-premises data center, at other cloud providers, or in edge networks as a {{site.data.keyword.satelliteshort}} Location to {{site.data.keyword.cloud_notm}}. Then, you use the capabilities of {{site.data.keyword.satelliteshort}} to run {{site.data.keyword.cloud_notm}} services on this infrastructure, and consistently deploy, manage, and control your app workloads.

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

In this getting started tutorial, you create your first {{site.data.keyword.satellitelong_notm}} location. Then, you create the location control plane with 3 compute hosts from your own infrastructure environment. These compute hosts can reside in an on-prem data center, in {{site.data.keyword.cloud_notm}}, or in other cloud providers.

## Prerequisites
{: #sat-prereqs}

1. You must have 3 compute hosts in your own infrastructure environment that meet certain requirements, such as RHEL 7 packages, public network connectivity, and the ability to log in to the host machines and run a script.
   *  All hosts must meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs).
   *  Depending on the provider, your hosts also might have [provider-specific requirements](/docs/satellite?topic=satellite-providers).
   *  3 hosts at a minimum are needed for the location control plane, for demonstration purposes. If you want to continue to use the location, add more hosts to the location control plane in multiples of 3, such as 6, 9, or 12 hosts.

   If your hosts cannot meet these host and provider requirements, you cannot add the hosts to {{site.data.keyword.satellitelong_notm}}. {{site.data.keyword.satelliteshort}} beta requirements are subject to change.
   {: important}

2. You must be the {{site.data.keyword.cloud_notm}} account owner, or have the [administrator permissions permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to the required {{site.data.keyword.cloud_notm}} services in {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).

## Step 1: Create your location
{: #create-location}

To use {{site.data.keyword.satelliteshort}}, you must create a location. A location represents a data center that you can later fill with your own infrastructure resources to run {{site.data.keyword.cloud_notm}} services or other workloads on your own infrastructure.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click **Create location**.
2. Enter a name and an optional description for your location.
3. Select the {{site.data.keyword.cloud_notm}} multizone metro that you want to use to manage your location. For more information about why you must select a {{site.data.keyword.cloud_notm}} multizone metro, see [Understanding supported {{site.data.keyword.cloud_notm}} multizone metros in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). Make sure to select the metro that is closest to where your host machines physically reside that you plan to add to your {{site.data.keyword.satelliteshort}} location to ensure low network latency between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}.
4. Click **Create location**. When you create the location, a location master is deployed to one of the zones that are located in the {{site.data.keyword.cloud_notm}} multizone metro that you selected. That process might take a few minutes to complete.
5. Wait for the master to be fully deployed and the location **State** to change to `Action required`.


## Step 2: Add compute hosts to your location
{: #add-hosts-to-location}

With your location set up, you can now add host machines to your location. All host machines must meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) and any [provider-specific requirements](/docs/satellite?topic=satellite-providers) for {{site.data.keyword.satelliteshort}} and can physically reside in your own on-premises data center, in other cloud providers, or in edge networks.
{: shortdesc}

1. From the **Hosts** tab, click **Add host**.
2. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use:satcp` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane.
3. Enter a file name for your script or use the name that is generated for you.
4. Click **Download script** to generate the host script and download the script to your local machine.

   Depending on the provider of the host, you might also need to update the [required RHEL 7 packages](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) on your hosts before you can run the script. For example, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-launch-template), [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-rhel-packages), or [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-providers#ibm-cloud-reqs-rhel-packages) RHEL package updates.
   {: note}

5. Log in to each host machine that you want to add to your location and run the script. The steps for how to log in to your machine and run the script vary by cloud provider. When you run the script on the machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} control plane.
   1. Retrieve the public IP address of your host.
   2. Copy the script from your local machine to your host.
      ```
      scp <path_to_script> root@<public_IP_address>:/tmp/attach.sh
      ```
      {: pre}

   3. Log in to your host.
      ```
      ssh root@<public_IP_address>
      ```
      {: pre}
   5. Run the script.
      ```
      nohup bash /tmp/attach.sh &
      ```
      {: pre}
6. As you run the scripts on each machine, check that your hosts show in the **Hosts** tab of your location dashboard. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane. You must add at least 3 compute hosts to your location to continue with the setup of your {{site.data.keyword.satelliteshort}} control plane.


## Step 3: Assign your hosts to the {{site.data.keyword.satelliteshort}} control plane
{: #assign-hosts-to-cp}

To complete the setup of your {{site.data.keyword.satelliteshort}} location, you must assign the 3 compute hosts that you added in the previous step to the {{site.data.keyword.satelliteshort}} control plane. The control plane runs the components to securely connect your location to {{site.data.keyword.cloud_notm}}. For more information, see the [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture).
{: shortdesc}

1. From the actions menu of each host machine that you added, click **Assign host**.
2. Select **Control plane** as your cluster and choose one of the available zones. Make sure that you assign each host to a different zone so that you spread all 3 hosts across all 3 zones in US East (`us-east-1`, `us-east-2`, and `us-east-3`). When you assign the hosts to the control plane, IBM bootstraps your machine. This process might take a few minutes to complete. During the bootstrapping process, the **Health** of your machine changes from `Ready` to `Provisioning`.
3. From the **Hosts** tab, verify that your hosts are successfully assigned to the {{site.data.keyword.satelliteshort}} control plane. The assignment is successful when a public IP address is added to your host and the **Health** status changes to **Normal**.

   After your hosts are successfully assigned to the control plane, it takes another 20-30 minutes until IBM monitoring is properly set up for your location. In addition, a DNS record is created for your location and the public IP addresses of your hosts are automatically registered and added to your DNS record to allow load balancing and health checking for your location. This process can take up to 30 minutes to complete. During this process, your location status continues to show an **action required** state, and you might see intermittent errors, such as `Satellite is attempting to recover` or `Verify that the Satellite location has a DNS record for load balancing requests to the location control plane`.
   {: note}

4. **For hosts from Amazon Web Services or Google Cloud Platform**: Manually register the DNS for the location control plane. For more information, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-control-plane) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-control-plane) provider topics.

5. **Google Cloud Platform only**: If you use GCP hosts for your {{site.data.keyword.satellitelong_notm}} location control plane, you must request modified maximum transmission unit (MTU) settings. [Open a support case](/docs/satellite?topic=satellite-get-help#help-support).

## What's next
{: #whats-next}

Now that your location is set up, you can choose among the following options:
- [Add more compute capacity to your location](/docs/satellite?topic=satellite-hosts#add-hosts).
- [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) on your own infrastructure.
- [Attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters) and start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui) with {{site.data.keyword.satelliteshort}} configurations.
- [Learn more about the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.
