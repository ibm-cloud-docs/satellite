---


copyright:
  years: 2020, 2024
lastupdated: "2024-02-07"

keywords: satellite, hybrid, attaching hosts, hosts, attach hosts, attach hosts to location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Downloading the host attachment script for your location
{: #host-attach-download}

To attach hosts to your location, you must download a host attachment script. After you download the script, you can run it on your hosts to attach them to your location. You can get the attachment script from the console or by running the `sat host attach` [command](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach-cli). 

When you attach a host with a Red Hat CoreOS (RHCOS) operating system, the attachment script is an ignition (`.ign`) file. When you attach a host with a RHEL operating system, the attachment script is a Shell script. 

When you download the host attachment script for your location, it contains a unique token that expires after 1 year. Plan to regenerate this token by downloading a new host attachment script at least once per year. For more information, see [Why is my host attach failing with error message `A0029 Access denied to specified controller`](/docs/satellite?topic=satellite-ts-host-expired-token)?
{: note}

## Download the host attachment script from the console
{: #host-download-console}

1. Navigate to the [**Locations** dashboard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to attach hosts.  
2. From the **Hosts** tab, click **Attach host**.
3. Optional: Enter any labels that you want to add to your hosts so that you can identify your hosts more easily later. Labels must be provided as key-value pairs. For example, you can use `use=satcp` or `use=satcluster` to show that you want to use these hosts for your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.redhat_openshift_notm}} cluster. By default, your hosts get a `cpu`, an `os`, and a `memory` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`. Note that the default value for `os` is `rhel`.
4. Enter a file name for your script or use the name that is generated for you.
5. Select **RHEL** or **RHCOS** to download the host script for your host system.
6. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which must be treated and protected as sensitive information.

## Download the host attachment script from the CLI
{: #host-download-cli}


Generate the host attachment script to from the CLI with the **`sat host attach`** command. You can specify the host operating system by using the `--operating-system` command option. When you run the **`sat host attach`** command to generate the script, you can include labels that identify the purpose of the hosts, such as `use:satloc`. Your hosts are automatically assigned labels for the CPU and memory size if these values can be detected on the host machine. For more information about labels, see [Using host auto assignment](/docs/satellite?topic=satellite-host-autoassign-ov).

The following example `host attach` command downloads a script for an RHCOS host.
            
```sh
ibmcloud sat host attach --location <location_name> [-hl "use=satloc"] --operating-system RHCOS
```
{: pre}

The following example `host attach` command downloads a script for a RHEL host.

```sh
ibmcloud sat host attach --location <location_name> [-hl "use=satloc"] --operating-system RHEL
```
{: pre}
            
Example output
```sh
Creating host registration script...
OK
The script to attach hosts to Satellite location 'mylocation' was downloaded to the following location:
 <filepath_to_script>/register-host_mylocation_attach_hypershift.ign
```
{: screen}
     

If your hosts are RHEL hosts, you must update the [required packages](/docs/satellite?topic=satellite-host-reqs) on your hosts before you can run the script. If your hosts are running the latest Red Hat CoreOS images, you do not need to update the packages.


