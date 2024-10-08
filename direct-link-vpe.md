---


copyright:
  years: 2024, 2024
lastupdated: "2024-10-08"

keywords: satellite, hybrid, multicloud, direct link, secure direct link, connector, agent

subcollection: satellite

content-type: tutorial
services: satellite, dl, vpc
account-plan: paid
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}


# Connecting to IBM Cloud via the private network by using Satellite Connector and Direct Link 2.0
{: #direct-link-vpe}
{: toc-content-type="tutorial"}
{: toc-services="satellite, dl, vpc"}
{: toc-completion-time="2h"}

In the following steps, you set up a Virtual Private Endpoint (VPE) gateway in your Virtual Private Cloud (VPC) to use with Direct Link for communication between your on-premises apps and IBM Cloud.

After setting up the VPE, you can communicate to IBM Cloud through the VPE by using Satellite Connector. 

## Prerequisites
{: #dl-vpe-prereq}

Before you begin, make sure you have the following resources and permissions.

- [A VPC](/docs/vpc?topic=vpc-getting-started).
- [{{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl).
- [The IBM Cloud CLI and plug-ins installed](/docs/cli?topic=cli-getting-started).
- **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.satellitelong_notm}}
- **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for Direct Link.
- **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for VPC.
- **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.registrylong_notm}}
- **Manager** {{site.data.keyword.cloud_notm}} IAM service access role for {{site.data.keyword.satellitelong_notm}}
- **Viewer** {{site.data.keyword.cloud_notm}} IAM platform access role for the resource group that you plan to use with {{site.data.keyword.satelliteshort}}


## Create a subnet for your VPE gateway
{: #dl-vpe-subnet-create}
{: step}

Create the subnet in the VPC where you want to create a VPE gateway.

1. Get your VPC ID.

    ```txt
    ibmcloud is vpcs
    ```
    {: pre}

1. Create a subnet. For more information and command options, see [Working with subnets for VPC](/docs/vpc?topic=vpc-subnets-configure&interface=cli#subnets-vpc-create).
    ```txt
    ibmcloud is subnet-create NAME VPC ((--zone ZONE_NAME --ipv4-address-count ADDR_COUNT) | --ipv4-cidr-block CIDR_BLOCK)
    ```
    {: pre}

    Example command to create a subnet in VPC `r001-a1aa1a11-5eaf-4df4-9de8-ba49a46008bc`
    ```txt
    ibmcloud is subnet-create test-subnet r001-a1aa1a11-5eaf-4df4-9de8-ba49a46008bc --zone us-south-1 --ipv4-address-count 4
    ```
    {: pre}


## Create a VPE gateway
{: #dl-vpe-gateway-create}
{: step}

1. List all VPE endpoints and make a note of the Satellite Link VPE endpoint.

    ```txt
    ibmcloud is endpoint-gateway-targets| grep satellite
    ```
    {: pre}

    Example output

    ```txt
    CRN                           crn:v1:public:satellite-link:us-south:::endpoint:d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    Parent                        us-south
    Name                          satellite_link-vpe
    Resource type                 provider_cloud_service
    Endpoint type                 cse
    Full qualified domain names   d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    Service location              us-south
    ```
    {: screen}

1. Create a VPE Gateway for Satellite Link. Give your gateway a name, provide the VPC ID, and specify the Link VPE that you found in the previous step.

    ```txt
    ibmcloud is endpoint-gateway-create --name NAME --vpc-id VPC-ID --target crn:v1:bluemix:public:satellite-link:us-south:::endpoint:d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    ```

    Example 
    ```txt
    ibmcloud is endpoint-gateway-create --name test-vpe --vpc-id r001-a1aa1a11-5eaf-4df4-9de8-ba49a46008bc --target crn:v1:public:satellite-link:us-south:::endpoint:d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    ```
    {: pre}

    Example output
    ```txt
    ID                  r001-11aa111b-48e5-4676-a0df-4755de9577f6
    Name                test-vpe
    CRN                 crn:v1:bluemix:public:is:us-south:a/9f19417983334c98bfea53579abf81e9::endpoint-gateway:r001-11aa111b-48e5-4676-a0df-4755de9577f6
    Target              crn:v1:public:satellite-link:us-south:::endpoint:d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    Target Type         provider_cloud_service
    VPC                 ID                                          Name
                        r001-a1aa1a11-5eaf-4df4-9de8-ba49a46008bc   my-vpc

    Private IPs         -
    Service Endpoints   d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    Lifecycle State     pending
    Health State        ok
    Security groups     ID                                          Name
                        r006-7749cde3-02c5-457b-a085-a111f1fcb3b5   overarch-plated-earwig-culture-retake-linguini

    Created             2023-09-28T16:40:33-04:00
    Resource Group      ID                                 Name
                        6b5746f5bed01111a754ef34b9bf1111   Default
    ```
    {: screen}

## Create a DNS instance and a customer resolver
{: #dl-vpe-dns-create}
{: step}

IBM DNS Services provides private DNS to Virtual Private Cloud (VPC) users. Private DNS zones are resolvable only on IBM Cloud, and only from explicitly permitted networks in an account.

To extend the DNS resolution to resolvers that reside on-premises, add a custom resolver in the VPC where the VPE gateway is created. Then, add a rule to forward the requests for the VPE gateway to IBM internal DNS addresses.

The default forwarding rule is automatically created to forward all DNS queries to DNS Services servers `161.26.0.7` and `161.26.0.8`. You don't need to add any forwarding rule in this custom resolver.

1. Create a DNS instance.

    ```txt
    ibmcloud dns instance-create NAME standard-dns
    ```
    {: pre}

    Example

    ```txt
    ibmcloud dns instance-create test-dns standard-dns
    ```
    {: pre}

    Example output

    ```txt
    Name                test-dns
    ID                  416ef1d0-fee1-4bec-bcf2-d8542192f96c
    Service Name        dns-svcs
    Service ID          b4ed8a30-936f-11e9-b289-1d079699cbe5
    Plan ID             2c8fa097-d7c2-4df2-b53e-2efb7874cdf7
    Resource Group ID   6b5746f5bed01111a754ef34b9bf1111
    Location            global
    State               active
    ```
    {: screen}


1. Create a custom DNS resolver.

    ```txt
    ibmcloud dns custom-resolver-create --name NAME --instance INSTANCE --location crn:v1:bluemix:public:is:us-south-1:a/9f19417983334c98bfea53579abf81e9::subnet:0717-331985b6-462c-4611-8d43-74e74c1aa69e --location crn:v1:bluemix:public:is:us-south-3:a/9f19417983334c98bfea53579abf81e9::subnet:0737-845992fa-694e-4ded-9106-abf320969fc5
    ```
    {: pre}

    Example command

    ```txt
    ibmcloud dns custom-resolver-create --name test-resolver --instance test-dns --location crn:v1:bluemix:public:is:us-south-1:a/9f19417983334c98bfea53579abf81e9::subnet:0717-331985b6-462c-4611-8d43-74e74c1aa69e --location crn:v1:bluemix:public:is:us-south-3:a/9f19417983334c98bfea53579abf81e9::subnet:0737-845992fa-694e-4ded-9106-abf320969fc5
    ```
    {: pre}

    Example output

    ```txt
    Creating Custom Resolver for service instance 'test-dns' ...
    OK

    ID                  63c5668c-22f2-4399-afce-8d3b3923e8f6
    Name                test-resolver
    Description
    Enabled             false
    Health              CRITICAL
    Locations
        ID              eccfd292-b97c-4599-905c-055c75284e1f
        Subnet CRN      crn:v1:bluemix:public:is:us-south-1:a/9f19417983334c98bfea53579abf81e9::subnet:0717-331985b6-462c-4611-8d43-74e74c1aa69e
        Enabled         true
        Healthy         false
        DNS Server IP

        ID              0060d71c-4054-45dd-9780-8dca2a82b760
        Subnet CRN      crn:v1:bluemix:public:is:us-south-3:a/9f19417983334c98bfea53579abf81e9::subnet:0737-845992fa-694e-4ded-9106-abf320969fc5
        Enabled         true
        Healthy         false
        DNS Server IP

    Created On          2023-08-28T21:07:26.000Z
    Modified On         2023-08-28T21:07:26.000Z
    ```
    {: screen}

## Optional: Create a DNS forwarding rule
{: #dl-vpe-dns-forward}
{: step}

For on-premises DNS, add a DNS rule that forwards the VPE gateway name requests to the custom DNS that you created in the previous.

Example command.
```txt
ibmcloud dns custom-resolver-forwarding-rule-create RESOLVER_ID --type zone --match HOSTNAME --dns-svcs IPs [--description DESCRIPTION] [-i, --instance INSTANCE] [--output FORMAT]
```
{: pre}

Example rule that forwards the requests to `d-01-ws.private.us-south.link.satellite.cloud.ibm.com` to the IP address `10.240.1.6` which is the address of the custom resolver created in the previous step.
```txt
ibmcloud dns custom-resolver-forwarding-rule-create 63c5668c-22f2-4399-afce-8d3b3923e8f6 --type zone --match d-01-ws.private.us-south.link.satellite.cloud.ibm.com --dns-svcs 10.240.1.6
```
{: pre}


## Test connectivity
{: #dl-vpe-test}
{: step}

You can test connectivity to the VPE gateway in one or more of the following ways.

* Running an `nslookup` command from the VPE gateway VPC should resolve to the internal IP address. For example: `10.240.1.4`.
    ```txt
    nslookup d-01-ws-vpe.private.us-south.link.satellite.cloud.ibm.com
    ```
    {: pre}

* Running a `curl` command from VPE gateway.
    ```txt
    curl d-01-ws-vpe.private.us-south.link.satellite.cloud.ibm.com
    ```
    {: pre}

* Running `nslookup` from on-prem VPC should also resolve to the internal IP address. For example: `10.240.1.4`
    ```txt
    nslookup d-01-ws-vpe.private.us-south.link.satellite.cloud.ibm.com
    ```
    {: pre}

* Running a `curl` command from your on-prem VPC should succeed.
    ```txt
    curl d-01-ws-vpe.private.us-south.link.satellite.cloud.ibm.com
    ```
    {: pre}


## Set up Satellite Connector
{: #dl-vpe-connector}
{: step}

Now you are ready to set up Satellite Connector and a Connector Agent.

When setting up your Connector Agent, use your VPE Gateway address, for example `d-01-ws-vpe.private.us-south.link.satellite.cloud.ibm.com`, as the `SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS` parameter.
{: important}

For more information, see the following links:

- [Creating a Connector](/docs/satellite?topic=satellite-create-connector&interface=ui).
- [Running a Connector agent](/docs/satellite?topic=satellite-run-agent-locally&interface=ui).
