---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my Ingress in a warning state?
{: #ts-degraded-ingress}



When you look at the status of your Ingress setup, you see an error message similar to the following example.
{: tsSymptoms}

```sh
ibmcloud ks ingress status --cluster <cluster_name_or_ID>
```
{: pre}

```sh
OK
                     
Ingress Status:   warning   
Message:          One or more routers are unhealthy. See http://ibm.biz/ingress-router-ts  
```
{: screen}

When you describe the default Ingress controller, you see that the Ingress operator is in a degraded state.

```sh
oc describe ingresscontroller default -n openshift-ingress-operator
```
{: pre}

```sh
....   
Message:               One or more other status conditions indicate a degraded state: LoadBalancerReady=False (LoadBalancerPending: The LoadBalancer service is pending)
Reason:                DegradedConditions
Status:                True
Type:                  Degraded
```
{: screen}


By default, {{site.data.keyword.satellitelong_notm}} sets up a default {{site.data.keyword.redhat_openshift_notm}} router in your cluster and exposes this router by using a load balancer service. Because {{site.data.keyword.satelliteshort}} does not own and control the host infrastructure, no load balancer can be created automatically, leading to the load balancer service to remain in a pending state and the Ingress operator to report a degraded state. However, Ingress functionality to your cluster continues to work.
{: tsCauses}

A degraded Ingress operator can prevent you from installing or updating other {{site.data.keyword.redhat_openshift_notm}} operators in your cluster.


While you can't fix the warning state of your Ingress, you can update the Ingress controller so that it no longer reports a degraded state. You can then install or update other {{site.data.keyword.redhat_openshift_notm}} operators in your cluster.
{: tsResolve}

1. Get the configuration for your existing Ingress controller in your cluster and save this configuration to a YAML file on your local machine.
    ```sh
    oc get ingresscontroller default -n openshift-ingress-operator -o yaml > ingress.yaml
    ```
    {: pre}

2. With your preferred editor, open the file and remove the following blocks:
    - `annotations`
    - `creationTimestamp`
    - `managedFields`
    - `status`
    - `finalizers`
    - `generation`
    - `resourceVersion`
    - `selfLink`
    - `uid`

    
    In addition, change the `endpointPublishingStrategy` to `Private` instead of `LoadBalancerService`. Your final YAML file looks similar to the following example.
    
    ```yaml
    apiVersion: operator.openshift.io/v1
    kind: IngressController
    metadata:
      name: default
      namespace: openshift-ingress-operator
    spec:
      defaultCertificate:
        name: mycluster-97db6e22bd363f060fa80637cd5e2463-0000
      endpointPublishingStrategy:
        type: Private
      nodePlacement:
        tolerations:
        - key: dedicated
          value: edge
    ```
   {: screen}

3. Scale down the IngressController operator.
    ```sh
    oc scale --replicas=0 deployment/ingress-operator -n openshift-ingress-operator
    ```
    {: pre}

4. Remove the finalizer from the current default Ingress controller.
    ```sh
    oc patch ingresscontroller default -n openshift-ingress-operator -p '{"metadata":{"finalizers":null}}' --type=merge
    ```
    {: pre}

5. Remove the existing Ingress controller from your cluster.
    ```sh
    oc delete ingresscontroller default -n openshift-ingress-operator
    ```
    {: pre}
    
6. Re-create the Ingress controller by using the YAML file that you created earlier. Note that you must re-create the Ingress controller before {{site.data.keyword.redhat_openshift_notm}} can automatically re-create the controller with the old configuration.
    ```sh
    oc apply -f ingress.yaml
    ```
    {: pre}

7. Scale up the IngressController operator.
    ```sh
    oc scale --replicas=1 deployment/ingress-operator -n openshift-ingress-operator
    ```
    {: pre}



8. Verify that the Ingress controller is not in a degraded state anymore.
    ```sh
    oc describe ingresscontroller default -n openshift-ingress-operator 
    ```
    {: pre}

    Example output
    ```sh
     ...
     Reason:                IngressControllerUnavailable
     Status:                False
     Type:                  Available
     Last Transition Time:  2021-11-04T21:13:37Z
     Status:                False
     Type:                  Degraded
    ```
    {: screen}
    
    
    
    
