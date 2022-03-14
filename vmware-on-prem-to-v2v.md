---

copyright:
  years:  2022
lastupdated: "2022-03-14"

keywords: image migration, migrate image, vmdk, vhd
content-type: tutorial
services: vpc, virtual-servers, RackWare RMM, IBM Cloud VPC VSI
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

# VMware VM On-Prem to IBM Cloud VPC VSI migration with RackWare RMM 
{: #migrating-images-vmware-vpc}
{: toc-content-type="tutorial"} 
{: toc-services="vpc, virtual-servers, RackWare RMM, IBM Cloud VPC VSI"} 
{: toc-completion-time="45m"}

To implement a data center transformation, RackWare RMM migration solution provides a seamless virtual-to-virtual replatforming for VMware virtual machine (VM) to IBM Cloud virtual server instance (VSI) migration. It allows the adoption of existing capabilities of IBM Cloud. Its intuitive GUI allows the user to move the OS, Application, and data from VMware ESXI to IBM VPC VSI.  
 
The steps show how to complete a V2V migration from VMware in On-Prem to IBM Cloud VPC. It supports migrating Windows Server 2012, 2012R2, 2016, and 2019, Red Hat Enterprise Linux (RHEL), CentOS, Ubuntu, and Debian Linux operating systems. 

![Topology](images/on-prem-final.png){: caption="Architecture Diagram"}
 
## Services used
{: #services-used-vmware}

- IBM Cloud VPC VSI 
- RackWare RMM 

## Before you begin
{: #before-begin-vmware}

- Check for correct permissions for IBM Cloud VPC. 
- While this list is not exhaustive, understand the capability differences between VMware and VPC such as: 
    - VPC does not support shared volumes or file-based volumes 
    - No GPU supports are allowed
    - Encrypted volumes are not supported

To improve data transfer rate, adjust bandwidth allocation of RMM server. To know how to change bandwidth allocation, see [Adjusting bandwidth allocation that uses the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}


## General steps

1.  Order RackWare RMM (IBM catalog tile) 

2. BYOL (Bring your own license) from RackWare 

3. Set up and provision IBM Cloud VPC and VSI

4. Connectivity between customer’s Data Center and IBM Cloud VPC 

5. Source and target preparation 

6. V2V migration setup 

7. Validation 

## Order RackWare RMM
{: #order-rackware-rmm-vmware}
{: step}

RackWare RMM tool is available on the IBM catalog marketplace. 

A VSI with the RackWare RMM software is installed into the VPC that was provided during the ordering process on the catalog page.
The RMM server uses a public IP address for reachability and to post provision the default login post for the VSI.

1. Order RackWare RMM server from the IBM Cloud Marketplace.

    a. Select your Resource Group.

    b. Enter the VPC in which the server is to be created.

    c. Enter the resource_group. 

    d. Provide the SSH key.

    e. Provide the API key.

    f. Provide the Region and Zone.

2. Login into the RackWare RMM Server.

    a. Change the default password.

    b. Create Users.

    c. Create an SSH Key.

    d. Upload the SSH key to the IBM Cloud VPC.
 
 ##  BYOL (Bring your own license) from RackWare
{: #license-rackware-bring-vmware}
{: step}

1. Generate a license file under the `/etc/rackware` by running this command:                    

        $ rwadm relicense 
 
    You need to purchase the license from RackWare by mailing the generated license file to licensing@rackwareinc.com or sales@rackwareinc.com. 

2. When the valid license is received, download the license file and place it under `/etc/rackware`. Restart the services to apply the license that use this command:
 
        $ rwadm restart 
 
3. Verify the validity of the license:

        $ rw rmm show 

## Connectivity options between the customer data center and the IBM Cloud VPC 
{: #connectivity-customer-vpc}
{: step}

- Use the [Direct Link](https://cloud.ibm.com/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) 2.0 connection to IBM Cloud. It is a costly solution and can be considered only if Direct Link 2.0 is already present. 

-  [Site-to-site VPN](https://cloud.ibm.com/docs/vpc?topic=vpc-vpn-overview)

- Public interface 

##  IBM Cloud VPC VSI setup
{: #cloud-vpc-vsi-setup}
{: step}

**Option 1: Manual**

The RackWare RMM solution handles the OS, application, and data movement. It does not need to set up a VPC target side; you need to handle the setup. You first set up the VPC infrastructure. At a bare minimum, you must set up a VPC, subnets, and the corresponding VSIs that you are planning to migrate. The new target VSI profile (vCPU and vMemory) does not need to match the source. However, as for the storage, it needs to be the same or greater in size.

This document does not provide the details for setting up the VPC infrastructure. It is described in each of the relevant VPC product document pages.
{: note}

1. Create a VPC. 
2. Create Subnets. 
3. Order the virtual server instance. 
    * SSH key (RMM SSH keys need to be added in addition to bastion SSH key)
    * Operating system name (Linux/Windows and their respective version) 
    * Security groups 
    * Secondary volume 

**Option 2: Auto-provision**

RMM can automatically provision a virtual server instance of VPC. Enable the wave level setting ``Autoprovision`` and then configure RMM with necessary details. Use these steps to use the auto-provision feature:

1. Click **Clouduser** menu under **Configuration** main menu on left side.
2. Click **Add** button, and the Add Cloud** form will pop up. Enter appropriate details for the following fields:
    - Name
    - Select ‘IBM cloud VPC’ as Cloud Provider
    - Select wanted Region where you want to auto provision VSI
    - Enter valid API key for your IBM cloud account
    - Once all details are filled, click **Add** button
3. Open wave where operation needs to be performed
4. Click text ’Not Configured’, next to ‘Autoprovision’ label, a pop up opens
    - Select added clouduser as **Environment**
    - Select region where VSI needs to be provisioned
    - Subnet name and VPC name are optional. If entered, these would be default names for VPC and Subnet during provision of VSI
    - Click **Add Host**, (plus icon on left top window)
    - Select **Target Type** as **Autoprovision**
    - Enter source details:

        - IP address
        - Friendly name
        - Select OS
        - Username
    - Enter Target details:
        - Only **Friendly Name** is required on target side
        - Use **right Sizing** from **Advanced Options** if the source has a boot volume greater than 250 GB, as VPC does not support boot volume greater than 250 GB
    - Once you close this form, RMM shows a warning to enter **IBM Cloud VPC Options**
    - Edit host and you see **IBM Cloud VPC Options** as an extra tab at the top
        - The VPC name is mandatory. It creates VPC with given name if not present in that region. All other fields are optional. If no value is entered in optional fields, then RMM finds relevant resource.
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
    - Click Modify
    - Finally, run replication
    
Target VSI boot volume cannot be greater than 250 GB, so if source machine’s boot volume is greater than 250 GB, use right sizing option of RMM.
{: note}

If "No Transfer" option is selected in "Sync Options", it does auto provision of target but actual data/applications are not migrated.
{: note}

Ensure that your VPC, subnet, and other necessary cloud components are setup before adding cloud user in RMM.
{: note}

## Source and Target compute preparation
{: #source-target-compute-prep-vmware}
{: step}

Before starting the migration, RackWare RMM server needs to SSH into the virtual machines. Thus, the RMM public SSH keys needs to be copied on both the source and target machines.
 
For Windows OS, you need to download the SSH key utility.  You can download it from RackWare RMM server. 
{:note: .note}
 
For Windows OS, the user is SYSTEM and you must key in the RMM SSH Key here to authenticate for both Source and Target machines.
{:note: .note}

In case, if user is using 'Auto Provision' feature, no need to setup target. Only friendly name for target machine is required.
{:note: .note}

## RackWare RMM V2V Migration
{: #rackware-rmm-v2v-migration}
{: step}

You can migrate the machines one-by-one or opt to run multiple simultaneous migrations. If you are running multiple simultaneous migrations, then download the CSV template from the RMM server and populate the appropriate fields.

1. Login into the RMM server.

2. Create a Wave.

    a. Define the Wave name.

    b. If multiple hosts, then download the template.

    - Populate the fields.
    - Upload the template.
 
3. Click the wave name to enter the source and target information:

    a. Click the plus sign.

    b. Add the source IP address or FQDN.

    c. Add the source username.

    d. Target Type = Existing System 

    e. Sync Type = Direct sync 
    
    f. Add the Target IP address or FQDN.

    g. Add the Target-friendly name.

    h. Add the Target username.

4. Start the migration 
 
The username field for the Linux environment is ‘root’. The username field for the Windows environment is 'SYSTEM'.
{:note: .note}
 
Alternatively, you can use the discovery helper script, which helps with the discovery of virtual machines on the VMware ESXI Host and also creates corresponding waves on the RMM server. The script asks for your vSphere host username, for the IP address of the vSphere to connect to, and the API where you discover your On-Prem or Classic VMware ESXI Host VMs. 
 
`$ ./discoveryTool -s <vSphere> -u <username of the Vspherehost>`

Example:

 `$ ./discoveryTool -s 10.10.10.9 -u administrator@vsphere.local`   
 
[For more information about discovering the VMware guest VMs that use discoveryTool](https://github.com/IBM-Cloud/vpc-migration-tools/blob/RMM-V2V-discoveryTool/v2v-discovery-tool-rmm/README.md){: external}

## Validation
{: #rackware-rmm-v2v-validation}
{: step}

Before decommissioning the source server, it is imperative to validate the target server. This following list is not exhaustive, but suggests some of the items to validate:

- Application 
- Licensing 
- Reachability (host level configuration changes) 
- Remove RMM SSH key after migration successful 
 
## More resources
{: #rackware-rmm-v2v-resources}


1.  [Discovery Tool](https://github.com/IBM-Cloud/vpc-migration-tools/blob/main/v2v-discovery-tool-rmm/README.md){: external}
2. [FAQs](/docs/cloud-infrastructure?topic=cloud-infrastructure-faqs-vmware)  
3. [RackWare usage guide](https://www.rackwareinc.com/cloud-migration){: external}