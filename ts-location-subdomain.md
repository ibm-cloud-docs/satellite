---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does the location subdomain not route traffic to control plane hosts?
{: #ts-location-subdomain}


After you assign hosts to your {{site.data.keyword.satelliteshort}} location control plane, you see a message similar to the following.
{: tsSymptoms}

```sh
R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.
```
{: screen}


The {{site.data.keyword.satelliteshort}} location control plane is inaccessible through the location subdomains due to one of the following reasons:
{: tsCauses}

* The hosts are behind a firewall that blocks traffic within the location.
* The DNS resolver for one or more hosts is not properly resolving the registered DNS endpoints.

Follow these steps to resolve your issue
{: tsResolve}

1. Review the location subdomains and check the **Records** for the IP addresses of the hosts that are registered in the DNS for the subdomain.
    ```sh
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}

2. For **every** host in your location, complete the following steps to log in to the host and check the DNS resolution.
    1. Log in to the host machine.
        * If the host is not assigned to the {{site.data.keyword.satelliteshort}} location control plane or a cluster, you can SSH into the host machine.
        * If the host is assigned to the {{site.data.keyword.satelliteshort}} location control plane or a cluster:
            1. [Remove the host from your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-host-remove).
            2. Reload the operating system of the host or reboot the instance.

            3. [Attach the host](/docs/satellite?topic=satellite-attach-hosts) back to your {{site.data.keyword.satelliteshort}} location, but do not assign it. Later, after you complete these troubleshooting steps, you can [re-assign the host](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) back to your {{site.data.keyword.satelliteshort}} location control plane or cluster.

            4. SSH into the host machine.
    2. Look up **each** location subdomain that you found in step 1. Check whether the IP address that resolves matches the host IP addresses that you found in step 1. If the host's DNS resolver does not resolve the subdomains to the expected IP addresses, ensure that your hosts have the [required minimum outbound connectivity](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound).
        ```sh
        nslookup <subdomain>
        ```
        {: pre}

    3. Use `netcat` on each location subdomain on port 30000 to test host connectivity. If any subdomain's `netcat` operation times out, ensure that your hosts meet the [host network requirements](/docs/satellite?topic=satellite-reqs-host-network).
        ```sh
        nc -zv <subdomain> 30000
        ```
        {: pre}

        Successful output:
        ```sh
        Ncat: Version 7.50 ( https://nmap.org/ncat )
        Ncat: Connection refused.
        ```
        {: screen}

3. If are still unable to resolve the issue, [open a support case](/docs/satellite?topic=satellite-get-help#help-support). In the support case description, include all debugging steps that you followed and the output from these steps.

