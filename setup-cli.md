---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-29"

keywords: satellite cli, install satellite cli, satellite cli commands

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
{:swift-ios: .ph data-hd-programlang='iOS Swift'}
{:swift-server: .ph data-hd-programlang='server-side Swift'}
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


# Installing the {{site.data.keyword.satelliteshort}} CLI
{: #setup-cli}

Set up the {{site.data.keyword.cloud_notm}} command-line interface (CLI), the {{site.data.keyword.satelliteshort}} plug-in, and other related CLIs.
{: shortdesc}

1.  Install the stand-alone [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli) (`ibmcloud`). 
    
    Plan to use the CLI often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} CLI (Linux/macOS only)](/docs/cli/reference/ibmcloud?topic=cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
    ```
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your username and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}
4.  Install the {{site.data.keyword.cloud_notm}} plug-in for {{site.data.keyword.containershort_notm}}. This plug-in includes `ibmcloud sat` commands to manage {{site.data.keyword.satelliteshort}} resources and `ibmcloud oc` to manage {{site.data.keyword.openshiftshort}} cluster resources.
    ```
    ibmcloud plugin install container-service
    ```
    {: pre}
5.  Install the {{site.data.keyword.cloud_notm}} plug-in for {{site.data.keyword.registrylong_notm}} (`ibmcloud cr`). Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by IBM, and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster.
    ```
    ibmcloud plugin install container-registry
    ```
    {: pre}
6.  Optional: To create a logging configuration for {{site.data.keyword.la_full_notm}} or a monitoring configuration for {{site.data.keyword.mon_full_notm}} for your cluster, install the {{site.data.keyword.containerlong_notm}} observability plug-in (`ibmcloud ob`).
    ```
    ibmcloud plugin install observe-service
    ```
    {: pre}
8.  Verify that the plug-ins are installed correctly.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Example output:
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status
    container-registry                     0.1.404
    container-service/kubernetes-service   0.4.66
    ```
    {: screen}
9.  To work with OpenShift Container Platform workloads, [install the `oc` CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).

<br />

## Updating the CLI
{: #update-sat-cli}

Update the CLIs regularly to use new features.
{: shortdesc}

1. Update the {{site.data.keyword.cloud_notm}} CLI. Download the [latest version](/docs/cli?topic=cli-getting-started) and run the installer.
2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
   ```
   ibmcloud login
   ```
   {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your username and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

3.  Update the {{site.data.keyword.satelliteshort}} CLI plug-in. The {{site.data.keyword.satelliteshort}} plug-in is part of the {{site.data.keyword.containerlong_notm}} CLI plug-in.
    1.  Install the update from the {{site.data.keyword.cloud_notm}} plug-in repository.
        ```
        ibmcloud plugin update kubernetes-service
        ```
        {: pre}

    2.  Verify the plug-in installation by running the following command and checking the list of the plug-ins that are installed.
        ```
        ibmcloud plugin list
        ```
        {: pre}

        Because the {{site.data.keyword.satelliteshort}} CLI is included in the {{site.data.keyword.containerlong_notm}} CLI plug-in, look for the `container-service/kubernetes-service ` plug-in in your CLI output.
        {: tip}

## Uninstalling the CLI
{: #uninstall-sat-cli}

If you no longer need the CLI, you can uninstall it.
{: shortdesc}

1.  Uninstall the {{site.data.keyword.containerlong_notm}} CLI plug-in.

    The {{site.data.keyword.satellitelong_notm}} CLI plug-in is included in the {{site.data.keyword.containerlong_notm}} plug-in. Removing the plug-in, automatically removes access to both CLI plug-ins.
    {: note}

    ```
    ibmcloud plugin uninstall kubernetes-service
    ```
    {: pre}

2.  Verify that the plug-in is uninstalled by running the following command and checking the list of the plug-ins that are installed.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    The `kubernetes-service` plug-in is not displayed in the results.

5.  [Uninstall the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-uninstall-ibmcloud-cli).

## CLI reference documentation
{: #cli-ref-docs}

For reference information about CLIs that you installed, see the documentation for those tools.
{: shortdesc}

-   [`ibmcloud` commands](/docs/cli/reference/ibmcloud?topic=cli-ibmcloud_cli#ibmcloud_cli)
-   [`ibmcloud sat` commands](/docs/satellite?topic=satellite-satellite-cli-reference)
-   [`ibmcloud oc` commands](/docs/openshift?topic=openshift-kubernetes-service-cli)
-   [`ibmcloud cr` commands](/docs/Registry?topic=container-registry-cli-plugin-containerregcli)
-   [`ibmcloud ob` commands](/docs/containers?topic=containers-observability_cli)
-   [`oc` commands](https://docs.openshift.com/container-platform/4.5/cli_reference/openshift_cli/developer-cli-commands.html){: external}
-   [`kubectl` commands](https://kubectl.docs.kubernetes.io/){: external}
