# ISTIO LEARNING LAB

![alt text][logo]

[logo]: Istio_DNE_Images/istio_banner.png "Logo Title Text 2"


## Istio Service Mesh

In this lab you will setup the Kubernetes command line tool Kubectl

## Objectives
Completion Time: 10 minutes
Setup Kubernetes CLI on the CentOS machine


# Lab 2: Setup Your New Cluster
We have a k8s cluster running from the previous module called "API Cluster 1".
 there are a few more steps to complete before we are ready to install our applications.

1. Setup Kubectl on the CENTOS machine to remotely control the cluster from your computer
2. Create a new namespace called myhero for our application
3. Enable Istio sidecar injection within the myhero namespace

## Setup Kubectl on your computer to remotely control the cluster from your computer
It is useful to run the Kubernetes CLI from a computer. This way you can switch between multiple clusters under your control.

From the CENTOS machine open a terminal window.

Take a look at your Kubernetes CLI current configuration by running:

```
Kubectl config view
```

Example output below
![alt text][kubectl_config_view_2]

[kubectl_config_view_2]:Istio_DNE_Images/kubectl_config_view_2.png "Config View"

This example shows that Kubernetes CLI is already installed and configured to use a local cluster on the laptop called minikube.

There is a Kubeconfig file behind every working Kubectl command. This file typically lives at $HOME/.kube/config.

There are three ways to point your Kubectl to a Kubeconfig:

1.  --kubeconfig flag, if specified
2.  use KUBECONFIG environment variable, if specified
3.  $HOME/.kube/config file

Above is the order of preference that takes effect while determining which Kubeconfig file is used.

For our lab we will use option 2 and point to our Kubeconfig using using an environment variable pointing to the Kubeconfig/Token you downloaded from your Kubernetes cluster in the previous lab.
```
echo $Home
```
```
export KUBECONFIG=$HOME/Downloads/kubeconfig.yaml
```

Example below
![alt text][Kubectl_switch_context]

[Kubectl_switch_context]:Istio_DNE_Images/KubeconfigENV.png "Config View"

If it is successful you should now be able to control the cluster you just built on CCP.

```
kubectl get nodes
```
You should now see the nodes of the remote cluster.

Well done. Your work here is done!
