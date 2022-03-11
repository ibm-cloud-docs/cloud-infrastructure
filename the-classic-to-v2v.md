---

copyright:
  years:  2021, 2022
lastupdated: "2022-03-11"

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

# VMWare VM (classic) to IBM Cloud VPC VSI migration with RackWare RMM  
{: #migrating-images-vmware-vpc-classic}

When initiating a complete data center transformation, RackWare RMM migration solution provides a seamless virtual-to-virtual replatforming for VMWare virtual machine (VM) to IBM Cloud virtual server instance (VSI) migrations. You can adopt the capabilities of IBM Cloud. Its intuitive GUI permits you to move the OS, Application, and data from VMware ESXI Classic to IBM VSI.  
 
We show you how to complete a V2V migration from VMware IBM Classic to IBM Cloud VPC. It supports migrating Windows Server 2012, 2012R2, 2016, and 2019, Red Hat Enterprise Linux (RHEL), CentOS, Ubuntu, and Debian Linux operating systems. 

![Topology](images/Classic.png){: caption="Architecture Diagram"}
 
## Services used
{: #services-used-classic}

- IBM Cloud VPC VSI 
- RackWare RMM 
- IBM Cloud Transit Gateway

## Before you begin
{: #before-begin-classic}

- Check for correct permissions for IBM Cloud VPC. 
- While this is not an exhaustive list, but understand the capability differences between VMWare and VPC such as: 
    - VPC does not support shared volumes or file-based volumes 
    - No GPU supports are allowed
    - Encrypted volumes are not supported

To improve data transfer rate, adjust bandwidth allocation of RMM server. To know how to change bandwidth allocation, see [Adjusting bandwidth allocation that uses the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}

## General steps

1.  Order RackWare RMM (IBM Catalog Tile) 

2. BYOL (Bring Your Own License) from RackWare 

3. (Optional) Order IBM Cloud Transit Gateway if you want to migrate over the private interface or if there is no internet connectivity/public interface. 

4. Set up and provision IBM Cloud VPC and VSI

5. Source and target preparation 

6. V2V migration setup 

7. Validation 

## Order RackWare RMM
{: #order-rackware-rmm-classic}
{: step}

RackWare RMM tool is available on the IBM Catalog Marketplace. 

A VSI with the RackWare RMM software will be installed into the VPC, which was provided while ordering on the Catalog page.
The RMM server has a public IP address for reachability and for default login post provisioning the VSI.

1. Order RackWare RMM server from the IBM Cloud Marketplace.

    a. Select your Resource Group.

    b. Enter the VPC in which this should be created.

    c. Enter the resource_group. 

    d. Provide the SSH key.

    e. Provide the API key.

    f. Provide the region and zone.

2. Login into the RackWare RMM Server.

    a. Change the default password.

    b. Create Users.

    c. Create an SSH Key.

    d. Upload the SSH key to the IBM Cloud VPC.
 
 ##  BYOL (Bring Your Own License) from RackWare
{: #license-rackware-bring-classic}
{: step}

1. Generate a license file under the `/etc/rackware` by running this command:                    

        $ rwadm relicense 
 
    You need to purchase the license from RackWare by mailing the generated license file to licensing@rackwareinc.com or sales@rackwareinc.com. 

2. Once the valid license is received, download the license file and place it under `/etc/rackware`. Then, restart the services to apply the license that uses this command:
 
        $ rwadm restart 
 
3. Verify the validity of the license:

        $ rw rmm show 

## (Optional) Transit Gateway for private connectivity
{: #connectivity-private-vpc}
{: step}

The IBM Cloud Transit Gateway provides connectivity between VMware IBM Classic and your VPC infrastructure. When connecting to the two entities, be aware that the VMware Classic infrastructure is on the 10.0.0.0 network and thus the VPC network needs to reside on a different network.  Otherwise, the network overlaps and there will be a communication connectivity issue. 
 
1. Order IBM Transit Gateway.

    a. Use local routing option 

2. Add connections.

    a. Classic Infrastructure 

    b. VPC 

    - Select Region 

##  IBM Cloud VPC VSI setup
{: #cloud-vpc-vsi-setup}
{: step}

The RackWare RMM solution only handles the OS, application, and data movement. It does not to set up a VPC target side; you need to handle this. You will first need to set up the VPC infrastructure. At a bare minimum, you need to set up a VPC, subnets, and the corresponding VSIs that you are planning to migrate. The new target VSI profile (`vCPU` and `vMemory`) does not need to match the source.  However, as for the storage, it needs to be the same or greater in size. 

This document does not provide the details for setting up the VPC infrastructure as this is well described in each of the relevant VPC product document pages.
{:note: .note}

1. Create a VPC.

2. Create Subnets.

3. Order the VSI:

    a. SSH Key (RMM SSH keys need to be added in addition to bastion SSH key)

    b. OS name (Same major version as the source)

    c. Security Groups 

    d. Secondary Volume (optional) 

## Source and Target compute preparation
{: #source-target-compute-prep-vmware}
{: step}

Before starting the migration, RackWare RMM server needs to SSH into the machines. Thus, the RMM public SSH keys need to be copied on both the source and target machines. 
 
For Windows OS, you need to download the SSH key utility.  You can download it from RackWare RMM server. 
{:note: .note}
 
For Windows OS, the user is SYSTEM and you must key in the RMM SSH Key here to authenticate for both Source and Target machines.
{:note: .note}

## RackWare RMM V2V Migration
{: #rackware-rmm-migration-classic}
{: step}

You can migrate the machines one-by-one or opt to perform multiple simultaneous migrations. If you are performing multiple simultaneous migrations, then download the CSV template from the RMM server and complete the appropriate fields.

1. Login into the RMM server.

2. Create a Wave.

    a. Define the Wave name.

    b. If multiple hosts, then download the template.

    - Complete the fields.
    - Upload the template.
 
3. Click the wave name to enter the source and target information:

    a. Click the plus sign.

    b. Add the source IP address or FQDN.

    c. Add the source Username.

    d. Target Type = Existing System 

    e. Sync Type = Direct Sync 
    
    f. Add the Target IP address or FQDN.

    g. Add the Target-friendly name.

    h. Add the Target Username.

4. Start the migration 
 
The username field for the Linux environment will be ‘root’. The username field for the Windows environment will be 'SYSTEM'.
{:note: .note}
 
Alternatively, you can use the discovery helper script, which helps with the discovery of virtual machines on the VMware ESXI Host and also creates corresponding waves on the RMM server. The script asks for your vSphere host username and for the IP address of the vSphere to connect to and the API where you discover your On-Prem/Classic VMware ESXI Host VMs. 
 
`$ ./discoveryTool -s <vSphere> -u <username of the Vspherehost>`

Example:

 `$ ./discoveryTool -s 10.10.10.9 -u administrator@vsphere.local`   
 
[For more information about discovering the VMware guest VMs that use discoveryTool](https://github.com/IBM-Cloud/vpc-migration-tools/blob/RMM-V2V-discoveryTool/v2v-discovery-tool-rmm/README.md)

## Validation
{: #rackware-rmm-validation-classic}

Before decommissioning the source server, it is imperative to validate the target server. This is not an exhaustive list but some of the items to validate are:

- Application 
- Licensing 
- Reachability (host level configuration changes) 
- Remove RMM SSH key after migration successful 
 
## More resources
{: #rackware-rmm-resources-classic}

1.  [Discovery Tool](https://github.com/IBM-Cloud/vpc-migration-tools/blob/main/v2v-discovery-tool-rmm/README.md)
2. [FAQs](/docs/cloud-infrastructure?topic=cloud-infrastructure-faqs-vmware)  
3. [RackWare usage guide](https://www.rackwareinc.com/cloud-migration) 
