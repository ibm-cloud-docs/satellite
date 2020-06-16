# Setting up the {{site.data.keyword.satelliteshort}} CLI
{: #setup-cli}

Set up the {{site.data.keyword.cloud_notm}} command line interface (CLI), the {{site.data.keyword.satelliteshort}} plug-in, and other related CLIs. 
{: shortdesc}

1.  Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started#idt-prereq). This installation includes: 
    * `ibmcloud oc`: Use this plug-in to create and manage {{site.data.keyword.openshiftlong_notm}} clusters. These clusters can be attached to a {{site.data.keyword.satelliteshort}} location to run your worklaods on.
    * `ibmcloud cr`: Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by IBM, and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster.
2.  To work with OpenShift Container Platform workloads, [install the `oc` CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).
3.  Log in to the {{site.data.keyword.cloud_notm}} CLI. If you have a federated account, include the `--sso` flag, or create an API key to log in.
    ```
    ibmcloud login -a test.cloud.ibm.com -r us-south -u <first.last@ibm.com> --sso
    ```
    {: pre}
4.  Install the staging environment repository.
    ```
    ibmcloud plugin repo-add stage https://plugins.test.cloud.ibm.com
    ```
    {: pre}
5.  Install the {{site.data.keyword.satelliteshort}} CLI plug-in.
    ```
    ibmcloud plugin install kubernetes-service -r stage
    ```
    {: pre}
6.  Verify that the {{site.data.keyword.satelliteshort}} plug-in is installed by running a help command.
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
