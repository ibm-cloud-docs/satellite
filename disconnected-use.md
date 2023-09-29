---

copyright:
  years: 2022, 2023
lastupdated: "2023-09-29"

keywords: satellite, hybrid, multicloud, disconnected use, disconnected usage, disconnect

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Disconnected use for Satellite components 
{: #disconnected-use}

Certain features of {{site.data.keyword.satellitelong}} can be used while disconnected temporarily. See the following FAWs to understand the disconnected use for these components.
{: shortdesc}

## Understanding disconnected usage
{: #understand-disconnected}

What does disconnected usage mean?
:   When {{site.data.keyword.satelliteshort}} Locations and {{site.data.keyword.openshiftlong_notm}} are disconnected from the parent `managed-from` region in {{site.data.keyword.cloud_notm}}, they can still run with some limitations for a certain amount of time. The limitations of disconnected usage include no security fixes and no alerts. For more information, see [Disconnected usage by component](#disconnect-usage-component).

How do I set how long my access token is valid for?
:   Edit the value of the `accessTokenMaxAgeSeconds` parameter to set the token validity in seconds. For more information, see [Setting the disconnected usage time](#disconnect-time). 

What happens when my token expires?
:   After your token expires, you lose the ability to work with the Location. When you run a command, you get error messages such as `error: You must be logged in to the server (Unauthorized)`. To recover, you must reconnect to the Location and log in again to retrieve a new token. Do not reload nodes before the Location is reconnected. If you reload nodes while the Location is disconnected, the location won't recover. 

Do I need to re-create the Location?
:   No, you don't need to re-create the Location. You can reconnect the existing Location and reauthenticate to continue working with your Location.

How do I reauthenticate? 
:   Reconnect your Location first and then log in again with your credentials. 

Do I have to recover etcd backup?
:   No, you don't need to recover etcd backup. The Location recovers automatically after you reconnect it and reauthenticate.

What happens if a location is disconnected for more than 7 days?
:   After you restore your connection, you might need to replace all hosts across the location with new infrastructure.

How do I know when to reconnect a location?
:   You can make a note or set a reminder to reconnect the location before the 7-day window expires. The timer starts when you request the token. 

## Setting the disconnected usage time
{: #disconnect-time}

{{site.data.keyword.satelliteshort}} Locations and {{site.data.keyword.openshiftlong_notm}} can run disconnected from the parent `managed-from` region in {{site.data.keyword.cloud_notm}} for up to 168 hours (7 days).
{: shortdesc}

You can modify this setting by changing the `accessTokenMaxAgeSeconds` value for all your OAuth clients. The default value for `accessTokenMaxAgeSeconds` parameter is 86400 seconds.

The `accessTokenMaxAgeSeconds` value starts counting when the user was last authenticated, not when the Location is disconnected. Note that a user must have access to IAM to authenticate.
{: important}

1. Get your OAuth clients by running `oc get oauthclients`.
    
    Example output
    ```sh
    NAME                           SECRET                                        WWW-CHALLENGE   TOKEN-MAX-AGE   REDIRECT URIS
    console                        OBXIvSxQx2t5ANYe5-xAEylpsbdytupjyjJicScdFsE   false           default         https://console-openshift-console.sl-disc2b-be7d0adc45c89a3b4c1f8e7bc127f800-0000.eu-de.containers.appdomain.cloud/auth/callback
    openshift-browser-client       Mohc6XH48KzDywCWf-oP2SaKbI70KL1O              false           default         https://sb5041d7cf0af9630b92f-6b64a6ccc9c596bf59a86625d8fa2202-ce00.eu-de.satellite.appdomain.cloud:30048/oauth/token/display
    ```
    {: screen}


2. Get your OAuth access tokens by running `oc get oauthaccesstokens`.

3. For each OAuth client that you retrieved in step 1, set the `accessTokenMaxAgeSeconds` value by running `oc  edit oauthclients console` and adding the `accessTokenMaxAgeSeconds: 604800` as the first line. Note that you must repeat this step for all your OAuth clients.

4. Optionally, run `oc get oauthclients console -o yaml` to check whether the first line of the yaml file was updated to `accessTokenMaxAgeSeconds: 604800`.

5. Refresh your cluster by running `ibmcloud ks cluster master refresh --cluster CLUSTERID` for the changes to take effect.

6. Run the following command to verify that the `accessTokenMaxAgeSeconds` value was updated for your OAuth clients.
    ```sh
    oc get oauthclients -o json | jq -j '.items[]|"Name:\t", .metadata.name, "\t", "value:\t", .accessTokenMaxAgeSeconds, "\n"'
    ```
    {: pre}
    
## Disconnected usage by component
{: #disconnect-usage-component}

The following tables explain the behavior and limitations of different components when your location is running disconnected.

### Cluster management
{: #cluster}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Create clusters | You can create clusters from the CLI and console. | You cannot create clusters. | N/A |
| Update clusters | You can update clusters from the CLI and console. | You cannot update clusters. | N/A |
| Remove clusters | You can remove clusters from the CLI and console. | You cannot remove clusters. | N/A |
| View status | You can view cluster status from the CLI and console. | You cannot view cluster status. | N/A |
| Add or remove nodes | You can remove or add nodes from the CLI and console. | You cannot remove or add nodes. | N/A |
| Autoscaling clusters | Autoscaling is managed by the Cluster Autoscaler Add-on. | Clusters cannot autoscale. | N/A |
{: caption="Disconnected cluster management" caption-side="bottom"}

### Apps
{: #apps}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Deployment | You can deploy apps by using your preferred methods, such as the `kubectl` command line, the OpenShift Console, CI/CD pipelines, and Sat Config. | You can deploy apps while disconnected by using the Kubernetes API. | Equal to the `accessTokenMaxAgeSeconds` value from last authentication |
| Removal | You can remove apps by using your preferred methods, such as the `kubectl` command line, the OpenShift Console, CI/CD pipelines, and Sat Config. | You can remove apps while disconnected by using the Kubernetes API. | Equal to the `accessTokenMaxAgeSeconds` value from last authentication |
| Scaling | You can scale apps by using your preferred methods, such as the `kubectl` command line, the OpenShift Console, CI/CD pipelines, and Sat Config. | You can scale apps while disconnected by using the Kubernetes API. | Equal to the `accessTokenMaxAgeSeconds` value from last authentication |
{: caption="Disconnected app management" caption-side="bottom"}

### IBM Cloud Catalo
{: #ibm-cloud-catalog}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Managing resources from the IBM Cloud Catalog | Search for and deploy resources from the catalog. | You cannot search for and deploy resources from the catalog. | Zero |
{: caption="Disconnected Cloud Catalog management" caption-side="bottom"}

### Identity and access management
{: #identity-access}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Authorization | Define user access roles by using IAM | You can't manage user access | N/A |
| Authentication | Authenticate to IBM Cloud by using IAM | You can't reauthenticate | N/A |
{: caption="Disconnected identity and access management" caption-side="bottom"}

### Secret management
{: #secret-management}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Secret and key management using Secret Manager  | You can use Secret Manager for your secrets. | Secrets Manager is not available | N/A |
{: caption="Disconnected secret management" caption-side="bottom"}

### Logging and monitoring
{: #log-monitor}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Application audit and access logging with {{site.data.keyword.cloudaccesstraillong_notm}} | N/A | N/A | N/A |
| Application monitoring with {{site.data.keyword.monitoringlong_notm}} | N/A | N/A | N/A |
| Application logging that uses other provider | You can use third-party logging tools. | You can use third-party logging tools. | Equal to the `accessTokenMaxAgeSeconds` from last authentication |
| System or Kubernetes logging that uses other provider | You can use third-party logging tools. | You can use third-party logging tools. | Equal to the `accessTokenMaxAgeSeconds` from last authentication |
| Alerting rules and paging for metrics | You can set up alerts in {{site.data.keyword.monitoringlong_notm}}. | Alerts are not triggered. | N/A |
{: caption="Disconnected logging and monitoring" caption-side="bottom"}

### Config management
{: #config-management}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| {{site.data.keyword.satelliteshort}} Config | You can create and manage configurations by using {{site.data.keyword.satelliteshort}} Config. | You cannot create and manage configurations by using {{site.data.keyword.satelliteshort}} Config. | N/A |
| {{site.data.keyword.satelliteshort}} Storage Config | You can create and manage configurations by using {{site.data.keyword.satelliteshort}} Storage Config.  | You cannot create and manage configurations by using {{site.data.keyword.satelliteshort}} Storage Config. | N/A |
{: caption="Disconnected config management" caption-side="bottom"}

### Networking
{: #networking}

| Feature | Connected behavior | Disconnected behavior | Maximum disconnection tolerance |
| -- | -- | -- | -- |
| Deploying or updating policies {{site.data.keyword.satelliteshort}} Service Mesh | You can deploy or update policies by using the CLI and console. | You can use the `kubectl` command to deploy and update policies. | Equal to the `accessTokenMaxAgeSeconds` value from last authentication |
{: caption="Disconnected network and service mesh management" caption-side="bottom"}

