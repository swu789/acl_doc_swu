# Deep Dive into the Microservice Carts

In this lab you'll first investigate the code structure of a microservice including all artifacts that are needed to build, containerize, and to deploy a service. Based on a solid understanding of the microservice, you will deploy it to a Kubernetes cluster to see the service in action. 

## Step 1: Familiarize with Microservice from a Code Perspective

1. The source code of carts is in `carts/src/`.
    * `main` - Contains the Java sources and the Application properties. 
    * `test` - Contains all unit tests to test the application logic. 

1. The `carts/pom.xml` file is used from Maven to build the Java artifact based on all dependencies. 

1. The `carts/version` file holds the current version of the microservice.

1. The `carts/Dockerfile` specifies the *Container Image* to run the microservice in a container.

1. The `carts/Jenkisfile` specifies the *Jenkins Pipeline* to build, test, deploy the microservice on a Kubernetes Cluster.

## Step 2. Familiarize with the Deployment and Service Specification

1. Open the file `carts/manifest/carts.yml`.

1. Consider the **Deployment** section.
    * Identify the **Container Image** definition.
    * Identify the **Resource** definition.
    * Identify the **Liveness and Readiness Probe** definition.

1. Consider the **Service** section.
    * Identify the **Port** definition.

## Step 3. Deploy Carts on Kubernetes Cluster

1. Copy the `manifest` folder to `lab-manifest`.

1. Open `carts.yml` and change namespace from `dev` to `lab-dev` (1x in deployment and 1x in service specification).

1. Add the namespace specification on top of `carts.yml`.
    ```
    ---
    apiVersion: v1
    kind: Namespace
    metadata:
      name: lab-dev
    ```

1. Add a LoadBalancer to the service specification (on the bottom) and save file.
    ```
    spec:
      ports:
      - name: http
        port: 80
        targetPort: 8080
      selector:
        app: carts
      type: LoadBalancer
    ```

1. Specify the container image at spec > containers > image. (Attention, ask instructor for carts version.)
    ```
    image: <your container registry>/library/sockshop/carts-<version>
    ```

1. Run kubectl apply command from `.\carts` directory.
    ```
    (bastion)$ kubectl apply -f lab-manifest
    ```

1. Check all ressources that have been created.
    ```
    (bastion)$ kubectl get deployments,pods,services -n lab-dev
    ```

1. Retrieve the external IP of your carts service and open it in a browser by adding the `/health` endpoint.

1. Delete all resources that have been created.
    ```
    (bastion)$ kubectl delete namespace lab-dev
    ```

1. Delete `lab-manifest` folder.

---

:arrow_forward: [Next Step: Deploy Microservice to Dev](../02_Deploy_Microservice_to_Dev)

:arrow_up_small: [Back to overview](../)
