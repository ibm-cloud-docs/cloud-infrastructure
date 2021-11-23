---

copyright:
  years:  2021
lastupdated: "2021-11-16"

keywords: migration, migrate, migrating, migrate infrastructure

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

# IBM classic bare metal to bare metal migration overview
{: #p-p-migration-bare-metal-overview}

The RackWare’s RMM solution simplifies the overall migration process of moving the operating system, applications, and data from one bare metal server to another bare metal server in the IBM cloud classic environment. The migration can occur either over the public or private interface of the compute resource. The only requirement is that RMM should be able to access both source and target server over SSH.

## Objective
{: #p-p-migration-bare-metal-objective}

- Prepare the source and target machines
- Learn what is needed for the supported network topology
- If bonding is configured, then learn how to work around it
- Learn how to use the RackWare RMM solution

## Limitations
{: #p-p-migration-bare-metal-limitations}

1.	NIC bonding is not supported in the Windows operating system. Before migration, NIC bonding must be unconfigured at hardware and OS levels for any server that has NIC bonding or teaming configured. In the case of Linux operating systems you do not need to make any changes.
2.	Encrypted volumes are not supported.
3.	Do not modify the target. If anything is modified out of control of RMM after the first migration, it can be wiped out and the result can be unexpected.
4.	The RackWare’s RMM solution handles only the OS, application, and data movement. So anything else needs to be setup by you (e.g. security groups, subnets etc.).
5. For Windows, data migration for block and file performance and endurance storage is not supported. Consider using third party tools such as `rsync` for data migration on block and file. For Linux it is supported.

## Supported operating systems
{: #p-p-migration-bare-metal-supported-os}

• CentOS 7.8, 7.9, 8.1, 8.2

• RHEL 7.2, 7.3, 7.4, 8.1

• Ubuntu 18.04, 20.04

• Debian 9, 10

• Windows 2012, 2012R2, 2016, 2019

## Supported topology
{: #p-p-migration-bare-metal-supported-topology}

Migration can be done over either the public or the private interface. While RMM uses SSH to communicate with the servers, it is recommended that you migrate using the private address. In addition, you get to leverage IBM's backbone network when migrating over the private interface.

Because RMM is deployed in IBM Cloud VPC, it requires a transit gateway for the RMM to communicate with the source and the target over the private interface. The source and target also need to be able to communicate with one another.

If the source and target does not have direct communication, then consider using passthrough with the RMM server. The RMM server acts as a proxy, and migration flows through the RMM server. For more information, search for passthrough in the [RMM user guide](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud). With passthrough migration time will likely increase.
{:note: .note}

To create an IBM Cloud Transit Gateway and establish connection between classic and VPC, review the following information:

[Planning for IBM Cloud Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-helpful-tips)

[Ordering IBM Cloud Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway)


For transit gateway and classic, there are 2 steps that are required:

1. Enable vlan spanning to allow communication between subnets/DCs in classic.
See [VLAN spanning](https://cloud.ibm.com/docs/vlans?topic=vlans-vlan-spanning).

2. To allow communication between VPC and classic, both transit gateway and enabling VRF in classic are required.
See [Planning for IBM Cloud Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-helpful-tips#general-considerations).

![Topology](images/p-p_classic_private_ip_routing2.png "Diagram showing the cloud service models"){: caption="Figure 1. Network topology of RMM and bare metal migration" caption-side="bottom"}

## Before you begin
{: #p-p-migration-bare-metal-supported-topology}
{: step}

1. For each volume on the source server  20% of the free space should be available to store the snapshot that will be created by RMM. 

2. Copy the RMM SSH public key to both the source and target.

    a. This may require modifying the servers' host route table or security rules.

    b. Update the name server or DNS

3. The order of target server, the CPU, and memory does not need to match, but the volumes should be equal or greater than the source.

4. Make sure to have ``/etc/fstab`` entry for automatic mounting of any file system on the target machine.

5. In case a native multipath is being used, blacklist all devices that are not going to part of the multipath. These are typically disks for local use by the server, like the root disks and any other disks that are not from SAN Storage.

## Remove NIC bonding
{: #p-p-migration-bare-metal-removing-nic-bond}
{: step}

To accomplish successful migration using RackWare’s RMM solution, you have to remove NIC bonding from both of the source and target machines, when using the Windows operating system. Depending on the type of port redundancy selected at the time of order, this may require a case to be opened with IBM support to remove the bonding configuration on both the network and OS.  See [port redundancy](https://cloud.ibm.com/docs/bare-metal?topic=bare-metal-network-options#network-port-redundancy) for more information.

| Network interface - Port redundancy type | Comments | IBM case needed? |
| ---------- | ---------- | ---------- |
| Automatic | Both OS and switch needs to be reconfigured. | Yes. Follow the steps below. |
| User managed | Reconfigure the OS interface. | No, self managed. |
| None | Depends, check OS if bonding is configured. | Yes only if bonding is configured on the OS.  Follow the steps below. |

1. Create a ticket with IBM support.

    a. Open the [Support Center](https://cloud.ibm.com/unifiedsupport/supportcenter).

    b. Click on **Create a Case**.

    c. Fill out or select the necessary details:

    * For **More categories**, select **Compute**

    * For **Topic** select, **Bare Metal Server**

    * For **Subtopic** select **Other**
    
    * Click **Next**

    d. For **Details** add the following information:
    
    * For **Subject** enter "RackWare bare metal migration unbonding interface"

    * For **Description** add the following details:
      * Remove bonding on both OS and network switches.
      * Time and date when to perform the changes.
      * Written permission to permit access the bare metal server to reconfigure the OS interface and rebooting the bare metal server.
      
    * For **Add resources**, select that the bare metal servers that this request should be performed on.
    
    * For **Data center settings**, select the data center location.

2. After you have been notified that the changes have been completed, go back in to verify the bonding interface has been removed:

    a. For Windows&reg;, the PowerShell command ``Get-NetLbfoTeam`` can be used.

NIC bonding is supported for Linux operating system. No need to make any changes to source and target machines that run the Linux operating system.
{:note: .note}

## Order a license
{: #p-p-migration-bare-metal-ordering-license}
{: step}

The Licenses required for migration to IBM Cloud is "bring your own license (BYOL)" .  The license is a subscription-based license, paid monthly, that enables you to migrate one or more servers during the subscription period. You will need to purchase the license directly from RackWare.

Follow these steps to get a license:

1. Order your license from [Rackware](mailto:sales@rackwareinc){: external}.

2. Run the ``rwadm relicense`` command on RMM CLI to generate a preinstall file.

3. After generating a preinstall file using the above method, send a license generation request to the [RackWare licensing team](mailto:sales@rackwareinc){: external} with the following information:

    a. RackWare RMM License ( Subject line )

    b. CompanyName
    
    c. LicenseCount

    d. PreinstallFile (attached)

    e  PurchaseOrder (attached)

4. Install the license.

    a. Once the valid license is received, download the license file and place it under ``/etc/rackware``, and restart the services to apply the license.

    ``$ rwadm restart``

    b. Verify the validity of the license.

    ``$ rw rmm show``

## Prepare source and target machines
{: #p-p-migration-bare-metal-source-target}
{: step}   

There are a few things that you need to do on the source and target machine for the migration:

1. The RackWare RMM server needs to connect with machines via SSH; thus the RMM public SSH keys need to be copied on both the source and target machines. 

2. If the source machine has both public and private interfaces, host routes need to be added to ensure the communication between the source and target machines. This done over the transit gateway path. Complete the following steps to prepare customer relevant machines:

    **Linux systems**

    Copy RackWare’s RMM SSH public key to both the source and target machines.

    **Windows systems**

    a. Copy the RackWare RMM SSH public key to both the source and target machines.

    b. You need to download the SSH key utility. You can download it from the RackWare RMM server, 

    (e.g. ``https://<RMM IP>/windows/RWSSHDService_x64.msi``)

    c. You are SYSTEM, and you need to key in the RMM SSH key to authenticate for both the source and target machines.

## Set up RackWare’s RMM waves for P-P migration (BareMetal to BareMetal)
{: #p-p-migration-bare-metal-setup-waves}
{: step}  

You can migrate machines over one-by-one or perform simultaneous migrations. If you are performing multiple, simultaneous migrations, download the CSV template from the RMM server and complete the appropriate fields.

1. Log in to the RMM server.

2. Create a ***Wave*** and define the ***Wave*** name.

3. If there are multiple hosts, download the CSV format template, fill in the appropriate fields, and then upload the CSV format template.

4. Select the ***Wave*** name to enter source and target information.

5. Select the "***+***" sign.

6. Add the source IP address or FQDN and add source username, (adding friendly name is optional).

7. Target Type = Existing System

8. Sync Type = Direct Sync

9. Add target IP address or FQDN.

10. Add a target-friendly name, and add the target username, (adding friendly name is optional).

11. The username field for Linux environments is ***root***. The username field for Windows environments is ***SYSTEM***.

## Perform migration
{: #p-p-migration-bare-metal-perform-migration}
{: step}  

1. Once the source and target host are added in wave and replication record, click on the **Sync Options** tab on right top of pop up screen,  select the **No Transfer** option and click on **Modify**. Then click on the play/arrow head icon to start replication. This will perform a dry run by checking the connection between the RMM and source/target machines. This will not migrate data. If the operation is successful then remove the **No Transfer** option using the same process.

2. Whenever you are ready, go ahead and click on **start replication** (the play/arrow head icon on the left top). This will start the actual migration. If you expand the replication record, it displays actual steps being executed in summary with necessary information.

3. Whether the operation is successful or whether it failed, you can see the job history in the replication record.

4. In case of failure, you can retrieve the log and review detailed information.

## Validate and perform post-migration activities
{: #p-p-migration-bare-metal-post-migration}
{: step}  

•	Access target machine

•	Check partitions / volumes

•	Check applications

•	Install any test application in target machine

•	Check networking routes

•	Application or operating system licenses

•	Remove RMM SSH key

•	After last data synch, reapply NIC bonding configuration if necessary, raise ticket to IBM support as described in [Remove NIC bonding](#p-p-migration-bare-metal-removing-nic-bond) with the only changes to the following areas:
   * **Subject** enter "Rackware bare metal migration bonding interface"
   * **Description** add the following details:
      * Bond both OS and network switches.
      * Type of bonding: Automatic or None
      * Time and date when to perform the changes.
      * Written permission to permit access the bare metal server to reconfigure the OS interface and rebooting the bare metal server.

## Help
{: #p-p-migration-bare-metal-help}

1. RackWare documentation

    a. [RackWare Cloud Migration](https://www.rackwareinc.com/cloud-migration){: external}

    b. [RackWare RMM Users Guide for IBM Cloud](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud){: external}


2.  FAQs about baremetal to metal

    [FAQS for bare metal to bare metal](/docs/cloud-infrastructure?topic=cloud-infrastructure-faqs-for-bare-metal-to-bare-metal)





