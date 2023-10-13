# Protect your High Availability workloads with Load Balancer and Cloud Armor

## Let's get started

This solution assumes you already have a project created and set up where you wish to host these resources. If not, and you would like for the project to create a new project as well,  please refer to the [github repository](https://github.com/GoogleCloudPlatform/cloud-foundation-fabric/tree/master/blueprints/data-solutions/gcs-to-bq-with-least-privileges) for instructions.

**Time to complete**: About 10 minutes

Click the **Start** button to move to the next step.

## Prerequisites

* Have an [organization](https://cloud.google.com/resource-manager/docs/creating-managing-organization) set up in Google cloud.
* Have a [billing account](https://cloud.google.com/billing/docs/how-to/manage-billing-account) set up.
* Have an existing [project](https://cloud.google.com/resource-manager/docs/creating-managing-projects) with [billing enabled](https://cloud.google.com/billing/docs/how-to/modify-project).

### Roles & Permissions

In order to spin up this architecture, you will need to be a user with the “__Project owner__” [IAM](https://cloud.google.com/iam) role on the existing project:

Note: To grant a user a role, take a look at the [Granting and Revoking Access](https://cloud.google.com/iam/docs/granting-changing-revoking-access#grant-single-role) documentation.

## Deploy the architecture

Before we deploy the architecture, you will need the following information:

* The __project ID__

Click on the button below, sign in if required and when the prompt appears, click on “confirm”. It will walk you through setting up your architecture.

[![Open in Cloud Shell](https://gstatic.com/cloudssh/images/open-btn.svg)](https://shell.cloud.google.com/cloudshell/editor?cloudshell_git_repo=https://github.com/GoogleCloudPlatform/deploystack-google-lb-and-armor&cloudshell_image=gcr.io%2Fds-artifacts-cloudshell%2Fdeploystack_custom_image&cloudshell_git_branch=main)

## Result

<center>
<h4>🎉 Congratulations! 🎉 </h4><br/>
At this point you should have successfully deployed the foundations to protect your High Availability workloads with Load Balancer and Cloud Armor.</center>

Next we are going to test the architecture and finally clean up your environment.

## Testing your architecture

1. Connect to the siege VM using SSH (from Cloud Console or CLI) and run the following command:

        siege -c 250 -t150s http://$LB_IP

2. In the Cloud Console, on the Navigation menu, click __Network Services > Load balancing__.
3. Click __Backends__, then click __http-backend__ and navigate to __http-lb__
4. Click on the __Monitoring__ tab.
5. Monitor the Frontend Location (Total inbound traffic) between North America and the two backends for 2 to 3 minutes. At first, traffic should just be directed to __us-east1-mig__ but as the RPS increases, traffic is also directed to __europe-west1-mig__. This demonstrates that by default traffic is forwarded to the closest backend but if the load is very high, traffic can be distributed across the backends.
6. Now, to test the IP deny-listing, run terraform as follows:

        terraform apply -var project_id=my-project-id -var enforce_security_policy=true

This applies a security policy to denylist the IP address of the siege VM.

7. To test this, run the following command from the siege VM and verify that you get a __403 Forbidden__ error code back.

        curl http://$LB_IP

## Cleaning up your environment

The easiest way to remove all the deployed resources is to run the following command in Cloud Shell:

``` {shell}
deploystack uninstall
```

The above command will delete the associated resources so there will be no billable charges made afterwards.

<!-- BEGIN TFDOC -->

## Variables & Outputs

For full information on variables and outputs please refer to the [README](https://github.com/GoogleCloudPlatform/deploystack-google-lb-and-armor#variables) file.

## Congratulations

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

You’re all set!
