---


copyright:
  years: 2025
lastupdated: "2025-04-17"

keywords: satellite cli map, satellite commands, satellite cli, satellite reference

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.satellitelong_notm}} CLI map
{: #icsat_map}

This page lists all `ibmcloud sat` commands as they are structured in the CLI. For more information about a specific command, click the command or see the [{{site.data.keyword.satellitelong_notm}} CLI reference](/docs/satellite?topic=satellite-satellite-cli-reference).



## `acl` commands
{: #icks_map_acl}

View and manage Satellite access control lists (ACLs).
{: shortdesc}

* [`ibmcloud sat acl create`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-create-cli)
* **`acl endpoint`**: Configure endpoints for an ACL.
    * [`ibmcloud sat acl endpoint add`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-endpoint-add-cli)
    * [`ibmcloud sat acl endpoint ls`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-endpoint-ls-cli)
    * [`ibmcloud sat acl endpoint rm`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-endpoint-rm-cli)
* [`ibmcloud sat acl get`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-get-cli)
* [`ibmcloud sat acl ls`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-ls-cli)
* [`ibmcloud sat acl rm`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-rm-cli)
* **`acl subnet`**: Configure subnets for an ACL.
    * [`ibmcloud sat acl subnet add`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-subnet-add-cli)
    * [`ibmcloud sat acl subnet rm`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-subnet-rm-cli)
* [`ibmcloud sat acl update`](/docs/satellite?topic=satellite-satellite-cli-reference#acl-update-cli)


## `agent` commands
{: #icks_map_agent}

Attach or view Satellite Connector Agents.
{: shortdesc}

* [`ibmcloud sat agent attach`](/docs/satellite?topic=satellite-satellite-cli-reference#agent-attach-cli)
* [`ibmcloud sat agent ls`](/docs/satellite?topic=satellite-satellite-cli-reference#agent-ls-cli)


## `cluster` commands
{: #icks_map_cluster}

Register and manage clusters for use with Satellite configurations.
{: shortdesc}

* [`ibmcloud sat cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-get-cli)
* [`ibmcloud sat cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-ls-cli)
* [`ibmcloud sat cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-register-cli)
* [`ibmcloud sat cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-unregister-cli)


## `config` commands
{: #icks_map_config}

View and manage Satellite Configuration.
{: shortdesc}

* [`ibmcloud sat config create`](/docs/satellite?topic=satellite-satellite-cli-reference#config-create-cli)
* [`ibmcloud sat config get`](/docs/satellite?topic=satellite-satellite-cli-reference#config-get-cli)
* [`ibmcloud sat config ls`](/docs/satellite?topic=satellite-satellite-cli-reference#config-ls-cli)
* [`ibmcloud sat config rename`](/docs/satellite?topic=satellite-satellite-cli-reference#config-rename-cli)
* [`ibmcloud sat config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#config-rm-cli)
* **`config version`**: View and manage Satellite configuration versions.
    * [`ibmcloud sat config version create`](/docs/satellite?topic=satellite-satellite-cli-reference#config-version-create-cli)
    * [`ibmcloud sat config version get`](/docs/satellite?topic=satellite-satellite-cli-reference#config-version-get-cli)
    * [`ibmcloud sat config version rm`](/docs/satellite?topic=satellite-satellite-cli-reference#config-version-rm-cli)


## `connector` commands
{: #icks_map_connector}

Create, view, and modify Satellite connectors.
{: shortdesc}

* [`ibmcloud sat connector create`](/docs/satellite?topic=satellite-satellite-cli-reference#connector-create-cli)
* [`ibmcloud sat connector get`](/docs/satellite?topic=satellite-satellite-cli-reference#connector-get-cli)
* [`ibmcloud sat connector ls`](/docs/satellite?topic=satellite-satellite-cli-reference#connector-ls-cli)
* [`ibmcloud sat connector rm`](/docs/satellite?topic=satellite-satellite-cli-reference#connector-rm-cli)


## `endpoint` commands
{: #icks_map_endpoint}

View and manage Satellite endpoints.
{: shortdesc}

* **`endpoint authn`**: Configure authentication settings for an endpoint.
    * [`ibmcloud sat endpoint authn get`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-authn-get-cli)
    * [`ibmcloud sat endpoint authn rotate`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-authn-rotate-cli)
    * [`ibmcloud sat endpoint authn set`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-authn-set-cli)
* [`ibmcloud sat endpoint create`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-create-cli)
* [`ibmcloud sat endpoint disable`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-disable-cli)
* [`ibmcloud sat endpoint enable`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-enable-cli)
* [`ibmcloud sat endpoint get`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-get-cli)
* [`ibmcloud sat endpoint ls`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-ls-cli)
* [`ibmcloud sat endpoint rm`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-rm-cli)
* [`ibmcloud sat endpoint update`](/docs/satellite?topic=satellite-satellite-cli-reference#endpoint-update-cli)


## `experimental` commands
{: #icks_map_experimental}

[Expires on 2024-11-25] Experiment with new commands. IMPORTANT: Commands here will retire after the [date] in their description.
{: shortdesc}

* **`experimental acl`**: [Deactivated on 2024-10-01! Use `ibmcloud sat acl` instead] View and manage Satellite access control lists (ACLs).
    * [`ibmcloud sat experimental acl create`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-create-cli)
    * [`ibmcloud sat experimental acl endpoint add`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-endpoint-add-cli)
    * [`ibmcloud sat experimental acl endpoint ls`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-endpoint-ls-cli)
    * [`ibmcloud sat experimental acl endpoint rm`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-endpoint-rm-cli)
    * [`ibmcloud sat experimental acl get`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-get-cli)
    * [`ibmcloud sat experimental acl ls`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-ls-cli)
    * [`ibmcloud sat experimental acl rm`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-rm-cli)
    * [`ibmcloud sat experimental acl subnet add`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-subnet-add-cli)
    * [`ibmcloud sat experimental acl subnet rm`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-subnet-rm-cli)
    * [`ibmcloud sat experimental acl update`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-acl-update-cli)
* **`experimental agent`**: [Deactivated on 2024-09-01! Use `ibmcloud sat agent` instead] Attach or view Satellite Connector Agents.
    * [`ibmcloud sat experimental agent attach`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-agent-attach-cli)
    * [`ibmcloud sat experimental agent ls`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-agent-ls-cli)
* **`experimental connector`**: [Deactivated on 2024-11-18! Use `ibmcloud sat connector` instead] Create, view, and modify Satellite connectors.
    * [`ibmcloud sat experimental connector create`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-connector-create-cli)
    * [`ibmcloud sat experimental connector get`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-connector-get-cli)
    * [`ibmcloud sat experimental connector ls`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-connector-ls-cli)
    * [`ibmcloud sat experimental connector rm`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-connector-rm-cli)
* **`experimental endpoint`**: [Expires on 2024-10-01] View and manage Satellite endpoints.
    * [`ibmcloud sat experimental endpoint authn get`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-endpoint-authn-get-cli)
    * [`ibmcloud sat experimental endpoint authn rotate`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-endpoint-authn-rotate-cli)
    * [`ibmcloud sat experimental endpoint authn set`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-endpoint-authn-set-cli)
* **`experimental location`**: [Deactivated on 2024-11-25! Use `ibmcloud sat location` instead] Create, view, and modify Satellite locations.
    * [`ibmcloud sat experimental location update`](/docs/satellite?topic=satellite-satellite-cli-reference#experimental-location-update-cli)


## `group` commands
{: #icks_map_group}

View and manage Satellite cluster groups. Cluster groups are used to subscribe clusters to Satellite configurations of Kubernetes resources.
{: shortdesc}

* [`ibmcloud sat group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#group-attach-cli)
* [`ibmcloud sat group create`](/docs/satellite?topic=satellite-satellite-cli-reference#group-create-cli)
* [`ibmcloud sat group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#group-detach-cli)
* [`ibmcloud sat group get`](/docs/satellite?topic=satellite-satellite-cli-reference#group-get-cli)
* [`ibmcloud sat group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#group-ls-cli)
* [`ibmcloud sat group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#group-rm-cli)


## `host` commands
{: #icks_map_host}

View and modify Satellite hosts.
{: shortdesc}

* [`ibmcloud sat host assign`](/docs/satellite?topic=satellite-satellite-cli-reference#host-assign-cli)
* [`ibmcloud sat host attach`](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach-cli)
* [`ibmcloud sat host get`](/docs/satellite?topic=satellite-satellite-cli-reference#host-get-cli)
* [`ibmcloud sat host ls`](/docs/satellite?topic=satellite-satellite-cli-reference#host-ls-cli)
* [`ibmcloud sat host rm`](/docs/satellite?topic=satellite-satellite-cli-reference#host-rm-cli)
* [`ibmcloud sat host update`](/docs/satellite?topic=satellite-satellite-cli-reference#host-update-cli)


## `key` commands
{: #icks_map_key}

View and manage Satellite Config keys.
{: shortdesc}

* [`ibmcloud sat key ls`](/docs/satellite?topic=satellite-satellite-cli-reference#key-ls-cli)
* [`ibmcloud sat key rm`](/docs/satellite?topic=satellite-satellite-cli-reference#key-rm-cli)
* [`ibmcloud sat key rotate`](/docs/satellite?topic=satellite-satellite-cli-reference#key-rotate-cli)


## `location` commands
{: #icks_map_location}

Create, view, and modify Satellite locations.
{: shortdesc}

* [`ibmcloud sat location create`](/docs/satellite?topic=satellite-satellite-cli-reference#location-create-cli)
* **`location dns`**: Set and manage subdomains for the hosts assigned to the control plane in a Satellite location.
    * [`ibmcloud sat location dns get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-get-cli)
    * [`ibmcloud sat location dns ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-ls-cli)
    * [`ibmcloud sat location dns register`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-register-cli)
* [`ibmcloud sat location get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-get-cli)
* [`ibmcloud sat location ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-ls-cli)
* [`ibmcloud sat location rm`](/docs/satellite?topic=satellite-satellite-cli-reference#location-rm-cli)
* [`ibmcloud sat location update`](/docs/satellite?topic=satellite-satellite-cli-reference#location-update-cli)


## `messages` commands
{: #icks_map_messages}

View the current user messages.
{: shortdesc}

* [`ibmcloud sat messages`](/docs/satellite?topic=satellite-satellite-cli-reference#messages-cli)


## `resource` commands
{: #icks_map_resource}

Search and view Kubernetes resources that are managed by a Satellite configuration.
{: shortdesc}

* [`ibmcloud sat resource get`](/docs/satellite?topic=satellite-satellite-cli-reference#resource-get-cli)
* **`resource history`**: Get history for a Kubernetes resource.
    * [`ibmcloud sat resource history get`](/docs/satellite?topic=satellite-satellite-cli-reference#resource-history-get-cli)
* [`ibmcloud sat resource ls`](/docs/satellite?topic=satellite-satellite-cli-reference#resource-ls-cli)


## `service` commands
{: #icks_map_service}

View Satellite service clusters.
{: shortdesc}

* [`ibmcloud sat service ls`](/docs/satellite?topic=satellite-satellite-cli-reference#service-ls-cli)


## `storage` commands
{: #icks_map_storage}

View and manage Satellite storage resources.
{: shortdesc}

* **`storage assignment`**: View and manage Satellite storage assignments.
    * **Beta** [`ibmcloud sat storage assignment autopatch disable`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-autopatch-disable-cli)
    * **Beta** [`ibmcloud sat storage assignment autopatch enable`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-autopatch-enable-cli)
    * [`ibmcloud sat storage assignment create`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-create-cli)
    * [`ibmcloud sat storage assignment get`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-get-cli)
    * [`ibmcloud sat storage assignment ls`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-ls-cli)
    * [`ibmcloud sat storage assignment patch`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-patch-cli)
    * [`ibmcloud sat storage assignment rm`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-rm-cli)
    * [`ibmcloud sat storage assignment update`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-update-cli)
* **`storage config`**: View and manage Satellite storage configurations.
    * [`ibmcloud sat storage config class add`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-class-add-cli)
    * [`ibmcloud sat storage config class get`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-class-get-cli)
    * [`ibmcloud sat storage config class ls`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-class-ls-cli)
    * [`ibmcloud sat storage config create`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-create-cli)
    * [`ibmcloud sat storage config get`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-get-cli)
    * [`ibmcloud sat storage config ls`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-ls-cli)
    * [`ibmcloud sat storage config param set`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-param-set-cli)
    * [`ibmcloud sat storage config patch`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-patch-cli)
    * [`ibmcloud sat storage config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-rm-cli)
* **`storage template`**: View the Satellite storage templates.
    * [`ibmcloud sat storage template get`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-template-get-cli)
    * [`ibmcloud sat storage template ls`](/docs/satellite?topic=satellite-satellite-cli-reference#storage-template-ls-cli)


## `subscription` commands
{: #icks_map_subscription}

View and manage Satellite subscriptions to deploy Kubernetes configuration files to your clusters.
{: shortdesc}

* [`ibmcloud sat subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#subscription-create-cli)
* [`ibmcloud sat subscription get`](/docs/satellite?topic=satellite-satellite-cli-reference#subscription-get-cli)
* **`subscription identity`**: Manage the identity used to apply the Satellite subscription.
    * [`ibmcloud sat subscription identity set`](/docs/satellite?topic=satellite-satellite-cli-reference#subscription-identity-set-cli)
* [`ibmcloud sat subscription ls`](/docs/satellite?topic=satellite-satellite-cli-reference#subscription-ls-cli)
* [`ibmcloud sat subscription rm`](/docs/satellite?topic=satellite-satellite-cli-reference#subscription-rm-cli)
* [`ibmcloud sat subscription update`](/docs/satellite?topic=satellite-satellite-cli-reference#subscription-update-cli)
