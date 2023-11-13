---

copyright:
  years: 2023, 2023
lastupdated: "2023-11-13"

keywords: satellite, hybrid, multicloud, troubleshoot terraform, update hosts

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}



# Why is my host attach script triggering a state change in Terraform?
{: #ts-host-terraform}

You download an updated host script for your location, which you must do yearly or else the [script expires](/docs/satellite?topic=satellite-ts-host-unassigned-unknown). If you use Terraform scripts to manage your {{site.data.keyword.satelliteshort}} location and hosts, the updated host script requires you to update all your existing hosts, forcing you to replace them with new hosts.
{: tsSymptoms}


Your `attach-host` script requires an update at least once a year. The updated script qualifies as a state change for Terraform and triggers an update requirement for all hosts, even those hosts that are attached to a cluster or the location control plane. For more information about your `attach-host` script updates, see [Why do my `unassigned` hosts have an Unresponsive status?](/docs/satellite?topic=satellite-ts-host-unassigned-unknown).
{: tsCauses}


To resolve this issue, update your Terraform script to include the following example argument to the script that creates or modifies your host instances. Replace `<host-script>` with the field name you used to supply the `attach-host` script to the host instances.
{: tsResolve}

```terraform
  lifecycle {
    ignore_changes = [
      <host-script>,
    ]
  }
```
{: pre}

For example, in the [Azure Terraform scripts](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples/satellite-azure){: external}, edit the `instance.tf` file and replace `<host-script>` with `custom_data`, which is the name of the field defined by the [`azurerm_linux_virtual_machine` resource](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_virtual_machine#custom_data){: external}.

```terraform
  lifecycle {
    ignore_changes = [
      custom_data,
    ]
  }
```
{: pre}

After you add this code, any hosts that are currently attached to your location, included hosts in an 'unassigned' state are ignored by the Terraform script. Any new hosts that you attach to your location use the updated attach script.



