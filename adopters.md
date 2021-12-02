<staging-internal>---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-02"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Adopter's guide for {{site.data.keyword.satelliteshort}}-enabled services
{: #adopters}

By onboarding as a {{site.data.keyword.satelliteshort}}-enabled service, you can bring your {{site.data.keyword.cloud}} service to a user's {{site.data.keyword.satelliteshort}} location anywhere, similar to how your service currently runs in {{site.data.keyword.cloud}} Public.
{: shortdesc}

This adopter's guide is internal-only, to help teams onboard their cloud service to {{site.data.keyword.satellitelong_notm}}. For more information, review the rest of the {{site.data.keyword.satelliteshort}} doc set. If you still have questions, try posting in the internal-only Slack channel, [#satellite-services](https://ibm-argonauts.slack.com/archives/C01FAN6A5RN){: external}.
{: note}

The staging environment (`test.cloud.ibm.com`) is subject to change, and is intended for development purposes. You may experience stability issues using the staging environment; please retry your tests periodically. Do not use staging for customer or production workloads.
{: important}


## Considerations before you begin
{: #adopters-prereqs}

Review the following considerations as you plan to onboard your offering as a {{site.data.keyword.satelliteshort}}-enabled service.
{: shortdesc}

Make sure that your apps run on OpenShift Container Platform
:    Prototype your service on a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-getting-started). {{site.data.keyword.satelliteshort}} manages only {{site.data.keyword.openshiftshort}} clusters, so using a {{site.data.keyword.openshiftlong_notm}} cluster for development helps to ease the transition to production environments on {{site.data.keyword.satelliteshort}}.

Make sure that you know how to run a {{site.data.keyword.satelliteshort}} location
:    Review the [{{site.data.keyword.satelliteshort}} location docs](/docs/satellite?topic=satellite-locations) for instructions and more information. Keep in mind that the hosts that you use must meet the minimum and any provider-specific requirements to use in {{site.data.keyword.satelliteshort}}. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs). 

     Want to use hosts from other cloud providers like AWS, Azure, or Google for your location? See [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
{: tip}

Ingestion tokens or other globally scoped API keys
:    In the customer's {{site.data.keyword.satelliteshort}} location, the API servers for each cluster, including your service's enablement cluster, run on the {{site.data.keyword.satelliteshort}} location control plane. The {{site.data.keyword.satelliteshort}} location control plane runs on customer-provided hosts in their backing infrastructure provider. Although these hosts are locked down as much as possible as part of the managed {{site.data.keyword.satelliteshort}} solution, such as disabling SSH, the customer can still access the hosts from their infrastructure provider. 

     Therefore, as you design how your service runs in {{site.data.keyword.satelliteshort}}, you must assume that the customer can access and use any API keys or other tokens that you configure in your service cluster. Make sure that any tokens, API keys, or other credentials are scoped specifically to that customer's account, including calls to {{site.data.keyword.cloud_notm}} IAM. You must not put your IAM operator or any other global API keys in the cluster.

Setting alerts for your service
:    Consider using a cloud-side proxy to set alerts for your service so that you can identify and address customer issues. For an example of how the {{site.data.keyword.satelliteshort}} team accomplishes alerts for locations, review the following documents about the autoresolver component.
     - [Autoresolver architectural diagram](https://github.ibm.com/alchemy-containers/armada-satellite-alert-autoresolver-server/blob/master/architecture/multishift%20alert%20autoresolver.png){: external}
     - [Autoresolver design doc](https://github.ibm.com/alchemy-containers/armada-satellite-alert-autoresolver-server/blob/master/architecture/AutoresolverDesign.md){: external}
     - The [autoresolver error messages](/docs/satellite?topic=satellite-ts-locations-debug) that are returned in the location status for users to troubleshoot.

Work in progress: {{site.data.keyword.cloud_notm}} IAM checks
:    A solution is currently under development to help services call IAM without storing an IAM operator token in the service cluster. The requirements under consideration include the following:
     - Allow services in {{site.data.keyword.cloud_notm}} to continue runtime operations for some to-be-determined, extended period of time when the {{site.data.keyword.satelliteshort}} location loses access to the public network.
     - Allow services to generate tokens that identify themselves without requiring the service to deploy their global identity API keys into the {{site.data.keyword.satelliteshort}} location.


## Onboarding your service to {{site.data.keyword.satelliteshort}}
{: #adopters-onboarding}

For users to consume your service anywhere, you must onboard your service to {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

1. Please provide a UX Design Hill describing your MVP for Satellite Onboarding for technical review by the Satellitee Architect and OM/PM. Follow the [EDT framework guidelines](https://www.ibm.com/design/thinking/page/framework/keys/hills){: external} to create your Hill. 
We are looking for an initial statement of intent. Your hill may evolve as you move forward with your UX Design.
Notify Sherri Midyette when your Hill is complete and ready for review.
2. Engage the Satellite UX Design team for guidance at the very beginning of your UX design process, or, if you have no UX designer, during your planning for UI before implementation. As a UX Designer / Design Researcher / UI Lead, study and follow the [Service Enablement Guide for Design](https://pages.github.ibm.com/argonauts-design/satellite-service-enablement){: external}.
   - Primary Satellite UX contact is Andras Kuti; backup contact is Gaby Moreno Cesar
3. Request that the {{site.data.keyword.satelliteshort}} team enables your service as a {{site.data.keyword.satelliteshort}}-enabled service in the Resource Management Console (RMC). To get started, fill out the [**Begin to Onboard your Service** GitHub Enterprise issue template](https://github.ibm.com/alchemy-containers/satellite-planning/issues/new/choose){: external}.
4. Wait for the {{site.data.keyword.satelliteshort}} team to complete the issue that you opened to enable your service in {{site.data.keyword.cloud_notm}} IAM.
5. Request that your service ID is added to the `roks.allow-byos` and `roks.allow-cloudlicense` allowlist feature flags. This enables your service ID to use the IBM licensing for OpenShift Container Platform. To get started, [submit a new Github Enterprise issue to the containers SRE team](https://github.ibm.com/alchemy-conductors/team/issues/new){: external}.  
6. For your **Production environment**, we require a review of your Service before you provide it to your users. Open an [**Approval to Offer Satellite-enabled Service in Production** GitHub Enterprise issue template](https://github.ibm.com/alchemy-containers/satellite-planning/issues/new/choose){: external} for a review with Satellite Leads. The Approval request includes further instructions.
7. Add a tag to your service entry in the {{site.data.keyword.cloud_notm}} catalog so that users know your service is enabled for {{site.data.keyword.satelliteshort}}. The steps vary depending on whether your offering is a **Service** or **Software** offering in the catalog.
    - For service offerings 
        The OM, Product Manager, or other Focal with catalog access must add the `satellite_enabled` tag to your offering.
        1. Log in to the global catalog for [staging](http://globalcatalog.test.cloud.ibm.com/){: external} or [prod](http://globalcatalog.cloud.ibm.com/){: external}, depending on the environment you want to enable.
        2. Search for and open your offering.
        3. Click the **Tags** tab.
        4. In the **Apply tags** field, enter the `satellite_enabled` tag.
        5. Click **Save**.

            Repeat these steps each time that you edit your offering in RMC, which overwrites your custom changes in the global catalog. The global catalog team is updating this limitation in a new onboarding process that is coming soon.
            {: important}

    - For software offerings
        1. Log in to the **IBM Cloud Partner Center** [staging](https://test.cloud.ibm.com/partner-center/sell){: external} or [prod](https://cloud.ibm.com/partner-center/sell){: external} website, depending on the environment that you want to enable.
        2. From the navigation menu, click **Sell**.
        3. Search for and open your offering.
        4. Click the **Catalog entry** tab.
        5. In the **Keywords** field, enter the `satellite_enabled` tag.

After you onboard your service, users can create a service instance in their {{site.data.keyword.satelliteshort}} location.


## Deploying your {{site.data.keyword.satelliteshort}}-enabled service cluster in the customer's location
{: #adopters-cluster-create}

After your service is enabled in {{site.data.keyword.satelliteshort}}, users can create an instance of your service in their {{site.data.keyword.satelliteshort}} location. To make your service available in the user's location, your service creates a cluster that you partially manage for the user.
{: shortdesc}

For API requests to {{site.data.keyword.satelliteshort}} location, you can use the following endpoints.
- Staging API endpoint: `https://containers.test.cloud.ibm.com`
- Production API endpoint: `https://containers.cloud.ibm.com`

To create a {{site.data.keyword.satelliteshort}}-enabled service cluster in a customer location:

1. The user must authorize your service to access their {{site.data.keyword.satelliteshort}} location with [service-to-service authorization in {{site.data.keyword.cloud_notm}} IAM](/iam/authorizations){: external}. For service authorization, the user must have the IAM **Administrator** platform role for the entire account where the {{site.data.keyword.satelliteshort}} location is.
    - **Source**: The source service is your service.
    - **Action**: The user grants access only to the actions that you requested in your [onboarding GitHub Enterprise issue](#adopters-onboarding), which might be one or more of the following actions.
        - `{{site.data.keyword.satelliteshort}} Cluster Creator`
        - `{{site.data.keyword.satelliteshort}} Link Source and Endpoint Controller`
        - `{{site.data.keyword.satelliteshort}} Link Source Access Controller`
    - **Target**: The target service is {{site.data.keyword.satellitelong_notm}}.
    - **Example CLI command**: The `<source>` is your service. The optional `--source-service-instance-name <source_service_instance>` flag can be used to scope the access policy to only a particular instance of your service to grant access to {{site.data.keyword.satelliteshort}}.
    
        ```sh
        ibmcloud iam authorization-policy-create <source> satellite "Satellite Cluster Creator","Satellite Link Administrator","Satellite Link Source Access Controller" [--source-service-instance-name <source_service_instance>]
        ```
        {: pre}

2. The user must set an API key in the region and resource group where you plan to create the service cluster.
    1. As the user's account owner, log in to {{site.data.keyword.cloud_notm}} and target the region and resource group where the {{site.data.keyword.satelliteshort}} location is managed from. The user can review which region and resource group the {{site.data.keyword.satelliteshort}} location is in from the [{{site.data.keyword.satelliteshort}} console](<staging>https://test.cloud.ibm.com/satellite/locations</staging><prod>https://cloud.ibm.com/satellite/locations</prod>){: external}.
    
        ```sh
        ibmcloud login -g <resource_group> -r <region>
        ```
        {: pre}

    2. As the user's account owner, set the API key.
    
        ```sh
        ibmcloud ks api-key reset
        ```
        {: pre}

3. Create a pair of tokens in {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) to access the user's account.
    1. Create a service ID.
    
        ```sh
        ibmcloud iam service-id-create <service-ID-name>
        ```
        {: pre}

    2. Create an access policy for the service ID to create clusters.
    
        ```sh
        ibmcloud iam service-policy-create <service-ID-name> --service-name containers-kubernetes --roles "Administrator,Manager"
        ```
        {: pre}

    3. Create and save an API key for the service ID, which you use to generate a service token for authentication with IAM later.
    
        ```sh
        ibmcloud iam service-api-key-create <API-key-name> <service-ID-name> | awk '/API Key/{ print $3 }' > ${HOME}/service-id-key
        ```
        {: pre}

    4. Find your service's Operator service ID. Each service has its own name, but you can search the output of the following command for a message such as `The Operator service id created by RMC for service <your-service-name>.`
    
        ```sh
        ibmcloud iam service-ids
        ```
        {: pre}

    5. Create and save an API key for your service's Operator service ID.
        ```sh
        ibmcloud iam service-api-key-create <API-key-name> <service-ID-name> | awk '/API Key/{ print $3 }' > ${HOME}/service-id-key
        ```
        {: pre}

    6. Generate an IAM token from your service's `<Operator-API-key>`. Call the `iam.test.cloud.ibm.com` endpoint URL for staging, or `iam.cloud.ibm.com` for prod.
        ```sh
        curl -k -X POST \
        --header "Content-Type: application/x-www-form-urlencoded" \
        --header "Accept: application/json" \
        --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
        --data-urlencode "apikey=$<Operator-API-key>" \
        "https://iam.test.cloud.ibm.com/identity/token" | jq -r '.access_token')
        ```
        {: codeblock}

    7. Use your service's `<Operator-token>` to generate an IAM token that is particular to your service `<CRN>`. Each service has its own way to return a CRN, such as running `ibmcloud resource service-instance <service-instance>`. Call the `iam.test.cloud.ibm.com` endpoint URL for staging, or `iam.cloud.ibm.com` for prod.
        ```sh
        curl -k -X POST \
        --header "Content-Type: application/x-www-form-urlencoded"\
        --header "Accept: application/json"\
        --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:iam-authz"\
        --data-urlencode "access_token=$<Operator-token>"\
        --data-urlencode "desired_iam_id=crn-<CRN>"\
        "https://iam.test.cloud.ibm.com/identity/token" | jq -r '.access_token'
        ```
        {: codeblock}

    8. Generate an IAM token for your `<service-ID-API-key>` that has permission to create a cluster. Call the `iam.test.cloud.ibm.com` endpoint URL for staging, or `iam.cloud.ibm.com` for prod.
        ```sh
        curl -k -X POST \
        --header "Content-Type: application/x-www-form-urlencoded" \
        --header "Accept: application/json" \
        --data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
        --data-urlencode "apikey=<service-ID-API-key>" \
        "https://iam.test.cloud.ibm.com/identity/token" | jq -r '.access_token'
        ```
        {: codeblock}

4. List the user's available {{site.data.keyword.satelliteshort}} locations. {{site.data.keyword.satelliteshort}} registers each location in Ghost and uses the IAM service-to-service authorization to let you see the user's {{site.data.keyword.satelliteshort}} location.
    * To list the user's {{site.data.keyword.satelliteshort}} locations, see the [`getControllers` API](https://containers.test.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-location/GetSatelliteLocations){: external}. Make sure to include the `X-Auth-Resource-Group` header with the ID of the resource group where you want to create the cluster in your target service account. To list available resource group IDs, run `ibmcloud resource groups` in your target service account.
    
5. Create a {{site.data.keyword.openshiftlong_notm}} cluster in the user's {{site.data.keyword.satelliteshort}} location, to manage your {{site.data.keyword.satelliteshort}}-enabled service. After you create the cluster, users can see the cluster in the {{site.data.keyword.satelliteshort}}-enabled service view of clusters in the UI, or by using the `ibmcloud sat services` CLI.
    - To create the cluster, see the [`createClusterRemoteLocation` API](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-service/CreateSatelliteClusterRemote){: external}.
    - In the `createClusterRemoteLocation` API, set the authorization headers. For the service CRN token to work for this API request, the user must previously authorize the service with at least the **{{site.data.keyword.satelliteshort}} Cluster Creator** action for {{site.data.keyword.satelliteshort}} in IAM for the {{site.data.keyword.cloud_notm}} account where the {{site.data.keyword.satelliteshort}} location exists.
        - **Authorization** header: The {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) service account token that was authorized by the user with service-to-service access to the user account. You previously made this token from your service ID API key.
        - **X-Auth-Resource-Group** header: The ID of the resource group where you want to create the cluster in your target service account. To list available resource group IDs, run `ibmcloud resource groups` in your target service account.
        - **X-Auth-Supplemental** header: The {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) [service CRN token](/docs/get-coding?topic=get-coding-grant-access) for the service that creates the cluster. You previously made this token from your Operator service ID API key for your service CRN. Curl parameter example:
            ```sh
            --header "X-Auth-Supplemental: Bearer $crn_token"
            ```
            {: codeblock}

    - In the `createClusterRemoteLocation` API, fill out the `clusterConfig` JSON fields. Review the following details about some of the fields.
        - **adminAgentOptIn**: Set this value to `true` to allow SatConfig to have the required access to manage {{site.data.keyword.openshiftshort}} resources for this cluster. If you do not set this value to `true`, then SatConfig cannot manage your cluster's resources. For more information and to set up this access after you create the service cluster, see [Running your service resources with {{site.data.keyword.satelliteshort}} Config](#adopters-config-resources).
        - **labels**: Provide host labels as key-value pairs for the default worker pool in the service cluster. These host labels are used for [host autoassignment](/docs/satellite?topic=satellite-hosts#host-autoassign-ov), so that if your service cluster requires more compute resources and the location has available (unassigned) hosts, the hosts are automatically assigned to the cluster with no action required from the user. Any host labels that you provide must be matched exactly by the labels on the hosts. Hosts are automatically labeled with `cpu` and `memory` (in bytes) labels, but because `memory` is in bytes, try to use only `cpu` host label. For example, to request hosts with 8 vCPUs and a custom label to identify your service:
            ```sh
            "labels":  {
                "cpu": "8",
                "service": "cp4d" 
                }
            ```
            {: codeblock}

            Host labels differ from Kubernetes labels that are set for the worker pool. To view host labels, run `ibmcloud oc worker-pool get -c <cluster> --worker-pool <worker_pool> --output json` in the CLI. 
            {: note}

        - **workerCount**: The number of worker nodes to create per zone. Alternatively, you can omit or set this value to `0`. Then, after the cluster is created, create a worker pool with the [`createSatelliteWorkerPool` API](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-cluster/CreateSatelliteWorkerPool){: external}.
	
6. The user must assign hosts to your service cluster to provide the worker nodes.
    - **Automatic assignment**: {{site.data.keyword.satelliteshort}} checks the host labels of the worker pools in your service cluster and compares the host labels with available (unassigned) hosts in the {{site.data.keyword.satelliteshort}} location. If the user has available hosts with matching labels, host assignment is [automatic](/docs/satellite?topic=satellite-hosts#host-autoassign-ov). Note that the matching is exact and based on the worker pool labels. The host can have more labels than the worker pool, as long as the host has all the requested labels and does not have conflicting zone requirements.
    - **Manual assignment**: The user can check if a service cluster needs more resources in the UI or by running `ibmcloud sat service ls` in the CLI. Then, the user can [manually assign the requested hosts](/docs/satellite?topic=satellite-hosts).
    
7. To use the internal cluster registry, you must [set up a backing storage device](/docs/openshift?topic=openshift-satellite-clusters#satcluster-internal-registry), such as Cloud Object Storage. If you do not set up the internal cluster registry, you cannot use the built-in {{site.data.keyword.openshiftshort}} build pipeline, but can use your own, such as storing an image in {{site.data.keyword.registrylong_notm}} and referring to this image in your {{site.data.keyword.openshiftshort}} deployments.

8. **Optional**: Verify that your cluster is created by listing the service clusters in the location. You can use the [`getServiceClusters` API method](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-service/GetSatelliteServiceClusters){: external} or the `ibmcloud sat sat service ls` CLI command.

Now your service cluster is created! The cluster master API server is ready to accept requests over the SatLink endpoint, even if the cluster does not have worker nodes yet. You can deploy your service's resources to the cluster, and Kubernetes keeps your resources in pending until the worker nodes become available to run workloads. The preferred way to deploy your resources is to use [{{site.data.keyword.satelliteshort}} Config](#adopters-config-resources).

         
## Running your service resources with {{site.data.keyword.satelliteshort}} Config
{: #adopters-config-resources}

Run your service as {{site.data.keyword.openshiftshort}} resources that are controlled through [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config). {{site.data.keyword.satelliteshort}} Config, based on the open source project [Razee](https://razee.io/){: external}, is a continuous delivery tool that you can use to manage your Kubernetes containerized application lifecycle, from deployment to updates and inventory.
{: shortdesc}

1. Make sure that {{site.data.keyword.satelliteshort}} Config has access to your service cluster.
    - If you [created your cluster](#adopters-cluster-create) with the **adminAgentOptIn** API field or `--enable-admin-agent` CLI option, you do not need to grant {{site.data.keyword.satelliteshort}} Config access.
    - If you did not grant access at cluster creation, [grant {{site.data.keyword.satelliteshort}} Config the appropriate access](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access) to view and manage the {{site.data.keyword.openshiftshort}} resources in your cluster.
2. Add your service cluster to a [cluster group](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-groups).
3. Containerize your service as a [Kubernetes application](/docs/openshift?topic=openshift-openshift_apps).
4. Assemble all of your Kubernetes resource configuration files into a single YAML file.
5. Create a {{site.data.keyword.satelliteshort}} configuration in the [UI](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui) or [CLI](/docs/satellite?topic=satellite-cluster-config#create-satconfig-cli) that uploads the YAML file of your service resources as a version.
6. Subscribe the cluster group to the version of your configuration so that your service resources are automatically deployed to the service cluster for you.


## Accessing endpoints with {{site.data.keyword.satelliteshort}} Link
{: #adopters-access-endpoints}

{{site.data.keyword.satelliteshort}} Link is a two-way secure tunnel that is created between the private network within {{site.data.keyword.cloud_notm}} and the customer's {{site.data.keyword.satelliteshort}} location. For more information, see the [{{site.data.keyword.satelliteshort}} Link docs](/docs/satellite?topic=satellite-link-location-cloud).
{: shortdesc}

If the user granted your service access to a {{site.data.keyword.satelliteshort}} Link IAM role through a service-to-service authorization, you can create endpoints in the {{site.data.keyword.satelliteshort}} location for your service cluster. You can create two types of endpoints, cloud or location endpoints.
- **Location**: Location endpoints allow you to access your service cluster resources from within the {{site.data.keyword.cloud_notm}} private network.
- **Cloud**: Cloud endpoints allow resources that run in your SatServ cluster to access resources and services that run on {{site.data.keyword.cloud_notm}} private endpoints.  

A {{site.data.keyword.satelliteshort}} Link tunnel is created automatically when the cluster is created, with 2 default endpoints:
- A location health check endpoint
- An endpoint that allows you to access your cluster's master API server

Accessing your {{site.data.keyword.satelliteshort}}-enabled service cluster with {{site.data.keyword.satelliteshort}} Link
:    To run `oc` commands to your cluster over the {{site.data.keyword.satelliteshort}} Link endpoints, set your cluster context with the {{site.data.keyword.satelliteshort}} Link endpoint. Note that even though you can log in to the cluster, the preferred way to deploy and edit the {{site.data.keyword.openshiftshort}} resources that run in the cluster is through [{{site.data.keyword.satelliteshort}} Config](#adopters-config-resources), not `oc` commands.
     ```sh
     ibmcloud oc cluster config --cluster <cluster_name_or-ID> --endpoint link
     ```
     {: pre}

Accessing an app that runs in your cluster with an {{site.data.keyword.openshiftshort}} service and a {{site.data.keyword.satelliteshort}} Link endpoint
:    You might have apps and other resources that run in your service cluster that you must talk to from outside the cluster, such as from the {{site.data.keyword.cloud_notm}} multizone region where your service master is. To access such apps, you can use {{site.data.keyword.satelliteshort}} Link in combination with an {{site.data.keyword.openshiftshort}} service.
     1. Create an {{site.data.keyword.openshiftshort}} service to make your app available outside the cluster, such as a route or node port.
     2. Create a `location` endpoint in {{site.data.keyword.satelliteshort}} Link with the service route, so that the route becomes available outside the {{site.data.keyword.satelliteshort}} location.
     3. Use the location endpoint to talk to your app in your service cluster from outside the cluster over the secured {{site.data.keyword.satelliteshort}} Link tunnel.
        In the UI, you might notice that this endpoint says `Public URL`. However, the URL is only "public" in the sense that the URL is accessible from outside the cluster. If the cluster is on only the private network with no public IP addresses, then the URL is not exposed on the public internet. Only users with access to the private network, such as via a VPN connection to the SatLoc network or SatLink, can use the location endpoint to access your app. 
        {: note}

## Setting up storage for your service clusters
{: #adopters-storage}

{{site.data.keyword.satelliteshort}} storage is available as a beta. With {{site.data.keyword.satelliteshort}} storage, you can choose a template to quickly create a storage configuration that you assign to each cluster that you want to install a storage driver in. For more information, see [Understanding {{site.data.keyword.satelliteshort}} storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
{: beta}

## Load Balancers to expose apps in your cluster publicly
{: #adopters-loadbalancer}

Currently, no load balancer is provided in {{site.data.keyword.satelliteshort}}. Your service can expose apps that run in a cluster by using routes, nodeports, and NLB-DNS. For more information, see [Exposing apps in {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-sat-expose-apps).
{: shortdesc}

June 2021 (date subject to change) is the target date to enable cloud provider load balancers.


## IAM Access
{: #adopters-iam}

As a {{site.data.keyword.satelliteshort}}-enabled service provider, you must be able to control access to the service clusters that you created in customer locations. For an overview of how {{site.data.keyword.satelliteshort}} implements {{site.data.keyword.cloud_notm}} IAM for access control, [see the docs](/docs/satellite?topic=satellite-iam#iam-resource-types).
{: shortdesc}



With {{site.data.keyword.satelliteshort}}, you can scope access to various [resources](/docs/satellite?topic=satellite-iam#iam-resource-types), such as locations, configurations, or links.

To permit certain sets of actions for those resources, IAM uses two types of roles: platform access and service access roles.
- [Platform access roles](/docs/satellite?topic=satellite-iam#understanding-satcon) enable users to perform tasks on service resources at the platform level, for example, assign user access for the service, create or delete instances, and bind instances to applications. 
- [Service access roles](/docs/satellite?topic=satellite-iam#iam-roles-service) enable users access to {{site.data.keyword.satelliteshort}} and the ability to call the {{site.data.keyword.satelliteshort}} APIs.


## Next steps after onboarding
{: #adopters-next-steps}

Review ways to maintain your service after onboarding to {{site.data.keyword.satelliteshort}}.
{: shortdesc}

Join the weekly {{site.data.keyword.satelliteshort}} providers call
:    Each week, the {{site.data.keyword.satelliteshort}} team hosts a meeting for service providers to discuss issues, pain points, feature requests, and other concerns that you have. To be invited to the call, contact the [{{site.data.keyword.satelliteshort}} integration focal](#feedback-support).
     This meeting is not a low-level technical discussion or a way to get debugging or other technical support. For more support, see [Feedback and issues](#feedback).
     {: note}

Use {{site.data.keyword.satelliteshort}} Config versions for app updates
:    When your service has a new version to roll out, use [{{site.data.keyword.satelliteshort}} Config](#adopters-config-resources) to upload a new version of your app.

Set up operational visibility into your service clusters
:    Integrate your service clusters with [logging](/docs/satellite?topic=satellite-health) and [monitoring](/docs/satellite?topic=satellite-monitor) tools to help you detect and troubleshoot issues.


## Deleting {{site.data.keyword.satelliteshort}} locations and clusters
{: #adopters-delete}

When you are done testing your service in your own {{site.data.keyword.satelliteshort}} location, you can clean up the {{site.data.keyword.satelliteshort}} location, clusters, and any infrastructure you used. 
{: shortdesc}

Before you begin, save any backup data that you want to keep about the clusters and location.

1. Remove the clusters from the {{site.data.keyword.satelliteshort}} location in the [API](https://containers.test.cloud.ibm.com/global/swagger-global-api/#/clusters/RemoveCluster){: external}, [CLI, or UI](/docs/satellite?topic=openshift-satellite-clusters#satcluster-rm).
2. Remove the hosts that were assigned to the location in the [API](https://containers.test.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-host/RemoveSatelliteHost){: external}, [CLI, or UI](/docs/satellite?topic=satellite-hosts#host-remove).
3. Remove the location in the [API](https://containers.test.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-location/RemoveSatelliteLocation){: external}, [CLI, or UI](/docs/satellite?topic=satellite-locations#location-remove).

To reuse the infrastructure hosts for other purposes such as a new {{site.data.keyword.satelliteshort}} location, you must reload the operating system on the hosts. If you no longer need the hosts, you can delete the hosts from the infrastructure provider. The host machines are not automatically reloaded or removed from the underlying infrastructure provider for you.
{: important}

## Setting up your staging {{site.data.keyword.cloud_notm}} account with access to production infrastructure
{: #adopters-setup-staging}

You can set up your staging account with access to production infrastructure if you want to test creating a {{site.data.keyword.satelliteshort}} cluster on classic infrastructure virtual server instances. If not, you can just use your staging account to access {{site.data.keyword.satelliteshort}}, but note that your machines must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs).
{: shortdesc}

Want to add hosts from other cloud providers like AWS, Azure, or Google? See [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
{: tip}

1. Set up access to a staging {{site.data.keyword.cloud_notm}} account.
    1. Go to [test.cloud.ibm.com](https://www.test.cloud.ibm.com/){: external}.
    2. Do **not** click **Log in**. Logging in tries to use you production {{site.data.keyword.cloud_notm}} credentials for the staging account, but you don't have a staging account yet.
        - If you do click **Log in**, you see an error similar to the following. Do not go to the #atlas-dev-environment Slack channel, as this Slach channel is not the right way to register for a staging account.
          ```sh
          Complete your account  This page can't be loaded because your infrastructure account is not fully configured as an IBM Cloud account in the test (integration) environment. To get help, mention this message in the #atlas-dev-environment Slack channel.
          ```
          {: screen}

        - You might need to remove your {{site.data.keyword.cloud_notm}} credentials and then reload the `test.cloud.ibm.com` staging site.
    3. Instead of logging in, sign up for a new account with your IBMid email, such as `first.last@ibm.com`. By signing up, you create a new account for staging.
    4. To log in to your staging account from the CLI, run the following command. If you have a federated account, include the `--sso` flag, or create an API key to log in.
        ```sh
        ibmcloud login -a test.cloud.ibm.com -r us-south -u <first.last@ibm.com> --sso
        ```
        {: pre}

        If you see an error similar to the following, try again in a different browser.
        ```sh
        Failure during registration, Too many registrations from this device."
        ```
        {: screen}

2. Set up access to a paid production {{site.data.keyword.cloud_notm}} account.
    1. If you don't already have a production account, [sign up for an {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-signup).
    2. Log in to [cloud.ibm.com](https://cloud.ibm.com/){: external}.
    3. From the menu bar, click **Manage > Access (IAM)**.
    4. Click **{{site.data.keyword.cloud_notm}} API keys**, then find your **Classic infrastructure API key**.
    5. Click the action menu **(...) > Details**, and copy your **API user name** and **API key**.
3. Link your staging account to your production infrastructure account.
    ```sh
    ibmcloud ks credential set classic --infrastructure-username <Classic infrastructure API user name>--infrastructure-api-key <Your Classic infrastructure API key> --region us-south
    ```
    {: pre}


## Creating the documentation for your {{site.data.keyword.satelliteshort}}-enabled service
{: #adopters-docs}

Because your service is in {{site.data.keyword.cloud_notm}}, you likely already have a [cloud docs collection](/docs/writing?topic=writing-get-started-onboarding). Review the following guidance to create {{site.data.keyword.satelliteshort}}-specific content in **your cloud docs collection**.
{: shortdesc}

### Required topics for {{site.data.keyword.satelliteshort}}-enabled service documentation
{: #docs-topics}

- [About, benefits, overview](#docs-about)
- [Charges](#docs-charges)
- [Creating a service instance doc](#docs-create)
- [How-to topics](#docs-how-to)
- [Identity and Access Management (IAM) docs](#docs-iam)
- [Known issues, limitations, responsibilities, service framework, and related topics](#docs-resp)
- [Logging, monitoring, and auditing docs](#docs-observability)
- [Troubleshooting doc](#docs-ts)
- [Updates to the {{site.data.keyword.satelliteshort}} documentation](#docs-sat)

### About, benefits, overview
{: #docs-about}

You might create {{site.data.keyword.satelliteshort}}-specific about topics to help educate users of the benefits of using your service in a {{site.data.keyword.satelliteshort}} location. The overview includes the differences between your "regular" offering and your offering on {{site.data.keyword.satelliteshort}}, if any. For example, see the [getting started](/docs/satellite?topic=satellite-getting-started) or the first couple [FAQs](/docs/satellite?topic=satellite-faqs) in the {{site.data.keyword.satelliteshort}} docs.
{: shortdesc}

### Charges
{: #docs-charges}

Create a topic that describes _how_ your service charges for usage in {{site.data.keyword.satelliteshort}}, so that your service can be included in the [What am I charged for?](/docs/satellite?topic=satellite-faqs#pricing) FAQ.

### Creating a service instance doc
{: #docs-create}

Explain how to create an instance of your service in a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

- Minimum service instance requirements: Include the minimum number of CPU and memory that a service instance requires.
- Review the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) and determine whether your service requires anything else.
- [Host autoassignment](/docs/satellite?topic=satellite-hosts#host-autoassign-ov): Identify any custom labels that your service requires to use host autoassignment.
- Document any unique steps to provisioning an instance of your service. For example, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-satellite-clusters).
- Any special configuration or links to information on how to use your service instance after creating the instance.


### How-to topics
{: #docs-how-to}

You might group all of the how-to topics that are specific to using your service in {{site.data.keyword.satelliteshort}} into a single page, or a twistie or pages.
{: shortdesc}

- Assess where your existing content might differ for service instances that run on {{site.data.keyword.satelliteshort}}.
- Be sure to include any special steps to remove your service instance on {{site.data.keyword.satelliteshort}}.

For example, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-satellite-clusters).

### Identity and Access Management (IAM) docs
{: #docs-iam}

Access is a key component of maintaining a secure environment for users of {{site.data.keyword.satelliteshort}}. Gather information on the types of access that is required to use your service with {{site.data.keyword.satelliteshort}}.
{: shortdesc}

- Assess the impact if your service cannot contact IAM, such as if the user disables the {{site.data.keyword.satelliteshort}} Link connection back to {{site.data.keyword.cloud_notm}}. For example, this might affect updating your service version, or users or processes invoking your service.
- Identify the name of your product in IAM for the API, CLI, and UI. You can find this value in the RMC catalog entry for your service (for the API/CLI), and in the IAM UI (for the UI).
- Identify the platform, service, or custom IAM roles that users need to create and manage service instances.
- Update your [IAM](/docs/get-coding?topic=get-coding-iam-docs) docs with any {{site.data.keyword.satelliteshort}}-specific permissions and concerns.

### Known issues, limitations, responsibilities, service framework, and related topics
{: #docs-resp}

Be sure to update the following pages in your doc set for any {{site.data.keyword.satelliteshort}}-specific guidance, and include the links to these pages when you [contact the {{site.data.keyword.satelliteshort}} docs team](#docs-contact).
{: shortdesc}

Known issues and limitations
:    In your [known issues and limitations](/docs/writing?topic=writing-known-issues-limitations-) page, add a heading for {{site.data.keyword.satelliteshort}}-specific limitations, especially if these limitations differ from your service on other providers like IBM public cloud.

Service framework requirements
:    As part of the service framework requirements for {{site.data.keyword.cloud_notm}}, you have several required doc pages that outline how your service complies with the framework. Make sure to update these topics for any {{site.data.keyword.satelliteshort}} impact, especially the following pages.
     - [High availability and disaster recovery](/docs/writing?topic=writing-ha-dr-content-guidance): In particular, outline differences in {{site.data.keyword.satelliteshort}} due to the customers bringing their own infrastructure.
     - [Responsibilities](/docs/writing?topic=writing-responsibilities-content-guidance): Note where users have different responsibilities when they use your service in {{site.data.keyword.satelliteshort}} instead of the regular public cloud.
     - [Security topics](/docs/writing?topic=writing-security-content-guidance): Consider whether users must follow different security procedures for your service  in {{site.data.keyword.satelliteshort}}.
     - [Architecture and workload isolation](/docs/writing?topic=writing-architecture-compute-isolation): For example, your service might have a different default setup in {{site.data.keyword.satelliteshort}} because the user might have to create a cluster with a minimum number of worker nodes across zones.
     - [Securing connection to your service](/docs/writing?topic=writing-service-endpoint-guidance): Consider the {{site.data.keyword.satelliteshort}} Link impacts to service connection. 
     - [Securing your service data](/docs/writing?topic=writing-data-doc-guidance): For example, you might store the data locally to the cluster in the {{site.data.keyword.satelliteshort}} location, which is on infrastructure that the customer owns. Describe how this might impact data retention and cleanup policies. In particular, customers want to know what data leaves their {{site.data.keyword.satelliteshort}} location and where the data goes.

### Logging, monitoring, and auditing docs
{: #docs-observability}

Consider any {{site.data.keyword.satelliteshort}} impact to your observability services.
{: shortdesc}

- [Logging](/docs/writing?topic=writing-logging-messages-sf-req)
- [Monitoring](/docs/writing?topic=writing-sysdig-sf-req)
- [Auditing](/docs/writing?topic=writing-atref)

Focus on the following areas.
- A list of {{site.data.keyword.satelliteshort}}-specific events for your service
- What identifiers can users see in the logs to know that an event is related to {{site.data.keyword.satelliteshort}}? 
- Include sample snippets with an explanation of the {{site.data.keyword.satelliteshort}} identifiers.
- Do you provide any suggested dashboards or views?

### Troubleshooting doc
{: #docs-ts}

Your service likely has error messages, debugging, or troubleshooting content that is specific to the service's usage in {{site.data.keyword.satelliteshort}}. 
{: shortdesc}

#### Error messages
{: #docs-ts-error}

Make sure that error messages are marked as specific to {{site.data.keyword.satelliteshort}}. In the help text, include the word `{{site.data.keyword.satelliteshort}}`. In your product's code, you might have a special type to programmatically identify the error as specific to {{site.data.keyword.satelliteshort}}, such as in the following example.

```sh
	"E0872": {
		"rc": 404,
		"code": "E0872",
		"description": "The specified Satellite location could not be found.",
		"type": "Satellite",
		"recoveryCLI": "To list the locations that you have access to, run 'ibmcloud sat location ls'."
	},
```
{: screen}

#### Debugging guide
{: #docs-ts-debugging}

Now that your service has error messages that are specific to {{site.data.keyword.satelliteshort}}, create a debugging guide to help users understand potential recovery steps for each error message. The debugging guide contains the following information.
1. Steps on how to check the status of your service.
2. If applicable, steps to check the health of various components in your service.
3. An explanation of each error message, including resolution steps or links to more troubleshooting topics.

For an example of a debugging guide, see [Location error messages](/docs/satellite?topic=satellite-ts-locations-debug).

#### Troubleshooting topics for particular use cases
{: #docs-ts-use-case}

See [Troubleshooting topics](/docs/writing?topic=writing-troubleshooting-topics).

#### Placement in the table of contents for your docs collection
{: #docs-ts-toc}

The debugging guide and each troubleshooting topic is a separate page in your doc collection. 

In the **Help** section of your `toc` file, create a twistie for {{site.data.keyword.satelliteshort}}, such as in the following example.

```yaml
 - topicgroup:
     label: Troubleshooting errors
     topics:
       - getting-help.md
       - debug-location.md
       - topicgroup:
           label: Locations
           topics:
             - debug-control-plane.md
             - ts-location-subdomain.md
             - ts-location-healthcheck.md
             - ts-location-listing-iam.md
       - topicgroup:
           label: Hosts
           topics:
             - debug-hosts.md
             - debug-login-host.md
	     - ts-host-ssh-login.md
             - ts-host-registration-fail.md
             - ts-host-assign-fail.md
             - ts-host-console-update-fail.md
...
```
{: codeblock}

#### Getting the troubleshooting doc added to the {{site.data.keyword.satelliteshort}} docs
{: #docs-ts-doc-added}

After you publish your doc, [contact the {{site.data.keyword.satelliteshort}} docs team](#docs-contact) to reuse your debug guide. By default, only your debug guide is reused, and the rest of the troubleshooting topics remain in your collection (but can be linked to from the debugging guide).

### Product interfaces (API, CLI, UI)
{: #docs-api-cli-ui}

As applicable, develop content specific to {{site.data.keyword.satelliteshort}} in your product's interfaces.
{: shortdesc}

- In your [API docs](/docs/api-docs?topic=api-docs-overview), you might have a subset of methods specific to {{site.data.keyword.satelliteshort}}.
- In your [CLI docs](/docs/writing?topic=writing-cli-cmd-ref), you might have a subset of commands specific to {{site.data.keyword.satelliteshort}}.
- In your product's UI, you might have a subset of pages specific to {{site.data.keyword.satelliteshort}}.

### Updates to the {{site.data.keyword.satelliteshort}} documentation
{: #docs-sat}

When you open the documentation issue, the {{site.data.keyword.satelliteshort}} documentation team is notified and reaches out to begin coordinating updates to the {{site.data.keyword.satelliteshort}} documentation. These updates include the following.
{: shortdesc}

- Listing your service as a {{site.data.keyword.satelliteshort}}-enabled service in the docs.
- Linking to or reusing any of your collection's pages.

To get started, open a **SatServ Doc Request** [GitHub Enterprise issue](https://github.ibm.com/alchemy-containers/satellite-documentation/issues/new/choose){: external}.

## More resources
{: #adopters-resources}

Still have questions? Try some of the following resources to learn more about managing your service on {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

### API docs
{: #adopters-api-docs}

- [SatConfig API](https://config.satellite.test.cloud.ibm.com/graphql){: external}
- [SatLoc API](https://containers.test.cloud.ibm.com/global/swagger-global-api/#/satellite-beta){: external}
    - Staging API endpoint: `https://containers.test.cloud.ibm.com`
    - Production API endpoint: `https://containers.cloud.ibm.com`
- [SatLink API](https://pages.github.ibm.com/IBM-Cloud-Platform-Networking/satellite-link-api/#/){: external}

### Community examples, scripts, and other resources
{: #adopters-community-resources}

The following resources are created by {{site.data.keyword.satelliteshort}} adopters, and might be helpful as you continue in your adopter journey. 
{: shortdesc}

These resources are not officially supported, but are provided as examples of what other teams do with {{site.data.keyword.satelliteshort}}.
{: important}

| Resource description | Team that provided the resource | Link to resource | Date added |
| --- | --- | --- | --- |
| Scripts to set up a {{site.data.keyword.satelliteshort}} location on {{site.data.keyword.cloud_notm}} VPC Gen 2 VSIs. | Garage | [GitHub Enterprise repo](https://github.ibm.com/garage-satellite-guild/try-sat){: external} | 18 Feb 2021 |
| Doc on setting up integration testing | Code Engine | [GHE markdown file](https://github.ibm.com/coligo/satellite-compute-provider/blob/main/TESTING.md){: external} | 18 Feb 2021 |
| Satellite Service Enablement Guide | UX / Design | [Satellite - Service Enablement Guide](https://pages.github.ibm.com/argonauts-design/satellite-service-enablement/) | 27 Aug 2021 |
{: caption="Satellite adopter community-provided resources" caption-side="top"}
{: summary="The rows are read from left to right. The first column is a description of the resource. The second column is the team that created the resource. The third column is a link to the resource."}

### Design docs
{: #adopters-design-docs}

You might find the following design helpful to review.
{: shortdesc}

- Product documentation (this documentation set). For a one-page reference of what the product docs include, see the [Sitemap](/docs/satellite?topic=satellite-sitemap).
- Seismic for product, sales, and marketing information. See [https://ibm.biz/sellSatellite](https://ibm.biz/sellSatellite){: external}.
- Internal [Argonauts tribe concept docs in GitHub](https://github.ibm.com/alchemy-containers/armada/search?q=satellite&unscoped_q=satellite){: external}.

### Training Videos
{: #adopters-training-videos}

The following videos are recordings of playbacks and training sessions that are related to {{site.data.keyword.satelliteshort}} functionality.
{: shortdesc}

#### Playbacks on {{site.data.keyword.satelliteshort}} components
{: #adopters-training-videos-playbacks}

- [{{site.data.keyword.satelliteshort}} location](https://ibm.ent.box.com/file/688862437327){: external}
- [{{site.data.keyword.satelliteshort}} Config](https://ibm.ent.box.com/file/691513027355){: external}
- [{{site.data.keyword.satelliteshort}} Link](https://ibm.box.com/s/71ma3mf38z48bxzac6boj6iq2925nhvn){: external}

#### Training sessions for SRE, ACS, and support teams
{: #adopters-training-videos-sre}

- [{{site.data.keyword.satelliteshort}} location for ACS](https://ibm.box.com/s/d4m844h8or2659q9dno9puej0uy2yda2){: external}
- [{{site.data.keyword.satelliteshort}} Config for ACS](https://ibm.box.com/s/2xybgbk871ew3zy9feorwp3liej7ft6v){: external}
- [{{site.data.keyword.satelliteshort}} Link for ACS](https://ibm.box.com/s/l3ijja6yu0v3u4oflpdnn1olraijcfwf){: external}
- [{{site.data.keyword.satelliteshort}} location and core concepts for SREs](https://ibm.box.com/s/emnpbye9633krxs7ovs6y7xo6o80ddf4){: external}

## Feedback and issues
{: #feedback}

For questions, post in Slack in the following Argonauts workspace channels.
{: shortdesc}

- [#satellite-services](https://ibm-argonauts.slack.com/archives/C01FAN6A5RN){: external} for help that is specific to {{site.data.keyword.satelliteshort}}-enabled services.
- [#satellite-users](https://ibm-argonauts.slack.com/archives/C01149RMSCU){: external} for general information.

### Troubleshooting
{: #feedback-ts}

Start by debugging your issue with the troubleshooting documentation.
{: shortdesc}

- [Location](/docs/satellite?topic=satellite-ts-locations-debug)
- [Hosts](/docs/satellite?topic=satellite-ts-hosts-debug)
- [Clusters](/docs/satellite?topic=satellite-ts-clusters-debug)

### Issue tracking
{: #feedback-issues}

You can review existing issues or open new issues.
{: shortdesc}

#### Review open issues
{: #feedback-issues-review-open}

- Business and feature commitments in [AHA](https://bigblue.aha.io/products/SATELLITE/feature_cards){: external}.
- {{site.data.keyword.satelliteshort}} planning repo: See the general [{{site.data.keyword.satelliteshort}} GitHub issues](https://github.ibm.com/alchemy-containers/satellite-planning/issues){: external} that are in progress.


#### Open an issue
{: #feedback-issues-open}

- For feature requests, open an [internal AHA item](https://internal-ibmcloud.ideas.aha.io/ideas/new){: external}. Include `{{site.data.keyword.satelliteshort}}` in the idea name.
- For AHA reference, see [Submitting Requirements to Cloud](https://github.ibm.com/ibmcloud/Portfolio-Offering/wiki/Submitting-Requirements-to-Cloud).
- For the status of your ideas, see [{{site.data.keyword.satelliteshort}} Structured Ideas Report](https://bigblue.aha.io/bookmarks/idea_grids/6838962877233537194/6838963497311680292).

#### Contact docs team
{: #docs-contact}

- For documentation, contact the doc team in the [#satellite-docs](https://ibm-argonauts.slack.com/archives/C0142NX6BGA){: external} channel in the Argonauts Workspace on Slack, or [open a GitHub issue](https://github.ibm.com/alchemy-containers/satellite-documentation/issues/new/choose){: external}.
- To onboarding your service's Satellite-specific docs to the Satellite doc collection, choose the **SatServ Doc Request** from the GitHub new issues.

### Additional support
{: #feedback-support}

Contact the {{site.data.keyword.satelliteshort}} integration focal, [Nathan LeViere](https://w3.ibm.com/bluepages/profile.html?uid=5D4351897){: external}.
{: shortdesc}

</staging-internal>
