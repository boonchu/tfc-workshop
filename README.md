# Introduction to monitoring AWS EKS with Datadog and Terraform Cloud

## Related tutorials and repositories

* [Provision an EKS Cluster (AWS)](https://developer.hashicorp.com/terraform/tutorials/kubernetes/eks)
* [Automate Monitoring with the Terraform Datadog Provider](https://developer.hashicorp.com/terraform/tutorials/applications/datadog-provider)
* [DataDog/ecommerce-workshop](https://github.com/DataDog/ecommerce-workshop/tree/main/deploy/generic-k8s/ecommerce-app)

## Prerequisites
To run this workshop code you will need the following:
1. A datadog learning center account [Free Account](https://learn.datadoghq.com/)
2. An AWS account to install a Kubernetes cluster (EKS) - Provided within the Datadog learning center lab. 
2. A Datadog account - Provided within the Datadog learning center lab. 
3. A Terraform Cloud account
4. A github account
5. Fork this repository

## Content

Each folder contains does something different.

* **aws-eks/**: Terraform configuration to define a three node cluster in EKS.
* **storedog-app/**: Terraform configuration to do the following:

  * Deploy an eCommerce application called Storedog with a Load Balancer
  * Deploy Datadog Agent on EKS cluster via helm
  * Deploy Datadog AWS Intergration & associated IAM policies.
* **datadog-resources/**: Terraform configuration to:

  * Create dashboards in Datadog
  * Create synthetic tests in Datadog
  * Create monitors in Datadog

## Setup
Proceed to the Datadog learning center and look for our lab `Introduction to monitoring AWS with Datadog & Terraform Cloud`
All steps for the lab are contained there. 

* Create three workspace (aws-eks, storedog-app, datadog-resources)
  * **General**
    * Set Terraform-Working-Directory to each workspace name for changing to sub folder in Github Project
    * Only `aws-eks` share remote-state-sharing with other workspaces (`storedog-app`, `datadog-resources`)
    * Only `storedog-app` share remote-state-sharing with `datadog-resources`
  * **VCS Control**
    * Connect to VCS Github App with `Version-Control-Workflow`
    * Choose `boonchu/tf-workshop``

* Create Variable Sets for using in three workspace
  * **Organization**
    * Tab to `settings` -> `Variable Sets`
      * Create `AWS` Variable Sets
        * Applied varibles to three workspaces
        * Include Terraform Variable `aws_region` = `eu-west-1`
        * Include Terraform Variable `vpc_name` = `aws-workshop`
        * Include Environment Variable `AWS_ACCESS_KEY_ID` = XXX
        * Include Environment Variable `AWS_SECRET_ACCESS_KEY`= XXX
      * Create `Datadog` Variable Sets
        * Applied variables to three workspaces
        * Include Sensitive String of Terraform Variable `datadog_api_key` = XXX
        * Include Sensitive String of Terraform Variable `datadog_app_key` = XXX
      * Create `Global Variable` Variable Sets
        * Applied variables to three workspaces
        * Include Terraform Variable `application_name` = `storedog`
        * Include Terraform Variable `eks_cluster_version` = `1.26`
        * Include Terraform Variable `org_name` = `aws-eks-datadog` # Whatever name is in your org...

* Apply Change to the **first workspace** `aws-eks` -> `New Run` -> `Plan and Apply`

* Open AWS Console to check status of EKS cluster

* Apply Change to the **second workspace** `storedog-app`
  * Deploy Storedog e-commernce application to the Kubernetes Cluster 
  * Deploy Datadog Agent with Helm `helm_release.datadog_agent`
  * Deploy IAM Polices for the Datadog AWS Intergration `aws_iam_policy.datadog_aws_integration`
  * Enable the Datadog AWS Intergration `datadog_integration_aws.sandbox`

`kubectl get all -n storedog`

  * Check LB URL from either terraform output or kubectl
  * Check Datadog UI integration status from `Integration` -> `AWS`
  * Check Datadog UI Metrics `Metrics` -> `Summary`
    * Search for `aws.ec2.cpuutilization`
    * In the upper-right corner of the details panel, click the Open in Metrics Explorer button.
  * Check Datadog UI `Logs` -> `Search`
  * Check Datadog UI `Dashboards` -> `Dashboard List`
    * Search for `AWS EC2 Overview`
  * Check Datadog UI `Infrastructure` -> `Kubernetes`

* Apply Change to the **third workspace** `datadog-resources`
  * what are we creating?
    * Storedog dashboard `datadog_dashboard.*`
    * Storedog monitor  `datadog_monitor.*`
    * Service Definition for our store-frontend service
    * API Synthetic Test on our Storedog address `datadog_synthetics_test.*`
    * Browser Synthetic Test on our Storedog address
    * Browser Synthetic Test with steps on our Storedog address