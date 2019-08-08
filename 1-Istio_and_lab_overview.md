![alt text][logo]

[logo]: Istio_DNE_Images/Istio_banner.png "Logo Title Text 2"
# ISTIO LEARNING LAB
<br>
### Prerequisites
Before you attempt this lab it will make much more sense if you have already completed the following modules:
1.	Cisco Container Platform
2.	Intro to Kubernetes (K8s)
3.	Completed setup and have Kubernetes CLI (kubectl)

## Objectives

Total Completion Time: 90 minutes

1. LAB 1: Setup Kubernetes on the CentOS machine
2. LAB 2:Instal myhero Application
3. Mission: Istio Traffic Routing
4. Mission: Istio Gradual Migration
5. Mission: Istio Delay Injection
6. Mission: Istio HTTP Abort Injection

In this lab you will learn the basics of using Istio service mesh on a microservices. BUt before we start let's understand what problem are we solving with Service mesh.

# A Brief History of Microservices and the Service mesh

In the "olden days" services such as routing, traffic management, load balancing and security were delivered by discrete hardware or software appliances. In most cases applications would leverage these services as traffic flowed across the network

![alt text][Service_Mesh_olddays]

[Service_Mesh_olddays]: Istio_DNE_Images/Service_Mesh_olddays.png "Olden_Days"

Todays modern applications are container based, built as microservices, and typically deployed on container orchestrators like kubernetes. The old way of delivering services to apps no longer works or scales for modern hardware and software stacks.

![alt text][Service_Mesh_missinglayer1]

[Service_Mesh_missinglayer1]: Istio_DNE_Images/Service_Mesh_missinglayer1.png "Missing_Layer"

A microservices architecture differs from other architectures in that individual microservices are built independently, loosely coupled and communicate with each other to provide the overall service/application.

The logic governing communication between services can be coded into each service but it can quickly become complex and an overhead for the developer.

A service mesh is an abstraction that takes the logic of service-to-service communication away from individual services and abstracts it to a layer of infrastructure.

![alt text][Service_Mesh_Istio]

[Service_Mesh_Istio]: Istio_DNE_Images/Service_Mesh_Istio.png "ISTIO"

There are a number of Services mesh solutions in the market, the current leader is Istio, an open source project from Google.

Istio is an open platform-independent service mesh
Open. (although Platform-independent initial development supports Kubernetes).

![alt text][Service_Mesh_Istio_Services]

[Service_Mesh_Istio_Services]: Istio_DNE_Images/Service_Mesh_Istio_Services.png "ISTIO"

Istio makes it easy to create a network of deployed services.

You add Istio support to services by deploying a special sidecar proxy throughout your environment that intercepts all network communication between microservices, then configure and manage Istio using its control plane functionality, which includes:

* Fine-grained control of traffic behavior
* Automatic load balancing
* Automatic metrics, logs, and traces
* Secure service-to-service communication

Istio is made up from a number of components.

* Pilot is used for Traffic Management
* Envoy Proxy is the High Performance Proxy. (Sidecar container)
* Mixer delivers Policy and Telemetry
* Citadel provides Identity and Credential Management

![alt text][Service_Mesh_Istio_Architecture]

[Service_Mesh_Istio_Architecture]: Istio_DNE_Images/Service_Mesh_Istio_Architecture.png "ISTIO"

For more information on Istio please take a look here [https://istio.io/docs/concepts/what-is-istio/]
