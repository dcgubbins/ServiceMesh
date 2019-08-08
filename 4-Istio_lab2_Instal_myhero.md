# ISTIO LEARNING LAB

![alt text][logo]

[logo]: Istio_DNE_Images/istio_banner.png "Logo Title Text 2"

<!-- TOC -->
## Istio Service Mesh#

In this lab you will install a microservices application called Myhero

## Objectives
Completion Time: 20 minutes

Enable Istio and Install the myhero Application


# Lab 3: Install MyHero
There are few steps we need to complete in order to Install our Application.

1. Create a myhero namespace and enable Istio
2. Git clone the application and Istio manifiest files
3. Identify the Istio system ingressgateway IP
4. Update the manifests with the gateway IP
5. Deploy the myhero app and test

## Create MyHero namespace
We need to create a namespace into which our application will be deployed. We will use the name 'myhero' for our namespace.

We also need to enable Istio in the namespace which will ensure all PODs deployed get an associated Istio sidecar.

We will use the Kubernetes Cli (Kubectl) to complete the next steps.

Create a new name on our Kubernetes cluster
```
kubectl create namespace myhero
```
Enable Istio within that namespcace
```
kubectl label namespace myhero istio-injection=enabled
```
Check Istio is enabled for the myhero namespce
```
kubectl get namespace -L istio-injection
```
## Find the Istio ingressgateway IP Address

Istio automatically installs a number of components at installation. One of these components is an ingressgateway which has an external IP address assigned to it. We will use this gateway IP for our application external communications. To find the ingressgateway IP address issue the following command:

```
kubectl -n istio-system get svc istio-ingressgateway
```

Example output below
![alt text][kubectl_get_istio_gateway_ip]

[kubectl_get_istio_gateway_ip]:Istio_DNE_Images/kubectl_get_istio_gateway_ip.png

## Create MyHero Manifests
All application and service deployments are made using manifest files. These files define your application and associated components. All the manifest you need to complete the labs in this module are stored on Github. You need to clone the repo to you local machine.

The Manifests can be found here

```bash
cd ~/containers-dne-labs/labs/containers-intro-istio/myhero_Templates
```

You will now have all the manifest files you need.
Some of the the manifests are currently templates as we need to make a change to a few of them adding an IP address.

We now need to update our Istio and Kubernetes manifests with this IP address and once edited we need to save them as .yml files.

The files that need editing are located in the Istio_Policies_Templates and myhero_Templates folders:

`Istio_myhero_app.template`<br>
`Istio_myhero_gateway.template`<br>
`Istio_myhero_UI_rule.template`<br>
`K8s_myhero_ui.templates`<br>

To edit the files use your favourite text editor.
The example below uses [ATOM](https://atom.io/)
![alt text][Istio-manifest-edit-example]

[Istio-manifest-edit-example]:Istio_DNE_Images/Istio-manifest-edit-example.png

Replace <Istio-ingressgateway-IP> with the IP address you recorded in the previous step.

e.g `app.<istio-ingressgateway-IP>xip.io` becomes `app.10.10.20.208.xip.io`

(Note that we are using an external DNS service xip.io to mitigate some Sandbox nuances)

***Don't forget to save your updated manifest files with the .yml extension.***

## Deploy the app
So now we have all out manifest updated we can deploy the app.
You can deploy either via the K8s dashboard or kubectl Cli.

Example Deploying via dashboard
![alt text][k8s_dash_create]

[k8s_dash_create]:Istio_DNE_Images/k8s_dash_create.png

```
kubectl -n myhero appy -f istio-myhero-app.yml
```

***Either way check you are deploying into the "myhero" namespace***

If you take a look at the new pods, you will immediately notice that they include 2 containers, instead of just 1 as before (ie. READY 2/2):

```
kubectl -n myhero get pods
```
You will be able to access myhero public IP address from your own browser and see the application working. Those new sidecar proxy containers that are intercepting all I/O are transparent to the service.

If it's working you will see the following screen.
![alt text][myhero_working]

[myhero_working]:Istio_DNE_Images/myhero_working.png

Vote for your favourite superhero to check the backend is working.

If you see this, then you are done.
![alt text][myhero_backend]

[myhero_backend]:Istio_DNE_Images/myhero_backend.png

Well Done You. Move onto the next Exercise
