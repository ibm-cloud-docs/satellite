---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-20"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# Clusters
{: #ts-clusters}

## Debugging clusters
{: #ts-clusters-debug}

See the [{{site.data.keyword.openshiftlong_notm}} troubleshooting documentation](/docs/openshift?topic=openshift-cs_troubleshoot).

## Cluster does not get Ingress subdomain
{: #cluster-subdomain-providers}

{: tsSymptoms}
After you create a cluster in your {{site.data.keyword.satelliteshort}} location, you assign hosts from your AWS or GCP cloud providers to the cluster. However, the cluster does not get an Ingress subdomain.

{: tsCauses}
The worker nodes for your cluster set the private IP of the AWS and GCP cloud provider hosts as the public IP for the Ingress subdomain DNS registration, which causes the registration to fail.

{: tsResolve}
Manually register the DNS subdomain for your cloud provider hosts. For more information, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-cluster-nlb) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-cluster-nlb) topics.

## Clusters cannot view or get updates to Kubernetes resources that are managed by {{site.data.keyword.satelliteshort}} Config
{: #satconfig-cluster-access-error}

{: tsSymptoms}
When you register a cluster to use with {{site.data.keyword.satelliteshort}} Config, you do not see the cluster resources show up in the resources list. Even though you subscribe the cluster to a configuration, no Kubernetes resources are created or updated in the cluster. 

{: tsCauses}
To use a cluster to use with {{site.data.keyword.satelliteshort}} Config, the proper components must be installed, and you must grant {{site.data.keyword.satelliteshort}} Config permissions in your cluster to manage Kubernetes resources.

{: tsResolve}
1.  Re-attach the cluster to {{site.data.keyword.satelliteshort}} Config. For more information, see [Registering existing {{site.data.keyword.openshiftlong_notm}} clusters with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters).
    1.  Get a `kubectl` command to register your cluster with {{site.data.keyword.satelliteshort}} Config.
        ```
        ibmcloud sat cluster register
        ```
        {: pre}

        Example output:
        ```
        kubectl apply -f "https://config.satellite.cloud.ibm.com/api/install/razeedeploy-job?orgKey=<orgApiKey>&args=--clustersubscription=<number>&args=--featureflagsetld=<number>&args=--mustachetemplate=<number>&args=--managedset=<number>&args=--remoteresources<number>&args=--remoteresource=<number>&args=--watch-keeper=<number>"
        ```
        {: screen}
    2.  Log in to your cluster. For more login options, see [Accessing {{site.data.keyword.openshiftshort}} clusters](/docs/openshift?topic=openshift-access_cluster).
        ```
        ibmcloud oc cluster config -c <cluster_name_or_ID> --admin
        ```
        {: pre}
    3.  Run the `kubectl` command that you previously retrieved.
2.  Grant {{site.data.keyword.satelliteshort}} Config permissions in your cluster to manage Kubernetes resources. The following command grants `cluster-admin` permissions for the entire cluster. For more options, see [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access).
    ```
    kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
    ```
    {: pre}


