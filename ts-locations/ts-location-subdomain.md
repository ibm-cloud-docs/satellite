---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

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
{:audio: .audio}
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
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
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


# Why does the location subdomain not route traffic to control plane hosts?
{: #ts-location-subdomain}


After you assign hosts to your {{site.data.keyword.satelliteshort}} location control plane, you see a message similar to the following.
{: tsSymptoms}

```
R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.
```
{: screen}

{: tsCauses}
The {{site.data.keyword.satelliteshort}} location control plane is inaccessible through the location subdomains due to one of the following reasons:
{: tsCauses}
* The hosts are behind a firewall that blocks traffic within the location.
* The DNS resolver for one or more hosts is not properly resolving the registered DNS endpoints.

Follow these steps to resolve your issue
{: tsResolve}

1. Review the location subdomains and check the **Records** for the IP addresses of the hosts that are registered in the DNS for the subdomain.
    ```
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}

2. For **every** host in your location, complete the following steps to log in to the host and check the DNS resolution.
    1. Log in to the host machine.
        * If the host is not assigned to the {{site.data.keyword.satelliteshort}} location control plane or a cluster, you can SSH into the host machine.
        * If the host is assigned to the {{site.data.keyword.satelliteshort}} location control plane or a cluster:
            1. [Remove the host from your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#host-remove).
            2. Reload the operating system of the host.
            3. [Attach the host](/docs/satellite?topic=satellite-hosts#attach-hosts) back to your {{site.data.keyword.satelliteshort}} location, but do not assign it. Later, after you complete these troubleshooting steps, you can [re-assign the host](/docs/satellite?topic=satellite-hosts#host-assign) back to your {{site.data.keyword.satelliteshort}} location control plane or cluster.
            4. SSH into the host machine.
    2. Look up **each** location subdomain that you found in step 1. Check whether the IP address that resolves matches the host IP addresses that you found in step 1. If the host's DNS resolver does not resolve the subdomains to the expected IP addresses, ensure that your hosts have the [required minimum outbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound).
        ```
        nslookup <subdomain>
        ```
        {: pre}

    3. Use `netcat` on each location subdomain on port 30000 to test host connectivity. If any subdomain's `netcat` operation times out, ensure that your hosts meet the [host network requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network).
        ```
        nc -zv <subdomain> 30000
        ```
        {: pre}

        Successful output:
        ```
        Ncat: Version 7.50 ( https://nmap.org/ncat )
        Ncat: Connection refused.
        ```
        {: screen}

3. If are still unable to resolve the issue, [open a support case](/docs/satellite?topic=satellite-get-help#help-support). In the support case description, include all debugging steps that you followed and the output from these steps.


