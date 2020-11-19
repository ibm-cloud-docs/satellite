---

copyright:
  years: 2020, 2020
lastupdated: "2020-11-19"

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
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Why don't cluster list or get updates to Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config?
{: #satconfig-cluster-access-error}

{: tsSymptoms}
When you register a cluster to use with {{site.data.keyword.satelliteshort}} Config, you do not see the cluster resources show up in the resources list. Even though you subscribe the cluster to a configuration, no Kubernetes resources are created or updated in the cluster.

{: tsCauses}
To use a cluster to use with {{site.data.keyword.satelliteshort}} Config, the proper components must be installed, and you must grant {{site.data.keyword.satelliteshort}} Config permissions in your cluster to manage Kubernetes resources.

{: tsResolve}
1.  Re-attach the cluster to {{site.data.keyword.satelliteshort}} Config. For more information, see [Registering existing {{site.data.keyword.openshiftlong_notm}} clusters with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).
    1.  Get a `kubectl` command to register your cluster with {{site.data.keyword.satelliteshort}} Config.
        ```
        ibmcloud sat cluster register
        ```
        {: pre}

        Example output:
        ```
        kubectl apply -f "https://config.satellite.cloud.ibm.com/api/install/razeedeploy-job?orgKey=<orgApiKey>&args=--clustersubscription=<number>&args=--featureflagsetld=<number>&args=--mustachetemplate=<number>&args=--managedset=<number>&args=--remoteresources<number>&args=--remoteresource=<number>&args=--watch-keeper=<number>"
        ```
        {: screen}
    2.  Log in to your cluster. If you are not connected to your location host network, include the `--endpoint link` flag. For more login options, see [Accessing {{site.data.keyword.openshiftshort}} clusters](/docs/openshift?topic=openshift-access_cluster).
        ```
        ibmcloud oc cluster config -c <cluster_name_or_ID> --admin [--endpoint link]
        ```
        {: pre}
    3.  Run the `kubectl` command that you previously retrieved.
2.  Grant {{site.data.keyword.satelliteshort}} Config permissions in your cluster to manage Kubernetes resources. The following command grants `cluster-admin` permissions for the entire cluster. For more options, see [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access).
    ```
    kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
    ```
    {: pre}
