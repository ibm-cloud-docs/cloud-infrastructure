---

copyright:
  years:  2021
lastupdated: "2021-07-01"

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


# Migrating VMDK or VHD images to VPC
{: #migrating-images-vpc}

You can convert your virtual machine (VM) to {{site.data.keyword.cloud}} virtual server instances to import your image to {{site.data.keyword.vpc_short}}, and then use a custom image to create new virtual server instances.
{: shortdesc}

## Before you begin
{: #before-you-begin-migrating-vmware-images}

Before you begin migrating your image conversion, review the following requirements:

1. You need an existing {{site.data.keyword.vpc_short}} environment. 
2. Understand {{site.data.keyword.vpc_short}} custom image requirements:
    * Is in qcow2 format
    * The boot volume (primary vHDD) doesn't exceed 100 GB
    * Is cloud-init enabled
    * Virtio drivers enabled
    * The operating system is supported as a stock image. For a list of supported stock images, see [Images](/docs/vpc?topic=vpc-about-images#stock-images).
3. Provision an instance of {{site.data.keyword.cos_full_notm}} if you don't have one. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).
4. You need a server with a browser that has access to both the internet and your {{site.data.keyword.cos_short}} bucket to perform the image conversion.

### Image conversion considerations
{: #image-conversion-considerations}

* VMs must be exported as VMDK or VHD.
* VMs with network attached storage (NAS) such as iSCSI or NFS are not automatically copied over. Data migration must be done as a separate process.
* IP addresses are not preserved. 
* Hypervisor specifics are not preserved, such as port groups and teaming.

## Step 1: Prepare the image conversion server
{: #step-1-prepare-image-conversion-server}

1. Install the {{site.data.keyword.cloud_notm}} CLI. For more information about installing the CLI, see [Installing the stand-alone {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).
2. Install the [{{site.data.keyword.cos_full_notm}} CLI plug-in](/docs/cli?topic=cli-install-devtools-manually#installing-ibm-cloud-object-storage-cli-plug-in).
3. Install the [{{site.data.keyword.vpc_short}} CLI plug-in](/docs/cli?topic=vpc-infrastructure-cli-plugin-vpc-reference).
4. Install the [Aspera plug-in](https://www.ibm.com/aspera/connect/?_ga=2.251043974.1048532583.1621835203-1020984874.1621835203){: external} for your system. This plug-in helps with the image upload to {{site.data.keyword.cos_short}}. For more information about Aspera, see [Using Aspera high-speed transfer](/docs/cloud-object-storage/basics?topic=cloud-object-storage-aspera#aspera-restricted-network).
5. Download and install QEMU. For more information about installing QEMU, see [Download QEMU](https://www.qemu.org/download/){: external}. For Windows systems, add the QEMU install path in the system’s environment variable.

## Step 2: Validate and prepare the VMs
{: #step-2-validate-prepare-vms}

Make sure that your VM meets the minimum requirements that are listed in the [Before you begin](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-vmware-vmdk-images-to-vpc#before-you-begin) section; however, for this step, the VM doesn't need to be in qcow2 format. The image conversion to qcow2 is covered in Step 3. 

As a best practice, take a snapshot or backup your VM before proceeding. 

You can run a [migration bash script](https://github.com/IBM-Cloud/vpc-migration-tools/tree/main/image-conversion){: external} for your system to help validate if the VMs meet the requirements.
{: tip}

### Linux systems
{: #step-2-linux-systems}

1. Complete steps 2-6 in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image#kernel-args) to prepare your image.
2. Edit the `fstab` file to remove entries other than "/" and "/boot".

### Windows systems
{: #step-2-windows-systems}

See [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image) on how to prepare your Windows image, specific information about cloud-init, and the location for downloading Virtio drivers for Windows.
1. Back up the administrator user files and settings.
2. Download and prepare cloud-init. 
3. Download and install Virtio drivers.
4. Reset network settings to default. This step disables network access to the VM.
  
    For Windows 2012 and 2012R2, run the following commands:
    
    ```
    netsh winsock reset
    ```
    {: pre}
  
    ```
    netsh int ip reset c:\resetlog.txt
    ```
    {: pre}
    
    For Windows 2016, go to _Network settings_ > _Network and internet   status_ > _Reset network_.
  
5. Restart the VM to activate the Virtio drivers.
6. Run the `sysprep` command. The `sysprep` command generalizes and removes system-specific IDs from the operating system.

    ```
    C:\Windows\System32\Sysprep\Sysprep.exe /oobe /generalize /shutdown "/  unattend:C:\Program Files\Cloudbase   Solutions\Cloudbase-Init\conf\Unattend.xml"
    ```
    {: pre}

## Step 3: Convert image to qcow2
{: #step-3-convert-image-to-qcow2}

1. Export the VM in a VMDK or VHD image file format.
2. Copy the VMDK or VHD image to the image conversion server.
3. Run the following QEMU command for your image file format to convert the image:

    For VMDK images:
    
    ```
    qemu-img convert -f vmdk -O qcow2 -o cluster_size=512k <vm_image_name.  vmdk> <vsi_image_name.qcow2> 
    ```
    {: pre}
    
    For VHD images:
    
    ```
    qemu-img convert -f vpc -O qcow2 -o cluster_size=512k <vm_image_name.  vhd> <vsi_image_name.qcow2> 
    ```
    {: pre}

If your VM has secondary vHDDs, a separate VMDK or VHD file is available for the VM. This secondary vHHD does not need to be converted to qcow2. This file is uploaded to {{site.data.keyword.cos_short}} in its native (VMDK or VHD) file format.
{: note}

You can run a [migration bash script](https://github.com/IBM-Cloud/vpc-migration-tools/tree/main/image-conversion){: external} to help with the image conversion. 
{: tip}

## Step 4: Upload to IBM Cloud Object Storage
{: #step-4-upload-to-cos}

For more information about uploading to {{site.data.keyword.cos_short}} by using the console, see [Using Aspera high-speed transfer](/docs/cloud-object-storage/basics?topic=cloud-object-storage-aspera#aspera-console).

The converted boot image can be uploaded through the migration script by opting 'y' when prompted to upload.
{: tip}

## Step 5: Create virtual server instance
{: #step-5-create-virtual-server-instance}

1. Import your image to {{site.data.keyword.vpc_short}}. For more information, see [Importing and managing custom images](/docs/vpc?topic=vpc-managing-images).
2. To create the virtual server instance, log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.  
3. Go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom images > Your custom image**. 
4. From the _Action_ menu, select **New virtual server**.

## Step 6: (Optional) Create secondary volume and attach the virtual server instance
{: #step-6-create-secondary-volume-attach-vsi}

This step is for {{site.data.keyword.cloud_notm}} virtual server instances that have secondary vHDD or secondary volumes.

The secondary volume needs to be equal to or greater than the secondary VMDK image size.
{: note}

### **Linux systems**
{: #step-6-linux-systems}


***For Centos/Redhat 7 and Ubuntu 16:***

1. Create n+1 secondary volumes. If your VM has one secondary vHDD, then you need to create two secondary volumes for the instance. The extra volume is temporary. 
2. Download the secondary (vHDD) VMDK or VHD file from {{site.data.keyword.cos_short}} to one of the empty secondary volumes. 
3. Install QEMU. 
4. Convert the (VMDK or VHD) image data to disk:

    For VMDK images:
    
    ```
    qemu-img convert -p -f vmdk <data_image.vmdk> /dev/vde
    ```
    {: pre}
    
    For VHD images:
    
    ```
    qemu-img convert -p -f vpc <data_image.vhd> /dev/vde
    ```
    {: pre}

5. Mount the volume:

    ```
    mount /dev/vde1 /mnt
    ```
    {: pre}

6. Format and mount the other empty secondary volume

    ```
    mount /dev/vdd1 /data
    ```
    {: pre}

7. Copy data over from the temporary volume (vde) to the target volume (vdd):

    ```
    cp –avf /mnt /data 
    ```
    {: pre}

8. Unmount and delete the volume.
9. Edit the `fstab` file to automount and be persistent across reboot.  
  


***For Centos/Redhat 8 , Ubuntu 18.04 and 20.04 , Debian 9 and 10:***

1. Create secondary volumes, if your VM has secondary vHDD. 
2. Copy the VMDK or VHD image file to the migrated VSI 
(Make sure you have enough space to copy the image file. If necessary attach a temporary volume with space for copying.)

  Skip step 3 if you opted ‘y’ to guestfs installation prompt when executing pre-check script.
  {: note}

3. Install guestfs library

     Centos/Redhat
    
     ```
     $ yum update -y
    
     $ yum install libguestfs-tools
    
     $ systemctl enable libvirtd
    
     $ systemctl start libvirtd
    
     $ systemctl status libvirtd
     ```
    
     Ubuntu/Debian
     
     ```
     $ apt-get update -y
     
     $ apt-get install -y libguestfs-tools
     ```

4. Convert the (VMDK or VHD) image data to `qcow2`:

    For VMDK images (to `qcow2`):
    
    ```
    qemu-img convert -p -f vmdk -O qcow2 -o cluster_size=512k   secondary_volume.vmdk secondary_volume.qcow2
    ```
    {: pre}
    
    For VHD images (to `qcow2`):
    
    ```
    qemu-img convert -p -f vpc -O qcow2 -o cluster_size=512k   secondary_volume.vhd secondary_volume.qcow2
    ```
    {: pre}

5. Mount the volume:

    ```
    guestmount -a secondary_volume.qcow2 -m /dev/sda1 --ro /mnt
    ```
    {: pre}

6. Format and mount the empty secondary volume

    ```
    mount /dev/vdd1 /data
    ```
    {: pre}

7. Copy data over from the ``/mnt`` to the target volume (vdd):

    ```
    cp –avf /mnt /data 
    ```
    {: pre}

8. After copy completed unmount and delete the image file, if no longer needed.
  
    ```  
    $ guestunmount /mnt
    ```

9. Edit the `fstab` file to automount and be persistent across reboot.

### Windows systems
{: #step-6-windows-systems}

1. Create secondary volume.
2. Copy the VMDK or VHD disk to Windows.
3. Mount the image as a disk.  


