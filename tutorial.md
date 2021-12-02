<staging>---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-02"

keywords: satellite, hybrid, multicloud

subcollection: satellite

content-type: tutorial
services: satellite, openshift
account-plan:
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}


# WIP - Setting up and deploying apps to clusters in your first Satellite location
{: #satellite-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite, openshift"}
{: toc-completion-time="2h"}

With {{site.data.keyword.satellitelong}}, you can deploy your apps anywhere you need them, whether in the cloud or on-premises. Follow along with a fictional supply chain company to learn how to create a location and attach host capacity to the location. This capacity is then used to run workloads on {{site.data.keyword.openshiftlong}} clusters and consistently deploy apps anywhere in your location.
{: shortdesc}

This tutorial is currently under revision. To create a location, refer to [Setting up Satellite locations](/docs/satellite?topic=satellite-locations). To create clusters, see [Creating Satellite clusters](/docs/openshift?topic=openshift-satellite-clusters).
{: important}

## Objectives
{: #tutorial-objectives}

In this tutorial, you work for a global supply chain company that uses different cloud and on-premises computing environments at each of its shipping ports. You want to make sure that the same version of your applications run in each port. However, you do not want to need a team of people to log in to each environment and manually deploy the app.

First, you create a {{site.data.keyword.satelliteshort}} location and attach capacity to the location with hosts from your cloud and on-premises environments. Then, you assign the hosts to {{site.data.keyword.openshiftlong_notm}} clusters to make the clusters available for running apps across your {{site.data.keyword.satelliteshort}} location. After your location is set up and your clusters are ready, you can create a {{site.data.keyword.satelliteshort}} configuration for your app. The configuration deploys the same version to all your clusters that run in the {{site.data.keyword.satelliteshort}} location, as well as in {{site.data.keyword.openshiftlong_notm}} clusters that run in {{site.data.keyword.cloud_notm}} locations.

The following diagram provides an overview of what you set up in this tutorial.

## Audience
{: #tutorial-audience}

This tutorial is intended for system administrators who want to set up their first {{site.data.keyword.satelliteshort}} location for their team to deploy apps across {{site.data.keyword.openshiftshort}} clusters that run in both {{site.data.keyword.cloud_notm}} and on-premises environments.
{: shortdesc}

## Prerequisites
{: #tutorial-prereqs}

Review the following prerequisites to get ready before you begin.
{: shortdesc}

1. Have a [paid {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-account-getting-started).
2. [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-clusters) in an {{site.data.keyword.cloud_notm}} location such as **Washington, DC**.
3. [Create a standard {{site.data.keyword.cos_full_notm}} service instance](/docs/cloud-object-storage/basics?topic=cloud-object-storage-provision#provision-instance) to store the configuration data of your {{site.data.keyword.satelliteshort}} control plane.
4. [Set up the {{site.data.keyword.satellitelong_notm}} CLI plug-in](/docs/satellite?topic=satellite-setup-cli).
5. Identify at least 6 host machines in your on-premises or cloud infrastructure that you want to attach to {{site.data.keyword.satellitelong_notm}}. These host machines must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs), such as having Red Hat Enterprise Linux 7 with no modifications.

    Want to add hosts from other cloud providers like AWS, Azure, or Google? See [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud).
    {: tip}

## Setting up {{site.data.keyword.satelliteshort}} locations
{: #tutorial-location}

You can create {{site.data.keyword.satelliteshort}} locations for each place that you like, such as your company's ports in the north part of a country. After you set up your locations, you can bring {{site.data.keyword.cloud_notm}} services and consistent application management to the machines that already exist in your environments in the location.
{: shortdesc}

1. Log in to your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` flag, or create an API key to log in.

    ```sh
    ibmcloud login -a https://cloud.ibm.com -r us-east [-sso]
    ```
    {: pre}

2. Create a {{site.data.keyword.satelliteshort}} location that is named `Port-NewYork`. When you create a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.IBM_notm}} automatically creates a bucket in your {{site.data.keyword.cos_full_notm}} instance. If you want to control which instance is used, see the [`ibmcloud sat location create` reference](/docs/satellite?topic=satellite-satellite-cli-reference#location-create).
    ```sh
    ibmcloud sat location create --managed-from wdc --name Port-NewYork
    ```
    {: pre}
    
    | Component| Description |
    | -------------- | -------------- |
    | `--managed-from dal10` | For the staging environment, use `dal10`. The {{site.data.keyword.cloud_notm}} multizone metro that your {{site.data.keyword.satelliteshort}} control plane resources are managed from. Select the metro that is nearest to where your physical machines are. |
    | `--name Port-NewYork` | Give your {{site.data.keyword.satelliteshort}} location a name, such as `Port-NewYork`. |
    {: caption="Table 1. Understanding this command's components" caption-side="top"}
    
3. Verify that your location is created. The status of the location shows `action required` because no control plane is set up for the {{site.data.keyword.satelliteshort}} location yet. You attach compute hosts for your {{site.data.keyword.satelliteshort}} control plane in the next lesson.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

    Example output
    
    ```sh
    Name           ID                     Status            Ready   Created        Hosts (used/total)   Managed From   
    Port-NewYork   aaabbbb1111b2cccc33d   action required   no      1 minute ago   0 / 3                Dallas   
    ```
    {: screen}

## Setting up your {{site.data.keyword.satelliteshort}} control plane
{: #tutorial-location-setup}

Now that you set up your {{site.data.keyword.satelliteshort}} location, you must attach compute hosts to your location to create the {{site.data.keyword.satelliteshort}} control plane. These hosts can reside in your on-prem data center, on edge devices, in {{site.data.keyword.cloud_notm}}, or with other cloud providers. For more information, see [Understanding {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-about).
{: shortdesc}

1. Identify at least 6 machines that you want to attach to the location as hosts so that you can run the {{site.data.keyword.satelliteshort}} control plane. These machines must meet the [requirements for hosts](/docs/satellite?topic=satellite-host-reqs), such as running Red Hat Enterprise Linux 7, and any provider-specific requirements.
2. Generate a script that you can copy and run on your machines to attach them as hosts to your location. You might want to include a label to identify the purpose of the hosts, such as `use=satloc` because the hosts provide compute capacity for the {{site.data.keyword.satelliteshort}} location. Your hosts automatically are assigned labels for the CPU and memory size if these values can be detected on the machine.

    ```sh
    ibmcloud sat host attach --location Port-NewYork [-hl "use=satloc"]
    ```
    {: pre}

    Example output

    ```sh
    Creating host registration script...
    OK
    The script to attach hosts to Satellite location 'Port-NewYork' was downloaded to the following location:

    <filepath_to_script>/register-host_Port-NewYork_123456789.sh
    ```
    {: screen}

3. On your local machine, find the script.
4. Log in to each machine that you want to attach to your `Port-NewYork` location. Depending on where your machines reside, you might need to update the Red Hat packages that run on them. The following steps show how you can log in to an {{site.data.keyword.cloud_notm}} classic virtual server to run the script that you previously downloaded.
    1. Retrieve the **public_ip** address of your machine.
        ```sh
        ibmcloud sl vs list
        ```
        {: pre}

    2. Retrieve the credentials to log in to your virtual machine.
        ```sh
        ibmcloud sl vs credentials <vm_ID>
        ```
        {: pre}

    3. Copy the script from your local machine to the virtual server instance.
        ```sh
        scp <filepath_to_script> root@<ip_address>:/tmp/attach.sh
        ```
        {: pre}

    4. Log in to your virtual machine.
        ```sh
        ssh root@<ip_address>
        ```
        {: pre}

    5. Refresh the Red Hat packages on your machine.
        ```sh
        subscription-manager refresh
        ```
        {: pre}

        ```sh
        subscription-manager repos --enable rhel-server-rhscl-7-rpms
        subscription-manager repos --enable rhel-7-server-optional-rpms
        subscription-manager repos --enable rhel-7-server-rh-common-rpms
        subscription-manager repos --enable rhel-7-server-supplementary-rpms
        subscription-manager repos --enable rhel-7-server-extras-rpms
        ```
        {: pre}

    6. Make the registration script an executable file.
        ```sh
        chmod +x /tmp/attach.sh
        ```
        {: pre}

    7. Run the registration script on your machine.
        ```sh
        nohup bash /tmp/attach.sh &
        ```
        {: pre}

        When you run the script on your machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} control plane.
        {: note}

    8. Exit the SSH session.  
        ```sh
        exit
        ```
        {: pre}

5. Verify that your hosts are attached to your `Port-NewYork` location. Your hosts are not yet assigned to the {{site.data.keyword.satelliteshort}} control plane, which is why all of them show an `unassigned` state without any cluster or worker node information.
    ```sh
    ibmcloud sat host ls --location Port-NewYork
    ```
    {: pre}

    Example output
    ```sh
    Name             ID                     State        Status   Cluster   Worker ID   Worker IP   
    machine-name-1   aaaaa1a11aaaaaa111aa   unassigned   -        -         -           -   
    machine-name-2   bbbbbbb22bb2bbb222b2   unassigned   -        -         -           -   
    machine-name-3   ccccc3c33ccccc3333cc   unassigned   -        -         -           -  
    ```
    {: screen}

6. Assign your host machine to the {{site.data.keyword.satelliteshort}} location so that you can run the {{site.data.keyword.satelliteshort}} control plane. When you assign the host to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. The process also disables the ability to SSH into the machine for security purposes. If you later remove the host from the {{site.data.keyword.satelliteshort}} location, you must reload the host machine to SSH into the machine again.
    ```sh
    ibmcloud sat host assign --location Port-NewYork  --cluster <location_ID> --host <host_ID> --zone <us-east-1>
    ```
    {: pre}
    
    | Component | Description |
    | -------------- | -------------- |
    | `--location Port-NewYork` | Enter the name of your {{site.data.keyword.satelliteshort}} location, such as `Port-NewYork`. |
    | `--cluster <location_ID>` | Enter the ID of the {{site.data.keyword.satelliteshort}} location where you want to assign the hosts to run the {{site.data.keyword.satelliteshort}} control plane. To view your location ID, run `ibmcloud sat location ls`. |
    | `--host <host_ID>` | Enter the host ID to assign to the location control plane. To view the host ID, run `ibmcloud sat host ls --location <location_name>` |
    | `--zone <us-east-1` | Enter the zone to assign the host in. When you repeat this command, change the zone so that you include all three zones in US East (`us-east-1`, `us-east-2`, and `us-east-3`). |
    {: caption="Table 2. Understanding this command's components" caption-side="top"}

7. Repeat the previous step for all three zones for the location control plane. For example, if your location is managed out of the {{site.data.keyword.cloud_notm}} **Washington, DC** multizone metro (US East region), assign hosts in `us-east-1`, `us-east-2`, and `us-east-3`.

8. Verify that your hosts are successfully assigned to your location. The assignment is successful when all hosts show a **Ready** status. If the **Status** of your machines shows `-`, the bootstrapping process is not yet completed and the health status could not be retrieved. Wait a few minutes, and then try again.
    ```sh
    ibmcloud sat host ls --location <location_name>
    ```
    {: pre}

    Example output
    ```sh
    Retrieving hosts...
    OK
    Name              ID                     State      Status   Cluster          Worker ID                                                 Worker IP   
    machine-name-1    aaaaa1a11aaaaaa111aa   assigned   Ready    infrastructure   sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.xx.xxx.xxx   
    machine-name-2    bbbbbbb22bb2bbb222b2   assigned   Ready    infrastructure   sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.xx.xxx.xxx   
    machine-name-3    ccccc3c33ccccc3333cc   assigned   Ready    infrastructure   sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.xx.xxx.xxx   
    ```
    {: screen}

9. Verify that {{site.data.keyword.satelliteshort}} automatically set up a subdomain and DNS records for your location. If not, [register the IP addresses of all three of your hosts](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-register).
    ```sh
    ibmcloud sat location dns ls --location <location_ID>
    ```
    {: pre}

    Example output
    ```sh
    Hostname                                                                                              Records                                                                                               Health Monitor   SSL Cert Status   SSL Cert Secret Name                                          Secret Namespace   
    a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c000.us-east.satellite.appdomain.cloud   169.xx.xxx.xxx, 169.xx.xxx.xxx, 169.xx.xxx.xxx                                                             None             created               a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c000   default   
    a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c001.us-east.satellite.appdomain.cloud   169.xx.xxx.xxx                                                                                        None             -                    -                                                             default   
    a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c002.us-east.satellite.appdomain.cloud   169.xx.xxx.xxx                                                                                         None             created              a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c002   default   
    a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c003.us-east.satellite.appdomain.cloud   169.xx.xxx.xxx                                                                                         None             created              a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c003   default   
    a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-ce00.us-east.satellite.appdomain.cloud      a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-c000.us-east.satellite.appdomain.cloud   None                created           a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-ce00   default
    ```
    {: screen}


## Attaching compute capacity for cluster workloads
{: #tutorial-clusters}

Now that you have the location for your New York port set up, create some clusters to run your workloads. In the [prerequisites](/docs/satellite?topic=satellite-satellite-tutorial#tutorial-prereqs), you created or identified a {{site.data.keyword.openshiftlong_notm}} cluster that runs in an {{site.data.keyword.cloud_notm}} location. Now, create a {{site.data.keyword.openshiftlong_notm}} cluster that runs in the {{site.data.keyword.satelliteshort}} location that you just set up. {{site.data.keyword.openshiftshort}} clusters extend the core container management capabilities of the community Kubernetes project with developer-focused tools that provide the best platform to run your apps securely.
{: shortdesc}

1. Create a {{site.data.keyword.satelliteshort}} cluster that runs {{site.data.keyword.openshiftshort}} version 4.4 in your `Port-NewYork` location.
    ```sh
    ibmcloud oc cluster create satellite --name mycluster --location Port-NewYork --version 4.4_openshift
    ```
    {: pre}

2. Note your cluster ID, and wait for your cluster to be in a `warning` state before continuing to the next step. The cluster might take 10 or so minutes to provision.
    ```sh
    ibmcloud sat cluster ls
    ```
    {: pre}

    Example output
    ```sh
    Name        ID                     State           Created         Workers   Location       Version                   Resource Group Name   Provider   
    mycluster    abcdef123gh4i5j6klm   deploying       5 minutes ago   0         Port-NewYork   4.4.11_1511_openshift     Default               satellite   
    ```
    {: screen}

3. Identify at least 3 more machines that you want to attach to each location as hosts to run your apps in your {{site.data.keyword.satelliteshort}} clusters. These machines must follow the [requirements for hosts](/docs/satellite?topic=satellite-host-reqs), such as running Red Hat Enterprise Linux 7, and any provider-specific requirements. Think about if your hosts have any shared characteristics that you might want to label them for ease of use later. For example, you might have some hosts from your receiving department with a `dept=rec` label, or have different hosts for `env=staging` and `env=prod` labels. The machine's CPU and memory sizes are automatically detected and added as labels.
4. Generate a script that you can copy and run on your machines to attach them as hosts to your location. Include any labels that you previously identified.
    ```sh
    ibmcloud sat host attach --location Port-NewYork [-hl "dept=rec"]
    ```
    {: pre}

    Example output
    ```sh
    The script to attach hosts to Satellite location 'Port-NewYork' was downloaded to the following location:

    <filepath/to/script>/register-host_Port-NewYork_123456789.sh
    ```
    {: screen}

5. On your local machine, find the script.
6. Log in to each machine that you want to attach to your {{site.data.keyword.satelliteshort}} cluster. Depending on where your machines reside, you might need to update the Red Hat packages that run on them. The following steps show how you can log in to an {{site.data.keyword.cloud_notm}} classic virtual server to run the script that you previously downloaded.
    1. Retrieve the **public_ip** address of your machine.
        ```sh
        ibmcloud sl vs list
        ```
        {: pre}

    2. Retrieve the credentials to log in to your virtual machine.
        ```sh
        ibmcloud sl vs credentials <vm_ID>
        ```
        {: pre}

    3. Copy the script from your local machine to the virtual server instance.
        ```sh
        scp <filepath_to_script> root@<ip_address>:/tmp/attach.sh
        ```
        {: pre}

    4. Log in to your virtual machine.
        ```sh
        ssh root@<ip_address>
        ```
        {: pre}

    5. Refresh the Red Hat packages on your machine.
        ```sh
        subscription-manager refresh
        ```
        {: pre}

        ```sh
        subscription-manager repos --enable rhel-server-rhscl-7-rpms
        subscription-manager repos --enable rhel-7-server-optional-rpms
        subscription-manager repos --enable rhel-7-server-rh-common-rpms
        subscription-manager repos --enable rhel-7-server-supplementary-rpms
        subscription-manager repos --enable rhel-7-server-extras-rpms
        ```
        {: pre}

    6. Make the registration script an executable file.
        ```sh
        chmod +x /tmp/attach.sh
        ```
        {: pre}

    7. Run the registration script on your machine.
        ```sh
        nohup bash /tmp/attach.sh &
        ```
        {: pre}

        When you run the script on your machine, the machine is made visible to your {{site.data.keyword.satelliteshort}} location, but is not yet assigned to the {{site.data.keyword.satelliteshort}} cluster.
        {: note}

    8. Exit the SSH session.  
        ```sh
        exit
        ```
        {: pre}

7. Check that your hosts are attached to your `Port-NewYork` location and note the host IDs. In the output, note that cluster and worker node information is blank because the hosts are not assigned to these resources yet.
    ```sh
    ibmcloud sat host ls --location Port-NewYork
    ```
    {: pre}

    Example output
    ```sh
    Name             ID                     State        Status   Cluster   Worker ID   Worker IP   
    machine-name-1   aaaaa1a11aaaaaa111aa   unassigned   -        -         -           -   
    machine-name-2   bbbbbbb22bb2bbb222b2   unassigned   -        -         -           -   
    machine-name-3   ccccc3c33ccccc3333cc   unassigned   -        -         -           -  
    ```
    {: screen}

8. Assign your three host machines to the {{site.data.keyword.satelliteshort}} cluster that you previously created. When you assign the host to the control plane, {{site.data.keyword.IBM_notm}} bootstraps your machine. This process might take a few minutes to complete. The script also disables the ability to SSH into the machine for security purposes. If you later remove the host from the {{site.data.keyword.satelliteshort}} location, you must reload the host machine to SSH into the machine again.
    ```sh
    ibmcloud sat host assign --location Port-NewYork  --cluster <cluster_ID> --host <host_ID>
    ```
    {: pre}

9. Check that your hosts are assigned to your cluster. In the output, the cluster and worker node information is now provided, and note the **Worker IP**.
    ```sh
    ibmcloud sat host ls --location Port-NewYork
    ```
    {: pre}

    Example output
    ```sh
    Name             ID                     State        Status   Cluster          Worker ID                                                 Worker IP   
    machine-name-1   aaaaa1a11aaaaaa111aa   assigned     Ready    mycluster   sat-satellitet-111a1a1111a1aa111a1a1aa111111a1aaa11111a   169.xx.xxx.xxx   
    machine-name-2   bbbbbbb22bb2bbb222b2   assigned     Ready    mycluster   sat-satellitet-bb2b2bb222b22b22b22bbbb22222b2222bb22222   169.xx.xxx.xxx   
    machine-name-3   ccccc3c33ccccc3333cc   assigned     Ready    mycluster   sat-satellitet-c3333c333c3c33c3333cc33c3333c33cccc33333   169.xx.xxx.xxx   
    ```
    {: screen}

10. Verify the cluster **State** is **normal** and the **Master status** is **Ready**.
    ```sh
    ibmcloud oc cluster get -c mycluster
    ```
    {: pre}

    Example output
    ```sh
    Name:                           mycluster   
    ID:                             abcdef123gh4i5j6klm   
    State:                          normal   
    ...

    Master         
    Status:     Ready (2 hours ago)   
    State:      deployed   
    Health:     normal   
    Version:    4.4.11_1511_openshift   
    Location:   Port-NewYork   
    URL:        https://a11aa111111aa1a111aa1-bbbb2b2bb22222b2222bbbb2222b2b2b-ce00.us-east.satellite.appdomain.cloud:3xxxx   
    ```
    {: screen}

Now you have a {{site.data.keyword.openshiftlong_notm}} cluster with worker nodes on your own infrastructure that run as hosts in your {{site.data.keyword.satelliteshort}} location! In the next lesson, you deploy an app to your cluster.

## Deploying an app across locations
{: #tutorial-app-config}

With a cluster set up in your {{site.data.keyword.satelliteshort}} port in New York set up, you are ready to deploy your first app. However, New York is not your only port. You wonder how you can deploy the same app in all your clusters across locations without logging in to each cluster and deploying each app separately.
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} Config, based on the open source project [Razee](https://razee.io/){: external}, is a continuous delivery tool that you can use to manage your Kubernetes containerized application lifecycle, from deployment to updates and inventory.

### Adding your clusters to {{site.data.keyword.satelliteshort}} Config
{: #tutorial-cluster-config}

First, add your clusters to {{site.data.keyword.satelliteshort}} Config so that the clusters can be subscribed to configuration updates. You can install the {{site.data.keyword.satelliteshort}} Config components in any cluster, including both of your clusters that run in your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} location. When you add clusters to {{site.data.keyword.satelliteshort}} Config, you can also see an inventory of all the Kubernetes resources that run in the clusters from a single pane of glass.
{: shortdesc}

1. Install the {{site.data.keyword.satelliteshort}} Config components in your {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} and {{site.data.keyword.cloud_notm}} locations.
    1. [Log in to your cluster](/docs/openshift?topic=openshift-access_cluster).
    2. Get a `kubectl` command to run in your cluster to install the {{site.data.keyword.satelliteshort}} Config components.
        ```sh
        ibmcloud sat cluster register
        ```
        {: pre}

        Example output
        ```sh
        TODO
        ```
        {: screen}

    3. Copy and run the `kubectl` command in your cluster.
    4. Repeat these steps for each cluster that you want to deploy your apps to.
2. Create a cluster group to organize your clusters together so that you can subscribe the clusters to a configuration at the same time.
    ```sh
    ibmcloud sat cluster-group create
    ```
    {: pre}

3. Add your clusters to the cluster group.
4. Verify that {{site.data.keyword.satelliteshort}} Config is set up in your clusters by checking the inventory of Kubernetes resources. You can see all of the Kubernetes resources that run in your registered clusters, and can filter by components like cluster, subscription, and Kubernetes `kind`, or search for particular strings.
    ```sh
    ibmcloud sat resource ls
    ```
    {: pre}

    Example output
    ```sh
    TODO
    ```
    {: screen}

### Subscribing clusters to configuration updates
{: #tutorial-subscribe-config}

Now that {{site.data.keyword.satelliteshort}} Config is installed in your clusters, you want to deploy a **hello world** app. You can containerize your hello world app as a Kubernetes resource configuration file, sometimes called a manifest file, in YAML format. Then, you upload this file as a version for a configuration in {{site.data.keyword.satelliteshort}} Config. Finally, you subscribe clusters or groups of clusters to receive the configuration and subsequent version updates.
{: shortdesc}

1. Create a {{site.data.keyword.satelliteshort}} configuration where you can upload different versions of Kubernetes resources.
    ```sh
    ibmcloud sat configuration create --name hello-world
    ```
    {: pre}

2. Create a file to describe the Kubernetes resources that you want to deploy and save the file as `hello-world.yaml`.
    ```sh
    TODO
    ```
    {: codeblock}

3. Upload your `hello-world-v1.yaml` file as a `test` version to your configuration.
    ```sh
    ibmcloud sat configuration version create --name v1-test --configuration hello-world --file-format yaml --read-config <file_path>/hello-world.yaml
    ```
    {: pre}

    Example output
    ```sh
    Creating configuration version...
    OK
    Configuration Version hello-world-v1 was successfully created with ID f5a689ef-79cd-4f8f-a3e2-959226b4ccf4.
    ```
    {: screen}
    
    | Parameter | Description |
    | -------------- | -------------- |
    | `--name v1-test` | Enter a name for your version, such as `v1-test`. |
    | `--configuration hello-world` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--file-format yaml` | Enter the file extension of your Kubernetes resource file. Supported extensions are `yaml`. |
    | `--read-config <file_path>/hello-world-v1.yaml` | Enter the relative file path to the Kubernetes resource file on your local machine. You can upload only one file per version, but the file can contain multiple Kubernetes resource configurations (such as `Deployment`, `Service`, and `Configmap` all in one file). |
    {: caption="Table 3. Understanding the command's components" caption-side="top"}

4. Subscribe your cluster group to the {{site.data.keyword.satelliteshort}} configuration and version.
    ```sh
    ibmcloud sat TODO
    ```
    {: pre}

5. Verify that your Kubernetes resources are deployed to your clusters.
    ```sh
    ibmcloud sat resource ls --cluster mycluster
    ```
    {: pre}

6. Check your hello world app.
    TODO


### Rolling out a new version of your app
{: #tutorial-config-version}

Your first test version of the **hello world** app works, so you are ready to promote the version to production so that your users can start to use the app. You also want to start development on the second version of the app, so you use {{site.data.keyword.satelliteshort}} Config to manage these app version changes.
{: shortdesc}

1. Update your `hello-world.yaml` Kubernetes resource configuration file for the production version of your app, which changes the way that the app is exposed to an external route. You can create a copy locally, but {{site.data.keyword.satelliteshort}} Config saves copies of each version, so you can also rewrite your original file.
    ```sh
    TODO
    ```
    {: codeblock}

2. Upload the Kubernetes resource configuration file as the production version 1 of your app.
    ```sh
    ibmcloud sat configuration version create --name v1-prod --configuration hello-world --file-format yaml --read-config <file_path>/hello-world.yaml
    ```
    {: pre}

3. Subscribe your cluster group to the production version of your app.
    ```sh
    ibmcloud sat TODO
    ```
    {: pre}

4. Update your `hello-world.yaml` Kubernetes resource configuration file to start on the second version of your app in your test environment. For example, you might want to refresh the text that is displayed.
    ```sh
    TODO
    ```
    {: codeblock}

5. Upload the Kubernetes resource configuration file as the test version 2 of your app.
    ```sh
    ibmcloud sat configuration version create --name v2-test --configuration hello-world --file-format yaml --read-config <file_path>/hello-world.yaml
    ```
    {: pre}

6. Update the subscription to your test app to use the new `v2-test` version.
    ```sh
    ibmcloud sat TODO
    ```
    {: pre}

7. Check the different test and production versions of your apps.
    TODO

## What's next?
{: #tutorial-next}

TODO

</staging>

