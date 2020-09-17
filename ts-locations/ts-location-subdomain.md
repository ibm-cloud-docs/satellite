---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-17"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
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
* The IP addresses are not the correct public IP addresses of the hosts that run the location control plane.
* The hosts are behind a firewall that blocks traffic to the location control plane.
* The hosts are from cloud providers like AWS or GCP that automatically register the private IP addresses of the hosts, so you must update the DNS records to use the public IP addresses.
* You reused the name of the location, and the location subdomains are still using the IP addresses of the previous location hosts.

{: tsResolve}
Update the location subdomain DNS entries.

1.  Review the location subdomains and check the **Records** for the IP addresses of the hosts that are registered in the DNS for the subdomain.
    ```
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}
2.  If you have a firewall, allow traffic from the hosts to the location control plane access through the firewall.
3.  Get the public IP addresses of your hosts. Refer to your provider documentation. For example steps, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-control-plane) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-control-plane) topics.
4.  Update the location subdomain DNS records with the correct public IP addresses of each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_name_or_ID> --ip <host_IP> --ip <host_IP> --ip <host_IP>
    ```
    {: pre}

    If you use hosts from cloud providers like AWS and GCP, you might also need to update the your cluster Ingress subdomain DNS entries. For more information, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-cluster-nlb) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-cluster-nlb) provider topics.
    {: note}
