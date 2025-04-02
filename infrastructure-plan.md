---


copyright:
  years: 2022, 2025
lastupdated: "2025-04-02"

keywords: satellite, hybrid, multicloud, plan infrastructure for satellite, satellite infrastructure, satellite supported os, satellite supported providers, satellite third party hosts

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Planning your environment for {{site.data.keyword.satelliteshort}} locations
{: #infrastructure-plan}

Plan how to set up your infrastructure environment to use with {{site.data.keyword.satellitelong}}. Your infrastructure environment can be an on-premises data center, in a public cloud provider, or on compatible edge devices anywhere.
{: shortdesc}

## Planning your infrastructure
{: #infra-plan-infra}

Before you create your location, choose your infrastructure provider, infrastructure zones, and your infrastructure hosts.

Your {{site.data.keyword.satelliteshort}} location starts with your infrastructure, such as a public cloud provider or on-prem. Your infrastructure provides the basis for the hosts and zones that you use to build out your {{site.data.keyword.satelliteshort}} location. For more information about the different responsibilities for your infrastructure and {{site.data.keyword.satelliteshort}} resources, see [Your responsibilities](/docs/satellite?topic=satellite-responsibilities).


![Concept overview of planning your infrastructure](/images/plan-sat-envirn.svg){: caption="Your Satellite location is built atop the zones and hosts in your infrastructure provider." caption-side="bottom"}

### Plan your infrastructure provider
{: #infra-plan-provider}

Choose the infrastructure provider that you want to use to create a {{site.data.keyword.satelliteshort}} location.

On-premises
:    You can use a data center with existing infrastructure. You might not even have a data center, but rather an edge location that meets the minimum hardware requirements, such as three racks at one of your company's local sites.
    
Supported bare metal servers
:    You can use a supported bare metal server as a host attached to your {{site.data.keyword.satelliteshort}} location, including {{site.data.keyword.baremetal_long}} for Classic. For more information, see [Bare Metal Server requirements](/docs/satellite?topic=satellite-assign-bare-metal#setup-bare-metal).

Non-{{site.data.keyword.IBM_notm}} cloud provider
:    You can use a cloud provider of your choice, such as Amazon Web Services (AWS), Google Cloud Platform (GCP), Microsoft Azure, or Alibaba Cloud.

{{site.data.keyword.cloud_notm}}
:    You can use [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) for testing purposes. While you can use other {{site.data.keyword.cloud_notm}} virtual servers, such as {{site.data.keyword.vsi_is_short}} for test environments, the only supported {{site.data.keyword.cloud_notm}} infrastructure to use in {{site.data.keyword.satelliteshort}} for production environments is {{site.data.keyword.baremetal_long}} for Classic that is running Red Hat CoreOS.

### Plan for a multizone location
{: #infra-plan-multizone}

In your infrastructure provider, identify a multizone location that meets the latency requirements.

Multizone
:    Your location must have at least three zones that are physically separate so that you can spread out hosts evenly across the zones to increase [high availability](/docs/satellite?topic=satellite-ha). For example, your cloud provider might have three different zones within the same region, or you might use three racks with three separate networking and power supply systems in an on-prem environment.
    
Latency between {{site.data.keyword.cloud_notm}} and the location
:    The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).
    
Latency between hosts in your location
:    Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.

### Plan your host systems
{: #infra-plan-compatible}

In each of the three zones in your infrastructure provider, plan to create compatible hosts to add to {{site.data.keyword.satelliteshort}}. The host instances in your infrastructure provider become the compute hosts to create your location control plane or to run the services in your {{site.data.keyword.satelliteshort}} location, similar to the worker nodes in a {{site.data.keyword.redhat_openshift_notm}} cluster.
- Each host must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}.
- Your hosts must be running on [official Red Hat certified hardware](https://catalog.redhat.com/hardware){: external}.

To calculate how many hosts you need, see [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).

To check your host set up, you can use the `satellite-host-check` script. For more information, see [Checking your host set up](/docs/satellite?topic=satellite-host-network-check).
{: tip}

## Planning your operating system
{: #infras-plan-os}
  
Choose your operating system for your hosts. You can choose Red Hat Enterprise Linux or Red Hat CoreOS. If you want to use Red Hat CoreOS for your managed services, you must create and enable a location to use RHCOS hosts. See [Creating a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations). 

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location).
{: note}

Red Hat Enterprise Linux 8 (RHEL 8)
:    RHEL 8 is default operating system that is supported for {{site.data.keyword.satelliteshort}} hosts running {{site.data.keyword.redhat_openshift_notm}} version 4.9 or later and on infrastructure hosts.

Red Hat Enterprise Linux 7 (RHEL 7)
:    RHEL 7 is the operating system that is supported for {{site.data.keyword.satelliteshort}} hosts running {{site.data.keyword.redhat_openshift_notm}} version 4.9 or earlier. Note that support for RHEL 7 hosts in your control plane ends on March 2nd, 2023. [Follow the steps](/docs/satellite?topic=satellite-host-update-location#migrate-cp-rhel8) to migrate your hosts to RHEL 8.
    
Red Hat CoreOS (RHCOS)
:    RHCOS is a minimal operating system for running containerized workloads securely and at scale. It is based on RHEL and includes automated, remote upgrade features. For more information about the key benefits of RHCOS, see [Red Hat Enterprise Linux CoreOS (RHCOS)](https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/architecture/architecture-rhcos){: external}. RHCOS is supported for {{site.data.keyword.satelliteshort}} hosts on {{site.data.keyword.redhat_openshift_notm}} version 4.9 or later. Red Hat CoreOS hosts don't support all services. For more information, see [Supported Satellite-enabled IBM Cloud services](/docs/satellite?topic=satellite-managed-services). To attach RHCOS hosts, your location must be [enabled for RHCOS](/docs/satellite?topic=satellite-locations#verify-coreos-location).

### Deciding whether to enable Red Hat CoreOS support for your location
{: #enable-coreos-loc}

When you create a location, you can select whether to enable Red Hat CoreOS support. Enabling Red Hat CoreOS support comes with both pros and cons. A Red Hat CoreOS enabled location unlocks more features, but it has a higher infrastructure requirement. On the other hand, a non Red Hat CoreOS enabled location supports a smaller feature set but can run at a smaller footprint, allowing more clusters per same capacity. For more information, see [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
The following table shows the features that are available only in Red Hat CoreOS-enabled locations. The table also shows the supported host types that can be used when setting up these features in your Red Hat CoreOS-enabled location.
| Feature | Supported host types |
| --- | --- | 
| HTTP proxy for outbound traffic | RHEL or RHCOS hosts |
| Bring your own key (BYOK) or keep your own key (KYOK) | RHEL or RHCOS hosts |
| Single node cluster topology | RHEL or RHCOS hosts |
| Direct link | RHCOS hosts only |
| OpenShift virtualization | RHCOS hosts only | 
{: caption="Supported host types for CoreOS location features" caption-side="bottom"}

To verify if you location is enabled for Red Hat CoreOS, see [Is my location enabled for Red Hat CoreOS](/docs/satellite?topic=satellite-locations#verify-coreos-location).

The bring your own key (BYOK) or keep your own key (KYOK) feature is supported in RHCOS enabled locations on {{site.data.keyword.openshiftshort}} 4.13 and later. It is supported on both RHEL and RHCOS hosts. You can encrypt only cluster secrets. This feature is not available during cluster or worker pool creation. You must run the `ibmcloud oc kms enable` command to enable it after the cluster or worker pool has been created. Note that this feature cannot be disabled after it is enabled.
{: note}



## Infrastructure credentials
{: #sat-infra-creds}

For {{site.data.keyword.satellitelong_notm}} to perform actions on your behalf in a cloud provider, you must provide credentials to the cloud provider.

### AWS credentials
{: #sat-infra-creds-aws}

Retrieve the Amazon Web Services (AWS) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your AWS cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your AWS account](/docs/satellite?topic=satellite-iam-common#permissions-aws) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. [Create a separate IAM user that is scoped to EC2 access](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html){: external}.
3. [Retrieve the access key ID and secret access key credentials for the IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html#access-keys-and-secret-access-keys){: external}.
4. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. The `client_id` is the ID of the access key and the `client_secret` is the secret access key that you created for the IAM user in AWS.
    ```json
    {
        "client_id":"string",
        "client_secret": "string"
    }
    ```
    {: screen}
    

### Azure credentials
{: #sat-infra-creds-azure}

Retrieve the Microsoft Azure credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your Azure cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your Azure account](/docs/satellite?topic=satellite-iam-common#permissions-azure) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. [Sign in to your Azure account](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli){: external} from the command line.
    ```sh
    az login
    ```
    {: pre}

3. List the available subscriptions in your account.
    ```sh
    az account list
    ```
    {: pre}

4. Set the subscription to create your Azure resources in.
    ```sh
    az account set --subscription="<subscription_ID>"
    ```
    {: pre}

5. Create a service principal identity with the Contributor role, scoped to your subscription. These credentials are used by {{site.data.keyword.satellitelong_notm}} to provision resources in your Azure account. For more information, see the [Azure documentation](https://learn.microsoft.com/en-us/cli/azure/azure-cli-sp-tutorial-1){: external}.
    ```sh
    az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_ID>" -n"<service_principal_name>"
    ```
    {: pre}

6. In the output, note the values of the `appID`, `password`, and `tenant` fields.
    ```json
    {
    "appId": "<azure-client-id>",
    "displayName": "<service_principal_name>",
    "name": "http://<service_principal_name>",
    "password": "<azure-secret-key>",
    "tenant": "<tenant-id>"
    }
    ```
    {: screen}

7. **Optional**: To provide the credentials during the creation of a {{site.data.keyword.satelliteshort}} location, format the credentials in a JSON file. 
    ```json
    {
        "app_id":"string",
        "tenant_id":"string",
        "password": "string"
    }
    ```
    {: screen}
    

### GCP credentials
{: #sat-infra-creds-gcp}

Retrieve the Google Cloud Platform (GCP) credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your GCP cloud on your behalf.
{: shortdesc}

1. [Create a service account and service account key](https://cloud.google.com/docs/authentication/client-libraries#creating_a_service_account){: external} with at least the required [GCP permissions](/docs/satellite?topic=satellite-iam-common#permissions-gcp). As part of creating the service account, a JSON key file is downloaded to your local machine.
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
    




### VMWare credentials
{: #sat-infra-creds-vmware}

  
Retrieve the VMWare credentials that {{site.data.keyword.satelliteshort}} can use to create {{site.data.keyword.satelliteshort}} resources in your VMWare cloud on your behalf.
{: shortdesc}

1. Verify that you have the required [permissions in your VMWare account](/docs/satellite?topic=satellite-iam-common#permissions-vmware) to create a {{site.data.keyword.satelliteshort}} location from a template.
2. Identify or [create a user](https://techdocs.broadcom.com/us/en/vmware-cis/cloud-director/vmware-cloud-director/10-6/map-for-vmware-cloud-director-tenant-portal-guide-10-6/managing-users-groups-and-roles-in-vcd-tenant/managing-users-in-your-vcd-tenant-portal-tenant/managing-users-in-your-vcd-tenant-portal-tenant.html){: external} with **Administrator** role.
3. Find your [network information](/docs/satellite?topic=satellite-loc-vmware-create-auto#vmware-network).
4. Provide this information on the [VMware Cloud Director template](/docs/satellite?topic=satellite-loc-vmware-create-auto#create-auto-vmware).
