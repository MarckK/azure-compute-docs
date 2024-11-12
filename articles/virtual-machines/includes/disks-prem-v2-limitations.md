---
 title: include file
 description: include file
 author: roygara
 ms.service: azure-disk-storage
 ms.topic: include
 ms.date: 11/08/2024
 ms.author: rogarana
ms.custom:
  - include file
  - ignite-2023
---
- Premium SSD v2 can be used with any premium storage-enabled [virtual machine size](/azure/virtual-machines/sizes/overview). Navigate to your desired virtual machines (VMs) size article to determine support for premium storage.
- Premium SSD v2 disks can't be used as an OS disk.
- Premium SSD v2 disks can't be used with Azure Compute Gallery.
- Currently, Premium SSD v2 disks are only available in [select regions](/azure/virtual-machines/disks-deploy-premium-v2#regional-availability).
- Premium SSD v2 disks can only be attached to zonal VMs. When creating a new VM or Virtual Machine Scale Set, specify the availability zone you want before adding Premium SSD v2 disks to your configuration.
- Encrypting Premium SSD v2 disks with customer-managed keys using Azure Key Vaults stored in a different Microsoft Entra ID tenant isn't currently supported.
- Encryption at host is supported on Premium SSD v2 disks with some limitations. For more information, see [Encryption at host](/azure/virtual-machines/disk-encryption#restrictions-1).
- Azure Disk Encryption (guest VM encryption via BitLocker/DM-Crypt) isn't supported for VMs with Premium SSD v2 disks. We recommend you to use encryption at rest with platform-managed or customer-managed keys, which is supported for Premium SSD v2. 
- Currently, Premium SSD v2 disks can't be attached to VMs in Availability Sets. 
- Azure Site Recovery isn't supported for VMs with Premium SSD v2 disks.
- Premium SSDv2 doesn't support host caching.
