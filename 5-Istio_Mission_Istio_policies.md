# ISTIO LEARNING LAB

![alt text][logo]

[logo]: Istio_DNE_Images/istio_banner.png "Logo Title Text 2"


## Objectives
Completion Time: 30 minutes<br>
In this lab you will use Istio to control traffic routing and responses.

## Istio Policies
Our MyHero app is up an running, so now we will use Istio to manage our traffic and services.

##### Mission: Istio Traffic Routing<br>
##### Mission: Istio Delay Injection<br>
##### Mission: Istio Gradual Migration<br>
##### Mission: Istio HTTP Abort Injection<br>

First things first. We need to modify our manifest as before with the ingressgateway address.

The manifest files are stored in the `Istio_Policies_Templates` directory.<br>
Run the command `cd /Istio_Policies_Templates` to move to the directory.

The manifest files we need to modify are:

- 0-ui_initial_status.template<br>
- 1-ui_all_to_v1.template<br>
- 2-ui_inject_delay.template<br>
- 3-app_HTTP_abort.template<br>
- 4-ui_50_to_v3.template<br>
- 5-ui_v1_mirror_to_v2.template<br>


## Mission 1: Traffic Routing
In this exercise we will use Istio to route all requests to our V1 pod only. To apply this policy issue the following command.

```
kubectl -n myhero apply -f 1-ui_all_to_v1.yml
```

If you review the manifest you will notice it includes a **VirtualService** that defines a rule to route all traffic going to myhero-ui service, so that it reaches exclusively pods labelled with v1. It also includes a **DestinationRule** that defines the different available versions (subsets).

Please review the applied route (you may use this in all subsequent use cases examples):

```
kubectl -n myhero describe virtualservice myhero-ui
```

```
kubectl -n myhero describe destinationrule myhero-ui-destinationrule
```


Refresh your browser several times while pointing to myhero-ui public IP and you will notice that now you only get the v1 header: "Make ONE voice heard!!!".

This way Istio allows you to deploy several different pods in the same service, and choose which one to use as default for your users.
This is useful for Canary Testing a new version of your app.


## Mission 2: Delay Injection

Now let's do something different and inject a delay to test the resiliency of our application. For each request from our browser to myhero-ui we will inject a 5 seconds delay.

```
kubectl -n myhero apply -f 2-ui_inject_delay.yml
```

Refresh your browser and you will experience a slower response time. That way you can test how your application microservices handle unexpected response time delays. If you review the manifest you will notice that you can define what specific traffic will be affected by this rule. In our case we are using 100% of traffic to simplify the example.

Maybe you are wondering why the total delay experienced by the end user is much higher than 5 seconds and more like in the 20-25 seconds range. Well, let's dig in a little bit. From IE, Chrome or Firefox press Ctrl+Shift+I or Alt+Cmd+I to enter the Developer tools, go to tab Network and refresh your browser. You will be able to see the 5 seconds delay for each request, and the accumulated total to explain why users get a much slower response time.

Remove the injected delay:
```
kubectl -n myhero apply -f 1-ui_all_to_v1.yml
```

## Mission 3: HTTP Abort Injection

In this case we will inject a fault where myhero-app responds with HTTP abort (HTTP status 500):

```
kubectl -n myhero apply -f 3-app_HTTP_abort.yml
```

Refresh your browser and you will notice now the list of available superheroes is not there anymore. This list was provided by myhero-app, so now that it is aborting its connections, our application cannot show the list anymore.

This way Istio allows us to insert artificial faults in our microservices, and test how those faults reflect in the overall application.

Remove the artificially injected fault:
```
kubectl -n myhero apply -f istio_myhero_app_rule.yml
```

## Mission: Gradual Migration to New Version

Istio allows us to migrate to new service versions in a gradual way. Let's suppose our myhero-ui versions (v1, v2, v3) are sequential ones with new features or bug fixes.

Our initial status is all traffic goes to v1, so let's migrate for example 50% of the traffic to the newest v3.
```
kubectl -n myhero apply -f 4-ui_50_to_v3.yml
```

Refresh your browser multiple times and you will see how 50% of the time you get myhero-ui v1 header ("Make ONE voice heard!!!") and the other 50% of time you get myhero-ui v3 header ("Make THREE voices heard!!!").

This way you can smoothly migrate traffic from one service version to another, being able to define how much traffic you want directed to each service no matter what is the size of the deployment.

Once v3 is completely stable, you can easily migrate 100% of your traffic to the new version:
```
kubectl -n myhero apply -f 4-ui_all_to_v3.yml
```

## Mission 4: Mirroring Traffic

Istio also allows you to mirror traffic, maybe from a live service to a newer one out of production, so you can test how that new one behaves when it gets real traffic.

For our test let's first direct all traffic back to myhero-ui v1.

```
kubectl -n myhero apply -f 1-ui_all_to_v1.yml
```

Now open a second terminal window and leave it monitoring logs for pods belonging to the v1 version:
```
export UI_V1_POD=$(kubectl -n myhero get pod -l app=myhero,version=v1 -o jsonpath={.items..metadata.name})
```
```
kubectl -n myhero logs -f $UI_V1_POD -c myhero-ui
```

You will see that every time you refresh your browser the logging reflects that activity.

Open a third terminal window and leave it monitoring logs for pods belonging to the v2 version:

```
export UI_V2_POD=$(kubectl -n myhero get pod -l app=myhero,version=v2 -o jsonpath={.items..metadata.name})
```
```
kubectl -n myhero logs -f $UI_V2_POD -c myhero-ui
```

Here you will notice that refreshing your browser does not generate any kind of logs for the v2 window.

Now let's mirror all traffic going to v1, so that it sends a copy to all pods belonging to v2:

```
kubectl -n myhero apply -f 5-ui_v1_mirror_to_v2.yml
```

If you refresh your browser again, you will notice in your logging terminal windows that now, not only v1 receives traffic, but also v2. Both logging terminal windows will show the same events.

YOU ARE DONE MY FRIEND! Well done :)

## Uninstalling Istio

Once you are done you with your testing you might want to stop using Istio, so you have different options:

Remove the namespace label and stop associated pods, so they are automatically recreated without the sidecar proxies:
```
kubectl label namespace myhero istio-injection-
kubectl -n myhero delete pod ...
```

```
kubectl delete -f install/kubernetes/istio-demo.yaml
```
