---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-02"

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
{:note .note}
{:note: .note}
{:note:.deprecated}
{:objectc data-hd-programlang="objectc"}
{:objectc: .ph data-hd-programlang='Objective C'}
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


# Why can't I access the {{site.data.keyword.openshiftshort}} console without a VPN on the VPC?
{: #ts-console-fail}

{: tsSymptoms}
When you create an {{site.data.keyword.openshiftshort}} cluster in your {{site.data.keyword.satelliteshort}} location, you cannot access the {{site.data.keyword.openshiftshort}} web console, or access to the console is intermittent.

You might see an error message similar to the following:
```
OpenShift web console unavailable
Wait until your ingress subdomain is available before you access the OpenShift web console.
```
{: screen}

{: tsCauses}
Review the following reasons why you are unable to reach the {{site.data.keyword.openshiftshort}} web console:
* The {{site.data.keyword.openshiftshort}} web console can be accessed only after your cluster's Ingress subdomain is created. For the Ingress subdomain to be created, hosts must be assigned as worker nodes in each zone of the default worker pool in your cluster. For example, if your default worker pool spans three zones, but hosts are assigned as worker nodes to the default worker pool in only two zones, the Ingress subdomain in your cluster cannot be created, and you cannot access the web console.
* The private IP addresses of your instances were automatically registered and added to your location's DNS record and the cluster's Ingress subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console.

{: tsResolve}
**Worker nodes do not exist in each zone of the default worker pool:**

If you use host autoassignment, [attach hosts](/docs/satellite?topic=satellite-hosts#attach-hosts) to your {{site.data.keyword.satelliteshort}} location in the zone where you do not have worker nodes so that hosts can be assigned to the default worker pool. If the hosts are not automatically assigned, you can also manually [assign {{site.data.keyword.satelliteshort}} hosts in that zone to your cluster](/docs/satellite?topic=satellite-hosts#host-assign).

**Host private IP addresses are used for Ingress subdomain:**

Connect to your hosts' private network to access to your cluster and open the {{site.data.keyword.openshiftshort}} web console. For example, you might connect to your on-premises local network, or use a VPN such as [Wireguard](/docs/openshift?topic=openshift-access_cluster#access_vpn) to connect to your cloud provider's private network. 

Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's service URL and your location's DNS record to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access). Note that making your location and cluster subdomains available outside of your hosts' private network to your authorized cluster users is not recommended for production-level workloads.

If you are still unable to access the {{site.data.keyword.openshiftshort}} web console after completing these steps, see [Debugging the OpenShift web console](/docs/openshift?topic=openshift-ocp-debug) in the {{site.data.keyword.openshiftlong_notm}} troubleshooting documentation.
{: note}
