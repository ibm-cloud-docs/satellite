---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-16"

keywords: satellite storage, satellite config, debug, troubleshoot, must gather

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Debugging storage
{: #storage-must-gather}

Complete the following steps to debug your Satellite storage configurations. 
{: shortdesc}

## Log in to your cluster
{: #storage-log-in}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.

    ```sh
    ibmcloud login
    ```
    {: pre}

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

1. Target the `Managed from` region of your {{site.data.keyword.satelliteshort}} location. For example, for `wdc` target `us-east`. For more information, see [{{site.data.keyword.satelliteshort}} regions](/docs/satellite?topic=satellite-sat-regions).

    ```sh
    ibmcloud target -r us-east
    ```
    {: pre}

1. If you use a resource group other than `default`, target it.

    ```sh
    ibmcloud target -g <resource-group>
    ```
    {: pre}
    

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

## Get your configuration details
{: #storage-config-details}

1. List your  storage configurations.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the details of the storage configuration you wish to troubleshoot and take note of the `storage-template-version` field. 
    ```sh
    ibmcloud sat storage config get --config <CONFIG_NAME> --json
    ```
    {: pre}

1. List your storage assignments.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Get the details of the storage assignment that uses your configuration and check that the `version` field matches your configuration version. 
    ```sh
    ibmcloud sat storage assignment get --assignment <ASSINGMENT_ID> --json
    ```
    {: pre}

1. If the `storage-template-version` and your assignment `version` do not match, upgrade the storage assignment. 
    ```sh
    ibmcloud sat storage assignment upgrade --assignment <ASSIGNMENT_ID>
    ```
    {: pre}

## Gather the `mustachetemplate-controller` pod logs
{: #storage-gather-controller-log}

1. List the pods in the `razeedeploy` namespace.
    ```sh
    oc get pods -n razeedeploy
    ```
    {: pre} 

1. Get the `mustachetemplate-controller` pod logs and check for any configuration or assignment creation errors.
    ```sh
    oc logs mustachetemplate-controller-xxx-xxx -n razeedeploy
    ```
    {: pre}


## Get the storage driver pod logs
{: #storage-gather-driver-pods}

Note that the namespaces and pod names are template specific. Te verify these details, refer to the [storage template](/docs/satellite?topic=satellite-sat-storage-template-ov#storage-template-ov-providers) page that you are using in your cluster. 
{: tip}

1. Get the storage driver pods from the namespace where they are deployed.
    ```sh
    oc get pods -n kube-system | grep POD-NAME
    ```
    {: pre}

1. Get the storage driver pod logs.
    ```sh
    oc get logs STORAGE-DRIVER-POD -n NAMESPACE
    ```
    {: pre}

## Get the storage class details
{: #storage-gather-sc-details}

1. Get the storage class details for your template.
    ```sh
    oc get sc
    ```
    {: pre}

## Get the details of your persisent volume claims
{: #storage-gather-pvc-details}

1. If you deployed apps that use storage, list your PVCs.

    ```sh
    oc get pvc
    ```
    {: pre}

    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    my-pvc                              1/1     Running   0          2m58s
    ```
    {: screen}

## Get the pod logs for your app
{: #storage-get-pod-logs}

1. Get a list of your app pods.
    ```sh
    oc get pods -n NAMESPACE
    ```
    {: pre}

1. Get the logs for the pod you want to troubleshoot.
    ```sh
    oc logs POD -n NAMESPACE
    ```
    {: pre}

## Review the details
{: #storage-review-details}

Review the error messages, logs, and details of your configurations and troubleshoot. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-sitemap#sitemap_storage) for further debugging steps.

## Contact Support
{: #contact-ibm-support}

If the issue persists, open a [support case](/docs/get-support?topic=get-support-using-avatar). In the case details, be sure to include any relevant log files, error messages, or command outputs.





