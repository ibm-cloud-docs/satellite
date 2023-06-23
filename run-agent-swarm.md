---

copyright:
  years: 2023, 2023
lastupdated: "2023-06-23"

keywords: satellite, connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Running your Connector agent as a service in Docker Swarm Mode for high availability
{: #run-agent-swarm}

In addition to running the container standalone as shown in the previous example, you can use Docker Swarm mode to run a cluster of up to 3 containers on multiple machines to provide high availability of the {{site.data.keyword.satelliteshort}} Connector agent.
{: shortdesc}

To configure {{site.data.keyword.satelliteshort}} Connectors, you must have Administrator access to the **Satellite** service in IAM access policies.
{: note}

1. Create a {{site.data.keyword.satelliteshort}} Connector agent compose file called `satellite-connector-agent.yaml` on your Swarm manager system and copy the following content to it:
    ```yaml
    version: '3.9'
    services:
      agent:
        image: icr.io/ibm/satellite-connector/satellite-connector-agent:latest
        environment:
        - SATELLITE_CONNECTOR_ID=/satellite-connector-id
        - SATELLITE_CONNECTOR_REGION=/satellite-connector-region
        - SATELLITE_CONNECTOR_IAM_APIKEY=/run/secrets/satellite-connector-iam-apikey
        - SATELLITE_CONNECTOR_TAGS={{.Node.Hostname}}
        deploy:
          replicas: 3
          restart_policy:
            condition: any
          update_config:
            parallelism: 2
            delay: 20s
            failure_action: rollback
            order: start-first
          resources:
            limits:
              cpus: '0.40'
              memory: 500M
            reservations:
              cpus: '0.04'
              memory: 200M 
        configs: 
          - source: satellite-connector-id
            uid: '1000'
            gid: '1000'
            mode: 0400
          - source: satellite-connector-region
            uid: '1000'     
            gid: '1000'     
            mode: 0400      
        secrets:
          - source: satellite-connector-iam-apikey
            uid: '1000'
            gid: '1000'
            mode: 0400
        logging:
          driver: "json-file"
          options:
            max-size: ${JSON_FILE_MAX_SIZE:-1m}
            max-file: ${JSON_FILE_MAX_FILE:-10}

    configs:
      satellite-connector-id:
        external: true
      satellite-connector-region:
        external: true

    secrets:
      satellite-connector-iam-apikey:
        external: true

    networks:
      default:
        external: true
        name: bridge
    ```
    {: codeblock}
  
1. Create a Docker config with the {{site.data.keyword.satelliteshort}} Connector ID.
    ```sh
    printf <satellite connector id value> | docker config create satellite-connector-id -
    ```
    {: pre}
  
1. Create a Docker config with the {{site.data.keyword.satelliteshort}} Connector region.
    ```sh
    printf <satellite connector region value> | docker config create satellite-connector-region -
    ```
    {: pre}
  
1. Create a Docker secret with your IAM API key.
    ```sh
    printf <Your IAM API key> | docker secret create satellite-connector-iam-apikey -
    ```
    {: pre}

By default, the Swarm compose file sets the `SATELLITE_CONNECTOR_TAGS` environment variable to the Swarm node's hostname. You can adjust this value to suit your needs within the compose file. In addition, the number of replicas is set to 3 by default. You can adjust this number lower, but only a maximum of 3 replicas are supported.

1. Deploy the {{site.data.keyword.satelliteshort}} Connector agent service.
      1. Make sure you are logged in to the {{site.data.keyword.registrylong}} either using the `ibmcloud cr login` CLI or with your API key `docker login -u iamapikey -p <your apikey> icr.io`.
      1. Deploy the stack.  
          ```sh
          docker stack deploy --compose-file satellite-connector-agent.yaml --with-registry-auth satellite_connector
          ```
          {: pre}  

1. Verify the {{site.data.keyword.satelliteshort}} Connector agent service is running.
    ```sh
    docker service ps satellite_connector_agent
    ```
    {: pre} 
    
    
    

