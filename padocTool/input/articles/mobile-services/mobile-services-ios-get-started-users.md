<properties
	pageTitle="Add Authentication to Existing Azure Mobile Services App (iOS) | JavaScript Backend | Windows Azure"
	description="Learn how to use Mobile Services to authenticate users of your iOS app through a variety of identity providers, including Google, Facebook, Twitter, and Microsoft."
	services="mobile-services"
	documentationCenter="ios"
	authors="krisragh"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.date="07/01/2015"
	wacn.date=""/>

# Add Authentication to Existing App

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../includes/mobile-services-selector-get-started-users.md)]

In this tutorial, you add authentication to the [Mobile Services Quick Start tutorial] using a supported identity provider.

We recommend that you complete the [Mobile Services Quick Start tutorial] first. Alternatively, just download the Quick Start iOS project: click Azure Portal > **Mobile Services** > your mobile service > the cloud sign on top left > **iOS** > **Create a New iOS App** > **Download and run your app** > **Objective-C** > **Download**. Remember to click **Create TodoItem Table** before you click **Download**, if you haven't already created the table.

##<a name="register"></a>Register App for Authentication

[AZURE.INCLUDE [mobile-services-register-authentication](../includes/mobile-services-register-authentication.md)]

##<a name="permissions"></a>Restrict Data Permissions to Authenticated Users

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../includes/mobile-services-restrict-permissions-javascript-backend.md)]

##<a name="add-authentication"></a>Add Authentication to App

[AZURE.INCLUDE [mobile-services-ios-authenticate-app](../includes/mobile-services-ios-authenticate-app.md)]

##<a name="store-authentication"></a>Store Authentication Tokens in App

[AZURE.INCLUDE [mobile-services-ios-authenticate-app-with-token](../includes/mobile-services-ios-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Next Steps

Next, learn [how to use the user ID value to filter returned data](/documentation/articles/mobile-services-javascript-backend-service-side-authorization).

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Next Steps]:#next-steps
[Storing authentication tokens in your app]:#store-authentication

<!-- Images. -->




[4]: ./media/mobile-services-ios-get-started-users/mobile-services-selection.png
[5]: ./media/mobile-services-ios-get-started-users/mobile-service-uri.png







[13]: ./media/mobile-services-ios-get-started-users/mobile-identity-tab.png
[14]: ./media/mobile-services-ios-get-started-users/mobile-portal-data-tables.png
[15]: ./media/mobile-services-ios-get-started-users/mobile-portal-change-table-perms.png


<!-- URLs. -->
[Service-side authorization of Mobile Services users]: /documentation/articles/mobile-services-javascript-backend-service-side-authorization
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Single sign-on for Windows Store apps by using Live Connect]: /documentation/articles/mobile-services-windows-store-dotnet-single-sign-on
[Mobile Services Quick Start tutorial]: /documentation/articles/mobile-services-ios-get-started
[Get started with data]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-with-data-ios
[Get started with authentication]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-with-users-ios
[Get started with push notifications]: /documentation/articles/mobile-services-javascript-backend-windows-store-dotnet-get-started-with-push-ios
[Authorize users with scripts]: /documentation/articles/mobile-services-ios-authorize-users-in-scripts

