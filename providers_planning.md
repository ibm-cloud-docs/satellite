---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-02"

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
{:note .note}
{:note: .note}
{:note:.deprecated}
{:objectc data-hd-programlang="objectc"}
{:objectc: .ph data-hd-programlang='Objective C'}
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



# Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}
{: #infrastructure-plan}

Plan how to set up your infrastructure environment to use with {{site.data.keyword.satellitelong}}. Your infrastructure environment can be an on-premises data center, in another cloud provider, or on compatible edge devices anywhere.
{: shortdesc}

Don't have your own infrastructure or want a managed solution? [Check out {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: tip}


## Planning your infrastructure zones and hosts to meet the {{site.data.keyword.satelliteshort}} requirements
{: #plan-zone-host-reqs}

Your {{site.data.keyword.satelliteshort}} location starts with your actual infrastructure that runs somewhere outside {{site.data.keyword.cloud_notm}}, such as another cloud provider or on-prem. Your infrastructure provides the basis for the hosts and zones that you use to build out your {{site.data.keyword.satelliteshort}} location. For more details on the different responsibilities for your infrastructure and {{site.data.keyword.satelliteshort}} resources, see [Your responsibilities](/docs/satellite?topic=satellite-responsibilities).
{: shortdesc}

![Concept overview of planning your infrastructure](/images/satellite-infra-plan.png){: caption="Figure 1. Your {{site.data.keyword.satelliteshort}} location is built atop the zones and hosts in your infrastructure provider." caption-side="bottom"}

1.  Choose the infrastructure provider that you want to use to create a {{site.data.keyword.satelliteshort}} location.
    * **On-premises**: You can use a data center with existing infrastructure, or order infrastructure from IBM with [{{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service). You might not even have a data center, but rather an edge location that meets the minimum hardware requirements, such as three racks at one of your company's local sites.
    * **Non-IBM cloud provider**: You can use a cloud provider of your choice, such as Amazon Web Services (AWS), Google Cloud Platform (GCP), or Microsoft Azure. For more information, see [Cloud infrastructure like AWS, Azure, and GCP](#create-options-cloud).
    * **{{site.data.keyword.cloud_notm}}**: You can use {{site.data.keyword.cloud_notm}} for testing and demonstration purposes only. For more information, see [Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ibm).
2.  In your infrastructure provider, identify a multizone location that meets the latency requirements.
    *   **Multizone**: Your location must have at least three zones that are physically separate so that you can spread out hosts evenly across the zones to increase [high availability](/docs/satellite?topic=satellite-ha). For example, your cloud provider might have three different zones within the same region, or you might use three racks with three separate networking and power supply systems in an on-prem environment.
    *   **Latency**: Environments that do not meet the latency requirements experience degraded performance.
        * **Between {{site.data.keyword.cloud_notm}} and the location**: The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane like {{site.data.keyword.openshiftshort}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-reqs#host-latency-mzr).
        * **Between hosts in your location**: Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or {{site.data.keyword.satelliteshort}}-enabled service. For example, in cloud providers such as AWS, this setup typically means that all of the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled service degradation, and in extreme cases, failures in your cluster applications.
3.  In each of the three zones in your infrastructure provider, plan to create compatible hosts to add to {{site.data.keyword.satelliteshort}}. The host instances in your infrastructure provider become the compute hosts to run the services in your {{site.data.keyword.satelliteshort}} location, like the worker nodes in a {{site.data.keyword.openshiftlong_notm}} cluster.
    * Each host must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}, such as RHEL 7 operating system; at least 4 CPU, 16 RAM, and 100GB storage per host; full network connectivity between hosts in the same location; and more.
    * To calculate how many hosts you need, see [Sizing your {{site.data.keyword.satelliteshort}} location](#location-sizing).
4.  Use your infrastructure to power your {{site.data.keyword.satelliteshort}} resources.
    1.  Create your {{site.data.keyword.satelliteshort}} location based on the zones and hosts in your infrastructure provider. For more information, see [Deciding how to create your {{site.data.keyword.satelliteshort}} location](#create-options).
    2.  [Assign hosts](/docs/satellite?topic=satellite-hosts) to {{site.data.keyword.satelliteshort}}-enabled services like {{site.data.keyword.openshiftlong_notm}} clusters. The hosts become the worker nodes that provide the compute power for the service.

## Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you attach to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

1. Set up a highly available {{site.data.keyword.satelliteshort}} location control plane with enough compute capacity to manage the resources in your {{site.data.keyword.satelliteshort}} location.
   *  **Minimum size**: To get started, you must attach and assign hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs) of 4 vCPU, 16 GB memory, and 100 GB storage. You assign these hosts evenly across 3 separate zones. For testing purposes such as a proof of concept, you can have a minimum of 3 hosts assigned to the control plane, but for production purposes, you must have a minimum of 6 hosts. As you continue to use your location, [you might need to scale the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale) in multiples of 3, such as 6, 9, or 12 hosts.
   *  **High availability**: When you assign hosts to the {{site.data.keyword.satelliteshort}} location control plane, assign the hosts evenly across each of the 3 available zones of your {{site.data.keyword.cloud_notm}} multizone metro that you selected during location creation. To make the control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might assign 2 hosts each that run in 3 separate availability zones in your cloud provider, or that run in 3 separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable. For more information, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).
   *  **Compute capacity**: {{site.data.keyword.satelliteshort}} monitors the available compute capacity of your location. When the location reaches 70% capacity, you see a warning status to notify you to attach more hosts to the location. If the location reaches 80% capacity, the status changes to **critical** and you see another warning that tells you to attach more hosts to the location.
2. Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then IBM can assign the hosts to your {{site.data.keyword.satelliteshort}} location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.
3. To decide on the size and number of hosts to attach to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations:
   * How many resources does my app require?
   * What else besides my app might use resources in the cluster?
   * What type of availability do I want my workload to have?
   * How many worker nodes (hosts) do I need to handle my workload?
   * How do I monitor resource usage and capacity in my cluster?

## Deciding how to create your {{site.data.keyword.satelliteshort}} location
{: #create-options}

Depending on your infrastructure provider, you have different options to create a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### On-premises infrastructure
{: #create-options-onprem}

For on-prem infrastructure, you can manually setup a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. To understand how your infrastructure is used in {{site.data.keyword.satelliteshort}} location, [review the planning steps](#plan-zone-host-reqs).
2. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).
3. [Attach 6 hosts to the {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
4. [Assign the 6 hosts to the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
5. [Attach at least 3 more hosts to the {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
6. [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) and assign the hosts as worker nodes to the cluster, so that you can run {{site.data.keyword.openshiftshort}} workloads in your location.

### Cloud infrastructure like AWS, Azure, and GCP
{: #create-options-cloud}

For cloud provider infrastructure, you can follow provider-specific guides.
{: shortdesc}

**AWS**: You can choose between manually setting up an AWS location, or using a {{site.data.keyword.bpshort}} template to automatically create and set up the location.
*   Automatic: [Automating your location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template).
*   Manual: [Adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws).

**Azure**: [Adding Azure hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-azure).

**GCP**: [Adding GCP hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-gcp)

**{{site.data.keyword.cloud_notm}}**: For testing and demonstration purposes only, see [Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ibm).


### IBM-managed infrastructure
{: #create-options-sat-is}

IBM can send infrastructure and set up a {{site.data.keyword.satelliteshort}} location for you. See [Getting started with {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: shortdesc}





## Providing {{site.data.keyword.satelliteshort}} with credentials to your cloud provider
{: #infra-credentials}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide credentials to the cloud provider. For example, you might [automate your location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template) or use a {{site.data.keyword.satelliteshort}}-enabled service that sets up a {{site.data.keyword.satelliteshort}} location for you.
{: shortdesc}

The credentials that you provide are stored and encrypted in etcd of the {{site.data.keyword.satelliteshort}} location control plane master. For more information, see [Securing your data](/docs/satellite?topic=satellite-data-security).


### AWS credentials
{: #infra-creds-aws}

Retrieve the Amazon Web Services (AWS) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your AWS cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your AWS account](/docs/satellite?topic=satellite-iam#permissions-aws) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. [Create a separate IAM user that is scoped to EC2 access](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html)){: external}.
3. [Retrieve the access key ID and secret access key credentials for the IAM user](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: external}.
4. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. The `client_id` is the ID of the access key and the `client_secret` is the secret access key that you created for the IAM user in AWS.
   ```json
   {
      "client_id":"string",
      "client_secret": "string"
   }
   ```
   {: screen}

### Microsoft Azure credentials
{: #infra-creds-azure}

Retrieve the Microsoft Azure credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your Azure cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your Azure account](/docs/satellite?topic=satellite-iam#permissions-azure) to create a {{site.data.keyword.satelliteshort}} location from a template.
1. [Sign in to your Azure account](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli){: external} from the command line.
   ```
   az login
   ```
   {: pre}
2. List the available subscriptions in your account.
   ```
   az account list
   ```
   {: pre}
3. Set the subscription to create your Azure resources in.
   ```
   az account set --subscription="<subscription_ID>"
   ```
   {: pre}
4. Create a service principal identity with the Contributor role, scoped to your subscription. These credentials are used by {{site.data.keyword.satellitelong_notm}} to provision resources in your Azure account. For more information, see the [Azure documentation](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli){: external}.
   ```
   az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_ID>" -n"<service_principal_name>"
   ```
   {: pre}
5. In the output, note the values of the `appID`, `password`, and `tenant` fields.
   ```
   {
   "appId": "<azure-client-id>",
   "displayName": "<service_principal_name>",
   "name": "http://<service_principal_name>",
   "password": "<azure-secret-key>",
   "tenant": "<tenant-id>"
   }
   ```
   {: screen}
6. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. 
   ```json
   {
      "app_id":"string",
      "tenant_id":"string",
      "password": "string"
   }
   ```
   {: screen}

### Google Cloud Platform credentials
{: #infra-creds-gcp}

Retrieve the Google Cloud Platform (GCP) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your GCP cloud on your behalf.
{: shortdesc}

1. [Create a service account and service account key](https://cloud.google.com/docs/authentication/getting-started#creating_a_service_account){: external} with at least the required [GCP permissions](/docs/satellite?topic=satellite-iam#permissions-gcp). As part of creating the service account, a JSON key file is downloaded to your local machine.
2. Open the JSON key file on your local machine, and verify that the format matches the following example. You can provide this JSON key file as your GCP credentials for actions such as creating a {{site.data.keyword.satelliteshort}} location.
   ```json
   {
      "type":"string",
      "project_id":"string",
      "private_key_id": "string",
      "private_key": "string",
      "client_email": "string",
      "client_id": "string",
      "auth_uri": "string",
      "token_uri": "string",
      "auth_provider_x509_cert_url": "string",
      "client_x509_cert_url": "string"
   }
   ```
   {: screen}




