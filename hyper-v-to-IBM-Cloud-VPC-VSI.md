---

copyright:
  years:  2021
lastupdated: "2022-03-08"

keywords: image migration, migrate image, vmdk, vhd

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

# Microsoft Hyper-V VM to Virtual (IBM VPC VSI) migration with RackWare
{: #migrating-images-vmware-vsi} 

The RackWare’s RMM solution simplifies the overall migration process of moving the operating system, applications, and data from Microsoft Hyper-V VM to IBM Cloud VPC VSI. The migration can occur either over the public or private interface of the compute resource. The only requirement is that RMM needs to be able to access both source and target server over SSH.  
 
We demonstrate how to complete a migration from Microsoft Hyper-V VM in On-Prem or IBM Classic to IBM Cloud VPC VSI. 

 
## Services used
{: #services-used-vmware-virtual}

- IBM Cloud VPC VSI 
- RackWare RMM 

## Supported operating systems
{: #supported-operating-systems-vmware-virtual}

- RHEL 7.x, 8.x 
- Ubuntu 18.04.x, 20.04.x 
- Debian 9.x, 10.x 
- Windows 2012, 2012 R2, 2016, 2019 

## Before you begin
{: #before-begin-vmware-virtual}

- Check for correct permissions for IBM Cloud VPC. 
- For each volume on the source server, 20% of the vacant space must be available to store the snapshot that created by RMM. 
- Copy the RMM SSH public key to both the source and target.
    - This procedure can require modifying the servers' host route table or security rules. 
    - Update the name server or DNS 
- The order of the target server, the CPU, and memory does not need to match, but the volumes must be equal to or greater than the source. 
- Make sure to have /etc/fstab entry for automatic mounting of any file system on the target device. 

To improve data transfer rate, adjust bandwidth allocation of RMM server. To know how to change bandwidth allocation, see [Adjusting bandwidth allocation that uses the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}

## General steps
{: #general-steps-vmware-virtual}

1.  Order RackWare RMM - BYOL (IBM Catalog Tile) 

2. BYOL (Bring Your Own License) from RackWare 

3. Set up and provision IBM Cloud VPC and VSI

4. Prepare the source and target 

5. Set up RackWare’s RMM wave for migration

6. Run migration 

7. Validation and Post-migration activities

## Order RackWare RMM catalog
{: #order-rackware-rmm-catalog-vmware}
{: step}

RackWare RMM - BYOL catalog is available on the IBM Catalog Marketplace. A VSI with the RackWare RMM software is installed into the VPC that was provided on the Catalog page. The RMM server has a public IP address for reachability. 

1. Order RackWare BYOL server from the IBM Cloud Marketplace.

    a. Select your Resource Group.

    b. Enter the VPC where this must be created.

    c. Enter the subnet. 

    d. Provide the SSH key.

    e. Provide the API key.

    f. Define the Region and Zone.

2. Login into the RackWare RMM Server.

    a. Change the default password.

    b. Create Users.

    c. Create an SSH Key.
 
 ##  BYOL (Bring Your Own License) from RackWare
{: #license-rackware-bring-vmware-vpc}
{: step}

The licenses that you need for migration to IBM Cloud are "Bring Your Own Licenses (BYOL)". The license is a subscription-based license that is paid monthly so you can migrate one or more servers during the subscription period. You must purchase the license directly from RackWare. 

Follow these steps to get a license: 

1. Order your license from RackWare. 

2. Generate a license file under the `/etc/rackware` by running this command:                    

        $ rwadm relicense 
 
    You must purchase the license from RackWare by mailing the generated license file to licensing@rackwareinc.com or sales@rackwareinc.com. 
3. After you generate a preinstall file that uses this command, send a license generation request to the RackWare licensing team with this information: 
    - RackWare RMM License (Subject line) 
    - CompanyName 
    - LicenseCount 
    - PreinstallFile (attached) 
    - PurchaseOrder (attached) 

2. Install the license.
Once the valid license is received, download the license file and place it under `/etc/rackware`.  Restart the services to apply the license that use this command:
 
        $ rwadm restart 
 
3. Verify the validity of the license:

        $ rw rmm show 


##  IBM Cloud VPC VSI setup
{: #cloud-vpc-vsi-rackware-setup}
{: step}

The RackWare RMM solution handles the OS, application, and data movement. It does not set up a VPC target side; you will need to handle this. You will first need to set up the VPC infrastructure. At a bare minimum, you will need to set up a VPC, subnets, and the corresponding VSIs that you are planning to migrate. The new target VSI profile (`vCPU` and `vMemory`) does not need to match the source.  However, as for the storage, it will need to be the same or greater in size. 

This document does not provide the details for setting up the VPC infrastructure as this is well described in each of the relevant VPC product document pages.
{:note: .note}

1. Create a VPC.

2. Create Subnet(s).

3. Order the VSI:

    a. SSH Key (RMM SSH keys need to be added in addition to bastion SSH key)

    b. OS name (same major version as the source)

    c. Security Group(s) 

    d. Secondary Volume (optional) 

## Source and Target compute preparation
{: #source-target-compute-prep-vmware}
{: step}

There are a few things that you need to do on the source and target device for the migration:

1. The RackWare RMM server needs to connect with devices via SSH; thus the RMM public SSH keys need to be copied on both the source and target devices. 
 
2. If the source device has both public and private interfaces, host routes need to be added to ensure the communication between the source and target devices. This is done over the transit gateway path / Direct Link 2.0 connection to IBM Cloud. Complete these steps to prepare customer relevant devices: 
    - Linux systems- Copy RackWare’s RMM SSH public key to both the source and target devices. 
    - Windows systems:

        a. Copy the RackWare RMM SSH public key to both the source and target devices.

        b. You need to download the SSH key utility. This can be done from the RackWare RMM server, 
        (e.g. https://<rmm IP>/windows/RWSSHDService_x64.msi) 

        c. You are SYSTEM, and you need to enter the RMM SSH key to authenticate for both the source and target devices. 

## Set up RackWare’s RMM waves for migration (Microsoft Hyper-V to IBM Cloud VPC VSI)
{: #rackware-rmm-v2v-vpc-migration}
{: step}

You can migrate the devices one-by-one or opt to perform multiple simultaneous migrations. If you are running multiple simultaneous migrations, download the CSV template from the RMM server and fill in the appropriate fields.

1. Login into the RMM server.

2. Create a Wave.

    a. Define the Wave name.

    b. If multiple hosts, download the CSV template.

    - Fill in the fields.
    - Upload the template.
 
3. click the wave name to enter the source and target information:

    a. Click the plus sign.

    b. Add the source IP address or FQDN.

    c. Add the source Username (adding friendly name is optional).

    d. Target Type = Existing System 

    e. Sync Type = Direct Sync 
    
    f. Add the Target IP address or FQDN.

    g. Add the Target friendly name (optional).

    h. Add the Target Username. The username field for Linux environments is root. The username field for Windows environments is SYSTEM. 

## Run the migration 
{: #rackware-rmm-perform-migration}
{: step}

1. Once the source and target host are added in the wave and replication record, click the **Sync Options** tab in the right top region of the pop-up screen, select the **No Transfer** option and click **Modify**. Next click the play or arrow head icon to start the replication. This will perform a dry run by checking the connection between the RMM and source or target devices. This dry run does not migrate data. If the operation is successful, remove the No Transfer option that uses the same process.
2. Whenever you are ready, go ahead and click start replication (the play/arrow head icon on the left at the extreme top of the screen). This will start the actual migration. If you expand the replication record, it displays the actual steps that are being run summarized with the necessary information. 
3. Whether the operation is successful or whether it failed, you can see the job history in the replication record. 
4. In an instance of failure, you can retrieve the log and review detailed information. 
 

## Validation
{: #rackware-rmm-v2v-rackware-validation}

Before you decommission the source server, it is imperative to validate the target server. This is not an exhaustive list but some of the items to validate are:

- Access the target device 
- Check partitions and volumes 
- Check applications 
- Install any test application in the target device 
- Check networking routes 
- Application or operating system licenses 
- Remove RMM SSH keys  
 
## Other resources
{: #rackware-rmm-v2v-resources}

1. RackWare documentation

    a. RackWare Cloud Migration 
    
    b. RackWare RMM Users Guide for IBM Cloud 
2. FAQs  
