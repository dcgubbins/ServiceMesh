# MyHero: A Microservices Application

In order to demonstrate the power of Istio we will install a microservices applications called MyHero.

Once installed we will then look at how Istio can be used to manage traffic to and from different microservices

MyHero is a simple app that allows you to vote for your favourite superhero. It is comprised of three services. Presentation, APP and Data. Each service communicates via API.

![alt text][myhero_1]

[myhero_1]: Istio_DNE_Images/myhero_1.png "MyHero"

Each service is defined by a manifest file. When deployed onto the kubernetes(k8s) cluster the Data and APP services comprise of one pod each and the Presentation service three pods.

![alt text][myhero_2]

[myhero_2]: Istio_DNE_Images/myhero_2.png "MyHero2"

myhero could work happily leveraging the native K8s networking and load balancing services. However, we are want to add the Service Mesh abstraction istio, so we need another set of manifest files to get the app up and running.

![alt text][myhero_istio_1]

[myhero_istio_1]: Istio_DNE_Images/myhero_istio_1.png "MyHero2"

When Istio is enabled wihtin a k8s namespace, it automatically installs a second container (the envoy sidecar). This sidecar proxies all traffic in and out of the pod and in so doing provides a policy control point.

In our initial myhero deployment the Istio manifest files define a number of traffic management services.

A __Gateway__ configures a load balancer for HTTP/TCP traffic, most commonly operating at the edge of the mesh to enable ingress traffic for an application.

A __VirtualService__ defines the rules that control how requests for a service are routed within an Istio service mesh.

A __DestinationRule__ configures the set of policies to be applied to a request after VirtualService routing has occurred.

With the Istio traffic management in place we can now control traffic to and between our application services.

OK. Now we understand the basics lets start the lab2 and install myhero.
