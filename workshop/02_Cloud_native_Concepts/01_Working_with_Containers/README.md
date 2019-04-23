# Working with Container Images and Containers

In this lab you'll learn how to create a container image and how to run a container based on this image. During this activity you will pull an image from a global container registry to get hands-on experience with container registries in general. 

## Step 1: Create a Dockerfile

1. Create a file `Dockerfile` and open it using an editor.
    ```
    (local)$ cd ~/acl-repo/01_Working_with_Containers
    (local)$ vi Dockerfile
    ```

1. Copy the following snippet into the `Dockerfile` and save it.
    ```
    FROM ubuntu:latest 
    COPY . /app 
    WORKDIR /app  
    CMD ["hello_world.sh"]
    ```

## Step 2. Build and Tag a Container Image

1. Build the container image (`-t` specifies the repository and a tag).
    ```
    (local)$ docker build -t acm-workshop/hello-world:latest .
    ```

1. (optional) Set another tag.
    ```
    (local)$ docker tag acm-workshop/hello-world:latest acm-workshop/hello-world:0.0.1 
    ```

1. List all container images on your local machine.
    ```
    (local)$ docker images
    ```

## Step 3. Run a Container

1. Run the container based on a container image.
    ```
    (local)$ docker run acm-workshop/hello-world:latest
    ```

## Step 4. Pull a Container Image from a Container Registry

1. Pull a container image from a container registry (*hub.docker.com* by default).
    ```
    (local)$ docker pull dynatraceacm/ticketmonster-load-generator:latest
    ```

1. Run the pulled container image (`-d` runs the container in detached mode).
    ```
    (local)$ docker run --rm dynatraceacm/ticketmonster-load-generator /bin/bash loadgeneration.sh http://ticketmonster-ui-v1-project-0.54.91.71.206.xip.io 10 15
    ```
