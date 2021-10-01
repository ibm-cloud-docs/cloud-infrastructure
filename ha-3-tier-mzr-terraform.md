---

copyright: 
  years:  2021
lastupdated: "2021-10-01"

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

# Creating a resilient three-tier highly available infrastructure VPC in multiple regions with Terraform
{: #create-three-tier-resilient-vpc-mzr} 
{: toc-content-type="tutorial"} 
{: toc-services="virtual-servers, vpc, loadbalancer-service"} 
{: toc-completion-time="30m"}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} VPC infrastructure resources so that you can rapidly build complex, cloud environments. For more information about Terraform on {{site.data.keyword.cloud_notm}}, see [Terraform on {{site.data.keyword.cloud_notm}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: shortdesc}

Terraform on {{site.data.keyword.cloud_notm}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} solutions. 
To create resources with Terraform, you use Terraform configuration files that describe the {{site.data.keyword.cloud_notm}} resources that you need and how you want to configure them. Based on your configuration, Terraform creates an execution plan and describes the actions that need to be run to create the resources. You can review the execution plan, change it, or run the plan. When you change your configuration, Terraform on {{site.data.keyword.cloud_notm}} can determine what changed and create incremental execution plans that you can apply to your existing {{site.data.keyword.cloud}} resources. 

## What is created
{: #created-vpc-ha-resilient-mzr}

A VPC is a private space in IBM Cloud where you can run an isolated environment with custom network policies. The variables that you define are used by the scripts to provision the virtual private cloud infrastructure resources for you. The devault values in the scripts are used to create these resources in two regions: 

|RESOURCE NAME      |             REGION1  |  REGION2  |   TOTAL        |
|-------------------|-----------------------|----------|------------------|
| Cloud Object Storage Instance    |                 01    |     -         |  01|
| Cloud Object Storage Bucket      |                 01    |     -         |  01|
| Cloud Object Storage Object      |                 01    |     -         |  01|
| VPC             |                 01    |     01        |  02|
| Security Groups    |              05    |     05        |  10|
| Subnets      |                    10    |     10        |  20|
| Bastion VSI        |              01    |     01        |  02|
| App VSI            |              05    |     05        |  10|
| web VSI           |               05    |     05        |  10|
| Database VSI    |                 06    |     06        |  12|
| Floating IP     |                 01    |     01        |  02|


The scripts use variables to specify your information, OS, Auto scale, and Cloud Object Storage (Cloud Object Storage):

* API and ssh key
* OS type and image
* Local machine OS
* Database OS, image, and bandwidth
* Minimum and maximum virtual server instances that are configured for the instance group and the CPU threshold for adding virtual server instances to the pool
* Global load balancer domain name and region code
* Cloud Object Storage plan and region information

## Script Files
{: #script-files-resilient-mzr}

The configuration and script files are provided on the [vpc-ha-iac GitHub repository](https://github.com/IBM-Cloud/vpc-ha-iac).

To create a resilient highly available architecture, you use the files in the multi-region folder.

For 3-tier highly available resilient architecture in multiple regions on virtual private cloud, modify the:

*	`example.userinput.auto.tfvars` file and follow the instruction in the file to add your API key, region, and other variables.
*	Modules (instances, load_balancers, security_groups, subnets) variables.tf or `.tf` file to further customize specific resource parameters. For example, security group has a boiler plate for security policies that might not encompass all the rules that are needed by your application. In that case, you need to modify it and add the appropriate rules. For more information, see Procedure Step 6.

All of the other configuration files are provided and do not need to be modified. 

The {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform on {{site.data.keyword.cloud_notm}} uses these configuration files to provision a VPC in your {{site.data.keyword.cloud_notm}} account.

The scripts take 20 - 30 min to create the VPC and components. 

## Support
{: #support-ha-resilient-mzr}

There are no warranties of any kind for the Terraform code, and there is no service or technical support available for these materials from IBM®. As a recommended practice, review carefully any materials that you download from this site before you use them on a live system.

Though the materials provided here are not supported by the IBM Service organization, your comments are welcomed by the developers, who reserve the right to revise, readapt or remove the materials at any time. To report a problem, or provide suggestions or comments, open a GitHub issue.

## Caveats
{: #limitations-ha-resilient-mzr}

*	The bastion server is important to access other VSI. To prevent the accidental deletion of that server we added a lifecycle block with `prevent_destroy=true` flag to protect this server. If you want to delete the server, before you run `terraform destroy`, update `prevent_destroy=false` in `compute.tf` of the Bastion module.

Don’t delete the bastion server or update its OS image after creation (`terraform apply`) because the bastion server is used to access other servers. If the Bastion server is removed, you lose access to your infrastructure. You should get a snapshot of the bastion server for backup and recovery.
{: note}

## Before you begin
{: #before-begin-ha-resilient-mzr}

[Create or retrieve an {{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-userapikey#create_user_key). The API key is used to authenticate with the IBM Cloud platform and to determine your permissions for IBM Cloud services.

[Create or retrieve your SSH key name](/docs/ssh-keys?topic=ssh-keys-getting-started-tutorial). The SSH key is used to SSH to the bastion server.

Retrieve the IP address of the server that will access your bastion server. You need this IP address to update the bastion server security group.

## Procedure
{: #procedure-ha-resilient-mzr}

Use these steps to configure the {{site.data.keyword.cloud_notm}} Provider plug-in and use Terraform to create a resilient infrastructure for a 3-tier application.

1.	If Terraform is not installed on your machine, [Install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli). You use Terraform v0.13.0 or greater and {{site.data.keyword.cloud_notm}} Provider plug in v1.26.0.

2.	Create a project folder in the Terraform installation folder, and change directory to your project folder.

    `mkdir myproject && cd myproject`

3.	Copy the files from [vpc-ha-iac GitHub repository](https://github.com/IBM-Cloud/vpc-ha-iac) folder to the project folder that you created in the Terraform installation folder.

4.  Use the instructions defined in the `example.userinput.auto.tfvars` file to rename the file and edit the variables to set the values for your configuration. You must modify:

    *  Your {{site.data.keyword.cloud_notm}} API key.
    *  Your 40-digit resource group ID.
    *  Your SSH for the region where you are creating resources. The key name should be the same for both regions where you are creating resources.
    *  Your public IP address.
    *  The Bastion, db, web, and app image IDs for each region.  The images are unique to each region. The sample images are for the jp-tok and us-east.  
    
    Variables that are defined in the example `userinput.auto.tfvars` file are automatically loaded by Terraform when the IBM Cloud Provider plug-in is initialized. You can reference the variables in every Terraform configuration file that you use. 

    Because the `userinput.auto.tfvars` file contains sensitive information, do not push this file to a version control system. Keep this file on your local system only.

5.  Modify the `main.tf` file and enter the regions where you are creating resources. The sample `main.tf file` uses `jp-tok` and `us-east`. Find and replace these values with the values for your regions.

6.	(Optional) You can edit the instances profile in the `instance_variables.tf` file in the `multi-region` folder. Currently, it is set with the lowest compute profile (cx2-2x4). For more options for profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). 

    |VSI Profile	|Variable Path => Variable Name|
    |-------|------------|
    |App VSI	|multi-region/ig_variables.tf => app_config|
    |Web VSI	|multi-region/ig_variables.tf => web_config|
    |DB VSI	   |multi-region/instance_variables.tf => db_profile|
    |Bastion VSI	|multi-region/bastion_variables.tf => bastion_profile|

7.	(Optional) Update the load balancers, image, port numbers, security groups, and port numbers. 

    * Load balancer ports
        * Location/File: ./lb_variables.tf and ./modules/load_balancers/[app and db].tf
        * Parameters: lb_port_number and copy or modify resource "ibm_is_lb_pool" code block
        * Notes: To add or change load balancers port and pool.

    * Security groups Module
        * Location/File: ./modules/security_groups/[web, app, db].tf
        * Parameters: rule
        * Notes: To change the number of VSI created for db (per zone per region). For app and web, this done in `userinput.auto.tfvars`.

    * Number of VSIs db
        * Location/File: ./common_variables.tf
        * Parameters: db_vsi_count
        * Notes: To change the number of VSI created per zone per tier. Default is 1

    * Autoscale scaling policy
        * Location/File: ./ig_variables.tf
        * Parameters: [web and app]_config
        * Notes: To Mmdify the scaling policy

8.	Initialize the Terraform CLI. 

    `terraform init`

9.	Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that are done to create the virtual private cloud instance in your account.

    `terraform plan`

10.	Verify that the plan shows all of the resources that you want to create and that the names and values are correct. If the plan needs to be adjusted, edit the `userinput.auto.tfvars` and `modules .tf` file and run `terraform plan` again.

11.	Create the virtual private cloud for SAP instance and IAM access policy in IBM Cloud.

    `terraform apply`

    The virtual private cloud and components are created and you see output similar to the terraform plan output. 

## Next Steps

If you need to rename your resources after they are created, modify the `example.userinput.auto.tfvars` and `modules.tf` file to change the names and run terraform plan and terraform apply again. 

Do not use the {{site.data.keyword.cloud_notm}} Dashboard and user interface to modify your VPC after it is created. The Terraform scripts create a complete solution and selectively modifying resources with the user interface might cause unexpected results. 

If you need to remove your VPC, go to your project folder and run `terraform destroy`. The Bastion server is protected for inadvertent deletion. Before run the `terraform destroy` command, you need to set the `prevent_destroy` flag to `false` in order to remove your VPC. The `prevent_destroy` flag is located in `./modules/bastion/compute.tf`.



