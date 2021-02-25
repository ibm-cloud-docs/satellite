---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-25"

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


# Why can't I see a location that another user gave me access to?
{: #ts-location-missing-location}

{: tsSymptoms}
You are granted access to another user's {{site.data.keyword.satelliteshort}} location. However, when you list locations, you do not see the location.

{: tsCauses}
The location owner might have scoped your {{site.data.keyword.satelliteshort}} access in {{site.data.keyword.cloud_notm}} IAM to only the `location` resource type, which prevents the location from returning unless you target the regional endpoint that the location is managed from.

{: tsResolve}
Target the regional endpoint, or ask the location owner to update your permissions.

## Target the regional endpoint
{: #ts-location-missing-location-target}

1.  Ask the location owner which [{{site.data.keyword.cloud_notm}} multizone region](/docs/satellite?topic=satellite-sat-regions) the {{site.data.keyword.satelliteshort}} location is managed from. For example, the owner can run `ibmcloud sat location get --location <location_name_or_ID>` and review the **Managed from** field.
2.  From the CLI, [target the regional endpoint](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_init), such as Washington, D.C. (`us-east`) in the following example.
    ```
    ibmcloud oc init --host https://us-east.containers.cloud.ibm.com
    ```
    {: pre}
3.  Verify that you can view the {{site.data.keyword.satelliteshort}} location.
    ```
    ibmcloud sat location ls
    ```
    {: pre}

If you still cannot view the {{site.data.keyword.satelliteshort}} location, ask the location owner to check your access policy. If the access policy is scoped to a particular location, the policy must be scoped to the location ID, not to the location's name.

## Ask the location owner to update your permissions
{: #ts-location-missing-location-perms}

Ask the location owner to update your access policy in {{site.data.keyword.cloud_notm}} IAM so that access to {{site.data.keyword.satelliteshort}} locations is no longer scoped to locations. The steps vary depending on how the location owner set up your access policy. The following commands provide examples for updating access group and individual policies from the CLI. For more information, see [Managing access for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-iam).

1.  Log in to {{site.data.keyword.cloud_notm}}. If you have a federated account, include the `--sso` flag.
    ```
    ibmcloud login [--sso]
    ```
    {: pre}
2.  List the access policies for the user or access group, and note the **Policy ID** that grants access to the {{site.data.keyword.satelliteshort}} location.
    *   For individual users:
        ```
        ibmcloud iam user-policies <user@email.com>
        ```
        {: pre}
    *   For access groups:
        ```
        ibmcloud iam access-group-policies <access_group>
        ```
        {: pre}
    
    Example output:
    ```
    Policy ID:   11a11111-bb2b-3c33-444d-ee5ee55ee55e
    Roles:       Viewer   
    Resources:                         
                Service Name    satellite      
                Resource Type   location   
    ```
    {: screen}
3.  Update the access policy so that the policy is no longer scoped to locations. 
    *   For individual users:
        ```
        ibmcloud iam user-policy-update <user@email.com> <policy_ID> --roles Viewer --service-name satellite
        ```
        {: pre}
    *   For access groups:
        ```
        ibmcloud iam access-group-policy-update <group> <policy_ID> --roles Viewer --service-name satellite
        ```
        {: pre}
    
    Example output:
    ```
    Policy ID:   11a11111-bb2b-3c33-444d-ee5ee55ee55e
    Version:     2-111aaa1111a1a1aa1a1a11aa11a1aa11
    Roles:       Viewer 
    Resources:                         
                Service Name    satellite      
    ```
    {: screen}