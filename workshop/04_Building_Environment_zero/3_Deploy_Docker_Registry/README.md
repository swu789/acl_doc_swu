# Deploy Docker Registry

We use the Docker registry to save the build artifacts, i.e. Docker containers. The registry allows us to host multiple versions of an artifact. 

## Steps
1. In Preparation for deploying the Docker registry we're going to use throughout the week, we want to create some Kubernetes namespaces (`~/manifests-env-zero/k8s-namespaces.yml`):
    - `cicd`: hosts all CI/CD related pods and services
    - `dev`: represents the development stage
    - `stage`: represents the staging stage
    - `production`: represents the production stage

    ```
    (bastion)$ cd
    (bastion)$ kubectl create -f manifests-env-zero/k8s-namespaces.yml
    ```

    Expected output:

    ![](../assets/kubectl-create-namespaces.png)

1. We specify that we want to have fast SSD storage by executing the following command (`~/manifests-env-zero/k8s-storage.yml`):

    ```
    (bastion)$ kubectl create -f manifests-env-zero/k8s-storage.yml
    ```

    Expected output:

    ![](../assets/kubectl-create-storageclass.png)

1. We create a `PersistentVolumeClaim` (PVC) where Docker images of the registry will be stored. We use a PVC to have persistent data, even when the pod is restarted. 

    ```
    (bastion)$ kubectl create -f manifests-env-zero/k8s-docker-registry-pvc.yml
    ```

    Expected output:

    ![](../assets/kubectl-create-registrypvc.png)

1. We create the service and the deployment for the Docker registry.

    ```
    (bastion)$ kubectl create -f manifests-env-zero/k8s-docker-registry-service.yml
    ```
    
    Expected output:

    ![](../assets/kubectl-create-registrysvc.png)

    ```
    (bastion)$ kubectl create -f manifests-env-zero/k8s-docker-registry-deployment.yml
    ```

    Expected output: 

    ![](../assets/kubectl-create-registrydpl.png)

1. Ensure the Docker registry service, the deployment, and the corresponding pods are running. **Write down the IP address of the service** - we'll need it in the next step.

    ```
    (bastion)$ kubectl -n cicd get services
    ```

    Expected output: 

    ![](../assets/kubectl-get-services.png)

    ```
    (bastion)$ kubectl -n cicd get deployments
    ```

    Expected output: 

    ![](../assets/kubectl-get-deployment.png)

    ```
    (bastion)$ kubectl -n cicd get pods
    ```

    Expected output: 

    ![](../assets/kubectl-get-pods.png)

---

[Previous Step: Check Prerequisites](../2_Fork_GitHub_Repositories) :arrow_backward: :arrow_forward: [Next Step: Deploy Jenkins](../4_Deploy_Jenkins)

:arrow_up_small: [Back to overview](../)
