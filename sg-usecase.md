---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Setting up {{site.data.keyword.satelliteshort}} as a Secure Gateway for on-prem solutions
{: #sg-usecase}

Deploy {{site.data.keyword.satellitelong_notm}} as a secure solution for connecting resources in a protected on-premises environment to cloud resources.
{: shortdesc}

## {{site.data.keyword.satelliteshort}} as a Layer 4 connection solution
{: #sg-alt}

While you can set up many possible solutions to enable secure connections between your on-premises network and {{site.data.keyword.cloud_notm}}, you can use {{site.data.keyword.satelliteshort}} to control client communications among your hybrid cloud deployments.
{: shortdesc}

For example, you might use a minimal {{site.data.keyword.satelliteshort}} location deployment as an alternative to the [{{site.data.keyword.SecureGateway}} solution](/docs/SecureGateway?topic=SecureGateway-getting-started-with-sg). {{site.data.keyword.satelliteshort}} provides the same application-level transport through common ports as {{site.data.keyword.SecureGateway}}, with greater client visibility and audit control. The {{site.data.keyword.satelliteshort}} Link functionality improves upon the {{site.data.keyword.SecureGateway}} client experience with a highly available and secure-by-default communication between the cloud and on-premises networks, third-party clouds, or network edge.

On-premises setup with a {{site.data.keyword.satelliteshort}} location
:   A minimum deployment of {{site.data.keyword.satelliteshort}} includes using three RHEL 7 hosts to set up a {{site.data.keyword.satelliteshort}} location control plane. These hosts might be in your on-premises network or in other clouds. Then, you can attach more hosts to your location and deploy {{site.data.keyword.cloud_notm}} managed services to run on these hosts. For example, you can deploy a {{site.data.keyword.redhat_openshift_notm}} cluster to your on-premises hosts that are attached to your {{site.data.keyword.satelliteshort}} location. Then, you can deploy any apps that need secure access to {{site.data.keyword.cloud_notm}} to your {{site.data.keyword.redhat_openshift_notm}} cluster.

Secure transport to {{site.data.keyword.cloud_notm}}
:   Next, your on-premises client that runs on the location hosts can use [{{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-link-cloud-create#link-location) as Layer 4 application transport between the location and other services that run in {{site.data.keyword.cloud_notm}} or your own applications that run within {{site.data.keyword.cloud_notm}}. You can use {{site.data.keyword.satelliteshort}} Link to create _location endpoints_, which allow resources in {{site.data.keyword.cloud_notm}} to securely access a resource in your on-premises {{site.data.keyword.satelliteshort}} location, and _cloud endpoints_, which allow resources in your on-premises {{site.data.keyword.satelliteshort}} location to access a resource that runs anywhere outside of the {{site.data.keyword.satelliteshort}} location. To allow access to a resource, authorization must granted in the Link endpoint's access control list.

When you evaluate whether the minimal {{site.data.keyword.satelliteshort}} location deployment is the best solution for your environment, keep the following considerations in mind.
- Location endpoints, or endpoints that expose resources that run in your {{site.data.keyword.satelliteshort}} location, are accessible only from within the {{site.data.keyword.cloud_notm}} private network or from resources that are connected to the {{site.data.keyword.cloud_notm}} private network.
- Although you can create endpoints for publicly accessible resources, the endpoints that are created are not publicly accessible, and can only be resolved by the {{site.data.keyword.satelliteshort}} Link components in {{site.data.keyword.cloud_notm}} (the Link tunnel server) or in your location (the Link connector).
- For more information about {{site.data.keyword.satelliteshort}} Link, including an architectural overview and FAQs, review [Understanding Link endpoints and {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-link-location-cloud).


## Setting up a secure connection to {{site.data.keyword.cloud_notm}} with {{site.data.keyword.satelliteshort}}
{: #sg-alt-setup}

The following example setup walks you through creating a minimal {{site.data.keyword.satelliteshort}} location setup with on-premises hosts, and securely connecting resources that you deploy in this {{site.data.keyword.satelliteshort}} location to {{site.data.keyword.cloud_notm}}.
{: shortdesc}

### Step 1: Deploy {{site.data.keyword.satelliteshort}} to your on-premises environment
{: #sg-example-loc}

As system administrator, you set up a {{site.data.keyword.satelliteshort}} location in your on-premises environment to run any applications that require access to {{site.data.keyword.cloud_notm}}.
{: shortdesc}

1. [Create a {{site.data.keyword.satelliteshort}} location](\/docs/satellite?topic=satellite-locations) in your on-premises infrastructure.
2. [Create a managed {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) in the {{site.data.keyword.satelliteshort}} location.
3. [Access the {{site.data.keyword.redhat_openshift_notm}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).
4. [Deploy the apps](/docs/openshift?topic=openshift-deploy_app) that require communication to apps, servers, or services that run outside of the {{site.data.keyword.satelliteshort}} location, such as in {{site.data.keyword.cloud_notm}}.

### Step 2: Set up secure communication channels by using {{site.data.keyword.satelliteshort}} Link
{: #sg-example-link}

Next, you create {{site.data.keyword.satelliteshort}} Link endpoints to allow apps that run in your {{site.data.keyword.satelliteshort}} location to access resources in {{site.data.keyword.cloud_notm}}, or vice versa.
{: shortdesc}

1. Create a [`cloud` endpoint](/docs/satellite?topic=satellite-link-cloud-create#link-cloud) to connect your {{site.data.keyword.satelliteshort}} location client app to a resource that runs in {{site.data.keyword.cloud_notm}}, or a [`location` endpoint](/docs/satellite?topic=satellite-link-cloud-create#link-location) to connect a resource that runs in {{site.data.keyword.cloud_notm}} to your {{site.data.keyword.satelliteshort}} location app.
2. For a `location` endpoint, [set up a source list](/docs/satellite?topic=satellite-link-cloud-create#link-sources) to limit and control access from {{site.data.keyword.cloud_notm}} to the app in your {{site.data.keyword.satelliteshort}} location.
3. [Audit events for endpoint actions](/docs/satellite?topic=satellite-link-cloud-monitor#link-audit).

Now, you have a managed {{site.data.keyword.satelliteshort}} location that runs in your on-premises environment and a secure communication channel to resources in {{site.data.keyword.cloud_notm}}.


