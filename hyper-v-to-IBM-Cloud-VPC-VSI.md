---

copyright:
  years:  2021, 2022
lastupdated: "2022-03-24"

keywords: migration, rackware rmm, hyper-v
content-type: tutorial
services: vpc, virtual-servers
account-plan: paid
completion-time: 60m
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

# Microsoft Hyper-V VM to IBM Cloud VPC migration with RackWare RMM
{: #migrating-images-vmware-vsi} 
{: toc-content-type="tutorial"} 
{: toc-services="vpc vsi, virtual-servers"} 
{: toc-completion-time="60m"}

The RackWare’s RMM solution simplifies the overall migration process of moving the operating system, applications, and data from Microsoft Hyper-V VM to {{site.data.keyword.cloud}} VPC virtual server instances. The migration can occur either over the public or private interface of the compute resource. The only requirement is that RMM is able to access both the source and target server over SSH.  
 
This guide demonstrates how to complete a migration from your Microsoft Hyper-V VM on premises or in {{site.data.keyword.cloud}} classic to {{site.data.keyword.cloud}} VPC. 

## Supported operating systems
{: #supported-operating-systems-vmware-virtual}

- RHEL 7.x, 8.x 
- Ubuntu 18.04.x, 20.04.x 
- Debian 9.x, 10.x 
- Windows 2012, 2012 R2, 2016, 2019 

## Before you begin
{: #before-begin-vmware-virtual}

- Check for correct permissions for {{site.data.keyword.cloud}} VPC. 
- For each volume on the source server, 20 percent of the unused space must be available to store the snapshot that is created by RMM. 
- Copy the RMM SSH public key to both the source and target servers.
    - This procedure can require modifying the servers' host route table or security rules. 
    - Update the name server or DNS 
- The order of the target server, the CPU, and memory does not need to match, but the volumes must be equal to or greater than the source. 
- Make sure to have a `/etc/fstab` entry for automatic mounting of any file system on the target server. 

To improve the data transfer rate, adjust the bandwidth allocation of the RMM server. For more information, see [Adjusting bandwidth allocation that uses the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}

## Order RackWare RMM
{: #order-rackware-rmm-catalog-vmware}
{: step}

You can order Rackware RMM-BYOL from the IBM catalog. A virtual server with the RackWare RMM software is installed into the VPC that you provide when you order. The RMM server has a public IP address for reachability.

1. Order [RackWare RMM BYOL](https://cloud.ibm.com/catalog/content/IBM-MarketPlace-P2P-1.3-22935832-bd76-49ab-b53e-12fc5d04c266-global) from the {{site.data.keyword.cloud}} catalog.

2. After you order, log in to the RackWare RMM server.

3. In the RMM server, change the default password, create users, and create an SSH key.

## Order a license
{: #license-rackware-bring-vmware-vpc}
{: step}

The license that you need to migrate to {{site.data.keyword.cloud}} VPC is a "Bring Your Own License (BYOL)". The license is a subscription-based license, paid monthly, that you use to migrate one or more servers during the subscription period. You need to purchase the license directly from RackWare. 

Follow these steps to get a license: 

1. Generate a license file under the `/etc/rackware` by running this command:                    

        $ rwadm relicense 
 
    You need to purchase the license from RackWare by mailing the generated license file to licensing@rackwareinc.com or sales@rackwareinc.com.

2. After generation, a preinstall file that uses this command, send a license generation request to the RackWare licensing team with the following information: 
    - RackWare RMM license (Subject line) 
    - CompanyName 
    - LicenseCount 
    - PreinstallFile (attached) 
    - PurchaseOrder (attached) 

3. Install the license.
After the valid license is received, download the license file and place it in `/etc/rackware`. Restart the services and apply the license by running the following command:
 
        $ rwadm restart 
 
4. Verify the validity of the license:

        $ rw rmm show 


##  Set up and provision VPC and virtual server instance
{: #cloud-vpc-vsi-rackware-setup}
{: step}

The RackWare RMM solution handles the OS, application, and data movement. It does not set up a target VPC. You must set up the VPC infrastructure and your virtual servers. At a bare minimum, you need to set up a VPC, subnets, and the corresponding virtual servers that you are planning to migrate. The new target instance profile (`vCPU` and `vMemory`) does not need to match the source.  However, as for the storage, it needs to be the same or greater in size. 

1. Create a VPC.

2. Create Subnets.

3. Order the virtual server instance:

    a. SSH Key (RMM SSH keys need to be added in addition to the bastion SSH key)

    b. OS name (same major version as the source)

    c. Security groups 

    d. Secondary volume 

Ensure that your VPC, subnet, and other necessary cloud components are set up before adding cloud user in RMM.
{: note}

## Prepare source and target servers
{: #source-target-compute-prep-vmware}
{: step}

There are a few things that you need to do on the source and target server for the migration:

1. The RackWare RMM server needs to connect with servers through SSH; thus the RMM public SSH keys need to be copied on both the source and target servers. 
 
2. If the source server has both public and private interfaces, host routes need to be added to ensure the communication between the source and target servers. This is done over the transit gateway path or Direct Link 2.0 connection to {{site.data.keyword.cloud}}. Complete the following steps to prepare your relevant servers: 
    - Linux systems- Copy RackWare’s RMM SSH public key to both the source and target servers. 

    - Windows systems:

        a. Copy the RackWare RMM SSH public key to both the source and target servers.

        b. You need to download the SSH key utility. This is done from the RackWare RMM server: 
        `<https://<RMM_IP>/windows/RWSSHDService_x64.msi>`

        c. You are SYSTEM, and you need to enter the RMM SSH key to authenticate for both the source and target servers. 

## Set up RackWare’s RMM waves
{: #rackware-rmm-v2v-vpc-migration}
{: step}

You can migrate the servers one-by-one or opt to do multiple simultaneous migrations. If you are running multiple simultaneous migrations, then download the CSV template from the RMM server and complete the appropriate fields.

1. Login into the RMM server.

2. Create a Wave.

    a. Define the Wave name.

    b. If you have multiple hosts, download the CSV template.

    - Complete the fields.
    - Upload the template.
 
3. Click the Wave name to enter the source and target information:

    a. Click the plus sign (+).

    b. Add the source IP address or FQDN.

    c. Add the source username (adding friendly name is optional).

    d. Target Type = Existing System 

    e. Sync Type = Direct Sync 
    
    f. Add the Target IP address or FQDN.

    g. Add the Target-friendly name (optional).

    h. Add the Target username. The username field for Linux environments is root. The username field for Windows environments is SYSTEM. 

Alternatively, you can use the discovery tool script, which helps with the discovery of virtual machines on the "System Center Virtual Machine Manager" (SCVMM) or Hyper-V host. The script also creates corresponding waves on the RMM server. The script asks for your SCVMM host username, the IP address of the SCVMM host to connect to, and the API where you discover your on-premises Hyper-V cluster virtual machines.

    $ ./discoveryTool -s SCVMM -u USERNAME -c CLUSTERNAME -d DOMAIN
Example:

    $ ./discoveryTool -s 10.10.10.1 -u administrator -c vCLUSTER -d DISCOVERY.LOCAL

Supported operating systems for Hyper-V discovery tool are Windows (2012, 2012
R2, 2016 and 2019) and RHEL 7.x and 8.x.
{:note: .note}

For more information about this click [here](https://github.com/IBM-Cloud/vpc-migration-tools/blob/main/v2v-discovery-tool-rmm/HyperV/README.md){: external}.

## Run the migration 
{: #rackware-rmm-perform-migration}
{: step}

1. When the source and target host are added in the wave and replication record, click the **Sync Options** tab in the right top region of the pop-up screen. Then, select the **No Transfer** option and click **Modify**. Then, click the play or arrow head icon to start the replication. This does a dry run by checking the connection between the RMM and source or target servers. This dry run does not migrate data. If the operation is successful, then remove the No Transfer option that uses the same process.
2. Whenever you are ready, go ahead and click start replication (the play or arrow head icon on the left top). This starts the actual migration. If you expand the replication record, it displays the actual steps tha are run, summarized with the necessary information. 
3. Whether the operation is successful or whether it failed, you can see the job history in the replication record. 
4. If there is a failure, you can retrieve the log and review detailed information. 
 

## Validate your migration
{: #rackware-rmm-v2v-rackware-validation}
{: step}

Before you decommission the source server, it is imperative to validate the target server. This is not an exhaustive list but see the following list of items to validate after your migration:

- Access the target server 
- Check partitions/volumes 
- Check applications 
- Install any test application in the target server 
- Check networking routes 
- Application or operating system licenses 
- Remove RMM SSH keys  
 
## Other resources
{: #rackware-rmm-v2v-resources}

1. [Discovery Tool](https://github.com/IBM-Cloud/vpc-migration-tools/blob/main/v2v-discovery-tool-rmm/HyperV/README.md)

2. [RackWare Cloud Migration](https://www.rackwareinc.com/cloud-migration)

3. [RackWare RMM Users Guide for IBM Cloud](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud)
