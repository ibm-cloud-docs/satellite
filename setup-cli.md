---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-10"

keywords: satellite cli, install satellite cli, satellite cli commands

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Installing the CLI plug-in for {{site.data.keyword.satelliteshort}} commands
{: #setup-cli}

Set up the {{site.data.keyword.cloud_notm}} command-line interface (CLI), the plug-in for {{site.data.keyword.satelliteshort}} commands, and other related CLIs.
{: shortdesc}

1. Install the stand-alone [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli) (`ibmcloud`). 

    Plan to use the CLI often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} CLI (Linux/macOS only)](/docs/cli/reference/ibmcloud?topic=cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
    ```sh
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your username and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

3. Install the {{site.data.keyword.cloud_notm}} plug-in for {{site.data.keyword.containershort_notm}}. This plug-in includes `ibmcloud sat` commands to manage {{site.data.keyword.satelliteshort}} resources and `ibmcloud oc` to manage {{site.data.keyword.redhat_openshift_notm}} cluster resources.
    ```sh
    ibmcloud plugin install container-service
    ```
    {: pre}

4. Install the {{site.data.keyword.cloud_notm}} plug-in for {{site.data.keyword.registrylong_notm}} (`ibmcloud cr`). Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by {{site.data.keyword.IBM_notm}} , and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster.
    ```sh
    ibmcloud plugin install container-registry
    ```
    {: pre}

5. Optional: To create a logging configuration for {{site.data.keyword.la_full_notm}} or a monitoring configuration for {{site.data.keyword.mon_full_notm}} for your cluster, install the {{site.data.keyword.containerlong_notm}} observability plug-in (`ibmcloud ob`).
    ```sh
    ibmcloud plugin install observe-service
    ```
    {: pre}

6. Verify that the plug-ins are installed correctly.
    ```sh
    ibmcloud plugin list
    ```
    {: pre}

    Example output
    ```sh
    Listing installed plug-ins...

    Plugin Name                            Version   Status
    container-registry                     0.1.404
    container-service/kubernetes-service   0.4.66
    ```
    {: screen}

7. To work with OpenShift Container Platform workloads, [install the `oc` CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).




## Updating the CLI
{: #update-sat-cli}

Update the CLIs regularly to use new features.
{: shortdesc}

1. Update the {{site.data.keyword.cloud_notm}} CLI. Download the [latest version](/docs/cli?topic=cli-getting-started) and run the installer.
2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
    ```sh
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your username and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

3. Update the CLI plug-in for {{site.data.keyword.satelliteshort}} commands. {{site.data.keyword.satelliteshort}} commands are part of the {{site.data.keyword.containerlong_notm}} CLI plug-in.
    1. Install the update from the {{site.data.keyword.cloud_notm}} plug-in repository.
        ```sh
        ibmcloud plugin update kubernetes-service
        ```
        {: pre}

    2. Verify the plug-in installation by running the following command and checking the list of the plug-ins that are installed.
        ```sh
        ibmcloud plugin list
        ```
        {: pre}

        Because {{site.data.keyword.satelliteshort}} commands are included in the {{site.data.keyword.containerlong_notm}} CLI plug-in, look for the `container-service/kubernetes-service` plug-in in your CLI output.
        {: tip}

## Uninstalling the CLI
{: #uninstall-sat-cli}

If you no longer need the CLI, you can uninstall it.
{: shortdesc}

1. Uninstall the {{site.data.keyword.containerlong_notm}} CLI plug-in.

    The {{site.data.keyword.satellitelong_notm}} CLI plug-in is included in the {{site.data.keyword.containerlong_notm}} plug-in. Removing the plug-in, automatically removes access to both CLI plug-ins.
    {: note}

    ```sh
    ibmcloud plugin uninstall kubernetes-service
    ```
    {: pre}

2. Verify that the plug-in is uninstalled by running the following command and checking the list of the plug-ins that are installed.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

    The `kubernetes-service` plug-in is not displayed in the results.

3. [Uninstall the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-uninstall-ibmcloud-cli).

## CLI reference documentation
{: #cli-ref-docs}

For reference information about CLIs that you installed, see the documentation for those tools.
{: shortdesc}

- [`ibmcloud` commands](/docs/cli/reference/ibmcloud?topic=cli-ibmcloud_cli#ibmcloud_cli)
- [`ibmcloud sat` commands](/docs/satellite?topic=satellite-satellite-cli-reference)
- [`ibmcloud oc` commands](/docs/openshift?topic=openshift-kubernetes-service-cli)
- [`ibmcloud cr` commands](/docs/Registry?topic=container-registry-cli-plugin-containerregcli)
- [`ibmcloud ob` commands](/docs/containers?topic=containers-observability_cli)
- [`oc` commands](https://docs.openshift.com/container-platform/4.5/cli_reference/openshift_cli/developer-cli-commands.html){: external}
- [`kubectl` commands](https://kubectl.docs.kubernetes.io/){: external}


