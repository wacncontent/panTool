<!-- rename to virtual-machines-windows-classic-connect-logon -->

<properties
	pageTitle="Log on to a VM | Azure"
	description="Use the Azure classic portal to log on to a Windows virtual machine created with the classic deployment model."
	services="virtual-machines"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.date="10/05/2015"
	wacn.date=""/>


# Log on to a Windows virtual machine using the Azure classic portal



In the Azure classic portal, you use the **Connect** button to start a Remote Desktop session and log on to a Windows VM.

Do you want to connect to a Linux VM? See [How to log on to a virtual machine running Linux](/documentation/articles/virtual-machines-linux-classic-log-on/).

> [AZURE.IMPORTANT] Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](/documentation/articles/resource-manager-deployment-model/).  This article covers using the classic deployment model. Azure recommends that most new deployments use the Resource Manager model.

## Connect to the virtual machine

1. Sign in to the Azure classic portal.

2. Click **Virtual Machines**, and then select the virtual machine.

3. On the command bar at the bottom of the page, click **Connect**.

	![Log on to the virtual machine](./media/virtual-machines-log-on-windows-server/connectwindows.png)
	
> [AZURE.TIP] If the Connect button isn't available, see the troubleshooting tips at the end of this article.

## Log on to the virtual machine

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## Troubleshooting tips

Here are a few things to try:

-	If the **Connect** button is inactive or you are having other problems with the Remote Desktop connection, try resetting the configuration. From the virtual machine dashboard, under **Quick Glance**, click **Reset remote configuration**.
-	For problems with your password, try resetting it. From the virtual machine dashboard, under **Quick Glance**, click **Reset password**.

If those tips don't work or aren't what you need, see [Troubleshoot Remote Desktop connections to a Windows-based Azure Virtual Machine](/documentation/articles/virtual-machines-windows-troubleshoot-rdp-connection/). This article walks you through diagnosing and resolving common problems.


