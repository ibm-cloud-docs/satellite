---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I see a location that another user gave me access to?
{: #ts-location-missing-location}

You are granted access to another user's {{site.data.keyword.satelliteshort}} location. However, when you list locations, you do not see the location.
{: tsSymptoms}

The location owner might have scoped your {{site.data.keyword.satelliteshort}} access in {{site.data.keyword.cloud_notm}} IAM to only the `location` resource type, which prevents the location from returning unless you target the regional endpoint that the location is managed from.
{: tsCauses}

Target the regional endpoint, or ask the location owner to update your permissions.
{: tsResolve}

## Target the regional endpoint
{: #ts-location-missing-location-target}

1. Ask the location owner which [{{site.data.keyword.cloud_notm}} multizone region](/docs/satellite?topic=satellite-sat-regions) the {{site.data.keyword.satelliteshort}} location is managed from. For example, the owner can run `ibmcloud sat location get --location <location_name_or_ID>` and review the **Managed from** field.
2. From the CLI, [target the regional endpoint](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_init), such as Washington, D.C. (`us-east`) in the following example.
    ```sh
    ibmcloud oc init --host https://us-east.containers.cloud.ibm.com
    ```
    {: pre}

3. Verify that you can view the {{site.data.keyword.satelliteshort}} location.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

If you still cannot view the {{site.data.keyword.satelliteshort}} location, ask the location owner to check your access policy. If the access policy is scoped to a particular location, the policy must be scoped to the location ID, not to the location's name.

## Ask the location owner to update your permissions
{: #ts-location-missing-location-perms}

Ask the location owner to update your access policy in {{site.data.keyword.cloud_notm}} IAM so that access to {{site.data.keyword.satelliteshort}} locations is no longer scoped to locations. The steps vary depending on how the location owner set up your access policy. The following commands provide examples for updating access group and individual policies from the CLI. For more information, see [Managing access for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-iam).

1. Log in to {{site.data.keyword.cloud_notm}}. If you have a federated account, include the `--sso` option.
    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

2. List the access policies for the user or access group, and note the **Policy ID** that grants access to the {{site.data.keyword.satelliteshort}} location.
    * For individual users
        ```sh
        ibmcloud iam user-policies <user@email.com>
        ```
        {: pre}

    * For access groups

        ```sh
        ibmcloud iam access-group-policies <access_group>
        ```
        {: pre}

        Example output
        ```sh
        Policy ID:   11a11111-bb2b-3c33-444d-ee5ee55ee55e
        Roles:       Viewer   
        Resources:                         
                Service Name    satellite      
                Resource Type   location   
        ```
        {: screen}

3. Update the access policy so that the policy is no longer scoped to locations. 
    - For individual users
        ```sh
        ibmcloud iam user-policy-update <user@email.com> <policy_ID> --roles Viewer --service-name satellite
        ```
        {: pre}

    - For access groups
        ```sh
        ibmcloud iam access-group-policy-update <group> <policy_ID> --roles Viewer --service-name satellite
        ```
        {: pre}

        Example output
        ```sh
        Policy ID:   11a11111-bb2b-3c33-444d-ee5ee55ee55e
        Version:     2-111aaa1111a1a1aa1a1a11aa11a1aa11
        Roles:       Viewer 
        Resources:                         
                    Service Name    satellite      
        ```
        {: screen}
