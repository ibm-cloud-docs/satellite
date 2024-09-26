---


copyright:
  years: 2022, 2024
lastupdated: "2024-09-26"

keywords: satellite, hybrid, multicloud, endpoint, link, endpoint secure

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Accessing your {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoints
{: #link-endpoint-secure}

By default, your {{site.data.keyword.openshiftlong_notm}} API {{site.data.keyword.satelliteshort}} link endpoints are protected to accept traffic from only the {{site.data.keyword.cloud_notm}} control plane. To access them from other sources, you must configure an access control list (ACL) for your endpoint.
{: shortdesc}

You can configure the ACL for your endpoint by following the instructions in [creating access control lists by using the console](/docs/satellite?topic=satellite-link-cloud-create&interface=ui#link-sources-ui) or by running the following CLI command.

```sh
ibmcloud sat acl create --name NAME --location LOCATION --endpoint ENDPOINT --subnet SUBNET [--subnet SUBNET ...]
```
{: pre}

To find the {{site.data.keyword.redhat_openshift_notm}} API endpoint in your {{site.data.keyword.satelliteshort}} location, run `ibmcloud sat endpoints --location LOCATION` and look in the output for the endpoint with a name that starts with `openshift-api-`.
{: tip}

The subnets that your ACL needs to allow depend on where you are trying to access the API server from, for example, where you are trying to run `kubectl` commands from. The actual source IP that is calling the endpoint might not be what you think it is, especially if your source is coming from a Kubernetes cluster or instance in VPC.

Use one of the following instructions, depending on your environment.

* **If you are running in VPC**, you can find your source IPs using the following command.

    ```sh
    ibmcloud is vpc VPC
    ```
    {: pre}

    Look for the `Cloud Service Endpoint source IP addresses` section in the output. For example:

    ```
    Cloud Service Endpoint source IP addresses:    Zone         Address
                                                   us-south-1   10.22.13.83
                                                   us-south-2   10.12.158.57
                                                   us-south-3   10.12.164.28
    ```
    {: screen}

    Use the Cloud Service Endpoint source IPs to configure your ACL. For example:

    ```sh
    ibmcloud sat acl create --name `myOpenShiftACL` --location myLocation --endpoint openshift-api-cqv7rh4w0pf9mjcsacd0` \
        --subnet 10.22.13.83 --subnet 10.12.158.57 --subnet 10.12.158.57
    ```
    {: pre}

* **If you are using a classic VM**, run the following command to find the IP of your virtual service instance.

    ```sh
    ibmcloud sl vs list
    ```
    {: pre}

    Example output:

    ```
    id          hostname      domain                          cpu   memory   public_ip        private_ip       datacenter   action
    146349551   myhost        mydomain.ibmcloud.private       1     2048     169.48.27.170    10.166.165.5     tor01
    ```
    {: screen}

    Use the `private_ip` of your virtual service instance to configure your ACL. For example:

    ```sh
    ibmcloud sat acl create --name `myOpenShiftACL` --location myLocation --endpoint openshift-api-cqv7rh4w0pf9mjcsacd0` \
        --subnet 10.166.165.5
    ```
    {: pre}

* **In all other cases**, you can find the {{site.data.keyword.redhat_openshift_notm}} API Satellite link endpoint by looking in the {{site.data.keyword.loganalysislong_notm}} logs for your {{site.data.keyword.satelliteshort}} location. To open these logs, click **Open Dashboard** under **Logging for Link**. You can set up a filter in the monitoring instance to filter out the value you need. For example, search for `flowlog: rejected by` in the log and you will see an IP. Add an ACL with a subnet matching that IP for your endpoint. This IP is logged when you use `oc` commands via link endpoint on the {{site.data.keyword.redhat_openshift_notm}} API. For more information, see [Logging for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-health).
