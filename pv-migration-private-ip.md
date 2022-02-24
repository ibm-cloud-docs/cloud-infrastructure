---

copyright:
   years: 2021
lastupdated: "2022-02-24"

keywords: migration, physical to virtual, migrate
content-type: tutorial
services: virtual-servers, vpc, transit-gateway, 
account-plan: paid
completion-time: 25m
subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:screen: .screen}  
{:codeblock: .codeblock}  
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}
{:step: data-tutorial-type='step'}

# Bare metal to virtual server migration on a private network using RackWare RMM 
{: #pv-migration-private-network} 
{: toc-content-type="tutorial"} 
{: toc-services="virtual-servers, vpc, transit-gateway"} 
{: toc-completion-time="25m"}

Bare metal to virtual server migration is the process of migrating from a physical bare metal server to a virtual server instance. The RackWare RMM solution simplifies the overall migration process of moving the operating system, applications, and data from the bare metal environment to an {{site.data.keyword.cloud}} virtual server instance.
{: shortdesc}

The migration can occur either over the public or private interface of the compute resource. The only requirement is all three components are reachable from one and another. In this tutorial, this document focuses over the private path as this is a bit more involved, which requires setting up a transit gateway for a communication channel between classic and VPC over the private interface.

## Objectives
{: #pv-migration-private-network-objectives}

* Prepare your source and target machines for migration
* Learn how to use the RackWare RMM solution
* Successfully complete a bare metal to virtual server migration for private networks
* Use the `setup` script for licensing provisioning, classic bare metal discovery, and setting up RMM migration waves

## Architecture
{: #pv-migration-private-network-architecture}

![Physical to virtual migration private IP diagram.](images/P2V-Private-1.svg){: caption="Figure 1. Migrating over private interface" caption-side="bottom"}

## Before you begin
{: #pv-migration-private-network-prereqs}

* Check for user permissions. Be sure that your user account has sufficient permissions to create and manage VPC resources. See the list of [required permissions](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources) for VPC.
* Understand the differences between classic and VPC infrastructures (the following list is not extensive):
    * VPC does not support shared volumes or file-based volumes
    * VPC does not support snapshot or replication
    * GPU is not supported in VPC
For more detail, see [Comparing {{site.data.keyword.cloud_notm}} classic and VPC infrastructure environments](/docs/cloud-infrastructure?topic=cloud-infrastructure-compare-infrastructure).

## Set up and provision VPC and virtual server instance
{: #set-up-provision-vpc-vsi}
{: step}

There are 2 different methods for setting up target the VSI, manual or with Rackware RMM auto provision feature as described below:

**Option 1: Manual**

The RackWare RMM solution handles only the OS, application, and data movement. It does not to set up a VPC on the target side; therefore, you need to set up the VPC infrastructure. At a bare minimum, you need to set up a VPC, subnets, and virtual server instances. This tutorial won't go through all of the details of setting up the VPC infrastructure. See the [Virtual Private Cloud (VPC) documentation](/docs/vpc?topic=vpc-getting-started) for further details.

1. Create a VPC. 
2. Create subnets. 
3. Order virtual server instance. 
    * SSH key 
    * Operating system name (Linux/Windows and their respective version) 
    * Security groups 
    * Secondary volume (optional) 

Encrypted volumes are not supported.
{: note}

**Option 2: Auto-provision**

1. Click on **Clouduser** menu under **Configuration** main menu on left-hand side.
2. Click on **Add** button, and the  **Add Cloud** form will pop up. Enter appropriate details for the following fields:
    - Name
    - Select ‘IBM Gen2’ as Cloud Provider
    - Select desired Region where you want to auto provision VSI
    - Enter valid API key for your IBM cloud account
    - Once all details are filled, click on the **Add** button
3. Open wave where operation needs to be performed
4. Click on text ’Not Configured’, next to ‘Autoprovision’ label, a pop up will open
    - Select added clouduser as **Environment**
    - Select region where VSI needs to be provisioned
    - Subnet name and VPC name are optional. If entered, these would be default names for VPC and Subnet during provision of VSI
    - Click on **Add Host**, (plus icon on left top panel)
    - Select **Target Type** as **Autoprovision**
    - Enter source details:

        - IP address
        - Friendly name
        - Select OS
        - Username
    - Enter Target details:
        - Only **Friendly Name** is required on target side
        - Use **Right Sizing** from **Advanced Options** if the source has a boot volume greater than 250 GB, as VPC does not support boot volume greater than 250 GB
    - Once you close this form, RMM will show a warning to enter **IBM Gen2 Options**
    - Edit host and you will see **IBM Gen2 Options** as an additional tab at the top
        - The VPC name is mandatory. It will create VPC with given name if not present in that region. All other fields are optional. If no value is entered in optional fields then RMM will find relevant resource.
        - Select Region
        - Resource Group
        - Subnet
        - Security Group
        - Zone
        - VSI Profile Name
        - Permit Resources Creation – Check this option if the given VPC name or subnet is not present. This is a simple permission flag for RMM to create resources.
        - Image Name
        - Image username (This field is optional as Linux has key-based authentication, so even if any value is entered, it would be ignored)
        - Image password (This field is optional as Linux has key-based authentication so even if any value is entered, it would be ignored)
        - SSH Keys: Enter either the name of the ssh key or the content of the public key to be present on the newly created VSI 
    - Click on Modify
    - Finally, run replication
    
Target VSI boot volume cannot be greater than 100GB, so if source machine’s boot volume is greater than 250 GB, use right sizing option of RMM.
{: note}

If "No Transfer" option is selected in "Sync Options", it does auto provision of target but actual data/applications are not migrated.
{: note}

## Order IBM Cloud Transit Gateway
{: #order-transit-gateway}
{: step}

{{site.data.keyword.tg_full_notm}} provides connectivity between classic and your VPC infrastructure. When connecting to the two entities, be aware that the classic infrastructure is on the `10.0.0.0` network, which means that the VPC network needs to be a on different network; otherwise, the networks overlap and cause communication issues. 

1. Order {{site.data.keyword.tg_full_notm}}
    * Use local routing option 
2. Add connections 
    * Classic infrastructure 
    * VPC 
     * Select region 
     * Select VPC 

## Order RackWare RMM
{: #order-rackware-rmm}
{: step}

RackWare RMM tool is available in the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external}. Upon ordering, a virtual server with RackWare RMM software is installed into your VPC of choice. The RMM server has a public IP address for reachability and default login. 

### Order RackWare RMM server from IBM Cloud catalog 
{: #order-rackware-rmm-server}

1. Select your resource group 
2. Enter which VPC this should be created in 
3. Enter subnet 
4. SSH key 
5. API key 
6. Region and zone 

### Login into RackWare RMM server
{: #login-rackware-rmm-server}

1. Change the default password 
2. Create users 
3. Create an SSH key
4. Use the `setup` script to request the license a three-month promotional license.

```
/opt/IBM/discoverTool -p
```
{: pre}
   
After three months, you will need to purchase the license from RackWare by emailing [sales@rackwareinc.com](mailto:sales@rackwareinc.com). 
{: important}
   
## Prepare source and target machines
{: #prepare-source-target-machines}
{: step}

There are a few things that need to be done on the source and target machine for the migration to work. The RackWare RMM server needs to SSH into the machines; thus, the RMM public SSH keys need to be copied onto both the source and target machines. In addition, if the source machine has both public and private interfaces, host routes need to be added to ensure the communication between the source and target machines occurs over the transit gateway path. Complete the following steps to prepare your relevant machines.

### Linux systems
{: #linux-systems}

1. Copy the RackWare RMM SSH public key to both the source and target machines.
2. Make sure that the `tar` package is installed on both the source and target machines.
3. If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for operating system:

```
ip route add <destination_network> via <Gateway_address> dev <private_ethernet_interface>
```
{: pre}

### Windows systems
{: #windows-systems}

1. Copy the RackWare RMM SSH public key to both the source and target machines.
2. You need to download the SSH key utility. You can download it from the RackWare RMM server.
3. The user is `SYSTEM`, and you need to key in the RMM SSH key to authenticate for both the source and target machines.
4. If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for operating system:

```
route ADD <destination_network> MASK <subnet_mask> <gateway_ip> <metric_cost>
```
{: pre}

In the case of a user using the **Auto Provision** feature, there is no need to set up a target. Only the friendly name for the target machine is required. 
{: note}

## Set up RackWare RMM waves for P2V migration
{: #set-up-rackware-rmm-p2v-migration}
{: step}

You can migrate the machines over one by one or do simultaneous migrations. If you are doing multiple, simultaneous migrations, download the CSV template from the RMM server and complete the appropriate fields.

1. Log in to the RMM server.
2. Create a _Wave_ and define _Wave_ name.
3. If there are multiple hosts, download the template, fill in the appropriate fields, and then upload the template.
4. Select the _Wave_ name to enter source and target information.
5. Select the "+" sign.
6. Add source IP address or FQDN and add source username. 
7. Target Type = Existing System
8. Sync Type = Direct Sync
9. Add target IP address or FQDN.
10. Add a target-friendly name, and add target username.
11. Start the migration.

The username field for Linux environments is `root`. The username field for Windows environments is `SYSTEM`. 
{: note}

Within the discovery script, there is a helper script is provided to help with the discovery of the classic bare metals and creating the waves for steps 2 and 3. The script asks for your classic username and API to discover your classic bare metals.

```
$ /opt/IBM/discoverTool -d
```
{: pre}

## Validate
{: #validate-pv-migration}
{: step}

Before you decommission the source server, you need to validate the target server. Make sure the validate the following:
* Application
* Licensing
* Reachability (host-level configuration changes)
* Remove RMM SSH key
