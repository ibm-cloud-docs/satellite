---


copyright:
  years: 2024, 2025
lastupdated: "2025-04-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Deploying multiple Connector Windows agents on a single machine
{: #connector-create-endpoints-ma}


Beginning with Windows agent version 1.2.0, multiple agents on a single Windows host are now supported. Microsoft .NET 3.5 is required on the Server to use version `>= 1.2.0` of the Windows agent.


Running multiple agents on the same Windows host is not supported for Satellite Connector **Cloud** [endpoints](/docs/satellite?topic=satellite-connector-create-endpoints&interface=cli#configure-connector-oncloud-port ) because the Cloud endpoint ingress is the Windows host that runs the agent. Cloud endpoints and running multiple Windows agents on a single host are mutually exclusive features. If you need to use Cloud endpoints with Windows agents, it is recommended that you install 1 agent per Windows host. 
{: note}

## Considerations
{: #ma-considerations}

- The service name of a Windows Agent has been updated in version `1.2.0` from `satelliteconnectorservice.exe` to `SatelliteConnectorService`. Because of this change, you must use the uninstall scripts that came with the agent you want to remove.

- In version `1.2.0`, to distinguish between the multiple Windows agents installed on a single Windows host, a new parameter was introduced in the agent config file named `SATELLITE_CONNECTOR_INSTANCE_NAME`. This parameter is appended to the Service Name and separated by a `-`. To install multiple Windows agents, you must now specify a config file when calling the installation or uninstallation scripts.

- The Windows agent runs as a node process on the Windows host. In both the IBM Cloud Console and the IBM Cloud CLI, the node process ID is appended to the Connector name and separated by a `.`.

- In versions earlier than `1.2.0` of the Windows agent, you weren't allowed to specify a config file for the installation or uninstallation scripts. Instead, the scripts automatically took the file `config.json` from the same directory and used it to install or uninstall a single Windows agent.


## Installing multiple Windows agents
{: #install-multiple}

To deploy multiple agents on a single hots, you must use a configFile parameter.


1. Copy the following configuration and save it to a file called `config_connector1.json`.

    ```json
    {
      "SATELLITE_CONNECTOR_ID": "<connector_id>",
      "SATELLITE_CONNECTOR_IAM_APIKEY": "<api_key>",
      "SATELLITE_CONNECTOR_TAGS": "<tags>",
      "SATELLITE_CONNECTOR_INSTANCE_NAME": "<instance_name>",
      "LOG_LEVEL": "<log_level>",
      "PRETTY_LOG": <true|false>
    }
    ```
    {: codeblock}

1. Populate the config file with your Connector details and credentials. **Do not** name the file `config.json`. The `SATELLITE_CONNECTOR_INSTANCE_NAME`is required and must start with a character, can contain only characters `(a-zA-Z)`, numbers `(0-9)` and `_`, and the total length must be less than 50 characters.


1. Run the installation script.

    ```txt
    ./install.ps1 -configFile config_connector1.json
    ```
    {: pre}


1. View the agents running on a Windows Server.
    ```txt
    PS >  Get-Service -Name SatelliteConnectorService* | Format-Table -AutoSize
    ```
    {: pre}
    
    ```sh
    Status  Name                                  DisplayName
    ------  ----                                  -----------
    Running SatelliteConnectorService             SatelliteConnectorService
    Running SatelliteConnectorService-connector_1 SatelliteConnectorService-connector_1
    Running SatelliteConnectorService-connector_2 SatelliteConnectorService-connector_2
    ```
    {: screen}

1. View the node processes running on a Windows Server.

    ```txt
    PS > get-process | Where-Object {$_ -match 'node'}
    ```
    {: pre}

    ```sh
    NPM(K) PM(M) WS(M) CPU(s)   Id SI ProcessName
    ------ ----- ----- ------   -- -- -----------
        24 61.05 40.44   0.89 1796  0 node
        39 10.43 24.29   0.11 3436  0 node
        39 10.74 29.70   0.17 4288  0 node
        24 61.60 39.86   0.70 5720  0 node
        24 61.44 40.64   0.81 5924  0 node
        39 10.63 29.81   0.17 7932  0 node
    ```
    {: screen}


1. Get the details of `connector_1`.

    ```sh
    ibmcloud sat connector get --connector-id U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzkwMjFpNnNlcnJxdTJlZyI
    ```
    {: pre}

    ```sh
    OK

    ID:                    U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzkwMjFpNnNlcnJxdTJlZyI
    Name:                  connector_1
    CRN:                   crn:v1:staging:public:satellite:us-south:a/1ae4eb57181a46ceade48465196706a7:U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzkwMjFpNnNlcnJxdTJlZyI::
    Managed From:          Dallas (us-south)
    Resource Group ID:     60735c4daed84fcea7d3fe99c298c9b3
    Resource Group Name:   Default
    State:                 created
    Created Date:          2025-04-11 03:43:16 -0500 (48 minutes ago)
    ```
    {: screen}

1. View the agent belonging to the connector `connector_1`.
    ```sh
    ibmcloud sat agent ls --connector-id U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzkwMjFpNnNlcnJxdTJlZyI
    ```
    {: pre}

    ```sh
    OK
    Name                               Release                                               Tags
    sat-link-e2e-wi/connector_1.1796   20250410-5f291315c59db895783cf54411765942806802ac_W   connector_1
    ```
    {: screen}


1. Get the details of `connector_2`.
    ```sh
    ibmcloud sat connector get --connector-id U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzlzMjF0OWFsMjQ2YXZjMCI
    ```
    {: pre}

    ```sh
    OK

    ID:                    U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzlzMjF0OWFsMjQ2YXZjMCI
    Name:                  connector_2
    CRN:                   crn:v1:staging:public:satellite:us-south:a/1ae4eb57181a46ceade48465196706a7:U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzlzMjF0OWFsMjQ2YXZjMCI::
    Managed From:          Dallas (us-south)
    Resource Group ID:     60735c4daed84fcea7d3fe99c298c9b3
    Resource Group Name:   Default
    State:                 created
    Created Date:          2025-04-11 03:43:19 -0500 (50 minutes ago)
    ```
    {: screen}

1. View the agent belonging to the connector `connector_2`.
    ```sh
    ibmcloud sat agent ls --connector-id U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkYzlzMjF0OWFsMjQ2YXZjMCI
    ```
    {: pre}

    ```sh
    OK
    Name                               Release                                               Tags
    sat-link-e2e-wi/connector_2.5924   20250410-5f291315c59db895783cf54411765942806802ac_W   connector_2
    ```
    {: screen}

## Installing a single agent
{: #single-agents}

Review the following steps to use the `install.ps1` script to install only a single agent.

- Don't specify the configFile parameter. This ensures backward compatibility for Agents < 1.2.0.
- The config file name must be `config.json`.
- The config file `config.json` must not contain a `SATELLITE_CONNECTOR_INSTANCE_NAME`.


1. Create a file called `config.json` and enter your Connector details.

    ```json
    {
      "SATELLITE_CONNECTOR_ID": "<connector_id>",
      "SATELLITE_CONNECTOR_IAM_APIKEY": "<api_key>",
      "SATELLITE_CONNECTOR_TAGS": "<tags>",
      "LOG_LEVEL": "<log_level>",
      "PRETTY_LOG": <true|false>
    }
    ```
    {: codeblock}

1. Run the installation command on your Windows host.
    ```sh
    ./install.ps1
    ```
    {: pre}

1. Get the details of your Connector.

    ```sh
    ibmcloud sat connector get --connector-id U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkbjZmMjFyOTJxdTgyZ3BzZyI
    ```
    {: pre}

    ```sh
    OK

    ID:                    U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkbjZmMjFyOTJxdTgyZ3BzZyI
    Name:                  connector
    CRN:                   crn:v1:staging:public:satellite:us-south:a/1ae4eb57181a46ceade48465196706a7:U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkbjZmMjFyOTJxdTgyZ3BzZyI::
    Managed From:          Dallas (us-south)
    Resource Group ID:     60735c4daed84fcea7d3fe99c298c9b3
    Resource Group Name:   Default
    State:                 created
    Created Date:          2025-04-11 04:06:33 -0500 (19 minutes ago)
    ```
    {: screen}

1. View the agent belonging to the connector.

    ```sh
    ibmcloud sat agent ls --connector-id U2F0ZWxsaXRlQ29ubmVjdG9yOiJjdnNkbjZmMjFyOTJxdTgyZ3BzZyI
    ```
    {: pre}

    ```sh
    OK
    Name                   Release                                               Tags
    sat-link-e2e-wi.5720   20250410-5f291315c59db895783cf54411765942806802ac_W   connector
    ```
    {: pre}



## FAQ
{: #multiple-faq}

Can I add a multi Windows agent to an existing single Windows agent installation?
:   Yes, download a Windows agent zip file >=1.2.0, unpack it to a different directory than the existing single Windows agent installation and configure an `SATELLITE_CONNECTOR_INSTANCE_NAME` in the config file not named `config.json` The use `install.ps1 -configFile <agent-config-file.json>` to install it.

Why is the `update-service.ps1` missing in Versions >= 1.2?
:   The `update-service.ps1` was an uninstall followed by a new install, so we decided to remove it. If you need to update a Windows agent, uninstall it, update the config file and install it again.

Which Windows agent version is installed on my Windows host?
:   The directory where you have installed the Agent on your Windows Server contains a file named `version.txt`, that contains the agent version. If the directory does not contain a file named `version.txt`, the version is <= 1.1.6.

Which Agent version have I downloaded?
:   After downloading the Windows Agent ZIP file with the command `ibmcloud sat agent attach --platform windows` and unpacking it with `Expand-Archive -Force -Path "$filename" -DestinationPath "."`, there is a file named `version.txt` that contains the agent version.
