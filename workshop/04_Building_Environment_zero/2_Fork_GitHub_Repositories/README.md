# Fork GitHub Repositories

## Data needed
* GitHub user and email address and password

## Steps
1. Create a new GitHub organization for your source code repositories. Go to https://github.com, login, and create a new organization by clicking the + icon in the upper right :one: corner and selecting "New Organization" :two:.

    ![](../assets/new-org.png)

1. Provide a unique name :one: - the form tells you if the name you've chosen is already taken or not. Provide your email address as billing email address :two: - no worries, this is free of charge, so there won't be any costs. At the bottom of this screen click "Create organization" :three:.

    ![](../assets/org-detail1.png)
    ![](../assets/org-detail2.png)

1. On the next screen scroll down to the bottom and click on the "skip this step" link :one:.

    ![](../assets/skip-details.png)
    
1. Now you're redirected to the homepage of your new organization.

    ![](../assets/org-ready.png)

---

1. Connect to the bastion host ussing ssh and your credentials.

1. Configure your git user name and email address in the commandline on the bastion host.

    ```
    (bastion)$ git config --global user.name "<GitHubUser>"
    (bastion)$ git config --global user.email "<GitHubEmail>"
    ```

1. Execute the `~/forkGitHubRepositories.sh` script in your home directory. This script takes the name of the GitHub organization you have created earlier.

    ```
    (bastion)$ chmod +x forkGitHubRepositories.sh
    (bastion)$ ./forkGitHubRepositories.sh <GitHubOrg>
    ```

    This script `clone`s all needed repositories and the uses the `hub` command ([hub](https://hub.github.com/)) to fork those repositories to the passed GitHub organization. After that, the script deletes all repositories and `clone`s them again from the new URL.

1. Ensure that all repositories have been cloned in the `~/repositories' folder on the bastion host - we need them later.

    ```
    (bastion)$ ls -1 ~/repositories/
    carts
    catalogue
    front-end
    k8s-deploy-production
    k8s-deploy-staging
    orders
    payment
    queue-master
    shipping
    sockshop-infrastructure
    user
    ```

---

[Previous Step: Check Prerequisites](../1_Check_Prerequisites) :arrow_backward: :arrow_forward: [Next Step: Deploy Docker Registry](../3_Deploy_Docker_Registry)

:arrow_up_small: [Back to overview](../)
