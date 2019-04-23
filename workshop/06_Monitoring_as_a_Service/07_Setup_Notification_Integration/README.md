# Setup Alerting Profile for Availability Issues and Integrate with a ChatOps Tool

In this lab you'll learn how to define an alerting profile for a particular problem identified by Dynatrace. Based on that alerting profile, an email notification will be send to a person who is in charge of the issue. 

## Step 1: Definition of an Alerting Profile
1. Go to **Settings**, **Alerting** and click on **Alerting profiles**.
1. Specify the name for the profile, e.g., `Sockshop Error Profile`.
1. For the sake of simplicity, **Delete** all rules that are set as default. 
1. Click **Create alerting rule** and select **Error** as *problem severity level*.
1. At *Filter problems by tag* select *Only include entities that have any tags*.
1. **Create tag filter** and *Choose a tag*: `[Environment]product:sockshop`.
1. Finally, click **Done**.

## Step 2: Set up Email Notification
1. Go to **Settings**, **Integration** and click on **Problem notifications**.
1. Click on **+ Set up notifications** and select **Email**.
1. Specify the name for the notification, e.g., `Service Notification`.
1. Specify the email address of the receiver.
1. Set the following email subject: `{State} Problem {ProblemID}: {ImpactedEntity}`.
1. Select the previous created Alerting profile: `Sockshop Error Profile`.
1. Finally, **Send test notification** before clicking on **Save**.

---

[Previous Step: Deploy Sockshop to Staging](../06_Deploy_Sockshop_to_Staging) :arrow_backward:

:arrow_up_small: [Back to overview](../)
