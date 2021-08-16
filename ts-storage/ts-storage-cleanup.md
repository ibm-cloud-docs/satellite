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


# Why is the namespace where my storage operator was deployed stuck in **Terminating** status?
{: #storage-namespace-terminating}

{: tsSymptoms}
When you remove a storage configuration from a cluster, the resources such as operator pods and storage classes are removed, but the namespace is stuck in `Terminating` status. 

{: tsCauses}
There are [finalizers](https://kubernetes.io/blog/2021/05/14/using-finalizers-to-control-deletion/){: external} that are preventing the remaining resources in the namespace and the namespace itself from being deleted. 

{: tsResolve}
Take the following steps to remove the resources and the namespace.

Do not delete or patch the resource finalizers in the `kube-system` namespace.
{: important}

1. Get the namespace that is stuck in `Terminating` status. Make a note of the resources that are listed in the `message: 'Some resources are remaining:` section.
    ```sh
    oc get ns <namespace> -o yaml
    ```
    {: pre}

    **Example output for a NetApp Trident namespace**
    ```sh
    status:
        conditions:
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: All resources successfully discovered
        reason: ResourcesDiscovered
        status: "False"
        type: NamespaceDeletionDiscoveryFailure
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: All legacy kube types successfully parsed
        reason: ParsedGroupVersions
        status: "False"
        type: NamespaceDeletionGroupVersionParsingFailure
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: All content successfully deleted, may be waiting on finalization
        reason: ContentDeleted
        status: "False"
        type: NamespaceDeletionContentFailure
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: 'Some resources are remaining: tridentbackends.trident.netapp.io has 1 resource instances, tridentnodes.trident.netapp.io has 3 resource instances, tridentversions.trident.netapp.io has 1 resource instances, tridentvolumes.trident.netapp.io has 1 resource instances'
        reason: SomeResourcesRemain
        status: "True"
        type: NamespaceContentRemaining
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: 'Some content in the namespace has finalizers remaining: trident.netapp.io
        in 6 resource instances'
        reason: SomeFinalizersRemain
        status: "True"
        type: NamespaceFinalizersRemaining
        phase: Terminating
    ```
    {: screen}

2. Run the `oc get` command to get the resources that are pending removal. In the example output, the `tridentbackends.trident.netapp.io has 1 resource instances`. Repeat this step for each resource that has remaining instances listed in the `message` section of the namespace YAML that you retrieved earlier.
    ```sh
    oc get <resource> -n <namespace>
    ```
    {: pre}

    Example command for the `tridentbackends.trident.netapp.io` resource.
    ```sh
    oc get tridentbackends.trident.netapp.io -n trident
    ```
    {: pre}

    **Example output**
    ```sh
    tbe-bhb5r
    ```
    {: screen}

    ```sh
    pvc-26d97ecd-c268-47ff-a2fd-a0a1a51a9e12
    ```
    {: screen}

3. For each resource that has remaining instances, run the following `kubectl patch` command to remove the finalizers and delete the resource. After all of the resources that have remaining instances have been patched, the namespace is removed.
    ```sh
    kubectl -n <namespace> patch <resource>/<instance> -p '{"metadata":{"finalizers":[]}}' --type=merge
    ```
    {: pre}

    Example command for the `tridentbackends.trident.netapp.io` resource.
    ```sh
    kubectl -n trident patch tridentbackends.trident.netapp.io/tbe-bhb5r -p '{"metadata":{"finalizers":[]}}' --type=merge
    ```
    {: pre}

4. After you have removed the finalizers on all of the remaining resources, run the `oc get ns` command to verify that namespace has been removed.
    ```sh
    oc get ns
    ```
    {: pre}







