---

copyright: 
  years:  2022
lastupdated: "2022-04-20"

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

# Creating a three-tier highly available infrastructure VPC in a multi-zone region with Schematics
{: #create-three-tier-resilient-vpc-mzr-schematics} 

## What is created
{: #created-vpc-ha-resilient-mzr-schematics}

A VPC is a private space in IBM Cloud where you can run an isolated environment with custom network policies. The variables that you define are used by the scripts to provision the virtual private cloud infrastructure resources for you. The default values in the scripts are used to create these resources in two regions: 

|Resource type    |  Number of resources created  |  
|-------------------|-----------------------|
| VPC             |                 01    | 
| Security Groups    |              05    |
| Subnets      |                    10    | 
| Bastion VSI        |              01    | 
| App VSI            |              05    |
| web VSI           |               05    |  
| Database VSI    |                 06    |  
| Floating IP     |                 01    |  


The scripts use variables to specify your account information, VSI server information, Load Balancer information, Auto scale, and Anti-Affinity

The scripts automatically install these software packages:

*  PHP
*  Apache
*  Maria DB
*  Word Press Application

## Caveats
{: #limitations-ha-resilient-mzr-schematics}

*	The bastion server is important to access other VSI. 

Don’t delete the bastion server or update its OS image after creation (**Apply plan**) because the bastion server is used to access other servers. If the Bastion server is removed, you lose access to your infrastructure. Get a snapshot of the bastion server for backup and recovery.
{: note}

## Before you begin
{: #before-begin-ha-resilient-mzr-schematics}

* [Create or retrieve an {{site.data.keyword.cloud_notm}} API key](/docs/account?topic=account-userapikey#create_user_key). The API key is used to authenticate with the IBM Cloud platform and to determine your permissions for IBM Cloud services.
* [Create or retrieve your SSH key name](/docs/ssh-keys?topic=ssh-keys-getting-started-tutorial). The SSH key is used to SSH to the bastion server.
* Collect the information that you need to specify for the script parameters:

    |Component type   | Parameter values needed |
    |-----------------|--------------------|
    |User-specific information  | - API key  \n - SSH key or keys  \n - Resource Group  \n - IP address or addresses  \n - Your local machine OS \n - Prefix to add to resource names |
    |OS type for each server  | -Bastion Server  \n - App Server  \n - Web Server  \n - Db Server  |
    |Region   | Regions where resources are deployed|
    |Image to use for region 1 VSIs |  -Bastion Server  \n - App Server  \n - Web Server  \n - Db Server  |
    |Image to use for region 2 VSIs |  -Bastion Server  \n - App Server  \n - Web Server  \n - Db Server  |
    |Load Balancer | - Domain Name  \n - Region Code  |
    |Auto scale App VSI parameters | - Min server  \n - Max servers  \n - CPU threshold  |
    |Auto scale Web VSI parameters | - Min server  \n - Max servers  \n - CPU threshold  |
    |Anti-Affinity VSI parameters  | - App spread strategy  \n - Web spread strategy  \n - Db spread strategy |

    Images are region-specific. To find the images for your region, open the {{site.data.keyword.cloud_notm}} shell and enter `ibmcloud target -r < region_name >` to set your region, then `ibmcloud is images` to see a list of images for that region.
    {: note}

## Create a personal access token
{: #create-personal-access-token-mzr-schematics}

To use the files in the GitHub repo in Schematics, you need a personal access token. When you create the token, copy it to a text file for future use. When you leave the create token page, you can no longer view the token.  

1.  Go to the [MZR GitHub repository](https://github.ibm.com/workload-eng-services/ha-mzr). 
2.  Select **Settings** from your account icon menu.
3.  Select **Developer settings**.
4.  Select **Personal Access Tokens**.
5.  Select **Generate new token**.
6.  Copy the new token and save it in a text file.

## Create the VPC with Schematics
{: #create-vpc-mzr-schematics}

1.	From the IBM Cloud menu, select [Schematics](https://cloud.ibm.com/schematics/overview).
2.  Click **Workspaces**.
3.	Click **Create workspace**.
4.  On the **Specify template** page:
    * Enter the URL of high availability MZR repo, https://github.ibm.com/workload-eng-services/ha-mzr. 
    * Enter your Personal Access Token.
    * Select the **Terraform version** that is listed in the readme file.
    * Click **Next**.  
5.  On the Workspace details page:
    * Enter a name for the workspace.
    * Select a **Resource group**.
    * Select a **Location** for your workspace. The workspace location does not have to match the resource location.
    * Select **Next**.
6.  Select **Create** to create your workspace. The files from the GitHub repo are loaded in to Schematics. It can take several minutes for the files to load.
7.  On the workspace **Settings** page, in the Input variables section, review the default input variables and provide values that match your solution. Some parameters have default values. Make sure to mark as “sensitive” the parameters that contain sensitive information like API keys. The values that you must specify include:

    |Component type   | Parameter values needed |
    |-----------------|--------------------|
    |User-specific information  | - API key  \n - SSH key name. If you need to enter multiple SSH keys, enter them in square brackets and quotation marks. For example, ["ibmcloud_ssh_key_name1,ibmcloud_ssh_key_name2,..."]  \n - Resource Group  \n - IP address. Enter the IP address in the format: ["public_ip_address1/32,public_ip_address2/32,..."]. For example, ["103.42.91.78/32"]  \n - Your local machine OS  \n - Prefix to add to resource names |
    |Region  | Regions where resources are deployed  |
    |OS type for each server  | -Bastion Server  \n - App Server  \n - Web Server  \n - Db Server  |
    |Image to use for region 1 VSIs |  -Bastion Server  \n - App Server  \n - Web Server  \n - Db Server  |
    |Image to use for region 2 VSIs |  -Bastion Server  \n - App Server  \n - Web Server  \n - Db Server  |
    |Load Balancer | - Domain Name  \n - Region Code  |
    |Auto scale App VSI parameters | - Min server  \n - Max servers  \n - CPU threshold  |
    |Auto scale Web VSI parameters | - Min server  \n - Max servers  \n - CPU threshold  |
    |Anti-Affinity VSI parameters  | - App spread strategy  \n - Web spread strategy  \n - Db spread strategy |

     For a more detailed description of each of the parameters, check the GitHub repo readme file. 

8.  Click **Save changes**.
9.	On the workspace Settings page, click **Generate plan**. Wait for the plan to complete. It can take several minutes for the plan to complete.
10.	Click **View log** to review the log files of your Terraform execution plan.
11.	Apply your Terraform template by clicking **Apply plan**. It can take 10 - 20 minutes for the apply to complete.
12.	Review the log file to ensure that no errors occurred during the provisioning, modification, or deletion process.