.. highlight:: shell

Prerequisites
=============

Kubernetes
----------

mlbench uses `Kubernetes <https://kubernetes.io/>`_ as basis for the distributed cluster. This allows for easy and reproducible installation and use of the framework on a multitude of platforms.

Since mlbench manages the setup of nodes for experiments and uses Kubernetes to monitor the status of worker pods, it needs to be installed with a service-account that has permission to manage and monitor ``Pods`` and ``StatefulSets``.

Additionally, `helm` requires a kubernetes user account with the ``cluster-admin`` role to deploy applications to a kubernetes cluster.

.. _google-cloud:

Google Cloud
^^^^^^^^^^^^

For Google Cloud see: `Creating a Cluster <https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-cluster>`_ and `Kubernetes Quickstart <https://cloud.google.com/kubernetes-engine/docs/quickstart>`_.

If you're planning to use GPUs in your cluster, see the `GPUs <https://cloud.google.com/kubernetes-engine/docs/how-to/gpus>`_ article, especially the "Installing NVIDIA GPU device drivers" section.

When creating a GKE cluster, make sure to use version ``1.10.9`` or above of kubernetes, as there is an issue with DNS resolution in earlier version. You can do this with the ``--cluster-version=1.10.9``
flag for the ``gcloud container clusters create`` command.

Make sure credentials for your cluster are installed correctly (use the correct zone for your cluster):

.. code-block:: bash

   gcloud container clusters get-credentials ${CLUSTER_NAME} --zone us-central1-a
   
AWS
^^^ 

For AWS see: `Installing Kubernetes on AWS with kops <https://kubernetes.io/docs/setup/custom-cloud/kops/>`_ or `Getting started with Amazon EKS <https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html>`_. 

To add GPU worker nodes to the cluster, refer to Setting up GPU enabled Kubernetes with kops (`1 <https://gist.github.com/stevenc81/1cad3a0ebca9303923d1cd4c3641f8bc>`_, `2 <https://medium.com/@amimahloof/running-gpu-enabled-instances-on-kubernetes-kops-aws-via-nvidia-device-plugin-cef527abea12>`_), `How to Automate Deep Learning Training with Kubernetes GPU-Cluster <https://github.com/Langhalsdino/Kubernetes-GPU-Guide>`_, or `GPUs & Kubernetes for Deep Learning <https://hackernoon.com/gpus-kubernetes-for-deep-learning-part-1-3-d8eebe0dd6fe>`_.

Helm
----

Helm charts are like recipes to install Kubernetes distributed applications. They consist of templates with some logic that get rendered into Kubernetes deployment `.yaml` files
They come with some default values, but also allow users to override those values.

Helm can be found `here <https://github.com/helm/helm/>`_

Helm needs to be set up with service-account with ``cluster-admin`` rights:

.. code-block:: bash

   kubectl --namespace kube-system create sa tiller
   kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
   helm init --service-account tiller
