# Configure Istio Components

In this lab, we enable Istio's automatic sidecar injection for one k8s namespace, we create and configure mandatory Istio components, and allow mesh external traffic to leave the service mesh.

## Data needed
* Dynatrace environment URL
* PaaS Token

## Steps

1. Enable Istio-Sidecar-injection, that will automatically inject Envoy container into application pods that are running in a namespace that is labeled with `istio-injection=enabled`

    ```
    (bastion)$ kubectl label namespace production istio-injection=enabled
    ```

    Ensure that the label `istio-injection` has only been applied to the production namespace.

    ```
    (bastion)$ kubectl get namespace -L istio-injection
    NAME           STATUS    AGE       ISTIO-INJECTION
    cicd           Active    10d
    default        Active    16d
    dev            Active    10d
    dynatrace      Active    3d
    istio-system   Active    9d        disabled
    kube-public    Active    16d
    kube-system    Active    16d
    production     Active    9d        enabled
    staging        Active    10d
    ```

1. Istio requires a `Gateway`, that is a load balancer at the edge of the mesh receiving incoming HTTP and TCP connections.

    ```
    (bastion)$ kubectl apply -f repositories/k8s-deploy-production/istio/gateway.yml
    ```

    `gateway.yml`:
    ```
    apiVersion: networking.istio.io/v1alpha3
    kind: Gateway
    metadata:
    name: sockshop-gateway
    spec:
    selector:
        istio: ingressgateway
    servers:
    - port:
        number: 80
        name: http
        protocol: HTTP
        hosts:
        - "*"
    ```

1. You can see the external IP address of your `Gateway` executing this command.

    ```
    (bastion)$ kubectl get svc istio-ingressgateway -n istio-system
    NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                      AGE
    istio-ingressgateway   LoadBalancer   172.21.109.129   1xx.2xx.10.12x  80:31380/TCP,443:31390/TCP,31400:31400/TCP   17h
    ```

1. A `VirtualService` can be bound to a `Gateway` to control the forwarding of traffic.

    ```
    (bastion)$ kubectl apply -f repositories/k8s-deploy-production/istio/virtual_service.yml
    ```

    `virtual_service.yml`:
    ```
    apiVersion: networking.istio.io/v1alpha3
    kind: VirtualService
    metadata:
    name: sockshop
    spec:
    hosts:
    - "*"
    gateways:
    - sockshop-gateway
    http:
    - route:
        - destination:
            host: front-end.production.svc.cluster.local
            subset: v1
    ```

1. At a later point in time, we'll have two different versions of the `front-end` service deployed. To prepare for this situation, we'll define `DestinationRule`s for both versions. A `DestinationRule` defines policies that apply to traffic intended for a service after a routing has occured.

    ```
    (bastion)$ kubectl apply -f repositories/k8s-deploy-production/istio/destination_rule.yml
    ```

    `destination_rule.yml`:
    ```
    apiVersion: networking.istio.io/v1alpha3
    kind: DestinationRule
    metadata:
    name: front-end-destination
    spec:
    host: front-end.production.svc.cluster.local
    subsets:
    - name: v1
        labels:
        version: v1
    - name: v2
        labels:
        version: v2
    ```

1. We'll define `ServiceEntry`s that add entries to Istio's internal service registry and allow for connection to services that are external to the service mesh, e.g. mesh-external hosted databases for the `carts` and `orders` service.

    ```
    (bastion)$ kubectl apply -f repositories/k8s-deploy-production/istio/service_entries_sockshop.yml
    ```

    `service_entries_sockshop.yml`:
    ```
    ---
    apiVersion: networking.istio.io/v1alpha3
    kind: ServiceEntry
    metadata:
    name: catalogue-db
    spec:
    hosts:
    - sockshop-catalogue-db.ckbkxcwrvff7.eu-west-1.rds.amazonaws.com
    ports:
    - number: 3306
        name: tcp
        protocol: TCP
    resolution: DNS
    location: MESH_EXTERNAL
    ---
    apiVersion: networking.istio.io/v1alpha3
    kind: ServiceEntry
    metadata:
    name: carts-orders-db
    spec:
    hosts:
    - "*.mongodb.net"
    addresses:
    - 18.214.93.113
    - 18.215.19.51
    - 54.82.134.106
    ports:
    - number: 27017
        name: tcp
        protocol: TCP
    location: MESH_EXTERNAL
    ```

1. Last but not least, we need to configure `ServiceEntry`s for the Dynatrace OneAgent, so the language specific components that run inside the pods can communicate with the Dynatrace servers.

    In order to do this, run the script in k8s-deploy-production.
    
    ```
    ./istioconfig.sh <TENANT URL> <PAAS TOKEN>
    E.G. ./istioconfig.sh dhgf1234.live.dynatrace.com gjhs84539gjdf34
    ```
    
    Ensure ```service_entry_oneagent.yml``` is filled in with your tenant and active gate component links.

    ```
    (bastion)$ pwd
    ~/repositories/k8s-deploy-production
    (bastion)$ nano istio/service_entry_oneagent.yml
    ```

    Apply the configuration.

    ```
    (bastion)$ pwd
    ~/repositories/k8s-deploy-production
    (bastion)$ kubectl apply -f istio/service_entry_oneagent.yml
    ```

    If there is issues, please refer to [Istio Manual Config](./IstioManualConfig.md)
---

[Previous Step: Install Istio](../1_Install_Istio) :arrow_backward: :arrow_forward: [Next Step: Deploy to Production](../3_Deploy_to_production)

:arrow_up_small: [Back to overview](../)
