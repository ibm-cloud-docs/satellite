---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud, edge, use case, machine learning

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Edge environments for AI, IoT, and machine learning
{: #edge-usecase}

Create an {{site.data.keyword.satellitelong}} location with {{site.data.keyword.redhat_openshift_notm}} clusters on compute infrastructure that is deployed at the edge near your Internet of Things (IoT) devices. Then, through your {{site.data.keyword.satelliteshort}} location, your apps can access a suite of {{site.data.keyword.cloud_notm}} artificial intelligence (AI) and machine learning services to maximize the value of your data wherever the data is located.
{: shortdesc}

## Solving common edge workload challenges with {{site.data.keyword.cloud_notm}}
{: #edge-challenges}

Common challenges of edge workloads include training the machine learning models and using predictive model inferencing. With {{site.data.keyword.satelliteshort}}, you can access the {{site.data.keyword.cloud_notm}} services that addresses these challenges where your edge workloads actually run.
{: shortdesc}

Training a machine learning model
:    Training your machine learning model typically involves significant compute resources for memory, GPU, and storage. Instead of installing and managing training model software onto your compute infrastructure, you can add the compute infrastructure to a {{site.data.keyword.satelliteshort}} location. Then, you can access {{site.data.keyword.cpd_full}}, which includes tools such as {{site.data.keyword.DSX_short}} and {{site.data.keyword.pm_full}} for data analysis and model training. By accessing these tools as cloud services, you simplify the installation and management of the software. You also can use these same cloud services across all your edge infrastructure, no matter the underlying infrastructure provider.

Model inferencing
:    Model inferencing is the task of using a trained model to make predictions, detect anomalies, and categorize data from your edge environment. Because of memory, storage, and latency requirements, model inferencing is most effectively run as near to your IoT sensors and other data sources as possible. You can create a {{site.data.keyword.satelliteshort}} location with managed {{site.data.keyword.redhat_openshift_notm}} clusters right where your data is located in your edge environments. Then, you can set up a serverless programming model such as Red Hat&trade; OpenShift&trade; Serverless&trade; to provide a simplified programming model with a REST interface to query your trained model to produce a prediction.


## Setting up your edge solution with {{site.data.keyword.satelliteshort}}
{: #edge-solution}

While you can set up many possible solutions to the challenges of your edge environment, you can use {{site.data.keyword.satelliteshort}} to provide a consistent, scalable experience across environments. An example setup is as follows.
{: shortdesc}

1. Set up machine learning and model training for your data.
2. Deploy {{site.data.keyword.satelliteshort}} with a serverless component to your edge environment.
3. Run model inferencing at the edge.

### Step 1: Set up machine learning and model training for your data
{: #edge-example-ml}

As an AI model developer, you prepare your edge data with machine learning and AI tools in {{site.data.keyword.cloud_notm}}. Before you begin, you must have access to {{site.data.keyword.DSX}} and {{site.data.keyword.pm_short}} instances, such as in an {{site.data.keyword.cloud_notm}} account or through {{site.data.keyword.cpd_full_notm}}.
{: shortdesc}

1. Upload the training data to [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).
2. Use [{{site.data.keyword.DSX}}](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/data-science.html){: external} and [{{site.data.keyword.pm_short}}](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml-overview.html){: external} to pull the training data from {{site.data.keyword.cos_full_notm}}, analyze the data, and train a model with TensorFlow, Keras, SciKit-Learn, or another popular machine learning algorithm. 

The trained model is saved back to {{site.data.keyword.cos_full_notm}}, so that the data does not take up storage space in your edge environment.

### Step 2: Deploy {{site.data.keyword.satelliteshort}} with a serverless component to your edge environment
{: #edge-example-serverless}

As the edge environment system administrator, you enable a serverless tool to simplify model inferencing at the edge.
{: shortdesc} 

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations) on your edge computing infrastructure.
2. [Create a managed {{site.data.keyword.redhat_openshift_notm}} cluster](/docs/satellite?topic=openshift-satellite-clusters) in the {{site.data.keyword.satelliteshort}} location.
3. [Access the {{site.data.keyword.redhat_openshift_notm}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).
4. Using the OperatorHub, [install the {{site.data.keyword.redhat_openshift_notm}} Serverless operator](https://www.redhat.com/en/topics/microservices/why-choose-openshift-serverless){: external}.
5. [Install the Knative Serving Operator](https://www.redhat.com/en/topics/microservices/why-choose-openshift-serverless){: external}.

You deployed {{site.data.keyword.satelliteshort}} with a serverless component.

### Step 3: Run model inferencing at the edge
{: #edge-example-inferencing}

As the AI developer, run model inferencing on your edge data by using the serverless processing that the edge administrator set up.
{: shortdesc}

1. Download the trained model from {{site.data.keyword.cos_full_notm}} to your local development environment.
2. Create a [Knative-compliant container image](https://knative.dev/docs/serving/samples/hello-world/helloworld-python/index.html){: external}.
3. [Deploy the image](https://developers.redhat.com/blog/2020/04/30/serverless-applications-made-faster-and-simpler-with-openshift-serverless-ga){: external} to your {{site.data.keyword.redhat_openshift_notm}} Serverless processor that runs in your {{site.data.keyword.satelliteshort}} cluster. You can use the {{site.data.keyword.redhat_openshift_notm}} web console in the developer perspective, or use the `kn` command line tool for {{site.data.keyword.satelliteshort}} Serverless.

Now, you have a managed {{site.data.keyword.satelliteshort}} location that runs on your edge environment and performs model inferencing on demand for your edge data through your AI-trained model and {{site.data.keyword.redhat_openshift_notm}} Serverless.
