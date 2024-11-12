---
title: Azure VM Watch
description: Learn how to use Azure VM Watch service offering
author:      iamwilliew 
ms.author:   wwilliams
ms.service: azure-virtual-machines
ms.topic: conceptual
ms.date:     09/20/2024
---

# VM watch - Enhancing VM health monitoring (Preview)

VM watch is a standardized, lightweight, and adaptable in-VM service offering for virtual machines and virtual machine scale sets. It runs health checks within the VM at configurable intervals and sends the results via a uniform data model to Azure. These health results are consumed by Azure's production monitoring AIOps (AI Operations) engines for regression detection and prevention. VM watch is delivered via the [Application Health VM extension](/azure/virtual-machines/extensions/health-extension?tabs=rest-api) to provide ease of deployment and manageability for customers. In addition, VM watch is offered at no extra cost for customers. 

## VM watch monitoring specifics

- **Ease of adoption**: VM watch is made available through the [Application Health VM extension](/azure/virtual-machines/extensions/health-extension?tabs=rest-api). 
- **Flexible Deployment:** Users can enable VM watch with ease via ARM template, PowerShell, or AZ CLI.
- **Compatibility:** VM watch operates seamlessly on both Linux and Windows environments. Also, VM watch is suitable for individual VMs and Virtual Machine Scale Set VMs alike.
- **Resource Governance:** VM watch provides efficient monitoring without impacting system performance. Resource caps are placed on the CPU and memory utilization of the VM watch process itself to protect the VM.
- **Ready Out-of-the-Box**: VM watch comes equipped with a suite of default tests, which are easily configurable to enable scenario specific tests. Detailed information regarding the tests (Checks, Metrics, and Event Logs) are given.

### Network

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **Outbound connectivity** | Check | Verify the network outbound connectivity from the Azure VM. |
| **DNS Resolution** | Check | Verify if one or more dns names can be resolved. |
| **SegmentsRetransmitted** | Metric | The number of TCP segments transmitted containing one or more previously transmitted octets. |
| **NormalizedSegmentsRetransmitted** | Metric | SegmentsRetransmitted / (SegmentsSent + SegmentsReceived) |
| **ConnectionResets** | Metric | Number of times TCP connections made a direct transition to the CLOSED state from either the ESTABLISHED state or the CLOSE_WAIT state |
| **NormalizedConnectionResets** | Metric | ConnectionResets / CurrentConnections |
| **FailedConnectionAttempts** | Metric | Number of times TCP connections made a direct transition to the CLOSED state from either the SYN_SENT state or the SYN_RCVD state. |
| **NormalizedFailedConnectionAttempts** | Metric | FailedConnectionAttempts / (ActiveConnectionOpenings + PassiveConnectionOpenings) |
| **ActiveConnectionOpenings** | Metric | Number of times TCP connections made a direct transition to the SYN_SENT state from the CLOSED state |
| **PassiveConnectionOpenings** | Metric | Number of times TCP connections made a direct transition to the SYN_RCVD state from the LISTEN state |
| **CurrentConnections** | Metric | Number of connections established |
| **SegmentsReceived** | Metric | Number of segments received, including those segments received in error |
| **SegmentsSent** | Metric | Number of segments sent, including those segments on current connections but excluding those segments containing only retransmitted octets |

 

### Disk

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **Azure Disk I/O** | Check | Verify file creation, write, read. Delete operations on each drive mounted to the VM |
| **FreeSpaceInBytes** | Metric | The free disk space of the target mount point |
| **UsedSpaceInBytes** | Metric | The used disk space of the target mount point |
| **CapacityInBytes**  | Metric | The disk space capacity of the target mount point |
| **UsedPercent**      | Metric | The used disk space percentage of the target mount point |
| **WriteOps**         | Metric | The write operations per seconds of the target disk/partition |
| **ReadOps**          | Metric | The read operations per seconds of the target disk/partition | 


### CPU

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **ProcessCoreUsage** | Metric | An instantaneous measurement of the percentage of a single CPU core that the target process is using (100 = 100%, a whole core) |
| **ProcessMachineUsage** | Metric | The percentage of the machine's total CPU that this process is using |
| **MachineTotalCpuUsage** | Metric | The VM's total instantaneous CPU utilization |

 

### Process

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **Process Creation** | Check | Starts a lightweight process to validate that process creation is possible |
| **Running Process(es)** | Check | Verify if the target process(es) are running |
| **UpTime** | Metric | How long the target process has been up and running since last process startup |

 

### IMDS

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **IMDS** | Check | Verify user can reach IMDS endpoint from within the VM and VM information is returned from the IMDS endpoint query |

 

### Clock

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **Clock Skew** | Check | Verify the clock skew between remote NTP server and the Azure VM. For Windows VM, fallback to check if Windows Time Service is synced with w32tm if remote NTP server is inaccessible |

 

### AzBlob

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **Azure Storage blob connectivity** | Check | Verify the connectivity to the Azure Storage Blob and download the Blob with MSI or SAS token. |

### Hardware

| **Signal Name** | **Type** | **Description** |
|:---:|:---:|:---:|
| **Hardware Health Monitor** | EventLog | Collect hardware health info from Windows event log, currently only disk related critical events are collected, including events with ID 7, 500, 504, 505, 512 and 549 |

### Next steps

More information on how to install VM watch: [Install VM watch](install-vm-watch.md)



