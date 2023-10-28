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
    * Only `aws-eks` share remote-state-sharing with other workspaces
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
      * Create `Global Variable` Variable Sets
        * Applied variables to three workspaces
        * Include Terraform Variable `application_name` = `storedog`
        * Include Terraform Variable `eks_cluster_version` = `1.26`
        * Include Terraform Variable `org_name` = `aws-eks-datadog` # Whatever name is in your org...

