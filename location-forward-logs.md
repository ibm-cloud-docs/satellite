---

copyright:
  years: 2022, 2022
lastupdated: "2022-11-09"

keywords: satellite, hybrid, multicloud, forwarding audit logs, location audit logs, kubernetes API audit logs

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Setting up log forwarding
{: #location-forward-logs}

You can set up log forwarding for Kubernetes audit logs or for worker node `syslogs` for an {{site.data.keyword.satellitelong_notm}} location.
{: shortdesc}

## Forwarding Kubernetes audit logs
{: #forwarding-audit-logs}

You can forward Kubernetes API audit logs for clusters in your {{site.data.keyword.satellitelong_notm}} location to either {{site.data.keyword.loganalysisshort}} or to a resource in the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

You must have a {{site.data.keyword.openshiftlong_notm}} cluster within the location to forward audit logs.

1. Assign additional hosts to your {{site.data.keyword.openshiftlong_notm}} cluster.
    
    ```sh
    {[icsat]} host assign --location LOCATIONID --cluster CLUSTERID
    ```
    {: pre}

2. [Follow the steps](/docs/openshift?topic=openshift-health-audit#audit-api-server-la) in {{site.data.keyword.openshiftlong_notm}} documentation to set up forwarding. To set up forwarding to a resource in the {{site.data.keyword.cloud_notm}} private network, complete steps 1-4. To set up forwarding to {{site.data.keyword.loganalysisshort}}, complete steps 1-7.
3. Note the `clusterID` that the service is deployed in.
4. Create a support ticket to enable forwarding.
    
    ```txt
    Title: Request: Enable Kube Audit Log Forwarding for Location: <LOCATIONID>
    =========
    Please enable Kube Audit Log Forwarding for Location: <LOCATIONID>

    Please forward the logs to cluster service <kube-audit-remote-private-ip|ibmcloud-kube-audit-service> in cluster: CLUSTERID
    ```
    {: screen}

    Example log forwarding request.
    ```txt
    Title: Request: Enable Kube Audit Log Forwarding for Location: 

    Please enable Kube Audit Log Forwarding for Location: abc1dc2w01edqjl9o42g

    Please forward the logs to cluster service ibmcloud-kube-audit-service in cluster: abcd1h3w0scd7l9mc6gg
    ```
    {: screen}

5. When your request is approved, audit logs are forwarded to the endpoint that you configured, whether a resource in the {{site.data.keyword.cloud_notm}} private network or {{site.data.keyword.loganalysisshort}}.


## Forwarding worker node `syslogs` to a remote endpoint
{: #location-forward-syslogs}

You can configure your worker node to forward `syslogs` to a remote server by adding the `rsyslog` configuration information to the node before you attach the host.

1. Log in to your worker node.
2. Open the `/etc/rsyslog.conf` file.
3. Add a line to forward the `syslog` files. 

    ```txt
    LOG_TYPE action(type="omfwd" target="REMOTE_SYSLOG_IP" port="REMOTE_SYSLOG_PORT" protocol="PROTOCOL")
    ```
    {: codeblock}

    For example, see the following sample line.

    ```txt
    authpriv.* action(type="omfwd" target="172.21.46.170" port="3000" protocol="tcp")
    ```
    {: codeblock}

4. Save the file.
5. Attach and assign the host to the location.

For more information about `rsyslog` configuration, see the [`rsyslog` man page](https://man7.org/linux/man-pages/man5/rsyslog.conf.5.html){: external}.



