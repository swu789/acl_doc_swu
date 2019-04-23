# Clone GitHub Repositories

In this lab, you will clone the GitHub repositories we've forked in lab 2 to your local environment. Then, you can edit those files in Visual Studio Code or any other editor of your choice.

## Data needed
* GitHub organization
* GitHub user and password

## Steps
1. Clone all projects from the GitHub organization you've created earlier to your local machine:

    ```
    ./cloneall.sh <ORGANIZATION>
    ```
    
    which will git clone these repos from your GitHub organization:
    
    ```
    carts
    catalogue
    front-end
    k8s-deploy-staging
    k8s-deploy-production
    orders
    payment
    queue-master
    shipping
    user
    ```

1. This concludes the lab

---

[Previous Step: Trigger Build Pipelines](../5_Trigger_Build_Pipelines) :arrow_backward:

:arrow_up_small: [Back to overview](../)
