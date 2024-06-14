---

copyright:
  years:  2021, 2024
lastupdated: "2024-06-14"

keywords: migration, migrate, migrating, migrate VM

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# Migrating workloads to {{site.data.keyword.vpc_short}} with Server Migration
{: #vm-migration-to-vpc}

Server Migration for {{site.data.keyword.cloud}} is a one-click solution where you can migrate from VMware VM or {{site.data.keyword.cloud}} classic virtual server instances to {{site.data.keyword.vpc_short}}, so you can adopt the native capabilities of {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## Supported operating systems
{: #vm-migration-to-vpc-supported-os}

| Image | Architectures |
|---------|---------|
| CentOS 7.x | x86-64 |
| CentOS Stream 8.x, 9.x | x86-64 |
| Debian 10.x, 11.x, 12.x | x86-64 |
| Fedora Core OS | x86-64 |
| Red Hat Enterprise Linux 7.x, 8.x, 9.x | x86-64 |
| Red Hat Enterprise Linux for SAP 7.x, 8.x, 9.x | x86-64 |
| Rocky Linux 8.x, 9.x | x86-64 |
| SUSE Linux Enterprise Server 12.x, 15.x | x86-64 |
| SUSE Linux Enterprise Server for SAP 12.x, 15.x | x86-64 |
| Ubuntu 20.04.x, 22.04.x, 24.04.x | x86-64 |
| Windows 2016, 2019, 2022 | x86-64 |
{: caption="Table 1. Supported x86_64 stock image operating systems" caption-side="top"}

For more informatio regarding supported operating systems, see the following topics:
- [x86 virtual server images](/docs/vpc?topic=vpc-about-images)

## Architecture diagram
{: #vm-migration-to-vpc-architecture}

![Architecture](images/DIY-Arch.svg){: caption="Figure 1. Architecture diagram" caption-side="bottom"}

## Before you begin
{: #vm-migration-to-vpc-supported-topology}

Before you begin migrating your virtual, review the following requirements:

1. Back up your source virtual machine that you plan to migrate.

2. You need an existing {{site.data.keyword.vpc_short}} environment.

3. Virtual machines must be configured to use BIOS boot mode.
   
    UEFI boot mode is not supported.
    {: note}

4. Allow root user to log in without a password for SSH key access. For example, `%sudo ALL=(ALL:ALL) NOPASSWD: ALL in  /etc/sudoers`

5. The operating system is supported as a stock image. For more information on stock images, see [Stock Image list](/docs/vpc?topic=vpc-about-images#stock-images). For end of support information, click [here](https://www.ibm.com/cloud/cloud-prod-life){: external}.

6. The primary disk (operating system) must not exceed 249 GB.

7. Configure at least one network adapter with auto.

8. Make sure that you have connection to VMware or classic from VPC.

9. Provision an {{site.data.keyword.cos_full_notm}} instance if you don't have one. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq&interface=cli).

## Migration considerations
{: #vm-migration-to-vpc-migration-considerations}

Review the following migration considerations for the appliance server:

* Virtual machines must be exported in VMDK or VHD format.
* Virtual machines with Network Attached Storage (NAS) such as iSCSI or NFS are not automatically copied over. Data migration must be done as a separate process.
* IP addresses are not preserved.
* Hypervisor specifics are not preserved, such as port groups and teaming.

## Order appliance server from {{site.data.keyword.cloud}} catalog
{: #vm-migration-to-vpc-create-appliance}

1. Order the appliance server from the {{site.data.keyword.cloud_notm}} catalog.
2. Provide all of the required parameters (the deployment values are case-sensitive), and click **Install**.
3. After you order, log in to the appliance server.
4. Upload the SSH key to {{site.data.keyword.vpc_short}}.

## Run appliance server
{: #vm-migration-to-vpc-step2}
{: step}

1. Log in to the appliance server.
2. Change the directory to `/opt/server-migration/scripts`.
3. Run `appliance.sh`.
4. Provide the IP address of the source machine that you want to migrate.
5. Select whether you're migrating from VMware or classic. The directory file and config file are created and the running script stops.
6. Update the details for the config file and then rerun the `appliance.sh` script.
7. Provide the IP address that is required to migrate.
8. Select “y” for the following question: `Do you want to continue already started migration [Default y].`
9. The appliance server checks for all the required permissions and validates the config file. If config file is valid and all the required permissions are present, then the appliance server creates an SSH key.
10. Copy the SSH key and paste it in the authorized file in the user's home directory in `ssh`. Then answer “y” for the following question: `Have you copied the SSH public key content and inserted in ssh/authorized file, please confirm.`

## Migrate with appliance server
{: #migrate-with-appliance-server}
{: step}

After you confirm that the SSH key is in the correct place, the appliance server completes the following for you:

1. The appliance server runs the pre-check on the source machine.
2. When the pre-checks pass, then the VMDK image is uploaded into the appliance server.
3. **Appliance server converts the image to supported format (qcow1)**. _Shouldn't this be qcow2?_
4. The appliance server uploads the converted image into an {{site.data.keyword.cos_full_notm}} bucket and then creates a custom image with the uploaded image in the bucket.
5. Then the appliance server creates a target migrated virtual server in {{site.data.keyword.vpc_short}}.

### Additional VMware steps
{: #additional-vmware-steps}

If you migrated from VMware and the migration is successful, you need to complete a few extra steps: 
1. Go to VMware datastore.
2. Download and copy the VMDK file of the OS disk. For example, VMware ESXi creates a compressed file that contains `disk.vmdk` and `disk-flat.vmdk`. Copy both of the files into the path appliance. 
3. Run the `appliance.sh` command again, and the remaining migration process is done by the appliance server.

## Validate your migration
{: #vm-migration-to-vpc-validate}
{: step}

After your migration, validate or update the following:

1. Application
2. Licensing
3. Reachability (host level configuration changes)
