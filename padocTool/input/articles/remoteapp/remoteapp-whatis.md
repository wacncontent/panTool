<properties 
	pageTitle="What is Azure RemoteApp?" 
	description="Learn about Azure RemoteApp." 
	services="remoteapp" 
	documentationCenter="" 
	authors="lizap" 
	manager="mbaldwin" 
	editor=""/>

<tags 
	ms.service="remoteapp" 
	ms.date="08/05/2015" 
	wacn.date=""/>

# What is Azure RemoteApp?

Azure RemoteApp brings the functionality of the on-premises Microsoft RemoteApp program, backed by Remote Desktop Services, to Azure. Azure RemoteApp helps you provide secure, remote access to applications from many different user devices.

When you move RemoteApp to Azure, you get to take advantage of the storage, scalability, and global reach of Azure without having to worry about a complex on-premises configuration. Microsoft provides maintenance of Azure, ensuring its reliability, freeing you up to focus on more important issues, like creating the best apps for your business to use. Another advantage of Azure RemoteApp is the accessibility - your users can access RemoteApp programs from Windows, iOS, Mac OS X, and Android devices. They can use your apps in the environment they prefer, while you use the Azure management portal to manage those apps. 

Read on for more information about RemoteApp, or, if we have already convinced you, [try it out now](http://www.windowsazure.cn/services/remoteapp/).

Have questions about Azure RemoteApp? Check out our [FAQ](/documentation/articles/remoteapp-faq).

Azure RemoteApp is part of the [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**New!** Want to learn more about Azure RemoteApp? Or ready to validate RemoteApp at scale? Join our weekly [ask the experts webinar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## RemoteApp collections
There are two kinds of RemoteApp collections:


- A **cloud collection** is hosted in and stores all data for programs in the Azure cloud. Users can access apps by logging in with their Microsoft account or corporate credentials synchronized or federated with Azure Active Directory.
- A **hybrid collection** is hosted in and stores data in the Azure cloud but also lets users access data and resources stored on your local network. Users can access apps by logging in with their corporate credentials synchronized or federated with Azure Active Directory.

### Cloud collection

The [cloud RemoteApp collection](/documentation/articles/remoteapp-create-cloud-deployment) offers a standalone way to host applications in the cloud. A cloud collection exists only in the Azure cloud, as opposed to connecting to your local network.

As part of the RemoteApp trial, we provide you with the Office 365 ProPlus or Office 2013 apps preinstalled and ready to share with your users. If you choose to leverage the available software, you can provision your service quickly.

An additional advantage of using the cloud collection with the Office apps is that the apps and operating system (upon which your service is built) are always kept up to date through regular updates, and Microsoft Anti-Malware endpoint protection provides continuous defense. Your end users use their Microsoft accounts or corporate credentials to access the apps. All that you, the administrator, need to worry about is figuring out who should have access to which apps.

You can also create a cloud collection to share a custom application or set of applications for your users. To do this, you need to [create a custom image](/documentation/articles/remoteapp-imageoptions) (which is how we publish apps to RemoteApp) and simply choose that image (instead of the Office 2013 image) when you create your collection. 

#### When to choose cloud

Choose a cloud collection when the application you want to share does not require a connection to any resource your company's private network (for example, through a VPN device). If the application uses resources on the Internet, OneDrive, or Azure, a cloud collection will work for you. It's also the quickest to create.


### Hybrid collection
The [hybrid RemoteApp collection](/documentation/articles/remoteapp-create-hybrid-deployment) lets you provide both a custom set of applications to your users and access to the data and resources in your local network. Unlike a custom image used with the cloud collection, the image you create for a hybrid collection runs apps in a domain-joined environment, granting full access to your local network and data.

By integrating Active Directory with Azure Active Directory (using DirSync), your users can use their corporate credentials to access apps and data. When you use a work account in Active Directory, you can take your corporate policies into the cloud to control the apps you offer through RemoteApp.

As long as you build your template image on Windows Server 2012 R2 with the RD Session Host role service, there are few limits on the apps you can publish for your users. If the apps function properly in that template image environment, your end users can access them through RemoteApp. 

#### When to choose hybrid

Choose a hybrid collection if you require a connection to resources on your company's private network. For example, if the application needs access to one of the following:

- File servers located on your intranet
- Quicken
- Databases behind a firewall

This is generally more useful for large companies with lots of resources on their private networks that can't be moved to the cloud.

### Updating your collection
One of the key differences between the hybrid and cloud collections is how software updates are handled. With a cloud collection that uses the preinstalled Office 365 ProPlus or Office 2013 image, you do not have to worry about any updates. The service maintains itself and rolls out updates on an ongoing basis, to both apps and the operating system.

For hybrid collections, as well as cloud collections that use a custom template image, you are in charge of maintaining the image and apps. For domain-joined images, you can control updates by using tools such as Windows Update, Group Policy, or System Center.

After you update your custom template image, you upload the new image to the Azure cloud and then update the collection to use the new image. (You can do this from the RemoteApp **Quick Start** page or the Dashboard.)

See [Update your collection](/documentation/articles/remoteapp-update) for more information.

## Supported RemoteApp clients
Azure RemoteApp is supported on the RemoteApp client apps for Windows and Windows RT, as well as the Microsoft Remote Desktop apps for Mac, iOS and Android. Your users can use these apps on their mobile or compute devices to access the new RemoteApp programs.

See [Accessing your apps in Azure RemoteApp](/documentation/articles/remoteapp-clients) for more information about the clients.

## Next steps
Go! Try it out! These articles help get you started with RemoteApp:

- [Create a RemoteApp image](/documentation/articles/remoteapp-imageoptions)
- [How to create a cloud collection of RemoteApp](/documentation/articles/remoteapp-create-cloud-deployment)
- [How to create a hybrid collection of RemoteApp](/documentation/articles/remoteapp-create-hybrid-deployment)
- [How does licensing work in RemoteApp?](/documentation/articles/remoteapp-licensing)
- [Best practices for using Azure RemoteApp](/documentation/articles/remoteapp-bestpractices)
- [Azure RemoteApp FAQ](/documentation/articles/remoteapp-faq)
 