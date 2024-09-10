---


copyright:
  years: 2023, 2024
lastupdated: "2024-09-10"

keywords: satellite, hybrid, multicloud, connector, create connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Connector
{: #create-connector}

A Connector provides a secure connection between a specific remote location and {{site.data.keyword.cloud_notm}}. Use the {{site.data.keyword.satelliteshort}} console to create your Connector.
{: shortdesc}

## Prerequisites
{: #create-connector-prereqs-ui}
{: ui}

- To configure {{site.data.keyword.satelliteshort}} Connectors, you must have Administrator access to the **Satellite** service in IAM access policies.
- Determine the region where you want to deploy the Connector. For a list of supported regions, see [Supported IBM Cloud regions](/docs/satellite?topic=satellite-sat-regions).


## Creating a Connector in the console
{: #create-connector-console}
{: ui}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Connectors**.
1. Click **Create connector**.
1. For **Name**, enter a unique name for your Connector. The default value is `connector`.
1. For **Tags**, enter one or more tags that can help you organize resources throughout your account by filtering by tags from your resource list. This field is optional. For more information, see [Working with tags](/docs/account?topic=account-tag).
1. For **Resource group**, assign a resource group to your connector. The default value is `Default`.
1. For **Description**, describe what this Connector is used for. This field is optional.
1. For the **Managed from** menu, select the {{site.data.keyword.cloud_notm}} region that is closest to where your infrastructure resides to ensure low network latency.
1. In the **Summary** panel, review your order details, and then click **Create connector**. 
{: #create-connector-steps}



## Prerequisites for creating a Connector
{: #create-connector-prereqs}
{: cli}

1. [Install the CLI](/docs/openshift?topic=openshift-cli-install&interface=cli).

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` option, or create an API key to log in.

    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

1. Follow the CLI prompts to select your account.

1. Target your region and resource group.

    ```sh
    ibmcloud target -r REGION -g GROUP
    ```
    {: pre}


## Creating a Connector in the CLI
{: #create-connector-cli}
{: cli}

1. Review the command parameters by running the following command.
    ```sh
    ibmcloud sat connector create --help
    ```
    {: pre}

1. Run the following command to create a Connector.

    ```sh
    ibmcloud sat connector create --name NAME --region REGION
    ```
    {: pre}

    Example command to create a Connector called `my-connector` in `us-south`.
    ```sh
    ibmcloud sat connector create --name my-connector --region us-south
    ```
    {: pre}

    Example output.
    ```sh
    Creating Satellite connector...
    OK
    Satellite connector my-connector was successfully created with ID xxxxxxXXXXXXaaaaaaaAAAAA11111
    ```
    {: screen}

1. Verify your Connector was created by listing the Connectors in your account.

    ```sh
    ibmcloud sat connector ls
    ```
    {: pre}

    Example output.
    ```sh
    OK
    ID                                                        Name             Managed From   State
    xxxxxxXXXXXXaaaaaaaAAAAA11111   my-connector     Dallas         created
    ```
    {: screen}


## Next steps
{: #connector-next-steps}

After creating a Connector, you must set up an agent to complete your set up.

- Set up a Connector agent. For more information, see [Running a Connector agent](/docs/satellite?topic=satellite-run-agent-locally).
- [Run the agent image in Swarm mode](/docs/satellite?topic=satellite-run-agent-swarm) for high-availability.
- [Follow the end-to-end example](/docs/satellite?topic=satellite-end-to-end) to run the agent, configure your endpoints, and add TLS support.
