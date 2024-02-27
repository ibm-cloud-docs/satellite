---

copyright:
  years: 2024, 2024
lastupdated: "2024-02-27"

keywords: satellite, recover, disaster recovery, troubleshooting

subcollection: satellite

---
{{site.data.keyword.attribute-definition-list}}


# Checking cluster health after recovery
{: #ts-recovery-validation}

Complete the following steps to debug your cluster state after an infrastructure event like a machine failure or network outage in the location.

You can complete these steps in order, or you can run the [cluster debug script](#ts-cluster-debug-script). The following steps are compiled into the debug script for you.
{: tip}

1. Check node health and ensure all nodes are `Ready`.

    ```sh
    kubectl get node | grep "NotReady"
    ```
    {: pre}
    
    If there are any nodes that are not ready, remove and replace them with `ibmcloud worker rm` and `ibmcloud sat host assign`.

1. Check that the Calico components are running.
    ```sh
    kubectl get pods -n calico-system | grep -v "1/1.*Running"
    ```
    {: pre}

    Restart any unhealthy pods by deleting them and allowing them to get re-created. If the pods are still not in the `Ready` state after deleting and re-creating them, remove and replace the infrastructure hosts that the unhealthy pods are on with `ibmcloud worker rm` and `ibmcloud sat host assign`.

1. Check that all `openshift-kube-proxy` pods are healthy.
    ```sh
    kubectl get pods -n openshift-kube-proxy | grep -v "2/2.*Running"
    ```
    {: pre}
    
    Restart any unhealthy pods by deleting them and allowing them to get re-created. If the pods are still not in the `Ready` state after deleting and re-creating them, remove and replace the infrastructure hosts that the unhealthy pods are on with `ibmcloud worker rm` and `ibmcloud sat host assign`.

1. Check that the OpenShift DNS pods are running.

    ```sh
    kubectl get pods -n openshift-dns | grep -v "2/2.*Running" | grep -v "1/1.*Running"
    ```
    {: pre}

    Restart any unhealthy pods by deleting them and allowing them to get re-created. If the pods are still not in the `Ready` state after deleting and re-creating them, remove and replace the infrastructure hosts that the unhealthy pods are on with `ibmcloud worker rm` and `ibmcloud sat host assign`.

1. Check that Ingress pods are running.
    ```sh
    kubectl get pods -n openshift-ingress | grep -v "1/1.*Running"
    ```
    {: pre}

    Restart any unhealthy pods by deleting them and allowing them to get re-created. If the pods are still not in the `Ready` state after deleting and re-creating them, remove and replace the infrastructure hosts that the unhealthy pods are on with `ibmcloud worker rm` and `ibmcloud sat host assign`.

1. **Optional**: If you are using OpenShift Data Foundation, ensure all the ODF pods operational.
    ```sh
    kubectl get pods -n openshift-storage | grep -v "1/1.*Running" | grep -v "2/2.*Running" | grep -v "3/3.*Running" | grep -v "5/5.*Running" | grep -v "6/6.*Running"
    ```
    {: pre}

    Restart any unhealthy pods by deleting them and allowing them to get re-created. If the pods are still not in the `Ready` state after deleting and re-creating them, remove and replace the infrastructure hosts that the unhealthy pods are on with `ibmcloud worker rm` and `ibmcloud sat host assign`.
    
    If you remove ODF nodes, you must also run the OSD removal command for that node. For more information, see [Cleaning up ODF](/docs/openshift?topic=openshift-ocs-manage-deployment&interface=ui#odf-cleanup)
    {: important}

## Running the cluster debug script
{: #ts-cluster-debug-script}

1. Save the following script as a file called `debug.sh`.

    ```sh
    #!/usr/bin/env bash
    MAX_WAIT_ATTEMPTS=80
    SLEEP_BETWEEN_ATTEMPTS=30
    set -x
    for ((c = 0; c <= MAX_WAIT_ATTEMPTS; c++)); do
        sleep $SLEEP_BETWEEN_ATTEMPTS
        kubectl get node -o wide >/tmp/nodedata
        kubectl get node -o yaml >/tmp/nodefulldata
        kubectl get pods -A -o wide >/tmp/poddata
        if grep "NotReady" /tmp/nodedata; then
            echo "nodes not ready:  replace node(s) with fresh infrastructure"
            grep "NotReady" /tmp/nodedata
            continue
        fi
        if grep "taint" /tmp/nodefulldata; then
            echo "nodes have unexpected taints:  investigate which nodes have taints and consider replacing node with fresh infrastructure"
            continue
        fi
        grep "calico-system " /tmp/poddata >/tmp/calicosystemdata
        grep -v "calico-system .*1/1.*Running" /tmp/calicosystemdata >/tmp/nonrunningcalicosystemdata
        if grep "calico-system" /tmp/nonrunningcalicosystemdata; then
            echo "calico-system pods are not fully running"
            grep "calico-system" /tmp/nonrunningcalicosystemdata
            continue
        fi
        grep "openshift-kube-proxy " /tmp/poddata >/tmp/openshiftkubeproxypoddata
        grep -v "openshift-kube-proxy .*2/2.*Running" /tmp/openshiftkubeproxypoddata >/tmp/nonrunningopenshiftkubeproxypoddata
        if grep "openshift-kube-proxy" /tmp/nonrunningopenshiftkubeproxypoddata; then
            echo "openshift-kube-proxy pods are not fully running"
            grep "openshift-kube-proxy" /tmp/nonrunningopenshiftkubeproxypoddata
            continue
        fi
        grep "openshift-dns " /tmp/poddata >/tmp/openshiftdnspoddata
        grep -v "openshift-dns .*2/2.*Running" /tmp/openshiftdnspoddata >/tmp/nonrunningopenshiftdnspoddata
        cp -f /tmp/nonrunningopenshiftdnspoddata /tmp/tmpnonrunningopenshiftdnspoddata
        grep -v "openshift-dns .*1/1.*Running" /tmp/tmpnonrunningopenshiftdnspoddata >/tmp/nonrunningopenshiftdnspoddata
        if grep "openshift-dns" /tmp/nonrunningopenshiftdnspoddata; then
            echo "openshift-dns pods are not fully running"
            grep "openshift-dns" /tmp/nonrunningopenshiftdnspoddata
            continue
        fi
        grep "openshift-storage " /tmp/poddata >/tmp/odfpoddata
        grep -v "openshift-storage .*1/1.*Running" /tmp/odfpoddata >/tmp/nonrunningodfpoddata
        cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
        grep -v "openshift-storage .*2/2.*Running" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
        cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
        grep -v "openshift-storage .*3/3.*Running" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
        cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
        grep -v "openshift-storage .*5/5.*Running" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
        cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
        grep -v "openshift-storage .*6/6.*Running" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
    cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
    grep -v "Evitcted" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
    cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
    grep -v "Completed" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
    cp -f /tmp/nonrunningodfpoddata /tmp/tmpnonrunningodfpoddata
    grep -v "Error" /tmp/tmpnonrunningodfpoddata >/tmp/nonrunningodfpoddata
        if grep "openshift-storage" /tmp/nonrunningodfpoddata; then
            echo "openshift-storage pods are not fully running: try restarting them with kubectl delete pod -n openshift-storage POD_NAME"
            continue
        fi
        export SUCCESSFULLY_VALIDATED_REBOOT=true
        break
    done
    if [[ "$SUCCESSFULLY_VALIDATED_REBOOT" == "true" ]]; then
    echo "core pieces validated: check cp4i namespace and consult debug documentation if needed: https://www.ibm.com/docs/en/cloud-paks/cp-integration/2023.4?topic=troubleshooting-known-limitations"
    exit 0
    fi
    if grep "NotReady" /tmp/nodedata; then
    echo "nodes not ready: replace node(s) with fresh infrastructure using ibmcloud worker rm and ibmcloud host assign"
    grep "NotReady" /tmp/nodedata
    exit 1
    fi
    if grep "taint" /tmp/nodefulldata; then
    echo "nodes have unexpected taints: investigate which nodes have taints and consider replacing node with fresh infrastructure"
    cat /tmp/nodefulldata
    exit 1
    fi
    if grep "calico-system" /tmp/nonrunningcalicosystemdata; then
    echo "calico-system pods are not fully running: try restarting them with kubectl delete pod -n calico-system POD_NAME"
    echo "if replacing pod does not work: then replace infrastructure pod is on with new infrastruture using ibmcloud worker rm and ibmcloud host assign"
    grep "calico-system" /tmp/nonrunningcalicosystemdata
    exit 1
    fi
    if grep "openshift-kube-proxy" /tmp/nonrunningopenshiftkubeproxypoddata; then
    echo "openshift-kube-proxy pods are not fully running: try restarting them with kubectl delete pod -n openshift-kube-proxy POD_NAME"
    echo "if replacing pod does not work: then replace infrastructure pod is on with new infrastruture using ibmcloud worker rm and ibmcloud host assign"
    grep "openshift-kube-proxy" /tmp/nonrunningopenshiftkubeproxypoddata
    exit 1
    fi
    if grep "openshift-dns" /tmp/nonrunningopenshiftdnspoddata; then
    echo "openshift-dns pods are not fully running: try restarting them with kubectl delete pod -n openshift-dns POD_NAME"
    echo "if replacing pod does not work: then replace infrastructure pod is on with new infrastruture using ibmcloud worker rm and ibmcloud host assign"
    grep "openshift-dns" /tmp/nonrunningopenshiftdnspoddata
    exit 1
    fi
    if grep "openshift-storage" /tmp/nonrunningodfpoddata; then
    echo "storage pods are not fully running: try restarting them with kubectl delete pod -n openshift-storage POD_NAME"
    echo "if replacing pod does not work: then replace infrastructure pod is on with new infrastruture using ibmcloud worker rm and ibmcloud host assign"
    grep "openshift-storage" /tmp/nonrunningodfpoddata
    exit 1
    fi
    ```
    {: codeblock}


1. Change the file permissions to make it executable.
    ```sh
    chmod +x deubg.sh
    ```
    {: pre}

1. Run the script.

    ```sh
    ./debug.sh
    ```
    {: pre}

1. Restart any unhealthy pods by deleting them and allowing them to get re-created. If the pods are still not in the `Ready` state after deleting and re-creating them, remove and replace the infrastructure hosts that the unhealthy pods are on with `ibmcloud worker rm` and `ibmcloud sat host assign`.
    
    If you remove ODF nodes, you must also run the OSD removal command for that node. For more information, see [Cleaning up ODF](/docs/openshift?topic=openshift-ocs-manage-deployment&interface=ui#odf-cleanup)
    {: important}

    
