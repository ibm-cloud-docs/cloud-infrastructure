---

copyright:
  years:  2021
lastupdated: "2021-05-17"

keywords: migration, migrate, migrating, migrate data, data migration

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

# Migrating data from IBM Cloud classic infrastructure to VPC
{: #data-migration-classic-to-vpc}

The following guide shows you how to connect your {{site.data.keyword.cloud}} classic infrastructure to VPC to migrate your data, especially for block or file volumes, as part your migration journey. While there are many different data migration tools, this guide uses `rsync`. `Rsync` is an open source utility that provides file transfer between two devices. It is available for both Linux and Windows platforms. 
{: shortdesc}

You can use other data migration tools instead of `rsync` to move your data to VPC.
{: note}

## Before you begin 
{: #before-you-begin-data-migration}

Before you begin migrating your data, review the following requirements and considerations:

1. You need an existing VPC environment. 
2. You are aware of the storage capabilities and differences between classic and VPC. 
3. Back up your data before you begin the migration process. 

## Step 1: Create IBM Cloud Transit Gateway and establish connection between classic and VPC
{: #step-1-create-ibm-cloud-transit-gateway-data-migration}

Your classic infrastructure and VPC infrastructure need to be able to reach each other, which can be achieved in many ways: public interfaces, VPN, transit gateway. If you need to move data from a few machines, you might want to use a public interface. However, if there is a lot of data from different sources or large data sets, then you should use {{site.data.keyword.tg_full_notm}}, which uses {{site.data.keyword.cloud_notm}} to interconnect between classic and VPC.  

Before you create your {{site.data.keyword.tg_full_notm}}, review the following requirements and considerations:

1. Make sure that VRF is enabled on the classic infrastructure.
2. Make sure that the IP network spaces don't overlap. Your VPC IP address should not be present in the classic infrastructure IP range. 
3. Ensure that your classic infrastructure data centers are able to connect to VPC. 

To create {{site.data.keyword.tg_full_notm}} and establish connection between classic and VPC, review the following information:
* [Planning for {{site.data.keyword.tg_full_notm}}](/docs/transit-gateway?topic=transit-gateway-helpful-tips)
* [Ordering {{site.data.keyword.tg_full_notm}}](/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway)

If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for your relevant operating system:

### Linux systems
{: #linux-systems-add-route}

```
ip route add <destination_network> via <Gateway_address> dev <private_ethernet_interface>
```
{: pre}

### Windows systems
{: #windows-systems-add-route}

```
route ADD <destination_network> MASK <subnet_mask> <gateway_ip>
```
{: pre}


## Step 2: Install `rsync`
{: #step-2-install-rsync}
 
### Linux systems
{: #linux-systems-install-rsync}
On most Linux systems, `rsync` is already installed. To verify whether `rsync` in installed, run the `rsync` command on your terminal. If it isn't installed, complete the following steps for your relevant operating system.

#### Ubuntu
{: #install-rsync-ubuntu}

1. You need to be the "root" user to install the package. 
2. Run `sudo apt-get install rsync`. 

#### Debian
{: #install-rsync-debian}

1. Log in with root. 
2. Run `apt-get update`. 

#### RHEL
{: #install-rsync-rhel}

1. Log in with root. 
2. Run `yum install rsync`.

#### CentOS
{: #install-rsync-centos}

1. Log in with root.
2. Run `yum -y install rsync`. 

### Windows systems
{: #windows-systems-install-rsync}

Complete the following steps to install `rsync` on your Windows 2012, 2012R2, or 2016 system. 

1. Download Cygwin setup-x86_64 (https://cygwin.com/install.html). 
2. Install downloaded setup (https://cygwin.com/setup-x86_64.exe). 
3. Follow through all of the steps until you see a list of all Linux packages. 
4. Select `rsync` (in net category section). 
5. Select OpenSSH (in net category section). 
6. Click Continue and finish installation. 
7. Open ‘Cygwin Terminal’, type `rsync` command, and press enter. 
8. If the output shows `rsync` command details with options, then it is installed. 

## Step 3: Install OpenSSH
{: #step-3-install-openssh}

### Linux systems
{: #linux-systems-openssh}

If you have a Linux system, there is no need to install OpenSSH because it is installed by default.

### Windows systems
{: #windows-systems-openssh}

#### Windows 2016
{: #windows-2016-openssh}

To install OpenSSH on your Windows 2016 system, review the following information: 

1. Microsoft's documentation on [Installation of OpenSSH for Windows Server 2019 and Windows 10](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse){: external}
2. GitHub instructions on [Installation of OpenSSH for Windows](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/administration/OpenSSH/OpenSSH_Install_FirstUse.md){: external}

#### Windows 2012 and 2012R2
{: #windows-2012-2012R2-openssh}

To install OpenSSH on your Windows 2012 or 2012R2 system, review the following information: 

1. Download OpenSSH-Win64.zip: https://github.com/PowerShell/Win32-OpenSSH/releases/download/v8.1.0.0p1-Beta/OpenSSH-Win64.zip 
2. Instructions for installation: https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH 

## Step 4: Generate SSH keys
{: #step-4-generate-ssh-keys}

### Linux systems
{: #linux-systems-generate-ssh-keys}

1. Generate the private-public key by using the command `ssh-keygen -t rsa` 
2. Save the generated keys to `{User’s home directory}/.ssh` 
3. Copy the public key to the destination machine by using the following path: `{user’s home directory}/.ssh/authorized_keys`

### Windows systems
{: #windows-systems-generate-ssh-keys}

1. Generate the private-public key by using the command `ssh-keygen -t rsa` 
2. Save the generated keys to `C:\Users\{username}\.ssh` 
3. Copy the public key to the destination machine by using the following path: `C:\Users\{username}\.ssh\authorized_keys` 

## Step 5: Run auto `rsync` script 
{: #step-5-run-rsync-script}

Download the auto `rsync` scripts [here](https://github.com/IBM-Cloud/vpc-migration-tools/tree/main/data-migration){: external}.

### Linux systems
{: #linux-systems-rsync-script}

1. Type `Bash <auto_rsync_file_name>` and hit enter. 
2. Enter the path of the source: `/home/Demo-12Jan/12Jan_test1.txt` 
3. Enter the path of the destination: `/home/19Jan` 
4. Enter the address of the target server: `192.168.0.5` 
5. Enter the target username: `root` 
6. (Optional) Enter custom options. For a list of custom options, see https://linux.die.net/man/1/rsync. 
7. Enter and `rsync` process starts. 

### Windows systems
{: #windows-systems-rsync-script}

1. Download or copy and paste file to directory `/cygdrive/c/cygwin64/home/Administrator` of source machine.
2. Open Cygwin terminal.
3. Type `cd /cygdrive/c/cygwin64/home/Administrator` 
4. Type `Bash ./<auto_rsync_file_name>` and hit enter. 
5. Enter the path of the source: `/cygdrive/c/home/Demo-12Jan/12Jan-test1.txt` 
6. Enter the path of the destination: `/cygdrive/c/home/19Jan/` 
7. Enter the address of the target server: `192.168.0.5` 
8. Enter the target username: `Administrator` 
9. (Optional) Enter custom options. 
10. Enter and `rsync` process starts. 
