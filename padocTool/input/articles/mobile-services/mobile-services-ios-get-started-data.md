<properties
	pageTitle="Add Mobile Services to an existing app (iOS) | Mobile Dev Center"
	description="Learn how to get started using Mobile Services to leverage data in your iOS app."
	services="mobile-services"
	documentationCenter="ios"
	authors="krisragh"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="07/01/2015"
	wacn.date=""/>

# Add Mobile Services to an existing app

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../includes/mobile-services-selector-get-started-data.md)]

In this tutorial, you download an existing app that stores data in-memory and change it to work with an Azure mobile service.

Completing the [Quick Start] is a pre-requisite before starting this tutorial. You will re-use the mobile service you create in the Quick Start.

##<a name="download-app"></a>Download GetStartedWithData Project

This tutorial is built on the [GetStartedWithData iOS app]. The app is identical to [Quick Start], except that added items are stored in memory.

Download the [GetStartedWithData iOS app]. In Xcode, open the project and examine **TodoService.m**. There are eight **// TODO** comments that specify the steps to make this app work.

##<a name="update-app"></a>Update App to Use Mobile Service for Data Access

[AZURE.INCLUDE [mobile-services-ios-enable-mobile-service-access](../includes/mobile-services-ios-enable-mobile-service-access.md)]

##<a name="test-app"></a>Test App

1. In Xcode, click **Run** to start the app. Add items to the todo list by typing in text and clicking **+**.

2. In the Azure Management Portal, click **Mobile Services**, and then click your mobile service. Click the **Data** tab, then click **Browse**. Notice that the **TodoItem** table now contains data, with id values generated by Mobile Services, and that columns have been automatically added to the table to match the TodoItem class in the app.

<!-- Anchors. -->
[Download the iOS app project]: #download-app
[Create the mobile service]: #create-service
[Add a data table for storage]: #add-table
[Update the app to use Mobile Services]: #update-app
[Test the app against Mobile Services]: #test-app
[Next Steps]:#next-steps

<!-- Images. -->

[0]: ./media/mobile-services-ios-get-started-data/mobile-quickstart-startup-ios.png







[8]: ./media/mobile-services-ios-get-started-data/mobile-dashboard-tab.png
[9]: ./media/mobile-services-ios-get-started-data/mobile-todoitem-data-browse.png



<!-- URLs. -->
[Validate and modify data with scripts]: /documentation/articles/mobile-services-windows-store-dotnet-validate-modify-data-server-scripts
[Refine queries with paging]: /documentation/articles/mobile-services-ios-add-paging-data
[Get started with Mobile Services]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-ios
[Get started with data]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-with-data-ios
[Get started with push notifications]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-with-push-ios

[Azure Management Portal]: https://manage.windowsazure.cn/
[Management Portal]: https://manage.windowsazure.cn/
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[GitHub]:  http://go.microsoft.com/fwlink/p/?LinkId=268622
[GitHub repo]: http://go.microsoft.com/fwlink/p/?LinkId=268784
[Quick Start]: /documentation/articles/mobile-services-javascript-backend-ios-get-started
[GetStartedWithData iOS app]: http://go.microsoft.com/fwlink/p/?LinkId=268622