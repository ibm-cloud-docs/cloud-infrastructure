---
copyright:
  years: 2022, 2023

lastupdated: "2023-11-14"

keywords: encryption, storage encryption, customer managed encryption, classic infrastructure encryption

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# Encryption options for {{site.data.keyword.cloud}} classic infrastructure storage
{: #encryption-classic-infrastructure}

By default {{site.data.keyword.cloud}} classic infrastructure includes provider-managed data-at-rest encryption capabilities. If your environment requires customer-managed encryption, you have several options that include {{site.data.keyword.keymanagementserviceshort}}, LUKS encryption for {{site.data.keyword.blockstoragefull}}, file-level encryption options for {{site.data.keyword.filestorage_full}}, and server-side encryption for {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

## Provider-managed encryption
{: #provider-managed-encryption}

{{site.data.keyword.cloud_notm}} classic infrastructure provides the following automatic data-at-rest encryption capabilities:

- {{site.data.keyword.cos_full_notm}} encrypts all objects by default with provider-managed keys.
- {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volume are secured automatically with provider-managed Industry-Standard AES-256 encryption. All snapshots and replicas of encrypted storage volumes are also encrypted by default. This feature can’t be turned off on a volume basis. All cluster-to-cluster traffic is encrypted with TLS.

## Customer-managed encryption
{: #customer-managed-encryption}

Some workloads or use cases require full customer control over the encryption, including key management. You can use the following information to learn about possible methods to achieve customer-managed encryption within the standard {{site.data.keyword.cloud}} classic infrastructure feature set. You can implement any of the following options: {{site.data.keyword.keymanagementserviceshort}}, LUKS encryption for {{site.data.keyword.blockstorageshort}}, file-level encryption options for {{site.data.keyword.filestorage_short}}, and server-side encryption for {{site.data.keyword.cos_full_notm}}. Most of these customer-managed encryption options require manual setup. 

### Key Protect
{: #key-protect}

[{{site.data.keyword.keymanagementservicefull}}](https://www.ibm.com/cloud/key-protect){: external} is one method that you can use to set up customer-managed encryption. For more information about {{site.data.keyword.keymanagementserviceshort}}, see [About Key Protect](/docs/key-protect?topic=key-protect-about). Whenever the following snippets call for a *passphrase* or *password*, you can use a key from Key Protect by using copy and paste or some shell scripting.

When you use {{site.data.keyword.cos_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}, a root key is used to encrypt buckets, so only the provisioning and the creation of the root key steps are necessary.

You can provision {{site.data.keyword.keymanagementserviceshort}} from the {{site.data.keyword.cloud_notm}} console or with the API. After you provision a Key Protect instance, you can create (or import) a customer-managed root key. This root key never leaves the HSM but it is used to encrypt and decrypt other keys. When the root key is available, you can create (or import) a standard key to directly encrypt and decrypt data. For more information about using {{site.data.keyword.keymanagementserviceshort}}, see the following topics:

* [Creating or importing keys](/docs/key-protect?topic=key-protect-getting-started-tutorial) 
* A data encryption key (DEK) ought to be stored in an encrypted (wrapped) format. Key Protect wraps the DEK by encrypting it with the root key.
    * [Retrieving a wrapped key](/docs/key-protect?topic=key-protect-wrap-keys&interface=ui)
    * [Retrieving a wrapped key with the API](https://cloud.ibm.com/apidocs/key-protect#wrapkey){: external} 
* After it is retrieved, the wrapped DEK can be stored on the local file system, and unwrapped when the key is needed for some file system operation (such as mounting, and encryption). 
    * [Unwrapping a wrapped key](/docs/key-protect?topic=key-protect-unwrap-keys&interface=ui)
    * [Unwrapping a wrapped key with the API](https://cloud.ibm.com/apidocs/key-protect#unwrapkey){: external}

If the root key is rotated or for some other reason an update is needed to the wrapped DEK, the previous API call returns the new DEK also. Store this new DEK and use it for future operations.
{: note}

For more information about key management (rotation, deletion, auditing), see the Key Protect [Getting started tutorial](/docs/key-protect?topic=key-protect-getting-started-tutorial) and the [API reference](https://cloud.ibm.com/apidocs/key-protect){: external}.

### {{site.data.keyword.blockstorageshort}} encryption with LUKS
{: #block-storage-luks}

You can use LUKS to encrypt {{site.data.keyword.blockstorageshort}} volumes. For more information, see [Achieving full disk encryption with LUKS in RHEL](/docs/BlockStorage?topic=BlockStorage-LUKSencryption). After the LUKS volume is mounted, key management is done through *cryptsetup*.

#### LUKS Key Slots
{: #luks-key-slots}

LUKS has 8 key slots.

```sh
[root@classic-byok-poc02 ~]# blkid | grep LUKS
/dev/sda: UUID="6e515660-7db7-41df-9b48-47d1489c7711" TYPE="crypto_LUKS"
/dev/mapper/3600a09803830564e455d4f3155527337: UUID="6e515660-7db7-41df-9b48-47d1489c7711" TYPE="crypto_LUKS"
[root@classic-byok-poc02 ~]# cryptsetup luksDump /dev/sda
LUKS header information
Version:       	2
Epoch:         	3
Metadata area: 	16384 [bytes]
Keyslots area: 	16744448 [bytes]
UUID:          	6e515660-7db7-41df-9b48-47d1489c7711
Label:         	(no label)
Subsystem:     	(no subsystem)
Flags:       	(no flags)

Data segments:
  0: crypt
	offset: 16777216 [bytes]
	length: (whole device)
	cipher: aes-xts-plain64
	sector: 512 [bytes]

Keyslots:
  0: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2i
	Time cost:  4
	Memory:     737817
	Threads:    2
	Salt:       c3 c9 89 88 35 3a fb 32 4e 64 a0 5c 58 33 c0 92
	            af c1 0c 98 78 d8 eb a0 b5 ea 67 4b 00 86 59 63
	AF stripes: 4000
	AF hash:    sha256
	Area offset:32768 [bytes]
	Area length:258048 [bytes]
	Digest ID:  0
Tokens:
Digests:
  0: pbkdf2
	Hash:       sha256
	Iterations: 106736
	Salt:       a4 57 7d 09 e8 cd 56 e7 ad 4c 14 d8 15 b4 38 88
	            84 63 a8 7d 2a e8 2d d8 46 b6 87 2f 87 fa a9 e0
	Digest:     09 aa 56 8f 14 dd c7 76 9a 51 5e 74 b9 1b 95 8a
	            14 a2 93 59 18 b3 70 df 54 53 c0 85 2c 3b 79 fd
```
{: screen}

#### Adding a key
{: #add-luks-key}

See the following example for adding a key.

```sh
[root@classic-byok-poc02 ~]# cryptsetup luksAddKey /dev/sda
Enter any existing passphrase:
Enter new passphrase for key slot:
Verify passphrase:
[root@classic-byok-poc02 ~]# cryptsetup luksDump /dev/sda
LUKS header information
Version:       	2
Epoch:         	4
Metadata area: 	16384 [bytes]
Keyslots area: 	16744448 [bytes]
UUID:          	6e515660-7db7-41df-9b48-47d1489c7711
Label:         	(no label)
Subsystem:     	(no subsystem)
Flags:       	(no flags)

Data segments:
  0: crypt
	offset: 16777216 [bytes]
	length: (whole device)
	cipher: aes-xts-plain64
	sector: 512 [bytes]

Keyslots:
  0: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2i
	Time cost:  4
	Memory:     737817
	Threads:    2
	Salt:       c3 c9 89 88 35 3a fb 32 4e 64 a0 5c 58 33 c0 92
	            af c1 0c 98 78 d8 eb a0 b5 ea 67 4b 00 86 59 63
	AF stripes: 4000
	AF hash:    sha256
	Area offset:32768 [bytes]
	Area length:258048 [bytes]
	Digest ID:  0
  1: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2i
	Time cost:  4
	Memory:     682321
	Threads:    2
	Salt:       ac 17 b7 43 2b 82 e1 68 a9 77 a3 8f ef 3b 42 10
	            9f 28 60 22 f3 ff 71 c5 03 cf 32 28 1a db e1 96
	AF stripes: 4000
	AF hash:    sha256
	Area offset:290816 [bytes]
	Area length:258048 [bytes]
	Digest ID:  0
Tokens:
Digests:
  0: pbkdf2
	Hash:       sha256
	Iterations: 106736
	Salt:       a4 57 7d 09 e8 cd 56 e7 ad 4c 14 d8 15 b4 38 88
	            84 63 a8 7d 2a e8 2d d8 46 b6 87 2f 87 fa a9 e0
	Digest:     09 aa 56 8f 14 dd c7 76 9a 51 5e 74 b9 1b 95 8a
	            14 a2 93 59 18 b3 70 df 54 53 c0 85 2c 3b 79 fd
[root@classic-byok-poc02 ~]#
```
{: screen}

#### Removing a key from a specific slot
{: #remove-luks-key}

See the following example for removing a key from a specific slot.

```sh
[root@classic-byok-poc02 ~]# cryptsetup luksKillSlot /dev/sda 1
Enter any remaining passphrase:
[root@classic-byok-poc02 ~]# cryptsetup luksDump /dev/sda
LUKS header information
Version:       	2
Epoch:         	5
Metadata area: 	16384 [bytes]
Keyslots area: 	16744448 [bytes]
UUID:          	6e515660-7db7-41df-9b48-47d1489c7711
Label:         	(no label)
Subsystem:     	(no subsystem)
Flags:       	(no flags)

Data segments:
  0: crypt
	offset: 16777216 [bytes]
	length: (whole device)
	cipher: aes-xts-plain64
	sector: 512 [bytes]

Keyslots:
  0: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2i
	Time cost:  4
	Memory:     737817
	Threads:    2
	Salt:       c3 c9 89 88 35 3a fb 32 4e 64 a0 5c 58 33 c0 92
	            af c1 0c 98 78 d8 eb a0 b5 ea 67 4b 00 86 59 63
	AF stripes: 4000
	AF hash:    sha256
	Area offset:32768 [bytes]
	Area length:258048 [bytes]
	Digest ID:  0
Tokens:
Digests:
  0: pbkdf2
	Hash:       sha256
	Iterations: 106736
	Salt:       a4 57 7d 09 e8 cd 56 e7 ad 4c 14 d8 15 b4 38 88
	            84 63 a8 7d 2a e8 2d d8 46 b6 87 2f 87 fa a9 e0
	Digest:     09 aa 56 8f 14 dd c7 76 9a 51 5e 74 b9 1b 95 8a
	            14 a2 93 59 18 b3 70 df 54 53 c0 85 2c 3b 79 fd
[root@classic-byok-poc02 ~]#
```
{: screen}

### {{site.data.keyword.filestorage_short}} file-level encryption
{: #file-storage-encryption}

{{site.data.keyword.filestorage_short}} doesn't support LUKS or similar volume-level encryption. Encryption must be done on a file level. Tools that were built upon FUSE can achieve file-level encryption, like *EncFS* or *gocryptfs*. On the {{site.data.keyword.cloud}} RHEL8 image *gocryptfs* is not available, but on Ubuntu 20.04 it is. You can install *gocryptfs* by using apt.

1. Mount a {{site.data.keyword.filestorage_short}}. In the following example, the volume is mounted on `/mnt/filestor`.
   ```sh
   root@classic-byok-poc01:/mnt# cat /etc/fstab
   LABEL=cloudimg-rootfs	/	 ext4	defaults,relatime	0 1
   # CLOUD_IMG: This file was created/modified by the Cloud Image build process
   LABEL=cloudimg-bootfs   /boot   ext3    defaults,relatime    0 0
   LABEL=SWAP-xvdb1	none	swap	sw,comment=cloudconfig	0	2
   fsf-fra0401g-fz.service.softlayer.com:/IBM02SEV1395451_375/data01 /mnt/filestor nfsvers=3 defaults 0 0
   ```
   {: screen}

1. Turn on encryption:
   ```sh
   root@classic-byok-poc01:/mnt# mkdir filestor/cipher
   root@classic-byok-poc01:/mnt# gocryptfs -init filestor/cipher
   Choose a password for protecting your files.
   Password: 
   Repeat: 
   
   Your master key is:

       3698766d-b3fdc176-33442fb3-dc4c4d61-
       d8a36aad-9a453019-710b96a5-7717d5ed

   If the gocryptfs.conf file becomes corrupted or you ever forget your password,
   there is only one hope for recovery: The master key. Print it to a piece of
   paper and store it in a drawer. This message is only printed once.
   The gocryptfs filesystem has been created successfully.
   You can now mount it using: gocryptfs filestor/cipher MOUNTPOINT
   ```
   {: screen}

1. Mount the encrypted storage as unencrypted and work with it.
   ```sh
   root@classic-byok-poc01:/mnt# ls
   filestor  plain
   root@classic-byok-poc01:/mnt# gocryptfs filestor/cipher plain
   Password: 
   Decrypting master key
   Filesystem mounted and ready.
   root@classic-byok-poc01:/mnt# cd plain
   root@classic-byok-poc01:/mnt/plain# uname -a >test
   root@classic-byok-poc01:/mnt/plain# cat test
   Linux classic-byok-poc01 5.4.0-107-generic #121-Ubuntu SMP Thu Mar 24 16:04:27 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
   root@classic-byok-poc01:/mnt/plain# cd ../filestor/cipher/
   root@classic-byok-poc01:/mnt/filestor/cipher# ls
   gocryptfs.conf  gocryptfs.diriv  KRCHWT02ixA0375d6ur3MQ
   ```
   {: screen}

1. You can change the key with the `passwd` option.
   ```sh
   root@classic-byok-poc01:/mnt/filestor# gocryptfs -passwd cipher
   ```
   {: screen}

### {{site.data.keyword.cos_full_notm}} server-side encryption
{: #cos-server-side-encryption}

{{site.data.keyword.cos_full_notm}} provides the following methods of customer-managed encryption:

* [Server-Side Encryption based on SSE-C headers](/docs/cloud-object-storage?topic=cloud-object-storage-sse-c)
* [Server-Side Encryption with IBM Key Protect](/docs/cloud-object-storage?topic=cloud-object-storage-kp)
