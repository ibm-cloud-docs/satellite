---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-11"

keywords: satellite, hybrid, multicloud

subcollection: satellite

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


# Debugging the health of the location control plane
{: #ts-locations-control-plane}

When you create a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations), IBM automatically sets up a master for the location control plane in {{site.data.keyword.cloud_notm}}. Additionally, you must assign at least three hosts to the {{site.data.keyword.satelliteshort}} location control plane as worker nodes to run location components that IBM configures. If the location control plane that runs on your hosts has issues, you can debug the location control plane.

1.  Get your {{site.data.keyword.satelliteshort}} location ID.
    ```
    ibmcloud sat location ls
    ```
    {: pre}
2.  List the **Hostnames** of the subdomains for your location control plane hosts.
    ```
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Retrieving location subdomains...
    OK
    Hostname                                                                                                 Records                                                                                                Health Monitor   SSL Cert Status   SSL Cert Secret Name                                          Secret  Namespace   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud   169.62.  196.20,169.62.196.23,169.62.196.30                                                                None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001.us-east.satellite.appdomain.cloud   169.62.  196.30                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002.us-east.satellite.appdomain.cloud   169.62.  196.20                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003.us-east.satellite.appdomain.cloud   169.62.  196.23                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00.us-east.satellite.appdomain.cloud    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00      default  
    ```
    {: screen}

3.  Check the health of the control plane location subdomains by curling each hostname endpoint. If the endpoint returns a `200` response for each host, the control plane worker node is healthy and serving Kubernetes traffic. If not, continue to the next step.
    ```
    curl -v http://<hostname>:30000
    ```
    {: pre}

    Example output of a failed response:
    ```
    * Rebuilt URL to: http://169.xx.xxx.xxx:30000/
    *   Trying 169.xx.xxx.xxx...
    * TCP_NODELAY set
    * Connection failed
    * connect to 169.xx.xxx.xxx port 30000 failed: Operation timed out
    * Failed to connect to 169.xx.xxx.xxx port 30000: Operation timed out
    * Closing connection 0
    curl: (7) Failed to connect to 169.xx.xxx.xxx port 30000: Operation timed out
    ```
    {: screen}

    Example output of a `200` response:
    ```
    * Rebuilt URL to: http://169.xx.xxx.xxx:30000/
    *   Trying 169.xx.xxx.xxx...
    * TCP_NODELAY set
    * Connected to 169.xx.xxx.xxx (169.xx.xxx.xxx) port 30000 (#0)
    > GET / HTTP/1.1
    > Host: 169.xx.xxx.xxx:30000
    > User-Agent: curl/7.54.0
    > Accept: */*
    >
    < HTTP/1.1 200 OK
    < content-length: 58
    < cache-control: no-cache
    < content-type: text/html
    < connection: close
    <
    <html><body><h1>200 OK</h1>
    Service ready.
    </body></html>
    * Closing connection 0
    ```
    {: screen}
3.  Find the **ID** of the host that did not return a `200` response. You can compare the `Host: 169.xx.xxx.xxx` from the previous step with the **Worker IP** in the output of the following command.
    ```
    ibmcloud sat host ls --location <location_ID> | grep infrastructure
    ```
    {: pre}

    Example output:
    ```
    Name     ID                     State        Status   Cluster          Worker ID                Worker IP   
    host1    aaaaa1a11aaaaaa111aa   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx   
    host2    bbbbbbb22bb2bbb222b2   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx  
    host3    ccccc3c33ccccc3333cc   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx  
    ```
    {: screen}
4.  [Add a host to the control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) in the same zone so that the location control plane has enough compute resources to continue running when you remove the unhealthy host.
5.  [Remove the unhealthy host from the location control plane](/docs/satellite?topic=satellite-hosts#host-remove).
6.  Optional: You can reload the operating system on the unhealthy host and try to attach and assign the host to {{site.data.keyword.satellitelong_notm}} again.
