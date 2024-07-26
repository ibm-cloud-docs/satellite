---


copyright:
  years: 2024, 2024
lastupdated: "2024-07-26"

keywords: satellite config, subscription, id, permissions, not authorized

subcollection: satellite
content-type: troubleshoot

---
{{site.data.keyword.attribute-definition-list}}


# Why does my {{site.data.keyword.satelliteshort}} Config rollout fail and result in a `not authorized` error?
{: #ts-satconfig-subid-perms}

When you create and apply a {{site.data.keyword.satelliteshort}} Config subscription to create, update, or delete Kubernetes resources, the rollout fails and you see a `403 not authorized` error. 
{: tsSymptoms}

The user ID that is used as the {{site.data.keyword.satelliteshort}} Config subscription identity (the subscription "owner") does not have the permissions required to make changes on the clusters that the rollout applies to. A {{site.data.keyword.satelliteshort}} Config rollout applies changes under the subscription owner's user ID, so if the listed subscription owner has lost their permissions -- such as by leaving the organization or changing to a new role -- the rollout fails. Or, the subscription identity is [incorrectly synced](/docs/satellite?topic=satellite-sat-config-subid) and the cluster cannot verify the user ID permissions. 
{: tsCauses}

Change the subscription identity to use a different user ID. That user becomes the new subscription "owner", and the {{site.data.keyword.satelliteshort}} Config rollout uses their permissions. 
{: tsResolve}

1. Verify that the current subscription identity belongs to a user that no longer has the necessary permissions. In the **status** section of the command output, find the **Impersonate-user** field and note the listed username, which might be listed as a single value or in the form of an email address, such as `username@ibm.com`. In the same **status** section, look for a `403` error with a message similar to the one in the example output.

    To find the subscription name, run `ibmcloud sat subscription ls`.
    {: tip}

    ```sh
    {[kubectl]} get rr -n razeedeploy <subscription-name> -o yaml
    ```
    {: pre}

    Example **status** section in the command output.

    ```yaml
    status:
    children:
        /api/v1/namespaces/default/configmaps/uploadedapp-cm-2:
        Impersonate-User: {}
        deploy.razee.io/Reconcile: "true"
    last-modified: []
    razee-logs:
        error:
        e2da78e5642c34f1b4f7916ab960a37571db4a9d: '1 errors occurred: Error applying
            file to kubernetes. StatusCode: 403 url: https://config.satellite.test.cloud.ibm.com/api/v1/channels/UploadedApp1/55a55aaa-5a5a-555a
            message: user does not have permissions to create resources of type ConfigMap'
    ```
    {: screen}

2. Change the subscription identity to a different user ID. Note that you can only update a subscription identity to use your own user ID, not the user ID of someone else. If you want someone else to be the subscription owner, they must run the command in their own account. 

    ```sh
    ibmcloud sat subscription identity set --subscription <subscription-name> 
    ```
    {: pre}

3. Try to apply the {{site.data.keyword.satelliteshort}} Config changes again. 
