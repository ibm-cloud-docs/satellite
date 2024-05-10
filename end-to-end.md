---


copyright:
  years: 2023, 2024
lastupdated: "2024-05-10"

keywords: satellite, connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.satelliteshort}} Connector end-to-end example
{: #end-to-end}

After the tunnel has been established, you can run an application container on your machine and access its endpoints from {{site.data.keyword.cloud_notm}}.
{: shortdesc}
  
To configure {{site.data.keyword.satelliteshort}} Connectors, you must have Administrator access to the **Satellite** service in IAM access policies.
{: note}

In this example, use a simple Nginx container.

## Creating a Docker container
{: #create-container}
{: step}
  
1. Create the following directories.  
    ```txt  
    ~/agent/nginx/etc/nginx
    ~/agent/nginx/www/data
    ```
    {: codeblock} 
  
1. Create a file called `index.html` in `~/agent/nginx/www/data` with the following value.
    ```txt
    Hello from ngnix running at my location.
    ```
    {: codeblock}  
  
1. Create a file called `nginx.conf` in `~/agent/nginx/etc/nginx` with the following value.  
    ```txt
    events {
    worker_connections  1024;
    }

    http {
    server {
        listen 80;
        root /www/data;

        location / {
        }
      }
    }
    ```
    {: codeblock}
  
1. Run the Nginx container.
    ```sh  
    docker run -d -p 80:80 -v ~/agent/nginx/etc/nginx:/etc/nginx:ro -v ~/agent/nginx/www/data:/www/data:ro nginx
    ```
    {: pre} 

    You now have a running Nginx container.
  
## Creating your Link endpoint
{: #create-link-endpoint}
{: step}

1. Create a Location link endpoint in your {{site.data.keyword.satelliteshort}} Connector on {{site.data.keyword.cloud_notm}}.
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/connectors){: external}, select your connector.
    1. From the User Endpoints tab, click **Create endpoint**.
    1. In the **Endpoint name** field, enter `MyNginx`.
    1. In the **Destination FQDN or IP** field, enter the IP address of the Nginx container, for example, `172.17.0.3`. To find the Nginx IP address, run `docker inspect <nginx container id> | grep IPAddress`. 
    1. In the **Destination port** field, enter `80`.
    1. Click **Next**.
    1. In the **Source protocol** field, select `TCP`.
    1. Leave the rest of fields blank.
    1. Click **Next**.
    1. Optionally, select an ACL rule or create a new ACL rule to control which clients can access location endpoint resources. If no ACL rule is selected, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your location.
    1. Click **Next**.
    1. Leave the connection settings at their default values.
    1. Click **Create endpoint**.

1. In the Endpoint details, you can see an **Endpoint Address** that refers to a CSE endpoint that is accessible from within the {{site.data.keyword.cloud_notm}} network. If you run a VSI instance or use the VPC VPN, you can curl your Nginx endpoint. For example:
    ```sh
    curl http://c-02.private.us-east.link.satellite.cloud.ibm.com:<port>
    Hello from ngnix running at my location.
    ``` 
    {: screen}

## Adding TLS support
{: #add-tls}
{: step}

This section modifies the previous example to add support for TLS to Nginx.

1. Create the following directories.  
    ```txt  
    ~/agent/nginx/etc/nginx/ssl/certs
    ~/agent/nginx/etc/nginx/ssl/private
    ```
    {: codeblock}

1. Create a self-signed certificate. When you are prompted for your DN, enter a value for the first field and leave the rest at the default values.
    ```sh
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ~/agent/nginx/etc/nginx/ssl/private/nginx-selfsigned.key -out ~/agent/nginx/etc/nginx/ssl/certs/nginx-selfsigned.crt
    ```
    {: pre}

1. Edit the `nginx.conf` file at `~/agent/nginx/etc/nginx` to add the SSL settings. The file looks similar to the following example.
    ```txt
    events {
    worker_connections  1024;
    }

    http {
    server {
        listen 80;
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/certs/nginx-selfsigned.crt;
        ssl_certificate_key /etc/nginx/ssl/private/nginx-selfsigned.key;
        root /www/data;

        location / {
        }
      }
    }
    ```
    {: codeblock}  

1. Restart the Nginx container and include the `expose` option.  If there is an instance currently running, stop it first and then restart.
    ```sh
    docker stop <nginx container id> 
    docker run -d --expose=443 -v ~/agent/nginx/etc/nginx:/etc/nginx:ro -v ~/agent/nginx/www/data:/www/data:ro nginx
    ```
    {: pre}

1. Create another Location type link endpoint as you did in the previous section that uses the following settings. 
    - Use a different name such as `MyNginx-ssl`.
    - For destination port specify `443`.
    - Keep the **Source protocol** as `TCP` as SSL termination is done at the nginx server.

1. Now if you select this endpoint, you see an Endpoint Address that refers to a CSE endpoint that is accessible from within the {{site.data.keyword.cloud_notm}} network. So if you run a VSI instance or use the VPC VPN, you can curl your Nginx endpoint. As  the target endpoint is using SSL, make sure to specify `https` in the curl command. Also, because a self-signed certificate is used, specify the `-k` option. For example:
    ```sh
    curl -k https://c-02.private.us-east.link.satellite.cloud.ibm.com:<port>
    Hello from ngnix running at my location.
    ```
    {: pre}

The Nginx container IP address might change when the Nginx container is restarted. If that happens, you must update the Link endpoint destination address. 

  

