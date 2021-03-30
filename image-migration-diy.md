---

copyright:
  years:  2021
lastupdated: "2021-03-25"

keywords: image migration, migrate image, vmdk

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

# Migrating VMware (VMDK) images to VPC
{: #migrating-vmware-vmdk-images}

You can convert your VMware virtual machine (VM) to {{site.data.keyword.cloud}} virtual server instances to import your image to {{site.data.keyword.vpc_short}}, and then use a custom image to create new virtual server instances.
{: shortdesc}

## Before you begin
{: #before-you-begin-migrating-vmware-images}

Before you begin migrating your image conversion, review the following requirements:

1. You need an existing {{site.data.keyword.vpc_short}} environment. 
2. Understand {{site.data.keyword.vpc_short}} custom image requirements:
    * Is in qcow2 format
    * The boot volume (primary vHDD) doesn't exceed 100 GB
    * Is cloud-init enabled
    * Has Virtio drivers
    * The operating system is supported as a stock image. For a list of supported stock images, see [Images](/docs/vpc?topic=vpc-about-images#stock-images).
3. Provision an instance of {{site.data.keyword.cos_full_notm}} if you don't have one. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).
4. You need a server with a browser that has access to both the internet and your {{site.data.keyword.cos_short}} bucket to perform the image conversion.

### Image conversion considerations
{: #image-conversion-considerations}

* VMware VM must be exported as VMDK.
* VMs that have network attached storage (NAS) such as iSCSI or NFS are not automatically copied over. Data migration must be done as a separate process.
* IP addresses are not preserved. 
* VMware ESXi specifics are not preserved, such as port groups, ESXi teaming, etc.

## Step 1: Prepare the image conversion server
{: #step-1-prepare-image-conversion-server}

1. Install the {{site.data.keyword.cloud_notm}} CLI. For details, see [Installing the stand-alone {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).
2. Install the [{{site.data.keyword.cos_full_notm}} CLI plugin](/docs/cli?topic=cli-install-devtools-manually#installing-ibm-cloud-object-storage-cli-plug-in).
3. Install the [{{site.data.keyword.vpc_short}} CLI plugin](/docs/cli?topic=vpc-infrastructure-cli-plugin-vpc-reference).
4. Install the Aspera plugin for the browser. This helps with the image upload to {{site.data.keyword.cos_short}}. For more information about Aspera, see [Using Aspera high-speed transfer](/docs/cloud-object-storage/basics?topic=cloud-object-storage-aspera#aspera-restricted-network).
    * [Windows plugin](https://d3gcli72yxqn2z.cloudfront.net/connect_latest/v4/bin/IBMAsperaConnectSetup-ML-3.11.1.58.exe)
    * [Linux plugin](https://d3gcli72yxqn2z.cloudfront.net/connect_latest/v4/bin/ibm-aspera-connect-3.11.1.58-linux-g2.12-64.tar.gz)
5. Download and install QEMU. For details, see [Download QEMU](https://www.qemu.org/download/){: external}. For Windows systems, add the QEMU’s installed path in the system’s environment variable.

## Step 2: Validate and prepare the VMware VMs
{: #step-2-validate-prepare-vms}

Make sure that your VM meets the minimum requirements listed in the [Before you begin](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-vmware-vmdk-images-to-vpc#before-you-begin) section; however, for this step, the VM doesn't have to be in qcow2 format. The image conversion to qcow2 is covered in Step 3. 

As a best practice, take a snapshot or backup your VM before proceeding. 

You can run a [bash script](https://github.com/IBM-Cloud/vpc-migration-tools/tree/main/image-conversion){: external} for your system to help validate if the VMs meet the requirements.
{: tip}

### Linux systems
{: #step-2-linux-systems}

1. Complete steps 2-6 in [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image#kernel-args) to prepare your image.
2. Edit the `fstab` file to remove entries other than "/" and "/boot".

### Windows systems
{: #step-2-windows-systems}

See [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image) on how to prepare your Windows image, specific information regarding cloud-init, and the location for downloading Virtio drivers for Windows.
1. Back up the administrator user files and settings.
2. Download and prepare cloud-init. 
3. Download and install Virtio drivers.
4. Reset network settings to default. Go to _Network Settings_ > _Network & Internet Status_ > _Reset Network_.
    This step will disable network access to the VM.
    {:note}
5. Reboot the VM to activate the Virtio drivers.
6. Run the `sysprep` command. The `sysprep` command generalizes and removes system-specific IDs from the operating system.

  ```
  C:\Windows\System32\Sysprep\Sysprep.exe /oobe /generalize /shutdown "/unattend:C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf\Unattend.xml"
  ```
  {: pre}

## Step 3: Convert image to qcow2
{: #step-3-convert-image-to-qcow2}

1. Export the VM in a VMDK image file format.
2. Copy the VMDK image to the image conversion server.
3. Run the following QEMU command to convert the image:

  ```
  qemu-img convert -f vmdk -O qcow2 -o cluster_size=512k <vm_image_name.vmdk> <vsi_image_name.qcow2> 
  ```
  {: pre}

If your VM has secondary vHDDs, there is a separate VMDK file for the VM. This does not need to be converted to qcow2. This file will be uploaded to {{site.data.keyword.cos_short}} in its native (VMDK) file format.
{: note}

You can run a [bash script](https://github.com/IBM-Cloud/vpc-migration-tools/tree/main/image-conversion){: external} to help with the image conversion. 
{: tip}

## Step 4: Upload to IBM Cloud Object Storage
{: #step-4-upload-to-cos}

For more information about uploading to {{site.data.keyword.cos_short}} using the console, see [Using Aspera high-speed transfer](/docs/cloud-object-storage/basics?topic=cloud-object-storage-aspera#aspera-console).

## Step 5: Create virtual server instance
{: #step-5-create-virtual-server-instance}

1. Import your image to {{site.data.keyword.vpc_short}}. For more information, see [Importing and managing custom images](/docs/vpc?topic=vpc-managing-images).
2. To create the virtual server instance, log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.  
3. Navigate to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images > Your Custom Image**. 
4. From the _Action_ menu, select **New virtual server**.

## Step 6: (Optional) Create secondary volume and attach the virtual server instance
{: #step-6-create-secondary-volume-attach-vsi}

This step is for {{site.data.keyword.cloud_notm}} virtual server instances that have secondary vHDD or secondary volumes.

The secondary volume should be equal to or greater than the secondary VMDK image size.
{: note}

### Linux systems
{: #step-6-linux-systems}

1. Create n+1 secondary volumes. If your VM has one secondary vHDD, then you need to create two secondary volumes for the instance. The extra volume is temporary. 
2. Download the secondary (vHDD) VMDK file from {{site.data.keyword.cos_short}} to one of the empty secondary volumes. 
3. Install QEMU. 
4. Convert the (VMDK) image data to disk:

  ```
  qemu-img convert -f vmdk <data_image.vmdk> /dev/vde
  ```
  {: pre}

5. Mount the volume:

  ```
  mount /dev/vde1/mnt
  ```
  {: pre}

6. Format and mount the other empty secondary volume

  ```
  mount /dev/vdd1/data
  ```
  {: pre}

7. Copy data over from the temporary volume (vde) to the target volume (vdd):

  ```
  "cp – avf /mnt/data" 
  ```
  {: pre}

8. Unmount and delete the volume.
9. Edit the `fstab` file to automount and be persistent across reboot.

### Windows systems
{: #step-6-windows-systems}

1. Create secondary volume.
2. Copy the VMDK disk to Windows.
3. Mount the image as a disk.  
