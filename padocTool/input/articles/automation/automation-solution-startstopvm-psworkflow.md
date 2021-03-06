<properties 
	pageTitle="Starting and stopping virtual machines with Azure Automation - PowerShell Workflow | Windows Azure"
   description="<!-- deleted by customization Graphical --><!-- keep by customization: begin -->PowerShell<!-- keep by customization: end --> version of Azure Automation solution including runbooks to start and stop classic virtual machines."
	services="automation"
	documentationCenter=""
	authors="bwren"
	manager="stevenka"
	editor="tysonn" />
<tags
	ms.service="automation"
	ms.date="09/25/2015"
	wacn.date=""/>

# Azure Automation solution - starting and stopping virtual machines

This Azure Automation solution includes runbooks to start and stop classic virtual machines.  You can use this solution for any of the following:  

- Use the runbooks without modification in your own environment. 
- Modify the runbooks to perform customized functionality.  
- Call the runbooks from another runbook as part of an overall solution. 
- Use the runbooks as tutorials to learn runbook authoring concepts. 

<!-- deleted by customization 
> [AZURE.SELECTOR]
- [Graphical](/documentation/articles/automation-solution-startstopvm-graphical)
- [PowerShell Workflow](/documentation/articles/automation-solution-startstopvm-psworkflow)

This is the PowerShell Workflow runbook version of this solution. It is also available using [graphical runbooks](/documentation/articles/automation-solutions-startstopvm-graphical).
-->

## Getting the solution

This solution consists of two PowerShell Workflow runbooks that you can download from the following links. <!-- deleted by customization  See the [graphical version](/documentation/articles/automation-solutions-startstopvm-graphical) of this solution for links to the graphical runbooks. -->

| Runbook | Link | Type | Description |
|:---|:---|:---|:---|
| Start-AzureVMs | [Start Azure Classic VMs](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell Workflow | Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name. |
| Stop-AzureVMs | [Stop Azure Classic VMs](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell Workflow | Stops all virtual machines in an automation account or all virtual machines with a particular service name.  |

<!-- keep by customization: begin -->
[AZURE.INCLUDE [automation-azurechinacloud-environment-parameter](../includes/automation-azurechinacloud-environment-parameter.md)]
<!-- keep by customization: end -->
## Installing the solution

### 1. Install the runbooks

After downloading the runbooks, you can import them using the procedure in [Importing a Runbook](http://msdn.microsoft.com/zh-cn/library/dn643637.aspx#ImportRunbook).

### 2. Review the description and requirements
The runbooks include commented help text that includes a description and required assets.  You can also get the same information from this article. 

### 3. Configure assets
The runbooks require the following assets that you must create and populate with appropriate values.

| Asset Type | Asset Name | Description |
|:---|:---|:---|:---|
| Credential | AzureCredential | Contains credentials for an account that has authority to start and stop virtual machines in the Azure subscription.  Alternatively, you can specify another credential asset in the **Credential** parameter of the **Add-AzureAccount** activity. |
| Variable | AzureSubscriptionId | Contains the subscription ID of your Azure subscription. |

## Using the solution

### Parameters

The runbooks each have the following parameters.  You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.

| Parameter | Type | Mandatory | Description |
|:---|:---|:---|:---|
| ServiceName | string | No | If a value is provided, then all virtual machines with that service name are started or stopped.  If no value is provided, then all classic virtual machines in the Azure subscription are started or stopped. |
| AzureSubscriptionIdAssetName | string | No | Contains the name of the [variable asset](#installing-the-solution) that contains the subscription ID of your Azure subscription.  If you don't specify a value, *AzureSubscriptionId* is used.  |
| AzureCredentialAssetName | string | No | Contains the name of the [credential asset](#installing-the-solution) that contains the credentials for the runbook to use.  If you don't specify a value, *AzureCredential* is used.  |

### Starting the runbooks

You can use any of the methods in [Starting a runbook in Azure Automation](/documentation/articles/automation-starting-a-runbook) to start either of the runbooks in this solution.

The following sample commands uses Windows PowerShell to run **StartAzureVMs** to start all virtual machines with the service name *MyVMService*.

	$params = @{"ServiceName"="MyVMService"}
	Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### Output

The runbooks will [output a message](/documentation/articles/automation-runbook-output-and-messages) for each virtual machine indicating whether or not the start or stop instruction was successfully submitted.  You can look for a specific string in the output to determine the result for each runbook.  The possible output strings are listed in the following table.

| Runbook | Condition | Message |
|:---|:---|:---|
| Start-AzureVMs | Virtual machine is already running  | MyVM is already running |
| Start-AzureVMs | Start request for virtual machine successfully submitted | MyVM has been started |
| Start-AzureVMs | Start request for virtual machine failed  | MyVM failed to start |
| Stop-AzureVMs | Virtual machine is already running  | MyVM is already stopped |
| Stop-AzureVMs | Start request for virtual machine successfully submitted | MyVM has been started |
| Stop-AzureVMs | Start request for virtual machine failed  | MyVM failed to start |

For example, the following code snippet from a runbook attempts to start all virtual machines with the service name *MyServiceName*.  If any of the start requests fail, then error actions can be taken. 

	$results = Start-AzureVMs -ServiceName "MyServiceName"
	foreach ($result in $results) {
		if ($result -like "* has been started" ) {
			# Action to take in case of success.
		}
		else {
			# Action to take in case of error.
		}
	}


## Detailed breakdown

Following is a detailed breakdown of the runbooks in this solution.  You can use this information to either customize the runbooks or just to learn from them.

### Parameters

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

The workflow starts by getting the values for the [input parameters](#using-the-solution).  If the asset names are not provided then default names are used.

### Output

    # Returns strings with status messages
    [OutputType([String])]

This line declares that the output of the runbook will be a string.  This is not required but is a best practice for when the runbook is used as a [child runbook](/documentation/articles/automation-child-runbooks) so that a parent runbook will know the output type to expect.

### Authentication

	# Connect to Azure and select the subscription to work against
	$Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
	$null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
	$SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

The next lines set the [credentials](/documentation/articles/automation-configuring#configuring-authentication-to-azure-resources) and Azure subscription that will be used for the rest of the runbook.
First we use **Get-AutomationPSCredential** to get the asset that holds the credentials with access to start and stop virtual machines in the Azure subscription. **Add-AzureAccount** then uses this asset to set the credentials.  The output is assigned to a dummy variable so that it isn't included in the runbook output.  

The variable asset with the subscription ID is then retrieved with **Get-AutomationVariable** and the subscription set with **Select-AzureSubscription**.

### Get VMs

	# If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
	{ 
		$VMs = Get-AzureVM -ServiceName $ServiceName
	}
    else 
	{ 
		$VMs = Get-AzureVM
	}

**Get-AzureVM** is used to retrieve the virtual machines the runbook will work with.  If a value is provided in the **ServiceName** input variable, then only the virtual machines with that service name are retrieved.  If **ServiceName** is empty, then all virtual machines are retrieved.

### Start/Stop virtual machines and send output

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
		if ($VM.PowerState -eq "Started")
		{
			# The VM is already started, so send notice
			Write-Output ($VM.InstanceName + " is already running")
		}
		else
		{
			# The VM needs to be started
        	$StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

	        if ($StartRtn.OperationStatus -ne 'Succeeded')
	        {
				# The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
	        }
			else
			{
				# The VM started, so send notice
				Write-Output ($VM.InstanceName + " has been started")
			}
		}
    }

The next lines step through each virtual machine.  First the **PowerState** of the virtual machine is checked to see if it is already running or stopped, depending on the runbook.  If it is already in the target state, then a message is sent to output, and the runbook ends.  If not, then **Start-AzureVM** or **Stop-AzureVM** is used to attempt to start or stop the virtual machine with the result of the request stored to a variable.  A message is then sent to output specifying whether the request to start or stop was submitted successfully.


## Related articles

- [Child runbooks in Azure Automation](/documentation/articles/automation-child-runbooks) 
- [Runbook output and messages in Azure Automation](/documentation/articles/automation-runbook-output-and-messages)