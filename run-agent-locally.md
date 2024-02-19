---


copyright:
  years: 2023, 2024
lastupdated: "2024-02-19"

keywords: satellite, connector, agent, windows

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Running a Connector agent
{: #run-agent-locally}

After you [create a {{site.data.keyword.satelliteshort}} Connector](/docs/satellite?topic=satellite-create-connector), complete the follow the steps to create an Agent and finalize your setup.   
{: shortdesc}

## Prerequisites
{: #agent-prepreqs}

- [Create a {{site.data.keyword.satelliteshort}} Connector](/docs/satellite?topic=satellite-create-connector).
- [Install the CLI](/docs/openshift?topic=openshift-cli-install&interface=cli).
- **Optional**: [Create a Service ID](https://cloud.ibm.com/iam/serviceids). Service IDs are recommended over using individual user credentials.
- Make sure the user or Service ID that runs agent has the  **Viewer** Platform role or **Reader** Service role for {{site.data.keyword.satelliteshort}} in IAM.
- [Create an API key](https://cloud.ibm.com/iam/apikeys) either by using your own login or your service ID. This API key is used by your Connector agent.
- Make sure your computing environment meets the [Minimum requirements](/docs/satellite?topic=satellite-understand-connectors&interface=ui#min-requirements) for running the agent image.
- [Review the agent parameters](#review-parameters).


## Reviewing the agent parameters
{: #review-parameters}

Configuration information is provided to the agent through the following environment variables. Any of these environment variables can either be set directly to a value or set to a path of a file that contains the value. The path value must be accessible from within the container and is therefore based on the mount point and not a local path on the host. See the following table for an example.

| Environment variable | Required | Description |
| --- | --- | --- |
| `SATELLITE_CONNECTOR_ID` | Yes | The ID of the Satellite Connector that the agent is bound to. You can find your Connector ID in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} or by running the `ibmcloud sat connector ls` command. |
| `SATELLITE_CONNECTOR_IAM_APIKEY` | Yes |Your IAM API key. For security purposes, consider storing your IAM API key in a file and then providing the file for this value. **Note**: In Windows environments, you must escape the slash in the file path. |
| `SATELLITE_CONNECTOR_TAGS` | No | A user defined string that can be helpful to identify your agent. This string can be any value that you find useful. The value must be less than or equal to 256 characters and is truncated if over 256 characters. The following characters are removed: `<>/{}%[]?,;@$&`. |
| `LOG_LEVEL` | No | Set the level of logging detail you want to receive for your agent. You can specify one of `fatal`, `error`, `warn`, `debug`, `info`, or `trace`. The default level is `info`. Typically, the `debug` and `trace` levels are used only when debugging. |
{: caption="Table 1. Environment variables for configuration" caption-side="bottom"}

## Running the agent on your container platform
{: #connector-agent-mac}
  
### Step 1: Creating the local configuration files
{: #create-config-file}

There are several ways to pass agent configuration environment variable information to the container. The following example uses configuration files. However, you can also use the `docker run --env` command to specify the values.

Be aware that if you use `--env` with your API key, the API key is exposed to the container environment and is visible on the output of `docker inspect` command.  You can secure your API key in a file and then use the file name in the environment variable. If you choose to use the file name, you must make sure that the file path that you specify in the environment variable is mounted to a file path in the container, as shown in the following example.
{: note}

The file names shown in the following steps are examples and can be tailored for your environment.
{: tip}


1. Create a directory for the configuration files, in this example `~/agent/env-files`.
1. Create a file in the `~/agent/env-files` directory called `apikey` with a single line value of your IBM Cloud API Key that can access the {{site.data.keyword.satelliteshort}} Connector.
1. Create a file in the `~/agent/env-files` directory called `env.txt` with the following values. Modify the `SATELLITE_CONNECTOR_ID` variable with your {{site.data.keyword.satelliteshort}} Connector ID and the `SATELLITE_CONNECTOR_REGION` with the region of the {{site.data.keyword.satelliteshort}} Connector you created previously. 

    ```sh
    SATELLITE_CONNECTOR_ID=<Your Satellite Connector ID>
    SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/apikey
    SATELLITE_CONNECTOR_TAGS=sample tag
    ```
    {: codeblock}
    
    Example `env.txt` with populated parameters.
    
    ```sh
    SATELLITE_CONNECTOR_ID=U2F0ZWxsaXRlQ29ubmVjdG9yOiJjanM4cnRzZjFsN2c0M3U4cmp1MBA
    SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/apikey
    SATELLITE_CONNECTOR_TAGS=test
    ```
    {: codeblock}
  
1. At this point, your directory contains 2 files and looks similar to the following example.
    ```sh
    env-files$ ls
    apikey  env.txt
    ```
    {: pre}

1. Complete the steps in the following section to pull the agent image.

### Step 2: Pulling the agent image
{: #pull-agent-image}

1. Log in to {{site.data.keyword.registrylong}}. Or log in to the repository directly from Docker with your API key.

    ```sh
    ibmcloud cr region-set icr.io
    ```
    {: pre}
    
    ```sh
    docker login -u iamapikey -p <your apikey> icr.io
    ```
    {: pre}

1. Pull the latest version of the published image. You can find the list of published versions from [IBM Satellite Connector Agent Release History](/docs/satellite?topic=satellite-cl-connector-agent-image).
    ```sh
    docker pull icr.io/ibm/satellite-connector/satellite-connector-agent:latest
    ```
    {: pre}

1. Complete the following steps to run the agent image.


### Step 3: Running the agent image
{: #run-agent-image}

1. To view available versions of agent image, run the following command.

    ```sh
    ibmcloud cr images --include-ibm|grep connector
    ```
    {: pre}
  
    Example output:
    ```sh
    icr.io/ibm/satellite-connector/satellite-connector-agent    latest    5f4e42c8d53e   ibm         1 day ago       124 MB   -
    icr.io/ibm/satellite-connector/satellite-connector-agent    v1.0.5    6eadd91be5c0   ibm         1 week ago      124 MB   -
    icr.io/ibm/satellite-connector/satellite-connector-agent    v1.1.0    5f4e42c8d53e   ibm         1 day ago       124 MB   -
    ```
    {: screen}

1. Mount your `env-files` directory to the container's `/agent-env-files` directory by using the `-v` option. You can use the latest version or a specific version of the published image.  
    
    If an environment variable is using a path to a file, that path must be a file path inside the container. To retrieve the file path, use the `-v` option on the `docker run` command. The `-v` option is specified by the local environment variable directory path, followed by the mounted path in the container and separated by `:`. For example, `-v ~/agent/env-files:/agent-env-files`, where `~/agent/env-files` is your local path and `/agent-env-files` is a path in your container. 
    {: tip}
    
    ```sh
    docker run -d --env-file ~/agent/env-files/env.txt -v ~/agent/env-files:/agent-env-files icr.io/ibm/satellite-connector/satellite-connector-agent:latest
    ```
    {: pre}   

  
    Example command using version 1.1.0 of the image, run the following command.

    ```sh
    docker run -d --env-file ~/agent/env-files/env.txt -v ~/agent/env-files:/agent-env-files icr.io/ibm/satellite-connector/satellite-connector-agent:v1.1.0
    ```
    {: pre}


1. You can verify the tunnel gets established to your Connector by looking at the logs of the container.
    ```sh
    docker logs CONTAINER-ID
    ```
    {: pre}
 
    Near the beginning of the log, you can find entries similar to the following examples.

    ```sh
    {"level":30,"time":"2023-06-20T16:12:20.133Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A02","msg":"Load SATELLITE_CONNECTOR_ID value from SATELLITE_CONNECTOR_ID environment variable."}
    {"level":30,"time":"2023-06-20T16:12:20.138Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A01","msg":"Load SATELLITE_CONNECTOR_IAM_APIKEY value from file /agent-env-files/apikey."}
    {"level":30,"time":"2023-06-20T16:12:20.140Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A02","msg":"Load SATELLITE_CONNECTOR_TAGS value from SATELLITE_CONNECTOR_TAGS environment variable."}
    {"level":30,"time":"2023-06-20T16:12:20.141Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A02","msg":"Load SATELLITE_CONNECTOR_REGION value from SATELLITE_CONNECTOR_REGION environment variable."}
    {"level":30,"time":"2023-06-20T16:12:20.142Z","pid":8,"hostname":"6b793f671c79","name":"connector-agent","msgid":"LA2","msg":"Connector id: U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI, region: us-south, release info: 20230610-dd48822928d35a84b31029a996fa9abc9d29fc93_A."}
    {"level":30,"time":"2023-06-20T16:12:20.392Z","pid":8,"hostname":"6b793f671c79","name":"tunneldns","msgid":"D04","msg":"DoTunnelDNSLookup DNS resolve c-01-ws.us-south.link.satellite.cloud.ibm.com to 169.61.31.178"}
    {"level":30,"time":"2023-06-20T16:12:21.560Z","pid":8,"hostname":"6b793f671c79","name":"utilities","msg":"MakeLinkAPICall GET /v1/connectors/U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI status code 200"}
    {"level":30,"time":"2023-06-20T16:12:21.563Z","pid":8,"hostname":"6b793f671c79","name":"agent_tunnel","msgid":"LAT03","msg":"Got configuration"}
    {"level":30,"time":"2023-06-20T16:12:21.565Z","pid":8,"hostname":"6b793f671c79","name":"agent_tunnel","msgid":"LAT04-wss://c-01-ws.us-south.link.satellite.cloud.ibm.com/ws","msg":"Connecting to wss://c-01-ws.us-south.link.satellite.cloud.ibm.com/ws"}
    {"level":30,"time":"2023-06-20T16:12:21.922Z","pid":8,"hostname":"6b793f671c79","name":"tunneldns","msgid":"D04","msg":"DoTunnelDNSLookup DNS resolve c-01-ws.us-south.link.satellite.cloud.ibm.com to 169.61.31.178"}
    {"level":30,"time":"2023-06-20T16:12:22.294Z","pid":8,"hostname":"6b793f671c79","name":"TunnelCore","msgid":"TC24","msg":"Tunnel open","connector_id":"U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI"}
    {"level":30,"time":"2023-06-20T16:12:22.299Z","pid":8,"hostname":"6b793f671c79","name":"connector_tunnel_base","msgid":"CTB26-U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI","msg":"Send connector information to tunnel server"}
    {"level":30,"time":"2023-06-20T16:12:22.307Z","pid":8,"hostname":"6b793f671c79","name":"connector_tunnel_base","msgid":"CTB27","msg":"Tunnel connected","connector_id":"U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI","cipher":{"name":"TLS_AES_256_GCM_SHA384","standardName":"TLS_AES_256_GCM_SHA384","version":"TLSv1.3"}}
    ```
    {: screen}

After setting up an agent, you can create Endpoints and ACLs to manage access to those endpoints. For more information, see [Creating and managing Connector endpoints](/docs/satellite?topic=satellite-connector-create-endpoints).


## Running the agent on Windows
{: #run-agent-windows}

Connector agent for Windows supports Windows 10 and later or Windows Server 2016 and later.
{: note}

### Step 1: Downloading the Connector agent files from the CLI
{: #windows-agent-download}

The {{site.data.keyword.satelliteshort}} Connector Agent CLI commands are in the `ibmcloud sat experimental` command group, but are fully functional and availabe for use in production. The commands move out the experimental group and become `ibmcloud sat connector agent` on 13 April 2024.
{: note}


1. From the CLI, run the following command to download the agent `.zip` file.

    ```sh
    ibmcloud sat experimental connector agent --platform windows
    ```
    {: codeblock}

    Example output.
    ```sh
    Downloading agent setup tools for windows...
    OK
    Satellite connector agent for windows was successfully returned /var/folders/17/y8wr4y_x1tb4yf__g3wr6g8m0000gp/T/windows_satellite_connector_4097559421.zip
    ```
    {: codeblock}

1. **Optional**: Verify the `sha512sum` of the `.zip` by running the following command in PowerShell.
    ```txt
    Get-FileHash -Algorithm SHA512 -Path c:\windows_satellite_connector_1420916628.zip
    ```
    {: pre}

1. Run the following command in PowerShell to extract the `.zip` file contents.

    ```txt
    Expand-Archive -Path 'C:\path\to\windows_satellite_connector_4097559421.zip' -DestinationPath ‘C:\path\to\extract'
    ```
    {: codeblock}

1. Complete the steps in the following section to update the configuration files that you extracted.



### Step 2: Updating the `config.json` file
{: #windows-agent-parameters}

Configuration information is provided to the agent through the following environment variables in the `config.json` file that you extracted in the previous step. Review the following parameters for the agent image.

1. Update the `config.json` that you extracted earlier with the appropriate values [for each parameter](#review-parameters).

    You must escape the slash in the file path.
    {: note}
    
    Example `config.json`.
    ```json
    {
    "SATELLITE_CONNECTOR_ID":"<Your Satellite Connector ID>",
    "SATELLITE_CONNECTOR_IAM_APIKEY":"<Your API Key>",
    "SATELLITE_CONNECTOR_TAGS":"sample tag",
    "LOG_LEVEL": "info",
    "PRETTY_LOG": true
    }
    ```
    {: codeblock}

    Example `config.json` with populated values.
    ```json
    {
    "SATELLITE_CONNECTOR_ID":"U2F0ZWxsaXRlQ29ubmVjdG9yOiJjanM4cnRzZjFsN2c0M3U4cmp1MBA",
    "SATELLITE_CONNECTOR_IAM_APIKEY":"C:\\path\\to\\apikey",
    "SATELLITE_CONNECTOR_TAGS":"sample tag",
    "LOG_LEVEL": "info",
    "PRETTY_LOG": true
    }
    ```
    {: codeblock}

1. Save the file.

1. Complete the steps in the following section to start the agent.


### Step 3: Starting the agent
{: #windows-agent-run}


1. Start the agent by running the `install` command in PowerShell.
    ```txt
    .\install
    ```
    {: codeblock}
    

1. Verify the agent is running by run the `Get-Service` command in PowerShell.
    ```txt
    Get-Service 'Satellite Connector Service'
    ```
    {: codeblock}

1. View the agent logs by running the `Get-Content` command in PowerShell.
    ```txt
    Get-Content 'C:\path\to\extract\logs\{connector-agent-{{yyyy-mm-dd.n}}.log}'
    ```
    {: codeblock}


1. **Optional**: Stop the agent by running the `uninstall` command in PowerShell.
    ```txt
    .\uninstall
    ```
    {: codeblock}

After setting up an agent, you can create Endpoints and ACLs to manage access to those endpoints. For more information, see [Creating and managing Connector endpoints](/docs/satellite?topic=satellite-connector-create-endpoints).


## Updating the agent on Windows
{: #update-agent-windows}

You can use the `update` command to apply configuration changes to your agent. When you run the command, the agent is stopped, uninstalled, and reinstalled. Complete the following steps to update your agent.

1. Before updating, review the changes in the [Connector Windows agent change log
](/docs/satellite?topic=satellite-cl-connector-windows-agent).

1. Modify the configuration parameters in the `config.json` file.

    Example `config.json`.
    ```json
    {
    "SATELLITE_CONNECTOR_ID":"<Your Satellite Connector ID>",
    "SATELLITE_CONNECTOR_IAM_APIKEY":"<Your API Key>",
    "SATELLITE_CONNECTOR_TAGS":"<tags>",
    "LOG_LEVEL": "info",
    "PRETTY_LOG": true
    }
    ```
    {: codeblock}

    Example `config.json` with populated values.
    ```json
    {
    "SATELLITE_CONNECTOR_ID":"U2F0ZWxsaXRlQ29ubmVjdG9yOiJjanM4cnRzZjFsN2c0M3U4cmp1MBA",
    "SATELLITE_CONNECTOR_IAM_APIKEY":"C:\\path\\to\\apikey",
    "SATELLITE_CONNECTOR_TAGS":"sample tag",
    "LOG_LEVEL": "info",
    "PRETTY_LOG": true
    }
    ```
    {: codeblock}

1. Run the `update` command in PowerShell.
    ```txt
    .\update
    ```
    {: codeblock}

1. Verify the agent is running by run the `Get-Service` command in PowerShell.
    ```txt
    Get-Service 'Satellite Connector Service'
    ```
    {: codeblock}

1. View the agent logs by running the `Get-Content` command in PowerShell.
    ```txt
    Get-Content 'C:\path\to\extract\logs\{connector-agent-{{yyyy-mm-dd.n}}.log}'
    ```
    {: codeblock}



## Next steps
{: #agent-next-steps}

After creating a Connector agent, you can create endpoints to connect from the IBM Cloud private network to a resource running on your Location. You can also control access your endpoints by creating access control list rules. For more information, see [Creating and managing Connector endpoints](/docs/satellite?topic=satellite-connector-create-endpoints).
  

