---

copyright:
  years: 2023, 2023
lastupdated: "2023-06-23"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why isn't my API key working?
{: #ts-connector-api}


My API key is not working.
{: tsSymptoms}

If your `SATELLITE_CONNECTOR_IAM_APIKEY` environment variable is set to a local path that contains your API key, verify if the following requirements are met:
{: tsResolve}

- The file must contain only a single line with your API key. Comment lines and additional lines are not supported.
- The value of the `SATELLITE_CONNECTOR_IAM_KEY` is the file path that is mounted inside your container. 
  
A common mistake is specifying the local host path to the API key file. However, this needs to be set to the mounted file path inside the container, which is specified via the `-v` option on the `docker run` command. For example, suppose you have a file called `~/env-files/api-key` that contains your API key on your local file system. The `SATELLITE_CONNECTOR_API_KEY` environment variable needs be set to the path that is mounted to the Docker container specified on the `-v` option to make this file available to the container. For example, if you are using the following `docker run` command:

```sh
docker run -d --env-file ~/agent/env-files/env.txt -v ~/agent/env-files:/agent-env-files icr.io/ibm/satellite-connector/satellite-connector-agent:v1.0.3
```
{: pre}
  
The local file system path `~/agent/env-files` is mounted to the container path `/agent-env-files`. Therefore, the `SATELLITE_CONNECTOR_IAM_APIKEY` environment variable value must be set to `/agent-env-files/api-key` instead of `~/agent/env-files/api-key`. For example, `SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/api-key`.





