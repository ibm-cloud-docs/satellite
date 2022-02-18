---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Debugging the health of the location control plane
{: #ts-locations-control-plane}

When you create a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations), {{site.data.keyword.IBM_notm}} automatically sets up a master for the location control plane in {{site.data.keyword.cloud_notm}}. Additionally, you must assign at least three hosts to the {{site.data.keyword.satelliteshort}} location control plane as worker nodes to run location components that {{site.data.keyword.IBM_notm}} configures. If the location control plane that runs on your hosts has issues, you can debug the location control plane.

1. Get your {{site.data.keyword.satelliteshort}} location ID.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

2. List the **Hostnames** of the subdomains for your location control plane hosts.
    ```sh
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    Retrieving location subdomains...
    OK
    Hostname                                                                                                 Records                                                                                                Health Monitor   SSL Cert Status   SSL Cert Secret Name                                          Secret  Namespace   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud   169.62.  196.20,169.62.196.23,169.62.196.30                                                                None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001.us-east.satellite.appdomain.cloud   169.62.  196.30                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002.us-east.satellite.appdomain.cloud   169.62.  196.20                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003.us-east.satellite.appdomain.cloud   169.62.  196.23                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00.us-east.satellite.appdomain.cloud    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00      default  
    ```
    {: screen}

3. Check the health of the control plane location subdomains by curling each hostname endpoint. If the endpoint returns a `200` response for each host, the control plane worker node is healthy and serving Kubernetes traffic. If not, continue to the next step.
    ```sh
    curl -v http://<hostname>:30000
    ```
    {: pre}

    Example output of a failed response
    ```sh
    * Rebuilt URL to: http://169.xx.xxx.xxx:30000/
    *   Trying 169.xx.xxx.xxx...
    * TCP_NODELAY set
    * Connection failed
    * connect to 169.xx.xxx.xxx port 30000 failed: Operation timed out
    * Failed to connect to 169.xx.xxx.xxx port 30000: Operation timed out
    * Closing connection 0
    curl: (7) Failed to connect to 169.xx.xxx.xxx port 30000: Operation timed out
    ```
    {: screen}

    Example output of a `200` response
    ```sh
    * Rebuilt URL to: http://169.xx.xxx.xxx:30000/
    *   Trying 169.xx.xxx.xxx...
    * TCP_NODELAY set
    * Connected to 169.xx.xxx.xxx (169.xx.xxx.xxx) port 30000 (#0)
    > GET / HTTP/1.1
    > Host: 169.xx.xxx.xxx:30000
    > User-Agent: curl/7.54.0
    > Accept: */*
    >
    < HTTP/1.1 200 OK
    < content-length: 58
    < cache-control: no-cache
    < content-type: text/html
    < connection: close
    <
    <html><body><h1>200 OK</h1>
    Service ready.
    </body></html>
    * Closing connection 0
    ```
    {: screen}

4. Find the **ID** of the host that did not return a `200` response. You can compare the `Host: 169.xx.xxx.xxx` from the previous step with the **Worker IP** in the output of the following command.
    ```sh
    ibmcloud sat host ls --location <location_ID> | grep infrastructure
    ```
    {: pre}

    Example output
    ```sh
    Name     ID                     State        Status   Cluster          Worker ID                Worker IP   
    host1    aaaaa1a11aaaaaa111aa   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx   
    host2    bbbbbbb22bb2bbb222b2   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx  
    host3    ccccc3c33ccccc3333cc   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx  
    ```
    {: screen}

5. [Add a host to the control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) in the same zone so that the location control plane has enough compute resources to continue running when you remove the unhealthy host.
6. [Remove the unhealthy host from the location control plane](/docs/satellite?topic=satellite-host-remove).
7. Optional: You can reload the operating system on the unhealthy host and try to attach and assign the host to {{site.data.keyword.satellitelong_notm}} again.


