---


copyright:
  years: 2022, 2026
lastupdated: "2026-08-27"

keywords: satellite, hybrid, multicloud, plan infrastructure for satellite, satellite infrastructure, satellite supported os, satellite supported providers, satellite third party hosts

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Planning your environment for {{site.data.keyword.satelliteshort}} locations
{: #infrastructure-plan}

Learn how to plan your infrastructure environment for {{site.data.keyword.satellitelong}}, including on-premises data centers, public cloud providers, and edge devices.
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
:    Use a data center with existing infrastructure, or an edge location — such as three racks at one of your company's local sites — that meets the minimum hardware requirements.
    
Supported bare metal servers
:    You can use a supported bare metal server as a host attached to your {{site.data.keyword.satelliteshort}} location, including {{site.data.keyword.baremetal_long}} for Classic. For more information, see [Bare Metal Server requirements](/docs/satellite?topic=satellite-assign-bare-metal#setup-bare-metal).

Non-{{site.data.keyword.IBM_notm}} cloud provider
:    You can use a cloud provider of your choice, such as Amazon Web Services (AWS), Google Cloud Platform (GCP), Microsoft Azure, or Alibaba Cloud.

{{site.data.keyword.cloud_notm}}
:    [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) is supported for testing purposes. For production environments, the only supported {{site.data.keyword.cloud_notm}} infrastructure is {{site.data.keyword.baremetal_long}} for Classic running Red Hat CoreOS. Other {{site.data.keyword.cloud_notm}} virtual servers, such as {{site.data.keyword.vsi_is_short}}, are supported for test environments only.

### Plan for a multizone location
{: #infra-plan-multizone}

In your infrastructure provider, identify a multizone location that meets the latency requirements.

Multizone
:    A {{site.data.keyword.satelliteshort}} location requires at least three physically separate zones to distribute hosts evenly for [high availability](/docs/satellite?topic=satellite-sat-ha-dr). For example, a cloud provider delivers three different zones within the same region, or an on-premises environment uses three racks with independent networking and power supply systems.
    
Latency between {{site.data.keyword.cloud_notm}} and the location
:    The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. Higher latency degrades performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).
    
Latency between hosts in your location
:    Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. Higher latency degrades performance, including provisioning and recovery times, available worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service stability, and in extreme cases, cluster application availability.

### Plan your host systems
{: #infra-plan-compatible}

In each of the three zones in your infrastructure provider, plan to create compatible hosts to add to {{site.data.keyword.satelliteshort}}. The host instances in your infrastructure provider become compute hosts for your location control plane or for services running in your {{site.data.keyword.satelliteshort}} location, filling the same role as worker nodes in a {{site.data.keyword.redhat_openshift_notm}} cluster.
- Each host must satisfy the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}.
- Your hosts must run on [official Red Hat certified hardware](https://catalog.redhat.com/hardware){: external}.

To calculate how many hosts you need, see [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).

Validate your host setup before attachment using the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}

## Planning your operating system
{: #infras-plan-os}
  
Choose your operating system for your hosts. {{site.data.keyword.satelliteshort}} supports Red Hat Enterprise Linux (RHEL) and Red Hat CoreOS (RHCOS). To use RHCOS hosts for your managed services, create and enable a location for RHCOS support. See [Creating a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location).
{: note}

Red Hat Enterprise Linux 9
:    RHEL 9 is a high-performance Linux platform with security and management features to help run your hybrid cloud workloads.

Red Hat CoreOS (RHCOS)
:    RHCOS is a minimal operating system designed for running containerized workloads securely and at scale. Built on RHEL, RHCOS includes automated remote upgrade capabilities that reduce operational overhead. For more information about the key benefits of RHCOS, see [Red Hat Enterprise Linux CoreOS (RHCOS)](https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/architecture/architecture-rhcos){: external}. RHCOS is supported for {{site.data.keyword.satelliteshort}} hosts on {{site.data.keyword.redhat_openshift_notm}} version 4.9 or later. Not all services support RHCOS hosts. For more information, see [Supported Satellite-enabled IBM Cloud services](/docs/satellite?topic=satellite-managed-services). To attach RHCOS hosts, your location must be [enabled for RHCOS](/docs/satellite?topic=satellite-locations#verify-coreos-location).

### Deciding whether to enable Red Hat CoreOS support for your location
{: #enable-coreos-loc}

When you create a location, you select whether to enable Red Hat CoreOS support. A Red Hat CoreOS-enabled location unlocks more features — including direct link, OpenShift virtualization, and BYOK/KYOK encryption — but requires more infrastructure. A location without Red Hat CoreOS support runs a smaller feature set, but operates at a reduced footprint and supports more clusters per unit of capacity. For a detailed comparison, see [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).

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

The bring your own key (BYOK) or keep your own key (KYOK) feature is supported in CoreOS enabled locations on {{site.data.keyword.openshiftshort}} 4.13 and later, on both RHEL and RHCOS hosts. This feature encrypts cluster secrets only and is not available during cluster or worker pool creation. Enable it after cluster or worker pool creation by running the `ibmcloud oc kms enable` command. This feature cannot be disabled once enabled.
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

1. [Create a service account and service account key](https://docs.cloud.google.com/docs/authentication/client-libraries#creating_a_service_account){: external} with at least the required [GCP permissions](/docs/satellite?topic=satellite-iam-common#permissions-gcp). As part of creating the service account, a JSON key file is downloaded to your local machine.
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
