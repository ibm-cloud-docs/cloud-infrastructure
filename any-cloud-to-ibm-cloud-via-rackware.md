---

copyright:
  years:  2022
lastupdated: "2022-03-14"

keywords: rackware, vpc, migration 
content-type: tutorial
services: vpc, virtual-servers
account-plan: paid
completion-time: 45m
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
{:step: data-tutorial-type='step'}

# AWS, Azure, GCP, and OCI workloads to IBM Cloud VPC VSI migration with Rackware RMM

{: #migrating-images-cloud-rackware}
{: toc-content-type="tutorial"}
{: toc-services="vpc, virtual-servers"}
{: toc-completion-time="45m"}


The RackWare RMM migration solution provides a seamless virtual-to-virtual replatforming for any cloud (AWS, Azure, GCP, and OCI (Baremetals)) to {{site.data.keyword.cloud}} virtual server instance migration, which allows the adoption of the existing capabilities of {{site.data.keyword.cloud_notm}}. Its intuitive GUI allows you to move the OS, application, and data from any cloud to {{site.data.keyword.vpc_short}} instances.

This guide shows you how to complete a migration from AWS, Azure, GCP, and OCI to {{site.data.keyword.vpc_short}}. 

## Supported operating systems

• CentOS 7.8, 7.9

• RHEL 7.2, 7.3, 7.4, 8.1

• Ubuntu 18.04, 20.04

• Debian 9, 10

• Windows 2012, 2012R2, 2016, 2019

## Architecture diagram
{: #any-cloud-architecture}

This diagram shows the architecture that you create with the guide.

![Architecture](images/any-cloud-1.png){: caption="Architecture Diagram"}

### General steps
{: #any-cloud-architecture}

1. Order RackWare RMM ({{site.data.keyword.cloud_notm}} catalog tile)
2. BYOL (Bring Your Own License) from RackWare
3. Establish connectivity between source server and {{site.data.keyword.vpc_short}} 
4. Set up and provision VPC and virtual server instance
5. Set up RackWare RMM waves for migration
6. Validate your migration

## Order RackWare RMM
{: #order-rackware}
{: step}

The RackWare RMM tool is available in the {{site.data.keyword.cloud_notm}} catalog. After you order, a virtual server with RackWare RMM software is installed into your VPC of choice. The RMM server has a public IP address for reachability and a default login.

1. Order the RackWare RMM server from the {{site.data.keyword.cloud_notm}} [catalog](https://cloud.ibm.com/catalog/content/IBM-MarketPlace-P2P-1.3-22935832-bd76-49ab-b53e-12fc5d04c266-global).
2. After you order, log in to the RackWare RMM server.
3. In the RMM server, change the default password, create users, and create an SSH key.
4. Upload the SSH key to {{site.data.keyword.vpc_short}}.

## BYOL (Bring Your Own License) from RackWare
{: #byol-rackware}
{: step}

The license required for migration to {{site.data.keyword.cloud_notm}} is "Bring Your Own License (BYOL)". The license is a subscription-based license (paid monthly) that asks you to migrate one or more servers during the subscription period. You need to purchase the license directly from RackWare.

Complete the following steps to get a license:

1. Order your license from RackWare.
2. Run the `rwadm relicense` command on the RMM CLI to generate a preinstall file.
3. After generating a preinstall file, send a license generation request to the RackWare licensing team with the following information:
    * RackWare RMM License (subject line)
    * Company name
    * License count
    * Preinstall file (attached)
    * Purchase order (attached)
4. Install the license.
5. After you receive a valid license, download the license file and place it under `/etc/rackware`. Restart the services to apply the license by running the following command:

    ```
    rwadm restart
    ```
    {: pre}

6. Validate the license by running the following command:
    
    ```
    rw rmm show
    ```
    {: pre}

## Establish connectivity between source server and IBM Cloud VPC
{: #connectivity-between-source-and-cloud}
{: step}

Your source and target need to be able to reach other and also the RMM. You can do this over the public internet using public IPs.

- Should have port 22 open with SSH accessible to RMM server

- Public interface 

## Set up and provision VPC and virtual server instance
{: #set-up-provision-source}
{: step}

There are two different methods for setting up target the VSI, manual or with Rackware RMM auto provision feature as described below:

### Option 1: Manual
{: #option-manual}

The RackWare RMM solution handles only the OS, application, and data movement. It does not set up a VPC on the target side. Therefore, you must set up the VPC infrastructure. At a bare minimum, you need to set up a VPC, subnets, and virtual server instances. This tutorial will not go through all of the details for setting up the VPC infrastructure. See the Virtual Private Cloud (VPC) documentation for further details.

1. Create a VPC.

2. Create subnets.

3. Order virtual server instance.

    - SSH key

    - Operating system name (Linux/Windows and their respective versions)

    - Security groups

    - Secondary volume (optional)
    
    Encrypted volumes are not supported.

### Option 2: Auto-provision
{: #option-auto}

1. Click **Cloud user** menu under the Configuration main menu on the left side.

2. Click Add button, and the **Add Cloud** form will be displayed. Enter the appropriate details for the following fields:

    - Name

    - Select ‘{{site.data.keyword.cloud}} VPC’ as Cloud Provider

    - Select the Region where you want to auto-provision VSI

    - Enter the valid API key for your {{site.data.keyword.cloud}} account

    - Once all the details are completed, click the **Add** button

3. Open the wave where the operation needs to be run

4. Click text **Not Configured**, next to the **Autoprovision** label, and a display window opens

    -  Select **added cloud user** as the Environment

    - Select the region where the VSI must be provisioned

    - Subnet name and VPC name are optional. If entered, these would be default names for the VPC and Subnet during the provision of the VSI

    - Click **Add Host**, (the plus icon in left top corner of the window)

    - Select the Target Type as Autoprovision

- Enter source details:

    - IP address

    - Friendly name

    - Select OS

    - Username

- Enter Target details:

    - Only a Friendly Name is required on the target side

    Use right Sizing from Advanced Options if the source has a boot volume greater than 250 GB, as VPC does not support boot volume greater than 250 GB
    {:note: .note}

    Once you close this form, RMM shows a warning to enter IBM Gen2 Options
    {:note: .note}


- Edit host and you see the IBM Gen2 Options as an extra tab at the top

    - The VPC name is mandatory. It creates VPC with given name if not present in that region. All other fields are optional. If no value is entered in optional fields, then RMM finds relevant resource.
    - Select Region
    - Resource Group
    - Subnet
    - Security Group
    - Zone
    - VSI Profile Name
    - Permit Resources Creation – Check this option if the given VPC name or subnet is not present. This is a simple permission flag for RMM to create resources.
    - Image Name
    - Image username (This field is optional, as Linux has key-based authentication, so even if any value is entered, it would be ignored)
    - Image password (This field is optional as Linux has key-based authentication so even if any value is entered, it would be ignored)
- SSH Keys: Enter either the name of the ssh key or the content of the public key to be present on the newly created VSI
- Click Modify
- Finally, run replication

The target VSI boot volume cannot be greater than 250 GB, so if the source machine’s boot volume is greater than 250 GB, use the right-sizing option of RMM.

If the **No Transfer** option is selected in **Sync Option**, it does an auto provision of the target but actual data and applications are not migrated.

Ensure that your VPC, subnet, and other necessary cloud components are setup before adding cloud user in RMM.

There are a few things that need to be done on the source and target server for the migration to work. The RackWare RMM server needs to SSH into the servers; thus, the RMM public SSH keys need to be copied onto both the source and target servers. In addition, if the source server has both public and private interfaces, host routes need to be added to ensure the communication between the source and target servers occurs over the transit gateway path. Complete the following steps to prepare your relevant servers.

### Linux systems
{: #linux-systems}

1. Copy the RackWare RMM SSH public key to both the source and target servers.
2. If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for the operating system:

```
ip route add <destination_network> via <Gateway_address> dev <private_ethernet_interface>
```
{: pre}

### Windows systems
{: #windows-systems}

1. Copy the RackWare RMM SSH public key to both the source and target servers.
2. You need to download the SSH key utility. You can download it from the Rackware RMM server: `<https://<RMM_IP>/windows/RWSSHDService_x64.msi>`

    `<RMM_IP>` is the IP address of your RMM server.
    {: note}

3. The user is `SYSTEM`, and you need to key in the RMM SSH key to authenticate for both the source and target servers.
4. If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for the operating system:

```
route ADD <destination_network> MASK <subnet_mask> <gateway_ip> <metric_cost>
```
{: pre}

If you use the auto-provision feature, there is no need to set up a target. Only the friendly name for the target server is required.
{: note}

## Set up RackWare RMM waves for migration
{: #setup-rackware-waves}
{: step}

You can migrate the servers over one by one or do simultaneous migrations. If you are doing multiple, simultaneous migrations, download the CSV template from the RMM server and complete the appropriate fields.

1. Log in to the RMM server.
2. Create a Wave and define Wave name.
3. If there are multiple hosts, download the template, complete the appropriate fields, and then upload the template.
4. Select the Wave name to enter source and target information.
5. Select the "+" sign.
6. Add source IP address or FQDN and add source username.
7. Target Type = Existing system
8. Sync Type = Direct sync
9. Add target IP address or FQDN.
10. Add a target-friendly name, and add target username.
11. Start the migration.

The username field for Linux environments is `root`.

The username field for Windows environments is `SYSTEM`.

## Validate your migration
{: #validating-your-migration}
{: step}

After your server migration, validate your compute resources, such as applications and data, and update or validate the following:

1. Access the target device.
2. Check partitions and volumes.
3. Check the applications.
4. Install any test application in target device.
5. Check networking routes.
6. Check or update your `yum` repos, application settings, and configuration file.
7. Check or update application or operating system licenses.
8. Remove the RMM SSH key.
