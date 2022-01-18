---

copyright:
  years: 2022
lastupdated: "2022-01-18"

keywords: satellite cli map, satellite commands, satellite cli, satellite reference

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.satellitelong_notm}} CLI Map
{: #icsat_map}

This page lists all `ibmcloud sat` commands as they are structured in the CLI. For more details on a specific command, click on the command or see the [{{site.data.keyword.satellitelong_notm}} CLI reference](/docs/containers?topic=containers-kubernetes-service-cli).

## ibmcloud sat cluster
{: #icsat_map_cluster}

[View and manage Satellite clusters](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-cluster-commands).
{: shortdesc}

* [`ibmcloud sat cluster get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get).
* [`ibmcloud sat cluster ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls).
* [`ibmcloud sat cluster register`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register).
* [`ibmcloud sat cluster unregister`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister).

## ibmcloud sat config
{: #icsat_map_config}

[Create, view, and manage Satellite configurations](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-config-configuration-commands).
{: shortdesc}

* [`ibmcloud sat config create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create).
* [`ibmcloud sat config get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-get).
* [`ibmcloud sat config ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-ls).
* [`ibmcloud sat config rename`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rename).
* [`ibmcloud sat config rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm).
* **`ibmcloud sat config version`**: View and manage Satellite configuration versions.
    * [`ibmcloud sat config version create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-create).
    * [`ibmcloud sat config version get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-get).
    * [`ibmcloud sat config version rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-rm).

## ibmcloud sat endpoint
{: #icsat_map_endpoint}

[Create, view, and manage Satellite endpoints](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-endpoint-commands).
{: shortdesc}

* [`ibmcloud sat endpoint create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-create).
* [`ibmcloud sat endpoint get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-get).
* [`ibmcloud sat endpoint ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-ls).
* [`ibmcloud sat endpoint rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-rm).
* [`ibmcloud sat endpoint update`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-update).

## ibmcloud sat group
{: #icsat_map_group}

[View and manage Satellite cluster groups](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-commands).
{: shortdesc}

* [`ibmcloud sat group attach`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach).
* [`ibmcloud sat group create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create).
* [`ibmcloud sat group detach`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach).
* [`ibmcloud sat group get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get).
* [`ibmcloud sat group ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls).
* [`ibmcloud sat group rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm).

## ibmcloud sat host
{: #icsat_map_host}

[View and modify Satellite host settings](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-host-commands).
{: shortdesc}

* [`ibmcloud sat host assign`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#host-assign).
* [`ibmcloud sat host attach`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#host-attach).
* [`ibmcloud sat host get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#host-get).
* [`ibmcloud sat host ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#host-ls).
* [`ibmcloud sat host rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#host-rm).
* [`ibmcloud sat host update`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#host-update).

## ibmcloud sat location
{: #icsat_map_location}

[Create, view, and modify Satellite locations](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-location-commands).
{: shortdesc}

* [`ibmcloud sat location create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#location-create).
* **`ibmcloud sat location dns`**: Create and manage subdomains for the hosts assigned to the control plane in a Satellite location.
    * [`ibmcloud sat location dns ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-ls).
    * [`ibmcloud sat location dns register`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-register).
* [`ibmcloud sat location get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#location-get).
* [`ibmcloud sat location ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#location-ls).
* [`ibmcloud sat location rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#location-rm).

## ibmcloud sat resource
{: #icsat_map_resource}

[Search and view Kubernetes resources that are managed by Satellite](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-resource-commands).
{: shortdesc}

* [`ibmcloud sat resource get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get).
* **`ibmcloud sat resource history`**: Get the history of a Kubernetes resource.
    * [`ibmcloud sat resource history get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-history-get).
* [`ibmcloud sat resource ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls).

## ibmcloud sat service
{: #icsat_map_service}

[View Satellite service clusters](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-service-commands).
{: shortdesc}

* [`ibmcloud sat service ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-service-ls).

## ibmcloud sat storage
{: #icsat_map_storage}

[View and manage Satellite storage resources](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-storage-commands).
{: shortdesc}

* **`ibmcloud sat storage assignment`**: View and manage Satellite storage assignments.
    * [`ibmcloud sat storage assignment create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).
    * [`ibmcloud sat storage assignment get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-get).
    * [`ibmcloud sat storage assignment ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-ls).
    * [`ibmcloud sat storage assignment rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-rm).
    * [`ibmcloud sat storage assignment update`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-update).
* **`ibmcloud sat storage config`**: View and manage Satellite storage configurations.
    * [`ibmcloud sat storage config create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    * [`ibmcloud sat storage config get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-get).
    * [`ibmcloud sat storage config ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-ls).
    * [`ibmcloud sat storage config rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-rm).
    * **`ibmcloud sat storage config sc`**: View and manage the storage classes in your Satellite storage configuration.
        * [`ibmcloud sat storage config sc add`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-add).
        * [`ibmcloud sat storage config sc get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-get).
        * [`ibmcloud sat storage config sc ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-ls).
* **`ibmcloud sat storage template`**: View Satellite storage templates.
    * [`ibmcloud sat storage template get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-template-get).
    * [`ibmcloud sat storage template ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-template-ls).

## ibmcloud sat subscription
{: #icsat_map_subscription}

[View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#sat-config-subscription-commands).
{: shortdesc}

* [`ibmcloud sat subscription create`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create).
* [`ibmcloud sat subscription get`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-get).
* [`ibmcloud sat subscription ls`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-ls).
* [`ibmcloud sat subscription rm`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-rm).
* [`ibmcloud sat subscription update`](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update).

## ibmcloud sat messages
{: #icsat_map_messages}

[View current messages from {{site.data.keyword.satellitelong_notm}}](https://cloud.ibm.com/docs/satellite?topic=satellite-satellite-cli-reference#cli-messages).



