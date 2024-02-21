---

copyright:
  years:  2022, 2024
lastupdated: "2024-02-20"

keywords: migration, migrate, cloud migration, on-premises
content-type: tutorial
services: vpc, virtual-servers
account-plan: paid
completion-time: 45m
subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# On-premises to {{site.data.keyword.vpc_short}} migration with RMM
{: #migrating-on-prem-cloud-vpc}
{: toc-content-type="tutorial"} 
{: toc-services="vpc, virtual-servers"} 
{: toc-completion-time="45m"}

The RackWare Management Module (RMM) migration solution provides a seamless replatforming of on-premises workloads to {{site.data.keyword.cloud}} virtual server instances. Use the intuitive GUI to move the OS, applications, and data from on-premises to {{site.data.keyword.vpc_short}} instances. 
{: shortdesc}

Follow the steps to complete a migration from on-premises to {{site.data.keyword.vpc_short}}. 

## Supported operating systems
{: #onprem2cloud-supported-operating-systems}

- CentOS 7.8, 7.9

- RHEL 7.2, 7.3, 7.4, 8.1

- Ubuntu 18.04, 20.04

- Debian 9.x, 10.x

- Windows 2012, 2012R2, 2016, 2019

The **Convert LVM** feature is only supported for RHEL 7.x and RHEL 8.x
{: note}

## Architecture diagram
{: #onprem2cloud-architecture}

This diagram shows the architecture that you are about to create.

![Architecture](images/On-prem_to_VPC.svg){: caption="Figure 1. Architecture diagram" caption-side="bottom"}

This architecture diagram is applicable for bare metal to bare metal, bare metal to virtual server, and virtual server to virtual server scenario in On-premises to {{site.data.keyword.vpc_short}} migration.
{: note}

## Order RMM
{: #onprem2cloud-order-rackware}
{: step}

The RMM tool is available in the {{site.data.keyword.cloud_notm}} catalog. After you order, a virtual server with RMM software is installed into your VPC of choice. The RMM server has a public IP address for reachability and a default login.

If public IP address is not attached to RMM server, then its 'Reserved IP' address can be used to access RMM server with [bastion host](/docs/solution-tutorials?topic=solution-tutorials-vpc-secure-management-bastion-server).
{: note}

1. Order the RMM server from the [{{site.data.keyword.cloud_notm}} catalog](/catalog/content/IBM-MarketPlace-P2P-1.3-22935832-bd76-49ab-b53e-12fc5d04c266-global){: external}.
2. After you order, log in to the RMM server.
3. In the RMM server, change the default password, create users, and create an SSH key.
4. Upload the SSH key to {{site.data.keyword.vpc_short}}.

## Bring Your Own License (BYOL) from RackWare
{: #byol-rackware-on-prem}
{: step}

The license required for migration to {{site.data.keyword.cloud_notm}} is Bring Your Own License (BYOL). You need to purchase the license directly from RackWare.

Complete the following steps to get a license:

1. Order your license from RackWare.
2. Run the following command on the RMM CLI to generate a preinstall file:

    ```sh
    rwadm relicense
    ```
    {: pre}

3. After you generate a preinstall file, send a license generation request to the RackWare licensing team with the following information:
    * RMM License (subject line)
    * Company name
    * License count
    * Preinstall file (attached)
    * Purchase order (attached)
4. Install the license.
    
    a. After you receive a valid license, download the license file and place it in `/etc/rackware`. Restart the services to apply the license by running the following command:

    ```sh
    rwadm restart
    ```
    {: pre}

    b. Verify the license by running the following command:
    
    ```sh
    rw rmm show
    ```
    {: pre}

## Establish connectivity between source server and {{site.data.keyword.vpc_short}} 
{: #onprem2cloud-connectivity-between-source-and-cloud}
{: step}

Your source and target server need to communicate with each other and the RMM. You can establish connectivity over the public internet with public IP addresses, or if you have a private-only environment, then you must set up either a VPN or Direct Link 2.0:

- Use [Direct Link 2.0 connection](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) to {{site.data.keyword.cloud_notm}}.
- Have port 22 open with SSH accessible to RMM server.
- Public interface (least recommended due to security concerns).

## Set up and provision VPC and virtual server instance
{: #onprem2cloud-prepare-source-and-target}
{: step}

You can set up target server manually or with the RMM auto-provision feature.

### Option 1: Manual
{: #onprem2cloud-option-manual}

The RMM solution handles only the OS, application, and data movement. It does not set up a VPC on the target side. Therefore, you must set up the {{site.data.keyword.vpc_short}} infrastructure. At a bare minimum, you need to set up a VPC, subnets, and virtual server instances. For more information, see the [Virtual Private Cloud (VPC) documentation](/docs/vpc?topic=vpc-getting-started).

1. Create a VPC.
2. Create subnets.
3. Order a virtual server instance.
    - SSH key
    - Operating system name (Linux or Windows and their respective versions)
    - Security groups
    - Secondary volume (optional)
    
    Encrypted volumes are not supported.
    {: note}

### Option 2: Auto-provision
{: #onprem2cloud-option-auto}

#### Setting up a cloud user
{: #onprem2cloud-setting-up-cloud-user}

1. Log in to the RackWare web console.
2. In the RackWare web console, go to **Configuration > Clouduser**.
3. When you add a cloud user, enter a name and select _{{site.data.keyword.vpc_short}}_ for the **Cloud Provider**. Select the region where you want to auto-provision the virtual server instance, and enter your {{site.data.keyword.cloud_notm}} API key.
4. Click **Add**.

#### Creating a wave and replication
{: #onprem2cloud-creating-wave}

A wave contains a single host or multiple hosts that are to be migrated. For this migration, you need to create one or more waves, provide information about the hosts in the wave, and then start the wave.

1. In the RackWare web console, go to **Replication > Waves**.
2. When you create a wave, select **Target Type** as **Autoprovision**.
3. Enter source and target details.

If source server has a Linux operating system and it has a boot volume greater than 100 GB, then leave the ‘Provision disk’ textbox empty and select ‘**Convert to LVM**’ option. This selection creates additional disk of the required size and converts all eligible volumes to LVM on the target.’ For Windows, no action is required. It is handled automatically by RMM.
{: note}

4. After you enter your source and target information, you need to provide your {{site.data.keyword.vpc_short}} information.
5. From the edit option in the **Actions** menu of your source, select the **{{site.data.keyword.vpc_short}} Options** tab, enter the relevant information, and click **Modify**.
6. Run the replication.
    
Ensure that your VPC, subnet, and other necessary cloud components are set up before you add a cloud user in RMM.
{: note}

#### Assigning the environment to wave
{: #onprem2cloud-assigning-wave}

1. In the RackWare web console, go to **Replication > Waves**.
2. Select the wave that needs to be migrated.
3. On the **Wave Detail** page, select the Autoprovision option as **Not configured**.
4. Select your cloud user for the **Environment**, enter the region where the virtual server instance needs to be provisioned, and apply the changes.

## Prepare source and target servers
{: #onprem2cloud-prepare-source-target-servers}
{: step}

There are a few things that need to be done on the source and target server for the migration to work. The RackWare RMM server needs to SSH into the servers; thus, the RMM public SSH keys need to be copied onto both the source and target servers. If the source server has both public and private interfaces, host routes need to be added to direct the communication between the servers over the transit gateway path. Complete the following steps to prepare your relevant servers.

### Linux systems
{: #onprem2cloud-linux-systems}

1. Copy the RMM SSH public key to both the source and target servers.
2. If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for the operating system:

```sh
ip route add <destination_network> via <Gateway_address> dev <private_ethernet_interface>
```
{: pre}

### Windows systems
{: #onprem2cloud-windows-systems}

1. Copy the RMM SSH public key to both the source and target servers.
2. You need to download the SSH key utility. You can download it from the RMM server: `<https://<RMM_IP>/windows/RWSSHDService_x64.msi>`

    `<RMM_IP>` is the IP address of your RMM server.
    {: note}

3. The user is `SYSTEM`, and you need to key in the RMM SSH key to authenticate for both the source and target servers.
4. If your compute resource has both public and private IP addresses, the host level route needs to be added for it to work properly. Run the following command on your classic compute resources for the operating system:

```sh
route ADD <destination_network> MASK <subnet_mask> <gateway_ip> <metric_cost>
```
{: pre}

If you use the auto-provision feature, you don't need to set up a target. Only the friendly name for the target server is required.
{: note}

## Set up RMM waves
{: #onprem2cloud-setup-rackware-rmm-on-prem}
{: step}

You can migrate the servers over one by one or run simultaneous migrations. If you are doing multiple, simultaneous migrations, download the CSV template from the RMM server and complete the appropriate fields.

1. Log in to the RMM server.
2. Create a _Wave_ and define the _Wave_ name.
3. If multiple hosts are to be migrated, download the template, complete the appropriate fields, and then upload the template.
4. Select the _Wave_ name to enter source and target information.
5. Select the "+" sign.
6. Add source IP address or FQDN and add source username. 
7. Target Type = Existing system
8. Sync Type = Direct sync
9. Add a target IP address or FQDN.
10. Add a target-friendly name, and add a target username.
11. Start the migration.

The username field for Linux environments is `root`. The username field for Windows environments is `SYSTEM`.
{: note}

## Validate your migration
{: #validating-your-migration-on-prem}
{: step}

After your server migration, validate your compute resources, such as applications and data. Update or validate the following items:
1. Access the target server.
2. Check partitions and volumes.
3. Check the applications.
4. Install any test application in the target server.
5. Check networking routes.
6. Check or update your `yum` repos, application settings, and configuration file.
7. Check or update application or operating system licenses.
8. Remove the RMM SSH key.
