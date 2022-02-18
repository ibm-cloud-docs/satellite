---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is the namespace where my storage operator was deployed stuck in **Terminating** status?
{: #storage-namespace-terminating}


When you remove a storage configuration from a cluster, the resources such as operator pods and storage classes are removed, but the namespace is stuck in `Terminating` status. 
{: tsSymptoms}


There are [finalizers](https://kubernetes.io/blog/2021/05/14/using-finalizers-to-control-deletion/){: external} that are preventing the remaining resources in the namespace and the namespace itself from being deleted.
{: tsCauses}


Take the following steps to remove the resources and the namespace.
{: tsResolve}

Do not delete or patch the resource finalizers in the `kube-system` namespace.
{: important}

1. Get the namespace that is stuck in `Terminating` status. Make a note of the resources that are listed in the `message: 'Some resources are remaining:` section.
    ```sh
    oc get ns <namespace> -o yaml
    ```
    {: pre}

    **Example output for a NetApp Trident namespace**
    ```sh
    status:
        conditions:
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: All resources successfully discovered
        reason: ResourcesDiscovered
        status: "False"
        type: NamespaceDeletionDiscoveryFailure
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: All legacy kube types successfully parsed
        reason: ParsedGroupVersions
        status: "False"
        type: NamespaceDeletionGroupVersionParsingFailure
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: All content successfully deleted, may be waiting on finalization
        reason: ContentDeleted
        status: "False"
        type: NamespaceDeletionContentFailure
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: 'Some resources are remaining: tridentbackends.trident.netapp.io has 1 resource instances, tridentnodes.trident.netapp.io has 3 resource instances, tridentversions.trident.netapp.io has 1 resource instances, tridentvolumes.trident.netapp.io has 1 resource instances'
        reason: SomeResourcesRemain
        status: "True"
        type: NamespaceContentRemaining
        - lastTransitionTime: "2021-05-18T17:40:31Z"
        message: 'Some content in the namespace has finalizers remaining: trident.netapp.io
        in 6 resource instances'
        reason: SomeFinalizersRemain
        status: "True"
        type: NamespaceFinalizersRemaining
        phase: Terminating
    ```
    {: screen}

2. Run the `oc get` command to get the resources that are pending removal. In the example output, the `tridentbackends.trident.netapp.io has 1 resource instances`. Repeat this step for each resource that has remaining instances listed in the `message` section of the namespace YAML that you retrieved earlier.
    ```sh
    oc get <resource> -n <namespace>
    ```
    {: pre}

    Example command for the `tridentbackends.trident.netapp.io` resource.
    ```sh
    oc get tridentbackends.trident.netapp.io -n trident
    ```
    {: pre}

    **Example output**
    ```sh
    tbe-bhb5r
    ```
    {: screen}

    ```sh
    pvc-26d97ecd-c268-47ff-a2fd-a0a1a51a9e12
    ```
    {: screen}

3. For each resource that has remaining instances, run the following `kubectl patch` command to remove the finalizers and delete the resource. After all the resources that have remaining instances have been patched, the namespace is removed.
    ```sh
    kubectl -n <namespace> patch <resource>/<instance> -p '{"metadata":{"finalizers":[]}}' --type=merge
    ```
    {: pre}

    Example command for the `tridentbackends.trident.netapp.io` resource.
    ```sh
    kubectl -n trident patch tridentbackends.trident.netapp.io/tbe-bhb5r -p '{"metadata":{"finalizers":[]}}' --type=merge
    ```
    {: pre}

4. After you have removed the finalizers on all the remaining resources, run the `oc get ns` command to verify that namespace has been removed.
    ```sh
    oc get ns
    ```
    {: pre}







