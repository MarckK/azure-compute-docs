---
title: Ephemeral OS disks
description: Learn more about ephemeral OS disks for Azure VMs.
author: viveksingla08
ms.service: azure-virtual-machines
ms.custom:
ms.topic: how-to
ms.date: 07/23/2020
ms.author: viveksingla
ms.subservice: disks
---

# Ephemeral OS disks for Azure VMs

**Applies to:** :heavy_check_mark: Linux VMs :heavy_check_mark: Windows VMs :heavy_check_mark: Flexible scale sets :heavy_check_mark: Uniform scale sets

Ephemeral OS disks are created on the local virtual machine (VM) storage and not saved to the remote Azure Storage. Ephemeral OS disks are ideal for stateless workloads, where applications can tolerate individual VM failures but are sensitive to VM deployment times or the reimaging of individual VM instances. With Ephemeral OS disk, you get lower read/write latency to the OS disk and faster VM reimage.

The key features of ephemeral disks are:

- Designed for stateless applications.
- Supported on all images - Marketplace, custom images, and [Azure Compute Gallery](./shared-image-galleries.md) (formerly known as Shared Image Gallery).
- Provides fast reimage to reset virtual machines (VMs) and scale set instances to their original boot state.
- Offers lower latency, similar to a temporary disk.
- Ensures no storage cost for operating system disks, as ephemeral OS disks are free.
- Supported in all Azure regions.

Key differences between persistent and ephemeral OS disks:

|   | Persistent OS Disk | Ephemeral OS Disk |
|---|---|---|
| **Size limit for OS disk** | 4* TiB | Cache, temp, or NVMe disk size for the VM size or 2,040 GiB, whichever is smaller. For the **cache, temp or NVMe size in GiB**, see [DSv3](sizes-general.md), [Esv3](sizes-memory.md), [M](sizes-memory.md), [FS](sizes-compute.md), and [GS](sizes-previous-gen.md#gs-series) |
| **VM sizes supported** | All | VM sizes with local storage such as DSv3, Esv3, Fs, FsV2, GS, M, Mdsv2, Bs, Dav4, Eav4 |
| **Disk type support**| Managed and unmanaged OS disk| Managed OS disk only|
| **Region support**| All regions| All regions|
| **Data persistence**| OS disk data written to OS disk are stored in Azure Storage| Data written to OS disk is stored on local VM storage and isn't persisted to Azure Storage. |
| **Stop-deallocated state**| VMs and scale set instances can be stop-deallocated and restarted from the stop-deallocated state | Not Supported |
| **Specialized OS disk support** | Yes| No|
| **OS disk resize**| Supported during VM creation and after VM is stop-deallocated| Supported during VM creation only|
| **Resizing to a new VM size**| OS disk data is preserved| Data on the OS disk is deleted, OS is reprovisioned |
| **Redeploy** | OS disk data is preserved | Data on the OS disk is deleted, OS is reprovisioned |
| **Stop/ Start of VM** | OS disk data is preserved | Not Supported |
| **Page file placement**| For Windows, page file is stored on the resource disk| For Windows, page file is stored on the OS disk (for cache placement, Temp disk placement & NVMe disk placement).|
| **Maintenance of VM/VMSS using [healing](understand-vm-reboots.md#unexpected-downtime)** | OS disk data is preserved | OS disk data isn't preserved  |
| **Maintenance of VM/VMSS using [Live Migration](maintenance-and-updates.md#live-migration)** | OS disk data is preserved | OS disk data is preserved  |

\* 4 TiB is the maximum supported OS disk size for managed (persistent) disks. However, many OS disks are partitioned with master boot record (MBR) by default and are limited to 2 TiB. For details, see [OS disk](managed-disks-overview.md#os-disk).

## Placement options for Ephemeral OS disks

Ephemeral OS Disk utilizes local storage within the VM. Since different VMs have different types of local storage (cache disk, resource disk, and NVMe disk), the placement option defines where the Ephemeral OS Disk is stored. Placement option however doesn't impact the performance or cost of Ephemeral OS disk. Its performance is dependent upon the VM's local storage. Depending upon the VM type, we offer three different types of placement:
 1. **NVMe Disk Placement (In Public Preview)**  - NVMe disk placement type is available on the latest generation VMs like Dadsv6, Ddsv6 etc. 
 2. **Temp Disk Placement**  - Temp disk placement type is available on VMs with Temp disk like Dadsv5, Ddsv5 etc.
 3. **Cache Disk Placement**  - Cache disk placement type is available on older VMs that had cache disk like Dsv2, Dsv3 etc.

[DiffDiskPlacement](/rest/api/compute/virtualmachines/list#diffdiskplacement) is the new property that can be used to specify where you want to place the Ephemeral OS disk. With this feature, when a Windows VM is provisioned, we configure the pagefile to be located on the OS Disk.

## Size requirements

You can choose to deploy Ephemeral OS Disk on NVMe disk, temp disk, or cache on the VM.
The image OS disk’s size should be less than or equal to the NVMe/temp/cache size of the VM size chosen.

For example, if you want to opt for **OS cache placement**: Standard Windows Server images from the marketplace are about 127 GiB, which means that you need a VM size that has a cache equal to or larger than 127 GiB. The Standard_DS3_v2 has a cache size of 127 GiB, which is large enough. In this case, the Standard_DS3_v2 is the smallest size in the DSv2 series that you can use with this image.

For example, if you want to opt for **Temp disk placement**: Standard Ubuntu server image from marketplace is about 30 GiB. To enable Ephemeral OS disk on temp, the temp disk size must be equal to or larger than 30 GiB. Standard_B4ms has a temp size of 32 GiB, which can fit the 30-GiB OS disk. Upon creation of the VM, the temp disk space would be 2 GiB.

For example, if you want to opt for **NVMe disk placement (In Public Preview)**: Standard Ubuntu server image from marketplace is about 30 GiB. To enable Ephemeral OS disk on NVMe, the NVMe disk size must be equal to or larger than 30 GiB. Standard_D2ads_v6 has a temp size of 110 GiB, which can easily fit the 30-GiB OS disk. However, Ephemeral OS disk occupies the entire NVMe disk and there's no NVMe disk space given back. One way to maximize the use of NVMe disk is by maximizing the OS disk Size property to 110 GiB. 


> [!IMPORTANT]
> If opting for temp disk placement the Final Temp disk size = (Initial temp disk size - OS image size).
> 
> If opting for NVMe disk placement (In Public Preview), Final NVMe Disk size = (Total no. of NVMe disks - NVMe Disks used for OS) * Size of each NVMe disk. where NVMe Disks used for OS is the minimum number of disks required for OS disk depending on the size of OS disk and the size of each NVMe disk.

If Ephemeral OS disk is using **Temp Disk Placement**, it shares the IOPS(input/output operations per second) with temp disk as per the VM size chosen by you. If Ephemeral OS disk is using **NVMe Disk Placement**, it provides the IOPS(input/output operations per second) of One NVMe disk as per the VM size chosen by you.

Basic Linux and Windows Server images in the Marketplace that are denoted with `[smallsize]` tend to be around 30 GiB and can use most of the available VM sizes.
Ephemeral disks also require that the VM size supports **Premium storage**. The sizes usually (but not always) have an `s` in the name, like DSv2 and EsV3. For more information, see [Azure VM sizes](sizes.md) for details around which sizes support Premium storage.

> [!NOTE]
>
> Ephemeral disk will not be accessible through the portal. You will receive a "Resource not Found" or "404" error when accessing the ephemeral disk which is expected.
>

## Unsupported features

- VM Image Capture
- Disk snapshots
- Azure Disk Encryption
- Azure Backup
- Azure Site Recovery
- OS Disk Swaps

## Trusted Launch for Ephemeral OS disks

Ephemeral OS disks can be created with Trusted launch. All regions are supported for Trusted Launch; not all virtual machines sizes are supported. Check [Virtual machines sizes supported](trusted-launch.md#virtual-machines-sizes) for supported sizes.
VM guest state (VMGS) is specific to trusted launch VMs. It's an Azure-managed blob and contains the unified extensible firmware interface (UEFI) secure boot signature databases and other security information. VMs using trusted launch by default reserve **1 GiB** from the **OS cache** or **temp storage** based on the chosen placement option for VMGS. The lifecycle of the VMGS blob is tied to that of the OS Disk.

For example, If you try to create a Trusted launch Ephemeral OS disk VM using OS image of size 56 GiB with VM size [Standard_DS4_v2](dv2-dsv2-series.md) using temp disk placement you would get an error as
**"OS disk of Ephemeral VM with size greater than 55 GB is not allowed for VM size Standard_DS4_v2 when the DiffDiskPlacement is ResourceDisk."**
This error occurs because the temp storage for [Standard_DS4_v2](dv2-dsv2-series.md) is 56 GiB, and 1 GiB is reserved for VMGS when using trusted launch.
For the same example, if you create a standard Ephemeral OS disk VM you wouldn't get any errors and it would be a successful operation.

> [!IMPORTANT]
>
> While using ephemeral disks for Trusted Launch VMs, keys and secrets generated or sealed by the vTPM after VM creation may not be persisted for operations like reimaging and platform events like service healing.
>
For more information on [how to deploy a trusted launch VM](trusted-launch-portal.md)

## Confidential VMs using Ephemeral OS disks

AMD-based Confidential VMs cater to high security and confidentiality requirements of customers. These VMs provide a strong, hardware-enforced boundary to help meet your security needs. There are limitations to use Confidential VMs. Check the [region](/azure/confidential-computing/confidential-vm-overview#regions), [size](/azure/confidential-computing/confidential-vm-overview#size-support), and [OS supported](/azure/confidential-computing/confidential-vm-overview#os-support) limitations for confidential VMs.
Virtual machine guest state (VMGS) blob contains the security information of the confidential VM.
Confidential VMs using Ephemeral OS disks by default **1 GiB** from the **OS cache** or **temp storage** based on the chosen placement option is reserved for VMGS. The lifecycle of the VMGS blob is tied to that of the OS Disk. **NVMe Disk placement** is currently not supported for Confidential VMs.
> [!IMPORTANT]
>
> When choosing a confidential VM with full OS disk encryption before VM deployment that uses a customer-managed key (CMK). [Updating a CMK key version](/azure/storage/common/customer-managed-keys-overview#update-the-key-version) or [key rotation](/azure/key-vault/keys/how-to-configure-key-rotation) is not supported with Ephemeral OS disk. Confidential VMs using Ephemeral OS disks need to be deleted before updating or rotating the keys and can be re-created subsequently.
>
For more information on [confidential VM](/azure/confidential-computing/confidential-vm-overview)

## Customer Managed key

You can choose to use customer managed keys or platform managed keys when you enable end-to-end encryption for VMs using Ephemeral OS disk. Currently this option is available only via [PowerShell](./windows/disks-enable-customer-managed-keys-powershell.md), [CLI](./linux/disks-enable-customer-managed-keys-cli.md), and SDK in all regions.

> [!IMPORTANT]
>
> [Updating a CMK key version](/azure/storage/common/customer-managed-keys-overview#update-the-key-version) or [key rotation](/azure/key-vault/keys/how-to-configure-key-rotation) of customer managed key is not supported with Ephemeral OS disk. VMs using Ephemeral OS disks need to be deleted before updating or rotating the keys and can be re-created subsequently.
>
For more information on [Encryption at host](./disk-encryption.md)

## Next steps

Create a VM with ephemeral OS disk using [Azure Portal/CLI/PowerShell/ARM template](ephemeral-os-disks-deploy.md).
Check out the [frequently asked questions on ephemeral os disk](ephemeral-os-disks-faq.md).
