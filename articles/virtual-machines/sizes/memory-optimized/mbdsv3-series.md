---
title: Mbdsv3 size series
description: Information on and specifications of the Mbdsv3-series sizes
author: mattmcinnes
ms.service: azure-virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 10/30/2024
ms.author: mattmcinnes
ms.reviewer: mattmcinnes
---

# Mbdsv3 sizes series

[!INCLUDE [mbdsv3-summary](./includes/mbdsv3-series-summary.md)]

## Host specifications
[!INCLUDE [mbdsv3-series-specs](./includes/mbdsv3-series-specs.md)]

## Feature support
[Premium Storage](../../premium-storage-performance.md): Supported <br>[Premium Storage caching](../../premium-storage-performance.md): Supported <br>[Live Migration](../../maintenance-and-updates.md): Not Supported <br>[Memory Preserving Updates](../../maintenance-and-updates.md): Not Supported <br>[Generation 2 VMs](../../generation-2.md): Supported <br>[Generation 1 VMs](../../generation-2.md): Not Supported <br>[Write Accelerator](../../how-to-enable-write-accelerator.md): Supported<br>[Accelerated Networking](/azure/virtual-network/create-vm-accelerated-networking-cli): Supported <br>[Ephemeral OS Disk](../../ephemeral-os-disks.md): Supported <br>[Nested Virtualization](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization): Not Supported <br>

## Sizes in series

### [Basics](#tab/sizebasic)

vCPUs (Qty.) and Memory for each size

| Size Name | vCPUs (Qty.) | Memory (GB) |
| --- | --- | --- |
| Standard_M16bds_v3 | 16 | 128 |
| Standard_M32bds_v3 | 32 | 256 |
| Standard_M48bds_v3 | 48 | 384 |
| Standard_M64bds_v3 | 64 | 512 |
| Standard_M96bds_v3 | 96 | 768 |
| Standard_M128bds_v3 | 128 | 1024 |
| Standard_M176bds_v3<sup>1</sup> | 176 | 1536 |
| Standard_M64bds_1_v3 | 64 | 1397 |
| Standard_M96bds_2_v3 | 96 | 1946 |
| Standard_M128bds_3_v3 | 128 | 2794 |
| Standard_M176bds_4_v3<sup>1</sup> | 176 | 3892 |

#### VM Basics resources
- [Check vCPU quotas](../../../virtual-machines/quotas.md)

#### Table definitions
- <sup>1</sup> [Disable SMT](/sql/sql-server/compute-capacity-limits-by-edition-of-sql-server#disable-smt-in-an-azure-virtual-machine) to run SQL Server on a VM with more than 64 vCores per NUMA node.

### [Local Storage](#tab/sizestoragelocal)

Local (temp) storage info for each size

| Size Name | Max Temp Storage Disks (Qty.) | Temp Disk Size (GiB) | Temp Disk Random Read (RR)<sup>1</sup> IOPS<sup>2</sup> | Temp Disk Random Read (RR)<sup>1</sup> Throughput (MB/s) |
| --- | --- | --- | --- | --- |
| Standard_M16bds_v3 | 1 | 400 | 10,000 | 100 |
| Standard_M32bds_v3 | 1 | 400 | 20,000 | 200 |
| Standard_M48bds_v3 | 1 | 400 | 40,000 | 400 |
| Standard_M64bds_v3 | 1 | 400 | 40,000 | 400 |
| Standard_M96bds_v3 | 1 | 400 | 40,000 | 400 |
| Standard_M128bds_v3 | 1 | 400 | 160,000 | 1600 |
| Standard_M176bds_v3 | 1 | 400 | 160,000 | 1600 |
| Standard_M64bds_1_v3 | 1 | 3000 | 40,000 | 400 |
| Standard_M96bds_2_v3 | 1 | 4500 | 40,000 | 400 |
| Standard_M128bds_3_v3 | 1 | 6000 | 160,000 | 1600 |
| Standard_M176bds_4_v3 | 1 | 8000 | 160,000 | 1600 |

#### Storage resources
- [Introduction to Azure managed disks](../../../virtual-machines/managed-disks-overview.md)
- [Azure managed disk types](../../../virtual-machines/disks-types.md)
- [Share an Azure managed disk](../../../virtual-machines/disks-shared.md)

#### Table definitions
- <sup>1</sup> Temp disk speed often differs between RR (Random Read) and RW (Random Write) operations. RR operations are typically faster than RW operations. The RW speed is usually slower than the RR speed on series where only the RR speed value is listed.
- <sup>2</sup> Read IOPS are optimized for sequential reads.<br> 
- Storage capacity is shown in units of GiB or 1024^3 bytes. When you compare disks measured in GB (1000^3 bytes) to disks measured in GiB (1024^3) remember that capacity numbers given in GiB may appear smaller. For example, 1023 GiB = 1098.4 GB.
- Disk throughput is measured in input/output operations per second (IOPS) and MBps where MBps = 10^6 bytes/sec.
- To learn how to get the best storage performance for your VMs, see [Virtual machine and disk performance](../../../virtual-machines/disks-performance.md).

### [Remote Storage](#tab/sizestorageremote)

Remote (uncached) storage info for each size

| Size Name | Max Remote Storage Disks (Qty.) | Uncached Premium SSD Disk IOPS | Uncached Premium SSD Throughput (MB/s) | Uncached Ultra Disk and Premium SSD v2 IOPS | Uncached Ultra Disk and Premium SSD v2 Throughput (MB/s) |
| --- | --- | --- | --- | --- | --- |
| Standard_M16bds_v3 | 64 | 44,000 | 1,000 | 64,000 | 1,000 |
| Standard_M32bds_v3 | 64 | 88,000 | 2,000 | 88,000 | 2,000 |
| Standard_M48bds_v3 | 64 | 88,000 | 2,000 | 120,000 | 2,000 |
| Standard_M64bds_v3 | 64 | 88,000 | 2,000 | 160,000 | 2,000 |
| Standard_M96bds_v3 | 64 | 260,000 | 4,000 | 260,000 | 4,000 |
| Standard_M128bds_v3 | 64 | 260,000 | 4,000 | 400,000 | 4,000 |
| Standard_M176bds_v3 | 64 | 260,000 | 6,000 | 650,000 | 6,000 |
| Standard_M64bds_1_v3 | 64 | 130,000 | 6,000 | 160, 000 | 6,000 |
| Standard_M96bds_2_v3 | 64 | 130,000 | 8,000 | 260,000 | 8,000 |
| Standard_M128bds_3_v3 | 64 | 260,000 | 8,000 | 400,000 | 10,000 |
| Standard_M176bds_4_v3 | 64 | 260,000 | 8,000 | 650,000 | 10,000 |

#### Storage resources
- [Introduction to Azure managed disks](../../../virtual-machines/managed-disks-overview.md)
- [Azure managed disk types](../../../virtual-machines/disks-types.md)
- [Share an Azure managed disk](../../../virtual-machines/disks-shared.md)

#### Table definitions
- <sup>1</sup>Some sizes support [bursting](../../disk-bursting.md) to temporarily increase disk performance. Burst speeds can be maintained for up to 30 minutes at a time.

- Storage capacity is shown in units of GiB or 1024^3 bytes. When you compare disks measured in GB (1000^3 bytes) to disks measured in GiB (1024^3) remember that capacity numbers given in GiB may appear smaller. For example, 1023 GiB = 1098.4 GB.
- Disk throughput is measured in input/output operations per second (IOPS) and MBps where MBps = 10^6 bytes/sec.
- Data disks can operate in cached or uncached modes. For cached data disk operation, the host cache mode is set to ReadOnly or ReadWrite. For uncached data disk operation, the host cache mode is set to None.
- To learn how to get the best storage performance for your VMs, see [Virtual machine and disk performance](../../../virtual-machines/disks-performance.md).


### [Network](#tab/sizenetwork)

Network interface info for each size

| Size Name | Max NICs (Qty.) | Max Network Bandwidth (Mb/s) |
| --- | --- | --- |
| Standard_M16bds_v3 | 8 | 8,000 |
| Standard_M32bds_v3 | 8 | 16,000 |
| Standard_M48bds_v3 | 8 | 16,000 |
| Standard_M64bds_v3 | 8 | 16,000 |
| Standard_M96bds_v3 | 8 | 25,000 |
| Standard_M128bds_v3 | 8 | 40,000 |
| Standard_M176bds_v3 | 8 | 50,000 |
| Standard_M64bds_1_v3 | 8 | 20,000 |
| Standard_M96bds_2_v3 | 8 | 20,000 |
| Standard_M128bds_3_v3 | 8 | 40,000 |
| Standard_M176bds_4_v3 | 8 | 40,000 |

#### Networking resources
- [Virtual networks and virtual machines in Azure](/azure/virtual-network/network-overview)
- [Virtual machine network bandwidth](/azure/virtual-network/virtual-machine-network-throughput)

#### Table definitions
- Expected network bandwidth is the maximum aggregated bandwidth allocated per VM type across all NICs, for all destinations. For more information, see [Virtual machine network bandwidth](/azure/virtual-network/virtual-machine-network-throughput)
- Upper limits aren't guaranteed. Limits offer guidance for selecting the right VM type for the intended application. Actual network performance will depend on several factors including network congestion, application loads, and network settings. For information on optimizing network throughput, see [Optimize network throughput for Azure virtual machines](/azure/virtual-network/virtual-network-optimize-network-bandwidth). 
-  To achieve the expected network performance on Linux or Windows, you may need to select a specific version or optimize your VM. For more information, see [Bandwidth/Throughput testing (NTTTCP)](/azure/virtual-network/virtual-network-bandwidth-testing).

### [Accelerators](#tab/sizeaccelerators)

Accelerator (GPUs, FPGAs, etc.) info for each size

> [!NOTE]
> No accelerators are present in this series.

---

[!INCLUDE [sizes-footer](../includes/sizes-footer.md)]

