---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-14"

keywords: satellite cli, install satellite cli, satellite cli commands

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}



# Setting up the {{site.data.keyword.satelliteshort}} CLI
{: #setup-cli}

Set up the {{site.data.keyword.cloud_notm}} command line interface (CLI), the {{site.data.keyword.satelliteshort}} plug-in, and other related CLIs.
{: shortdesc}

By installing the {{site.data.keyword.cloud_notm}} CLI, you automatically install all of the CLI plug-ins that you need to work with {{site.data.keyword.satelliteslong_notm}}.
{: note}

1.  Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started#idt-prereq). This installation includes:
    * `ibmcloud sat`: Use this plug-in to create and manage {{site.data.keyword.satelliteshort}} locations.
    * `ibmcloud oc`: Use this plug-in to create and manage {{site.data.keyword.openshiftlong_notm}} clusters. These clusters can be attached to a {{site.data.keyword.satelliteshort}} location to run your workloads on.
    * `ibmcloud cr`: Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by IBM, and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster.
2.  To work with OpenShift Container Platform workloads, [install the `oc` CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).
3.  Verify that the {{site.data.keyword.satelliteshort}} plug-in is installed by running a help command.
    ```
    ibmcloud sat help
    ```
    {: pre}

    Example output:
    ```
    NAME:
        ibmcloud sat - [Experimental] Manage IBM Cloud Satellite clusters.
    ```
    {: screen}

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
