---

copyright:
  years: 2023, 2023
lastupdated: "2023-10-26"

keywords: satellite, hybrid, multicloud, connector, agent, logs

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Why do I see a `rejected by Sources` error in my agent container logs?
{: #ts-connector-endpoints-acls}



When you check your agent container logs by running `podman logs CONTAINER-ID` you see a message similar to the following.
{: tsSymptoms}


```sh
Oct 24 10:14:46 satellite crn:v1:bluemix:public:satellite:us-east:a/XX:XX:link:XX 50 flowlog: rejected by Sources when client 10.16.XX.XXX:XXXXX connecting to 172.18.XX.XXX:XXXXX, conn_type: location
Oct 24 10:14:55 satellite crn:v1:bluemix:public:satellite:us-east:a/XX:XX:link:XX 50 flowlog: rejected by Sources when client 10.16.XX.XXX:XXXXX connecting to 172.18.XX.XXX:XXXXX, conn_type: location
```
{: screen}

A common problem with Connector endpoints is having an incorrect access control list setup. Complete the following steps to check your ACL and enable the correct IPs for your agent. In the previous example, the `10.16.XX.XXX:XXXXX` IP must be added to your endpoint ACL.
{: tsCauses}

Endpoints are accessible only from inside IBM Cloud via the private network.
{: note}

1. Review your ACL and make sure you are allowing the IPs either via IPs or CIDRs that you are using to access the endpoint.

1. Update your endpoint ACL. For more information, see [Creating an access control list rule for your endpoint](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-rule-console).
    
1. If you are running in VPC, you must also add the exit gateways for your applications or VSIs running to your ACLs. You can find these in the VPC dashboard in the **Cloud Service Endpoint source addresses** section.

1. If the issue persists, open a [support case](/docs/get-support?topic=get-support-using-avatar). In the case details, be sure to include any relevant log files, error messages, or command outputs.

