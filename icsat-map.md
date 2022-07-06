---

copyright:
  years: 2022
lastupdated: "2022-07-06"

keywords: satellite cli map, satellite commands, satellite cli, satellite reference

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.satellitelong_notm}} CLI map
{: #icsat_map}

This page lists all `ibmcloud sat` commands as they are structured in the CLI. For more information about a specific command, click on the command or see the [{{site.data.keyword.satellitelong_notm}} CLI reference](/docs/containers?topic=containers-kubernetes-service-cli).

## `ibmcloud sat cluster`
{: #icsat_map_cluster}

[View and manage Satellite clusters](/docs/satellite?topic=satellite-satellite-cli-reference#sat-cluster-commands).
{: shortdesc}

* [`ibmcloud sat cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get).
* [`ibmcloud sat cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls).
* [`ibmcloud sat cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register).
* [`ibmcloud sat cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister).

## `ibmcloud sat config`
{: #icsat_map_config}

[Create, view, and manage Satellite configurations](/docs/satellite?topic=satellite-satellite-cli-reference#sat-config-configuration-commands).
{: shortdesc}

* [`ibmcloud sat config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create).
* [`ibmcloud sat config get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-get).
* [`ibmcloud sat config ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-ls).
* [`ibmcloud sat config rename`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rename).
* [`ibmcloud sat config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm).
* **`ibmcloud sat config version`**: View and manage Satellite configuration versions.
    * [`ibmcloud sat config version create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-create).
    * [`ibmcloud sat config version get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-get).
    * [`ibmcloud sat config version rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-rm).

## `ibmcloud sat endpoint`
{: #icsat_map_endpoint}

[Create, view, and manage Satellite endpoints](/docs/satellite?topic=satellite-satellite-cli-reference#sat-endpoint-commands).
{: shortdesc}

* [`ibmcloud sat endpoint create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-create).
* [`ibmcloud sat endpoint get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-get).
* [`ibmcloud sat endpoint ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-ls).
* [`ibmcloud sat endpoint rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-rm).
* [`ibmcloud sat endpoint update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-update).

## `ibmcloud sat group`
{: #icsat_map_group}

[View and manage Satellite cluster groups](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-commands).
{: shortdesc}

* [`ibmcloud sat group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach).
* [`ibmcloud sat group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create).
* [`ibmcloud sat group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach).
* [`ibmcloud sat group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get).
* [`ibmcloud sat group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls).
* [`ibmcloud sat group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm).

## `ibmcloud sat host`
{: #icsat_map_host}

[View and modify Satellite host settings](/docs/satellite?topic=satellite-satellite-cli-reference#sat-host-commands).
{: shortdesc}

* [`ibmcloud sat host assign`](/docs/satellite?topic=satellite-satellite-cli-reference#host-assign).
* [`ibmcloud sat host attach`](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach).
* [`ibmcloud sat host get`](/docs/satellite?topic=satellite-satellite-cli-reference#host-get).
* [`ibmcloud sat host ls`](/docs/satellite?topic=satellite-satellite-cli-reference#host-ls).
* [`ibmcloud sat host rm`](/docs/satellite?topic=satellite-satellite-cli-reference#host-rm).
* [`ibmcloud sat host update`](/docs/satellite?topic=satellite-satellite-cli-reference#host-update).

## `ibmcloud sat key`
{: #icsat_map_key}

View and manage {{site.data.keyword.satelliteshort}} Config keys.
{: shortdesc}

* [`ibmcloud sat key ls`](/docs/satellite?topic=satellite-satellite-cli-reference#key-ls)
* [`ibmcloud sat key rm`](/docs/satellite?topic=satellite-satellite-cli-reference#key-rm)
* [`ibmcloud sat key rotate`](/docs/satellite?topic=satellite-satellite-cli-reference#key-rotate)


## `ibmcloud sat location`
{: #icsat_map_location}

[Create, view, and modify Satellite locations](/docs/satellite?topic=satellite-satellite-cli-reference#sat-location-commands).
{: shortdesc}

* [`ibmcloud sat location create`](/docs/satellite?topic=satellite-satellite-cli-reference#location-create).
* **`ibmcloud sat location dns`**: Create and manage subdomains for the hosts assigned to the control plane in a Satellite location.
    * [`ibmcloud sat location dns get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-get).
    * [`ibmcloud sat location dns ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-ls).
    * [`ibmcloud sat location dns register`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-register).
* [`ibmcloud sat location get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-get).
* [`ibmcloud sat location ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-ls).
* [`ibmcloud sat location rm`](/docs/satellite?topic=satellite-satellite-cli-reference#location-rm).

## `ibmcloud sat resource`
{: #icsat_map_resource}

[Search and view Kubernetes resources that are managed by Satellite](/docs/satellite?topic=satellite-satellite-cli-reference#sat-resource-commands).
{: shortdesc}

* [`ibmcloud sat resource get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get).
* **`ibmcloud sat resource history`**: Get the history of a Kubernetes resource.
    * [`ibmcloud sat resource history get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-history-get).
* [`ibmcloud sat resource ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls).

## `ibmcloud sat service`
{: #icsat_map_service}

[View Satellite service clusters](/docs/satellite?topic=satellite-satellite-cli-reference#sat-service-commands).
{: shortdesc}

* [`ibmcloud sat service ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-service-ls).

## `ibmcloud sat storage`
{: #icsat_map_storage}

[View and manage Satellite storage resources](/docs/satellite?topic=satellite-satellite-cli-reference#sat-storage-commands).
{: shortdesc}

* **`ibmcloud sat storage assignment`**: View and manage Satellite storage assignments.
    * [`ibmcloud sat storage assignment create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).
    * [`ibmcloud sat storage assignment get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-get).
    * [`ibmcloud sat storage assignment ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-ls).
    * [`ibmcloud sat storage assignment rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-rm).
    * [`ibmcloud sat storage assignment update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-update).
    * [`ibmcloud sat storage assignment upgrade`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-upgrade).
* **`ibmcloud sat storage config`**: View and manage Satellite storage configurations.
    * [`ibmcloud sat storage config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    * [`ibmcloud sat storage config get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-get).
    * [`ibmcloud sat storage config ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-ls).
    
    * [`ibmcloud sat storage config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-rm).
    * [`ibmcloud sat storage config upgrade`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-upgrade).
    * **`ibmcloud sat storage config sc`**: View and manage the storage classes in your Satellite storage configuration.
        * [`ibmcloud sat storage config sc add`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-add).
        * [`ibmcloud sat storage config sc get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-get).
        * [`ibmcloud sat storage config sc ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-ls).
* **`ibmcloud sat storage template`**: View Satellite storage templates.
    * [`ibmcloud sat storage template get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-template-get).
    * [`ibmcloud sat storage template ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-template-ls).

## `ibmcloud sat subscription`
{: #icsat_map_subscription}

[View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters](/docs/satellite?topic=satellite-satellite-cli-reference#sat-config-subscription-commands).
{: shortdesc}

* [`ibmcloud sat subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create).
* [`ibmcloud sat subscription get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-get).
* [`ibmcloud sat subscription identity set`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-identity-set)
* [`ibmcloud sat subscription ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-ls).
* [`ibmcloud sat subscription rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-rm).
* [`ibmcloud sat subscription update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update).

## `ibmcloud sat messages`
{: #icsat_map_messages}

[View current messages from {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-satellite-cli-reference#cli-messages).



