---

copyright:
  years:  2021, 2022
lastupdated: "2022-03-08"

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

RackWare’s RMM solution simplifies the overall migration process of moving the operating system, applications, and data from one bare metal server to another in the IBM cloud classic environment. The migration can occur either over the public or private interface of the compute resource. The only requirement is that RMM must be able to access both source and target server over SSH.

## Objective
{: #p-p-migration-bare-metal-objective}

- Prepare the source and target devices
- Learn what is needed for the supported network topology
- If bonding is configured, then learn how to work around it
- Learn how to use the RackWare RMM solution

## Limitations
{: #p-p-migration-bare-metal-limitations}

1.	NIC bonding is not supported in the Windows operating system. Before migration, NIC bonding must be unconfigured at hardware and OS levels for any server that has NIC bonding or teaming configured. With Linux operating systems, you do not need to make any changes.
2.	Encrypted volumes are not supported.
3.	Do not modify the target. If anything is modified out of control of RMM after the first migration, it can be wiped out and the result can be unexpected.
4.	The RackWare’s RMM solution handles only the OS, application, and data movement. So anything else needs to be set up by you (for example security groups, subnets, and so on).
5. For Windows, data migration for block and file performance and endurance storage is not supported. Consider that uses third-party tools such as `rsync` for data migration on block and file. For Linux it is supported.

## Supported operating systems
{: #p-p-migration-bare-metal-supported-os}

• CentOS 7.8, 7.9

• RHEL 7.2, 7.3, 7.4, 8.1

• Ubuntu 18.04, 20.04

• Debian 9, 10

• Windows 2012, 2012R2, 2016, 2019

## Supported topology
{: #p-p-migration-bare-metal-supported-topology}

Migration can be done over either the public or the private interface. While RMM uses SSH to communicate with the servers, migrate that uses the private address. In addition, you get to use IBM's backbone network during migration over the private interface.

Because RMM is deployed in IBM Cloud VPC, it requires a transit gateway for the RMM to communicate with the source and the target over the private interface. The source and target also need to be able to communicate with one another.

If the source and target does not have direct communication, then consider that uses pass-through with the RMM server. The RMM server acts as a proxy, and migration flows through the RMM server. For more information, search for pass-through in the [RMM user guide](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud). The pass-through migration time increases.
{:note: .note}

To create an IBM Cloud Transit Gateway and establish connection between classic and VPC, review the following information:

[Planning for IBM Cloud Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-helpful-tips)

[Ordering IBM Cloud Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway)


For transit gateway and classic, you need to follow two steps:

1. Enable vlan spanning to allow communication between subnets and DCs in classic.
See [VLAN spanning](https://cloud.ibm.com/docs/vlans?topic=vlans-vlan-spanning).

2. To allow communication between VPC and classic, both transit gateway and enabling VRF in classic are necessary.
See [Planning for IBM Cloud Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-helpful-tips#general-considerations).

![Topology](images/p-p_classic_private_ip_routing2.png "Diagram showing the cloud service models"){: caption="Figure 1. Network topology of RMM and bare metal migration" caption-side="bottom"}

## Before you begin
{: #p-p-migration-bare-metal-supported-topology}
{: step}

1. For each volume on the source server 20% of the unused space must be available to store the snapshot that created by RMM. 

2. Copy the RMM SSH public key to both the source and target.

    a. This process can require modifying the servers' host route table or security rules.

    b. Update the name server or DNS

3. The order of target server, the CPU, and memory does not need to match, but the volumes must be equal or greater than the source.

4. Make sure to have ``/etc/fstab`` entry for automatic mounting of any file system on the target device.

5. In case a pre-defined multipath is being used, block all devices that are not going to part of the multipath. These are typically disks for local use by the server, like the root disks and any other disks that are not from SAN Storage.

## Remove NIC bonding
{: #p-p-migration-bare-metal-removing-nic-bond}
{: step}

To accomplish successful migration that uses RackWare’s RMM solution, you must remove NIC bonding from both of the source and target devices, when employing the Windows operating system. Depending on the type of port redundancy selected at order time, it requires that a case is opened with IBM support to remove bonding configuration on both network and OS. See [port redundancy](https://cloud.ibm.com/docs/bare-metal?topic=bare-metal-network-options#network-port-redundancy) for additional information.

| Network interface - Port redundancy type | Comments | IBM case needed? |
| ---------- | ---------- | ---------- |
| Automatic | Both OS and switch needs to be reconfigured. | Yes. Follow the steps below. |
| User managed | Reconfigure the OS interface. | No, self-managed. |
| None | Depends, check OS if bonding is configured. | Yes only if bonding is configured on the OS.  Follow the steps below. |

1. Create a ticket with IBM support.

    a. Open the [Support Center](https://cloud.ibm.com/unifiedsupport/supportcenter).

    b. Click **Create a Case**.

    c. Complete or select the necessary details:

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
      
    * For **Add resources**, select that the bare metal servers that this request must be performed on.
    
    * For **Data center settings**, select the data center location.

2. After you have been notified that the changes have been completed, go back in to verify that the bonding interface is removed:

    a. For Windows&reg;, the PowerShell command ``Get-NetLbfoTeam`` can be used.

NIC bonding is supported for Linux operating system. No need to make changes on source and target devices that run the Linux operating system.
{:note: .note}

## Order a license
{: #p-p-migration-bare-metal-ordering-license}
{: step}

The Licenses required for migration to IBM Cloud are "bring your own license (BYOL)" . The license is a subscription-based license, paid monthly, that permits you to migrate one or more servers during the subscription period. You need to purchase the license directly from RackWare.

Follow these steps to get a license:

1. Order your license from [RackWare](mailto:sales@rackwareinc){: external}.

2. Run the ``rwadm relicense`` command on RMM CLI to generate a preinstall file.

3. After generating a preinstall file that uses the above method, send a license generation request to the [RackWare licensing team](mailto:sales@rackwareinc){: external} with the following information:

    a. RackWare RMM License (Subject line )

    b. CompanyName
    
    c. LicenseCount

    d. PreinstallFile (attached)

    e PurchaseOrder (attached)

4. Install the license.

    a. Once the valid license is received, download the license file and place it under ``/etc/rackware``, and restart the services to apply the license.

    ``$ rwadm restart``

    b. Verify the validity of the license.

    ``$ rw rmm show``

## Prepare source and target devices
{: #p-p-migration-bare-metal-source-target}
{: step}   

There are a few things that you need to do on the source and target device for the migration:

1. The RackWare RMM server needs to connect with devices that use SSH; thus the RMM public SSH keys need to be copied on both the source and target devices. 

2. If the source device has both public and private interfaces, host routes need to be added to ensure the communication between the source and target devices. This done over the transit gateway path. Complete the following steps to prepare customer relevant devices:

    **Linux systems**

    Copy RackWare’s RMM SSH public key to both the source and target devices.

    **Windows systems**

    a. Copy the RackWare RMM SSH public key to both the source and target devices.

    b. You need to download the SSH key utility. You can download it from the RackWare RMM server, 

    (For example ``https://<RMM IP>/windows/RWSSHDService_x64.msi``)

    c. You are SYSTEM, and you need to key in the RMM SSH key to authenticate for both the source and target devices.

## Set up RackWare’s RMM waves for P-P migration (BareMetal to BareMetal)
{: #p-p-migration-bare-metal-setup-waves}
{: step}  

You can migrate devices over one-by-one or perform simultaneous migrations. If you are performing multiple, simultaneous migrations, download the CSV template from the RMM server and complete the appropriate fields.

1. Log in to the RMM server.

2. Create a ***Wave*** and define the ***Wave*** name.

3. If there are multiple hosts, download the CSV format template, complete the appropriate fields, and then upload the CSV format template.

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

1. Once the source and target host are added in wave and replication record, click the **Sync Options** tab on right top of pop up screen,  select the **No Transfer** option and click **Modify**. Then click the play/arrow head icon to start replication. This will perform a dry run by checking the connection between the RMM and source/target devices. This will not migrate data. If the operation is successful then remove the **No Transfer** option that uses the same process.

2. Whenever you are ready, go ahead and click **start replication** (the play/arrow head icon on the left top). This will start the actual migration. If you expand the replication record, it displays actual steps being executed in summary with necessary information.

3. Whether the operation is successful or whether it failed, you can see the job history in the replication record.

4. In case of failure, you can retrieve the log and review detailed information.

To improve data transfer rate, adjust bandwidth allocation of RMM server. To know how to change bandwidth allocation, see [Adjusting bandwidth allocation that uses the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}

## Validate and perform post-migration activities
{: #p-p-migration-bare-metal-post-migration}
{: step}  

•	Access target device

•	Check partitions / volumes

•	Check applications

•	Install any test application in target device

•	Check networking routes

•	Application or operating system licenses

•	Remove RMM SSH key

•	After last data synch, reapply NIC bonding configuration if necessary, raise ticket to IBM support as described in [Remove NIC bonding](#p-p-migration-bare-metal-removing-nic-bond) with the only changes to the following areas:
   * **Subject** enter "RackWare bare metal migration bonding interface"
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





