---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does the host registration script fail?
{: #host-registration-script-fails}


When you SSH into your own infrastructure machine that you want to attach as a {{site.data.keyword.satelliteshort}} host and run the host registration script, you see a message similar to the following example.
{: tsSymptoms}

```sh
No package rh-python36 available.
Error: Nothing to do
```
{: screen}


Your machine does not meet the minimum requirements to become a {{site.data.keyword.satelliteshort}} host. In particular, you must have the following packages installed on your RHEL 7 machine.
{: tsCauses}

```sh
Repository 'rhel-server-rhscl-7-rpms' is enabled for this system.
Repository 'rhel-7-server-optional-rpms' is enabled for this system.
Repository 'rhel-7-server-rh-common-rpms' is enabled for this system.
Repository 'rhel-7-server-supplementary-rpms' is enabled for this system.
Repository 'rhel-7-server-extras-rpms' is enabled for this system.
```
{: screen}

To resolve this issue, follow these steps.
{: tsResolve}

1. Add the required packages to your machine. For example, in {{site.data.keyword.IBM_notm}} Cloud infrastructure you can run the following commands to add the required packages. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
    1. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
        ```sh
        subscription-manager refresh
        ```
        {: pre}

        If you see an error such as `Network error, unable to connect to server. Please see /var/log/rhsm/rhsm.log for more information.`, check the security group and other network settings for your machine to make sure that you have connectivity to the internet.
        {: tip}

    2. Enable the package repositories on your machine.
        ```sh
        subscription-manager repos --enable rhel-server-rhscl-7-rpms
        subscription-manager repos --enable rhel-7-server-optional-rpms
        subscription-manager repos --enable rhel-7-server-rh-common-rpms
        subscription-manager repos --enable rhel-7-server-supplementary-rpms
        subscription-manager repos --enable rhel-7-server-extras-rpms
        ```
        {: pre}

2. Make sure that your machine meets the other [host minimum requirements](/docs/satellite?topic=satellite-host-reqs), such as minimum CPU and memory sizes.
3. [Run the registration script](/docs/satellite?topic=satellite-attach-hosts) on your machine again.


