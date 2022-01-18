---

copyright: 
  years:  2021, 2022
lastupdated: "2022-01-18"

keywords: high availability, regions, zones, resiliency

services: virtual-servers, vpc, loadbalancer-service

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

# Creating a resilient three-tier highly available infrastructure VPC in a single availability zone with Terraform
{: #create-three-tier-resilient-vpc-szr} 

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent provisioning of {{site.data.keyword.cloud_notm}} VPC infrastructure resources so that you can rapidly build complex, cloud environments. The {{site.data.keyword.cloud_notm}} VPC infrastructure that is created uses Intel Xeon CPUs and additional Intel technologies. For more information about Terraform on {{site.data.keyword.cloud_notm}}, see [Terraform on {{site.data.keyword.cloud_notm}} getting started tutorial](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: shortdesc}

To create resources with Terraform, you use Terraform configuration files that describe the {{site.data.keyword.cloud_notm}} resources that you need and how you want to configure them. Based on your configuration, Terraform creates an execution plan and describes the actions that need to be run to create the resources. You can review the execution plan, change it, or run the plan. When you change your configuration, Terraform on {{site.data.keyword.cloud_notm}} can determine what changed and create incremental execution plans that you can apply to your existing {{site.data.keyword.cloud_notm}} resources. 

## What is created
{: #created-vpc-ha-resilient-szr}

A VPC is a private space in IBM Cloud where you can run an isolated environment with custom network policies. The variables that you define are used by the scripts to provision the virtual private cloud infrastructure resources for you. The default values in the scripts are used to create these resources in a single region: 

|Resource Name      |             Quantity  | 
|-------------------|-----------------------|  
| VPC             |                 01    |     
| Security Groups    |              05    |     
| Subnets      |                    10    |     
| Bastion VSI        |              01    |     
| App VSI            |              05    |     
| web VSI           |               05    |     
| Database VSI    |                 06    |     
| Floating IP     |                 01    |  
| Placement Groups   |              03    |   

The scripts automatically install these software packages:

*  PHP
*  Apache
*  Maria DB
*  Word Press Application


## Script Files
{: #script-files-resilient-szr}

The configuration and script files are provided on the [single-availability-zone-automation](https://github.com/IBM-Cloud/vpc-ha-iac).

To create a resilient highly available architecture, you use the files in the single-region folder. When you run the scripts, you are prompted for information about your account and the resources you are creating. 

All of the other configuration files are provided and do not need to be modified. 

The {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform on {{site.data.keyword.cloud_notm}} uses these configuration files to provision a VPC in your {{site.data.keyword.cloud_notm}} account.

The scripts take 20 - 30 min to create the VPC and components. After the VPC and components are created, the script installs software packages on the virtual server instances.

## Support
{: #support-ha-resilient-szr}

There are no warranties of any kind for the Terraform code, and there is no service or technical support available for these materials from IBM®. As a recommended practice, review carefully any materials that you download from this site before you use them on a live system.

Though the materials provided here are not supported by the IBM Service organization, your comments are welcomed by the developers, who reserve the right to revise, readapt or remove the materials at any time. To report a problem, or provide suggestions or comments, open a GitHub issue.

## Caveats
{: #limitations-ha-resilient-szr}

*	The bastion server is important to access other VSI. To prevent the accidental deletion of that server we added a lifecycle block with `prevent_destroy=true` flag to protect this server. If you want to delete the server, before you run `terraform destroy`, update `prevent_destroy=false` in `compute.tf` of the Bastion module.

Don’t delete the bastion server or update its OS image after creation (`terraform apply`) because the bastion server is used to access other servers. If the Bastion server is removed, you lose access to your infrastructure. You should get a snapshot of the bastion server for backup and recovery.
{: note}

## Before you begin
{: #before-begin-ha-resilient-szr}

The scripts prompt you for information that is needed to create the VPC and other resources. Before you run the scripts, you need:

1.  Information about your account:
    * Your {{site.data.keyword.cloud_notm}} API key. [Create or retrieve an {{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-userapikey#create_user_key). The API key is used to authenticate with the IBM Cloud platform and to determine your permissions for IBM Cloud services.
    * Your 40-digit resource group ID.
    * Your SSH for the region where you are creating resources. [Create or retrieve your SSH key name](/docs/ssh-keys?topic=ssh-keys-getting-started-tutorial). The SSH key is used to SSH to the bastion server.
    * Your public IP address. You can use: https://www.whatismyip.com/.
    * Your local machine type, for example, Windows, Linux, or Mac
    * A prefix to use for your resources so that you can easily identify them. 
2.  Information about the virtual server instances that you are creating:
    * The region the resources are created in. For example, us-south, or br-sao.
    * For each server, Bastion, app, web, db, you need the:
        * Custom image ID for the server. Images are unique to regions. To find the images for your region, open the {{site.data.keyword.cloud_notm}} shell and enter `ibmcloud target -r < region_name >` to set your region, then `ibmcloud is images` to see a list of images for that region.
        * OS image to use, Windows or Linux.
3.  Information for the Instance Groups for Auto scale:
    * The CPU percentage for the App Instance Group
    * The minimum number of servers to have in the App Instance Group
    * The maximum nuber of server to have in the App Instance Group
    * The bandwith per second in GB. 
4.  Information for Anti-affinity:
    * The VSI spread strategy for the db server (host or power spread)
    * The VSI spread strategy for the web server (host or power spread)
    * The VSI spread strategy for the app server (host or power spread)


## Procedure
{: #procedure-ha-resilient-szr}

Use these steps to configure the {{site.data.keyword.cloud_notm}} Provider plug-in and use Terraform to create a resilient infrastructure for a 3-tier application.

1.	If Terraform is not installed on your machine, [Install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli). You use Terraform v0.13.0 or greater and {{site.data.keyword.cloud_notm}} Provider plug in v1.26.0.

2.	Create a project folder in the Terraform installation folder, and change directory to your project folder.

    `mkdir myproject && cd myproject`

3.	Copy the files from [vpc-ha-iac GitHub repository](https://github.com/IBM-Cloud/vpc-ha-iac) single-region folder to the project folder that you created in the Terraform installation folder.

4.  Enter your solution specific information in the `sample.userinput.auto.tfvars` file.  Follow the instructions in the file comments. When you are done, remove the `#` at the beginning of each line and save the file as `userinput.auto.tfvars`.   If you do not modify and save the `userinput.auto.tfvars`, the script prompts you for the information during the `terraform plan` command.

    You update the information that you collected in the Before you begin section. 
    * Your {{site.data.keyword.cloud_notm}}.
    * Your 40-digit resource group ID.
    * Your SSH for the region where you are creating resources.
    * Your public IP address. 
    * Your local machine type, for example, Windows, Linux, or Mac
    * A prefix to use for your resources so that you can easily identify them. 
    * The region the resources are created in. For example, us-south-1, or br-sao.
    * For each server, Bastion, app, web, db, you enter the:
        * Custom image ID for the server. 
        * OS image to use, Windows or Linux.
        * The CPU percentage for the App Instance Group
        * The minimum number of servers to have in the App Instance Group for Auto scale.
        * The maximum nuber of server to have in the App Instance Group for Auto scale.
        * The bandwith per second in GB. 
        * The VSI spread strategy for the db server (host or power spread) for anti-affinity. Power spread is recommended here.
        * The VSI spread strategy for the web and app servers (host or power spread) for anti-affinity. Host spread is recommended here if more than 4 are created for each server type.

5.	Initialize the Terraform CLI. 

    `terraform init`

6.	Create a Terraform execution plan. The Terraform execution plan summarizes all the actions that are done to create the virtual private cloud instance in your account.

    `terraform plan`

    The scripts prompt you for information about your resources if you did not specify them in the `userinput.auto.tfvars` file.  

7.	Verify that the plan shows all of the resources that you want to create and that the names and values are correct. If the plan needs to be adjusted, edit the `userinput.auto.tfvars` and `modules .tf` file and run `terraform plan` again.

8.	Create the virtual private cloud for SAP instance and IAM access policy in IBM Cloud.

    `terraform apply`

    The virtual private cloud and components are created and you see output similar to the terraform plan output. 

## Next Steps

If you need to rename your resources after they are created, modify the `example.userinput.auto.tfvars` and `modules.tf` file to change the names and run terraform plan and terraform apply again. 

Do not use the {{site.data.keyword.cloud_notm}} Dashboard and user interface to modify your VPC after it is created. The Terraform scripts create a complete solution and selectively modifying resources with the user interface might cause unexpected results. 

If you need to remove your VPC, go to your project folder and run `terraform destroy`. The Bastion server is protected for inadvertent deletion. Before you run the `terraform destroy` command, you need to set the `prevent_destroy` flag to `false` to remove your VPC. The `prevent_destroy` flag is located in `./modules/bastion/compute.tf`.



