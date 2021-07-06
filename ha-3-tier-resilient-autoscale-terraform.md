---

copyright: 
  years:  2021
lastupdated: "2021-07-06"

keywords: high availability, regions, zones, resiliency

content-type: tutorial

services: virtual-servers, vpc, loadbalancer-service

account-plan: paid

completion-time: 30m

subcollection: cloud-infrastructure

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:step: data-tutorial-type='step'}

# Creating a resilient three-tier highly available infrastructure VPC with Autoscale by using Terraform
{: #create-three-tier-resilient-vpc-autoscale} 
{: toc-content-type="tutorial"} 
{: toc-services="virtual-servers, vpc, loadbalancer"} 
{: toc-completion-time="30m"}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} VPC infrastructure resources so that you can rapidly build complex, cloud environments.
{: shortdesc}

Terraform on {{site.data.keyword.cloud_notm}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} solutions. For more information about Terraform on {{site.data.keyword.cloud_notm}}, see [Terraform on {{site.data.keyword.cloud_notm}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).

To create resources with Terraform, you use Terraform configuration files that describe the {{site.data.keyword.cloud_notm}} resources that you need and how you want to configure them. Based on your configuration, Terraform creates an execution plan and describes the actions that need to be run to create the resources. You can review the execution plan, change it, or run the plan. When you change your configuration, Terraform on {{site.data.keyword.cloud_notm}} can determine what changed and create incremental execution plans that you can apply to your existing {{site.data.keyword.cloud}} resources. 

## Script Files
{: #script-files-resilient-autoscale}

The configuration and script files are provided on the [vpc-ha-iac GitHub repository](https://github.com/IBM-Cloud/vpc-ha-iac).

To create a resilient highly available architecture, you use the files in the 3-tier-autoscale folder.

For 3-tier highly available resilient architecture with Autoscale on virtual private cloud, modify the:

*	`example.userinput.auto.tfvars` file and follow the instruction in the file to add your API key, region, and variables.
*	Modules (instances, load_balancers, security_groups, subnets) variables.tf or `.tf` file to further customize specific resources parameters. For example, security group has a boiler plate for security policies that might not encompass all the rules that are needed by your application. In that case, you need to modify it and add the appropriate rules. For more information, see Procedure Step 6.

All of the other configuration files are provided and do not need to be modified. 

The IBM Cloud Provider plug-in for Terraform on IBM Cloud uses these configuration files to provision a VPC in your IBM Cloud account.

## What is created
{: #created-vpc-ha-resilient-autoscale}

A VPC is a private space in IBM Cloud where you can run an isolated environment with custom network policies. The variables that you define are used by the scripts to provision the following virtual private cloud infrastructure resources for you: 
*	1 VPC
*	4 security groups
*	4 subnets
*	10 virtual server instances (but can be modified per your requirement). Note, if autoscale is used then the number fluctuates based on your compute resource load criteria.
*	2 floating IP (1 for bastion and 1 for public application load balancer)
*	3 application load balancers (1 public and 2 private)

The scripts take 30-40 min to create the VPC and components. 

## Support
{: #support-ha-resilient-autoscale} 

There are no warranties of any kind for the Terraform code, and there is no service or technical support available for these materials from IBM®. As a recommended practice, review carefully any materials that you download from this site before you use them on a live system.

Though the materials provided here are not supported by the IBM Service organization, your comments are welcomed by the developers, who reserve the right to revise, readapt or remove the materials at any time. To report a problem, or provide suggestions or comments, open a GitHub issue.

## Limitations
{: #limitations-ha-resilient-autoscale}

*	Windows image is not supported by these scripts.
*	As the bastion server is important to access other VSI. So to prevent the accidental deletion of that server we are adding a lifecycle block with `prevent_destroy=true` flag to protect this server. If you want to delete the server, before you run `terraform destroy`, update `prevent_destroy=false` in `compute.tf` of Bastion module.

Don’t delete the bastion server or update its OS image after creation (`terraform apply`) because the bastion server is used to access other servers. If the Bastion server is removed, you lose access to your infrastructure. You should get a snapshot of the bastion server for backup and recovery.
{: note}

## Before you begin
{: #before-begin-ha-resilient-autoscale}

[Create or retrieve an {{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-userapikey#create_user_key). The API key is used to authenticate with the IBM Cloud platform and to determine your permissions for IBM Cloud services.

[Create or retrieve your SSH key name](/docs/ssh-keys?topic=ssh-keys-getting-started-tutorial). The SSH key is used to SSH to the bastion server.

Retrieve the IP address of the server that will access your bastion server. You need this IP address to update the bastion server security group.

## Procedure
{: #procedure-ha-resilient-autoscale}

Use these steps to configure the {{site.data.keyword.cloud_notm}} Provider Plug-in and use Terraform to create a resilient infrastructure for a 3-tier highly availalbe resilient architecture with Autoscale application.

1.	If Terraform is not installed on your machine, [Install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli). You use Terraform v0.13.0 or greater and {{site.data.keyword.cloud_notm}} Provider plug-in v1.26.0.

2.	Create a project folder in the Terraform installation folder, and change directory to your project folder.

    `mkdir myproject && cd myproject`

3.	Copy the files from `vpc-ha-iac/3-tier-autoscale` folder to the project folder that you created in the Terraform installation directory.

4.	Rename the `example.userinput.auto.tfvars` variable file to `userinput.aut.tfvars` and edit variable following the instructions in the file.

    Variables that are defined in the example `userinput.auto.tfvars` file are automatically loaded by Terraform when the IBM Cloud Provider plug-in is initialized. You can reference the variables in every Terraform configuration file that you use. 

    Because the `userinput.auto.tfvars` file contains sensitive information, do not push this file to a version control system. Keep this file on your local system only.

5.	Modify the image ID. in the  file. For more options for image, see [Images](/docs/vpc?topic=vpc-about-images).

    *  For the Bastion server, modify the `bastion_image` parameter in the `./modules/bastion/variables.tf` file 
    *  For the web, app, and db servers, edit the parent project folder `variables.tf` file and modify the:
        - web_config code block `instance_image` parameter 
        - app_config code block `instance_image` parameter 
        - db_image code block `default` parameter


6.	(Optional) You can edit the instances profile in the `variables.tf` file. Currently, it is set with the lowest compute profile (cx2-2x4). For more options for profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). 

7.	(Optional) Update the `variables.tf` file in the corresponding `modules` folder to modify port numbers, security groups, and port numbers. 

    * Load balancers Module
        * Location/File: ./modules/load_balancers/variables.tf
        * Parameters: lb_port_number
        * Notes: To add or change load balancers port.

    * Security groups Module
        * Location/File: ./modules/security_groups/[app, db, web, load_balancer].tf
        * Parameters: *_rule_*
        * Notes: To add more security group rules, create more resource blocks.

8. (Optional) Update the `default` parameter in the ip_count code block in the parent project folder `variables.tf` file to change the number of IP address for the subnet. 

9.	Initialize the Terraform CLI. 

    `terraform init`

10.	Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that are done to create the virtual private cloud instance in your account.

    `terraform plan`

11.	Verify that the plan shows all of the resources that you want to create and that the names and values are correct. If the plan needs to be adjusted, edit the `example.userinput.auto.tfvars` and `modules .tf` file and run `terraform plan` again.

12.	Create the virtual private cloud for SAP instance and IAM access policy in IBM Cloud.

    `terraform apply`

    The virtual private cloud and components are created and you see output similar to the terraform plan output. 

## Next Steps

If you need to rename your resources after they are created, modify the `example.userinput.auto.tfvars` and `modules.tf` file to change the names and run terraform plan and terraform apply again. 

Do not use the {{site.data.keyword.cloud_notm}} Dashboard and user interface to modify your VPC after it is created. The Terraform scripts create a complete solution and selectively modifying resources with the user interface might cause unexpected results. 

If you need to remove your VPC, go to your project folder and run `terraform destroy`.


