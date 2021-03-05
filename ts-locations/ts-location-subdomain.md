---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-05"

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
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
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
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Why does the location subdomain not route traffic to control plane hosts?
{: #ts-location-subdomain}

{: tsSymptoms}
After you assign hosts to your {{site.data.keyword.satelliteshort}} location control plane, you see a message similar to the following.

```
R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.
```
{: screen}

{: tsCauses}
The location control plane is not publicly accessible. The IP addresses that are registered with the DNS for your location subdomains might have one of the following issues:
* The IP addresses are not the correct IP addresses of the hosts that run the location control plane.
* The hosts are behind a firewall that blocks traffic to the location control plane.
* You reused the name of the location, and the location subdomains are still using the IP addresses of the previous location hosts.

{: tsResolve}
Check and update the host IP addresses that are registered with the location subdomain DNS entries.

1. Get the IP addresses of your hosts. For hosts that are publicly available, get the public IP addresses. For hosts that are only privately available, get the private IP addresses. Refer to your provider documentation.

2. Verify that the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable.
  1. [Set up LogDNA for {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-health#setup-logdna).
  2. In the [**Logging** dashboard](https://cloud.ibm.com/observe/logging){:external}, click **View LogDNA** for your {{site.data.keyword.la_short}} instance. The LogDNA dashboard opens.
  3. Check the `Endpoint health status` logs. These logs report the results of health checks for the {{site.data.keyword.satelliteshort}} Link tunnel server endpoint.
      * If logs report `Successfully checked endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is reachable. Continue to the next step.
      * If logs report `Failed to reach endpoint`, your {{site.data.keyword.satelliteshort}} Link tunnel server endpoint is unreachable. If you have a firewall, allow traffic from the hosts to the location control plane access through the firewall. For example, see [AWS Security group](/docs/satellite?topic=satellite-aws-secgroup). If you do not have a firewall, or if after you modify your firewall you still see the `R0036` message or `Failed to reach endpoint` logs, continue to step 2.

3.  Review the location subdomains and check the **Records** for the IP addresses of the hosts that are registered in the DNS for the subdomain.
    ```
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}

4.  For each location subdomain, check whether the IP address that is actually registered with the subdomain matches the host IP addresses that you found in step 1. If any of the registered IP addresses do not match the IP addresses from step 1, [open a support case](/docs/satellite?topic=satellite-get-help#help-support).
    ```
    nslookup <subdomain>
    ```
    {: pre}

5.  Curl each location subdomain on port 30000.
    ```
    curl http://<subdomain>:30000
    ```
    {: pre}

    Example output:
    ```
    <html><body><h1>200 OK</h1>
    Service ready.
    </body></html>
    ```
    {: screen}

6.  If any subdomains did not return a `200 OK` message, update the location subdomain DNS records with the correct public or private IP addresses of each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_name_or_ID> --ip <host_IP> --ip <host_IP> --ip <host_IP>
    ```
    {: pre}
