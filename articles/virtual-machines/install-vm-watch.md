---
title: Install VM Watch
description: Getting started guide to installing VM watch on virtual machines
author: iamwilliew 
ms.author: wwilliams
ms.service: azure-virtual-machines
ms.topic: get-started
ms.date:     09/23/2024
ms.subservice: monitoring
---

# Install VM watch (Preview)

Users can enable VM watch with ease via [ARM template](/azure/azure-resource-manager/templates/), [PowerShell](/powershell/), or [AZ CLI](/cli/azure/) on Azure Virtual Machines and Azure Virtual Machine Scale Sets. VM watch can be enabled on both Linux and Windows virtual machines. VM watch is delivered through the [Application Health VM extension](/azure/virtual-machines/extensions/health-extension?tabs=rest-api) for ease of adoption. The code in this document details the steps to install the application health virtual machine extension and enable VM watch.
> [!Note]
> The code segments require user input. Any labels within `<>` in the code, need to be replace with values specific to your installation. Here is a list of parameters with instructions on what to replace with.
> 
> | **Parameters** | **Description** |
> |---|---|
> | **`<your subscription id>`** | This is your Azure subscription id in which you want to install VM watch. |
> | **`<your vm name>`** | This is the name of your virtual machine to which the extension is being installed.  |
> | **`<your resource group name>`** | This is the name of the resource group in your Azure subscription that your vm will be assigned to. |
> | **`<your location>`** | This is the Azure region in which your VM is installed. |
> | **`<your extension name`** | This name will be assigned to the Application Health VM extension being installed. |
> | **`<application health extension type>`** | Specifies whether Windows or Linux Application Health Extension is to be installed.|
> | **`<your vm scale set name>`** | This is your VM scale set name in which you want to install VM watch. |

## Pre-requisites
 ### 1. Register feature
  Execute the code using Azure Command Line to register for adopting VM watch.
  
  ```
  az feature register --name VMWatchPreview --namespace Microsoft.Compute --subscription <your subscription id>
  az provider register --namespace Microsoft.Compute --subscription <your subscription id>

  ```
  ---
  #### Validate feature registration

  Validate you registered for the VM watch feature successfully by running the command.

  ```
  az feature show --namespace Microsoft.Compute --name VMWatchPreview --subscription <your subscription id>
  ```
---
 ### 2. Ensure VM is installed
 
For more information on how to create a VM and/or virtual machine scale set, see [Quickstart guide - Windows](/azure/virtual-machines/windows/quick-create-portal) for Windows and [Quickstart guide - Linux](/azure/virtual-machines/linux/quick-create-portal?tabs=ubuntu) for Linux.

> [!Important]
> If Application health extension is already installed on the VM, ensure the settings `autoUpgradeMinorVersion` is set to `true` and `enableAutomaticUpgrade` is set to `true`.
 
## Installing VM watch on Azure Virtual Machines

> [!Important]
> The code segment is identical for both Windows and Linux except for the value of the parameter `<application health extension type>` passed in to the Extension Type.
>
> Please replace
> `<application health extension type>` with `"ApplicationHealthLinux"` for Linux and `"ApplicationHealthWindows"` for Windows installations. 

#### [CLI](#tab/cli-1)
```
az vm extension set --resource-group <your resource group> --vm-name <your vm name> --name <application health extension type> --publisher Microsoft.ManagedServices --version 2.0 --settings '{"vmWatchSettings": {"enabled": true}}' --enable-auto-upgrade true 
```
#### [PowerShell](#tab/powershell-1)
```
Set-AzVMExtension -ResourceGroupName "<your resource group>" -Location "<your vm region>" -VMName "<your vm name>" -Name "<your extension name>" -Publisher "Microsoft.ManagedServices" -ExtensionType "<application health extension type>" -TypeHandlerVersion "2.0" -Settings @{"vmWatchSettings" = @{"enabled" = $True}} -EnableAutomaticUpgrade $True 
```
#### [ARM template](#tab/ARM-template-1)

```
        "type": "Microsoft.Compute/virtualMachines/extensions", 
        "apiVersion": "2019-07-01", 
        "name": "[concat('<your vm name>', '/', '<your extension name')]", 
        "location": "<your vm region>", 
        "dependsOn": [ 
            "[resourceId('Microsoft.Compute/virtualMachines', parameters('<your vm name>'))]" 
        ], 

        "properties": { 
            "enableAutomaticUpgrade": true, 
            "publisher": "Microsoft.ManagedServices", 
            "typeHandlerVersion": "2.0", 
            "type": "<application health extension type>", 
            "settings": { 
                "vmWatchSettings": { 
                    "enabled": true
                 } 
             } 
         }  
```
---
### Validate Application Health VM extension is installed in the Azure VM
On successful installation, navigate to the [Azure portal](https://portal.azure.com) to confirm that the ***Application Health VM extension*** was successfully installed.

**Windows**

:::image type="content" source="./media/vm-watch/windows-vm-watch-virtual-machine.png" alt-text="Screenshot of the Windows VM installation." lightbox="./media/vm-watch/windows-vm-watch-virtual-machine.png":::


**Linux**

:::image type="content" source="./media/vm-watch/linux-vm-watch-virtual-machine.png" alt-text="Screenshot of the Linux VM installation."lightbox="./media/vm-watch/linux-vm-watch-virtual-machine.png":::

To confirm that ***VM watch*** was enabled on this VM, navigate back to the Overview Page and click on the JSON view for the VM. Ensure the configuration exists in the JSON.

```
  "settings": {  
      "vmWatchSettings": {  
          "enabled": true  
      }
  }
```
---

## Installing VM watch on Azure Virtual Machine Scale Sets

> [!Important]
> The code segment is identical for both Windows and Linux except for the value of the parameter `<application health extension type`> passed in to the Extension Type.
> Please replace
> `<application health extension type>` with `"ApplicationHealthLinux"` for Linux and `"ApplicationHealthWindows"` for Windows installations.  

#### [CLI](#tab/cli-2)
```
az vmss extension set --resource-group '<your resource group name>' --vmss-name '<your vm scale set name>' --name <application health extension type> --publisher Microsoft.ManagedServices --version 2.0 --settings '{"vmWatchSettings": {"enabled": true}}' --enable-auto-upgrade true
```
#### [PowerShell](#tab/powershell-2)
```
### Define the scale set variables 
$vmScaleSetName = "<your vm scale set name>" 
$vmScaleSetResourceGroup = "<your resource group name>" 

### Define the setting to enable vmwatch 
$publicConfig = @{"vmWatchSettings" = @{"enabled" = $true}} 
$extensionName = "<your extension name>" 
$extensionType = "<application health extension type>" 
$publisher = "Microsoft.ManagedServices" 

### Get the scale set object 
$vmScaleSet = Get-AzVmss ` 
  -ResourceGroupName $vmScaleSetResourceGroup ` 
  -VMScaleSetName $vmScaleSetName

### Add the Application Health extension to the scale set model 
Add-AzVmssExtension -VirtualMachineScaleSet $vmScaleSet ` 
  -Name $extensionName ` 
  -Publisher $publisher ` 
  -Setting $publicConfig ` 
  -Type $extensionType ` 
  -TypeHandlerVersion "2.0" ` 
  -EnableAutomaticUpgrade $True

### Update the scale set 
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup ` 
  -Name $vmScaleSetName ` 
  -VirtualMachineScaleSet $vmScaleSet

### Upgrade instances to install the extension 
Update-AzVmssInstance -ResourceGroupName $vmScaleSetResourceGroup ` 
  -VMScaleSetName $vmScaleSetName ` 
  -InstanceId '*' 
```
#### [ARM template](#tab/ARM-template-2)

```
   "type": "Microsoft.Compute/virtualMachineScaleSets",
   "apiVersion": "2024-07-01",
   "name": "<your vm scale set name>",
   "location": "<your vm region>",  
   "properties": {  
        "virtualMachineProfile": {
            "extensionProfile": {
               "extensions": [
                    {
                        "name": "[concat(variables('<your vm scale set name>'), '/', '<your extension name>')]",  
                        "properties": {  
                            "publisher": "Microsoft.ManagedServices",  
                            "type": "<application health extension type>",  
                            "typeHandlerVersion": "2.0",  
                            "autoUpgradeMinorVersion": true,
                            "enableAutomaticUpgrade": true,
                            "settings": {  
                                "vmWatchSettings": {  
                                    "enabled": true  
                                }  
                            }  
                        } 
                    }  
                ]  
            }  
        }  
    }     

```
---

### Validate Application Health VM extension is installed in the Azure virtual machine scale set

On Successful installation, navigate to the [Azure portal](https://portal.azure.com) to confirm that the ***Application Health VM extension*** was successfully installed in the virtual machine scale set.

**Windows**

:::image type="content" source="./media/vm-watch/windows-vm-watch-vm-scale-sets.png" alt-text="Screenshot of the Windows Virtual Machine Scale Set installation."lightbox="./media/vm-watch/windows-vm-watch-vm-scale-sets.png":::
**Linux**

:::image type="content" source="./media/vm-watch/linux-vm-watch-vm-scale-sets.png" alt-text="Screenshot of the Linux Virtual Machine Scale Set installation."lightbox="./media/vm-watch/linux-vm-watch-vm-scale-sets.png":::
---

To confirm that ***VM watch*** was enabled on this VM, navigate back to the Overview Page and click on the JSON view for the VM. Ensure the configuration exists in the JSON.

```
  "settings": {  
      "vmWatchSettings": {  
          "enabled": true  
      }
  }
```
## Next steps

- [Application Health Extension](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension?tabs=azure-cli)
- [Azure CLI](/cli/azure/install-azure-cli)
- [Azure portal](https://portal.azure.com/) 
