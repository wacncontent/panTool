



For details about the VM Agents and how they work to support VM Extensions, see [Windows](/documentation/articles/virtual-machines-windows-classic-manage-extensions/) or [Linux](/documentation/articles/virtual-machines-linux-classic-manage-extensions/) VM Agent and VM Extensions Overview.

## Azure VM Extensions
VM Extensions implement most of the critical functionality that you want to use with your VMs, including basic functionality like resetting passwords, configuring RDP, and many, many others. Because new extensions are added all the time, the number of possible features your VMs support in Azure continues to increase. By default, several basic VM extensions are installed when you create your VM from the Image Gallery, including **IaaSDiagnostics** (currently Windows VMs only), **VMAccess**, and **BGInfo** (also currently Windows only). However, not all extensions are implemented on both Windows and Linux at any specific time, due to the constant flow of feature updates and new extensions.

## Connectivity and Basic Management
The following extensions are critical to enabling, re-enabling, or disabling basic connectivity with your VMs once they are created and running.

| VM Extension Name | Feature Description | More Information |
| --- | --- | --- |
| VMAccessAgent (Windows) |Create, update, and reset user information and RDP connection configurations. |[Windows](/documentation/articles/virtual-machines-windows-classic-extensions-customscript/) |
| VMAccessForLinux (Linux) |Create, update, and reset user information and SSH connection configurations. |[Linux](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |

## Deployment and Configuration Management
The following extensions support different kinds of deployment and configuration management scenarios and features. See also the section on VM Operations and Management, below.

| VM Extension Name | Feature Description | More Information |
| --- | --- | --- |
| **Octopus Deploy** (DSC Extension-based) |Supports automated deployment of ASP.NET web applications and Windows Services into development, test, and production environments. |[Getting Started with Octopus Deploy](http://docs.octopusdeploy.com/display/OD/Getting%20started) |
| **Visual Studio Release Manager** (DSC Extension-based) |Supports continuous deployment with Visual Studio. |[Automate deployments with Release Management](https://msdn.microsoft.com/Library/vs/alm/Release/overview) |
| **DSC** |PowerShell DSC (Desired State Configuration) Extension. |[Azure PowerShell DSC (Desired State Configuration) extension](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx) |
| **CustomScriptExtension** (Windows)**CustomScriptForLinux** (Linux) |Invokes custom scripts on the VM at any time: startup or during lifetime. |[Custom Script Extension](/documentation/articles/virtual-machines-windows-classic-extensions-customscript/) |
| **AzureCATExtensionHandler** |Consumes the diagnostic data collected by **IaaSDiagnostics** and few other data sources such as [Azure Storage Analytics Metrics](https://msdn.microsoft.com/zh-cn/library/azure/hh343270.aspx) and transforms it into an aggregated data set appropriate for SAP Host control process to consume |[Azure Enhanced Monitoring for SAP](https://azure.microsoft.com/blog/2014/06/04/azure-enhanced-monitoring-for-sap/) |

## Security and Protection
The extensions in this section provide critical security features for your Azure VMs.

| VM Extension Name | Feature Description | More Information |
| --- | --- | --- |
| **TrendMicroDSA** |Enables TrendMicro's Deep Security platform support to provide intrusion detection and prevention, firewall, anti-malware, web reputation, log inspection, and integrity monitoring. |[How to install and configure Trend Micro Deep Security as a Service on an Azure VM](/documentation/articles/virtual-machines-windows-classic-install-trend/) |
| **IaaSAntimalware** |Microsoft Antimalware for Azure Cloud Services and Virtual Machines is a real-time protection capability that helps identify and remove viruses, spyware, and other malicious software, with configurable alerts when known malicious or unwanted software attempts to install itself or run on your system. |[Antimalware for Azure Cloud Services and Virtual Machines](/documentation/articles/azure-security-antimalware/) |
| **SymantecEndpointProtection** |Symantec Endpoint Protection 12.1.4 enables security and performance across physical and virtual systems. |[How to install and configure Symantec Endpoint Protection on an Azure VM](/documentation/articles/virtual-machines-windows-classic-install-symantec/) |

## VM Operations and Management
Supports common operations management features and behavior. See also the section on Deployment and Configuration Management, above.

| **VM Extension Name** | Feature Description | More Information |
| --- | --- | --- |
| **AzureVmLogCollector** |You can use the **AzureVMLogCollector** Extension on-demand to perfom one-time collection of logs from one or more Cloud Service VMs (from both web roles and worker roles) and transfer the collected files to an Azure storage account - all without remotely logging on to any of the VMs. |[AzureLogCollector Extension](/documentation/articles/virtual-machines-windows-log-collector-extension/) |
| **IaaSDiagnostics** |Enables, disables, and configures Azure Diagnostics, and is also used by the **AzureCATExtensionHandler** to support SAP monitoring. |[Azure Virtual Machine Monitoring with Azure Diagnostics Extension](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| **OSPatchingForLinux** |Enables the Azure VM administrators to automate the VM OS updates with the customized configurations. You can use the OSPatching Extension to configure OS updates for your virtual machines, including: Specify how often and when to install OS patches, specify what patches to install, and configure the reboot behavior after updates |[OS Patching Extension Blog Post](https://azure.microsoft.com/blog/2014/10/23/automate-linux-vm-os-updates-using-ospatching-extension/). See also the readme and source on GitHub at [OS Patching Extension](https://github.com/Azure/azure-linux-extensions). |

## Developing and Debugging
These VM Extensions are listed here for completeness, as they provide support for Visual Studio-related features and are not intended to be used directly.

| VM Extension Name | Feature Description | More Information |
| --- | --- | --- |
| **VS14CTPDebugger** |Supports remote debugging from VS using the Azure SDK 2.4 |[Remote Debugging in Visual Studio](https://msdn.microsoft.com/zh-cn/library/y7f5zaaa.aspx) |
| **VS2013Debugger** |Supports remote debugging from VS using the Azure SDK 2.4 | |
| **VS2012Debugger** |Supports remote debugging from VS using the Azure SDK 2.4 | |
| **WebDeployForVSDevTest** |Installs and configures IIS and Web Deploy on Windows Server. Removing or disabling it is not supported. | |

## Miscellaneous Features
These extensions provide support for other VM features that might be useful.

| VM Extension Name | Feature Description | More Information |
| --- | --- | --- |
| **BGInfo** |Presents a consolidated picture of useful server information on the desktop when using RDP. |[BGInfo Extension](https://msdn.microsoft.com/zh-cn/library/mt589195.aspx) |
