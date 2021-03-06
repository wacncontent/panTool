<properties
	pageTitle="Add Mobile Services to an existing app (Xamarin.Android) | Windows Azure"
	description="Learn how to store and access data from your Azure Mobile Services Xamarin.Android app."
	documentationCenter="xamarin"
	authors="ggailey777"
	manager="dwrede"
	services="mobile-services"
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.date="08/18/2015" 
	wacn.date=""/>

# Add Mobile Services to an existing app

[AZURE.INCLUDE [mobile-services-selector-get-started-data](../includes/mobile-services-selector-get-started-data.md)]	

This topic shows you how to use Azure Mobile Services to leverage data in a Xamarin.Android app. In this tutorial, you will download an app that stores data in memory, create a new mobile service, integrate the mobile service with the app, and then login to the Azure Management Portal to view changes to data made when running the app.

> [AZURE.NOTE] This tutorial is intended to help you better understand how Mobile Services enables you to use Azure to store and retrieve data from a Xamarin.Android app. As such, this topic walks you through many of the steps that are completed for you in the Mobile Services quickstart. If this is your first experience with Mobile Services, consider first completing the tutorial [Get started with Mobile Services](/develop/mobile/tutorials/get-started-xamarin-android).

This tutorial walks you through these basic steps:

1. [Download the Xamarin.Android app project][GitHub] 
2. [Create the mobile service]
3. [Add a data table for storage]
4. [Update the app to use Mobile Services]
5. [Test the app against Mobile Services]

> [AZURE.IMPORTANT] To complete this tutorial, you need an Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see [Azure Trial](/pricing/1rmb-trial target="_blank).

This tutorial requires the [Azure Mobile Services Component], [Xamarin.Android], and Android SDK 4.2 or a later version. 

> [AZURE.NOTE] The downloaded GetStartedWithData project requires targetting Android 4.2 or a later version. However, the Mobile Services SDK requires only Android 2.2 or a later version.

## <a name="download-app"></a>Download the GetStartedWithData project

This tutorial is built on the [GetStartedWithData app][GitHub], which is a Xamarin.Android app. The UI for this app is identical to the app generated by the Mobile Services Android quickstart, except that added items are stored locally in memory. 

1. Download the `GetStartedWithData` sample app and extract the files on your computer. 

2. In Xamarin Studio, click **File** then **Open**, browse to where you extracted the GetStartedWithData sample project, and select **XamarinTodoQuickStart.Android.sln** and open it.

3. Locate and open the **TodoActivity** class

   	Notice that there are `// TODO::` comments that specify the steps you must take to make this app work with your mobile service.

5. From the **Run** menu, click **Start Without Debugging**, you will then be asked to pick an emulator or attached USB Android device.

	> [AZURE.IMPORTANT] You can run this project using an Android phone, or using the Android emulator. Running with an Android phone  requires you to download a phone-specific USB driver.
	> 
	> To run the project in the Android emulator, you must define a least one Android Virtual Device (AVD). Use the AVD Manager to create and manage these devices.

6. In the app, type meaningful text, such as _Complete the tutorial_, and then click **Add**.

   	![][13]

   	Notice that the saved text is stored in an in-memory collection and displayed in the list below.

## <a name="create-service"></a>Create a new mobile service in the Management Portal

[AZURE.INCLUDE [mobile-services-create-new-service-data](../includes/mobile-services-create-new-service-data.md)]

## <a name="add-table"></a>Add a new table to the mobile service

To be able to store app data in the new mobile service, you must first create a new table.  

1. In the Management Portal, click **Mobile Services**, and then click the mobile service that you just created.

2. Click the **Data** tab, then click **+Create**.

   	![][5]

   	This displays the **Create new table** dialog.

3. In **Table name** type _TodoItem_, then click the check button.

  	![][6]

  	This creates a new storage table **TodoItem** with the default permissions set, which means that any user of the app can access and change data in the table. 

    > [AZURE.NOTE] The same table name is used in Mobile Services quickstart. However, each table is created in a schema that is specific to a given mobile service. This is to prevent data collisions when multiple mobile services use the same database.

4. Click the new **TodoItem** table and verify that there are no data rows.

5. Click the **Columns** tab and verify that there is only a single **id** column, which is automatically created for you.

  	This is the minimum requirement for a table in Mobile Services. 

    > [AZURE.NOTE] When dynamic schema is enabled on your mobile service, new columns are created automatically when JSON objects are sent to the mobile service by an insert or update operation.

You are now ready to use the new mobile service as data storage for the app.

## <a name="update-app"></a>Update the app to use the mobile service for data access

Now that your mobile service is ready, you can update the app to store items in Mobile Services instead of the local collection. 

1. If you don't already have **Azure Mobile Services** listed in the Components folder, you can get it by right-clicking **Components**, choosing **Get More Components** and then searching for **Azure Mobile Services**.

  	This adds the Mobile Services SDK component to the project.

2. Open the **AndroidManifest.xml** file and ensure the following permission line exists:

		<uses-permission android:name="android.permission.INTERNET" />

  	This enables the app to access Mobile Services in Azure.

3. From the **Solution** window, open the **TodoActivity** class, and uncomment the following line of code: 

		using Microsoft.WindowsAzure.MobileServices;
 
4. We will remove the in-memory list currently used by the app, so we can replace it with a mobile service. In the **TodoActivity** class, comment out the following line of code, which defines the existing **todoItemList** list.

		public List<TodoItem> todoItemList = new ArrayList<TodoItem>();

5. Once the previous step is done, the project will indicate build errors. Search for the three remaining locations where the `todoItemList` variable is used and comment out the sections indicated. 

6. We now add our mobile service. Uncomment the following lines of code:

        private MobileServiceClient client; // Mobile Service Client references
        private IMobileServiceTable<TodoItem> todoTable; // Mobile Service Table used to access data   

7. In the Management Portal, click **Mobile Services**, and then click the mobile service you just created.

8. Click the **Dashboard** tab and make a note of the **Site URL**, then click **Manage keys** and make a note of the **Application key**.

   	![][8]

  	You will need these values when accessing the mobile service from your app code.

9. In the **Constants** class, uncomment the following member variables:

        public const string ApplicationURL = @"AppUrl";
        public const string ApplicationKey = @"AppKey";
        
10. Replace **AppUrl** and **AppKey** in the above variables with the values retrieved from the Management Portal above.

11. In the **OnCreate** method, uncomment the following lines of code that define the **MobileServiceClient** variable:

		// Create the Mobile Service Client instance, using the provided
		// Mobile Service URL and key
		client = new MobileServiceClient(
			Constants.ApplicationURL,
			Constants.ApplicationKey).WithFilter(filter);

		// Get the Mobile Service Table instance to use
		todoTable = client.GetTable<TodoItem>();    

  	This creates a new instance of MobileServiceClient that is used to access your mobile service. It also creates the MobileServiceTable instance that is used to proxy data storage in the mobile service.

12. Find the ProgressFilter class at the bottom of the file and uncomment it. This class displays a 'loading' indicator while MobileServiceClient is running network operations.

13. Uncommment these lines of the **CheckItem** method:

		try {
			await todoTable.UpdateAsync(item);
			if (item.Complete)
				adapter.Remove(item);
		} catch (Exception e) {
			CreateAndShowDialog(e, "Error");
		}

   	This sends an item update to the mobile service and removes checked items from the adapter.
    
14. Uncommment these lines of the **AddItem** method:
	
		try 
		{
			// Insert the new item
			await todoTable.InsertAsync(item);

			if (!item.Complete) 
				adapter.Add(item);			
		} 
		catch (Exception e) 
		{
			CreateAndShowDialog(e, "Error");
		}   		

  	This code creates a new item and inserts it into the table in the remote mobile service.

15. Uncommment these lines of the **RefreshItemsFromTableAsync** method:

		try {
			// Get the items that weren't marked as completed and add them in the adapter
			var list = await todoTable.Where(item => item.Complete == false).ToListAsync ();

			adapter.Clear();

			foreach (TodoItem current in list)
				adapter.Add(current);
		} 
        catch (Exception e) 
        {
			CreateAndShowDialog(e, "Error");
		}

	This queries the mobile service and returns all items that are not marked as complete. Items are added to the adapter for binding.
		

Now that the app has been updated to use Mobile Services for backend storage, it's time to test the app against Mobile Services.

## <a name="test-app"></a>Test the app against your new mobile service

1. From the **Run** menu, click **Start Without Debugging** to start the project. You will be asked to pick an existing emulator image or an attached USB Android device.

	This executes your app, built with Xamarin.Android, that uses the client library to send a query that returns items from your mobile service.

2. As before, type meaningful text, then click **Add**.

   	This sends a new item as an insert to the mobile service.

3. In the [Management Portal], click **Mobile Services**, and then click your mobile service.

4. Click the **Data** tab, then click **Browse**.

   	![][9]
  
   	Notice that the **TodoItem** table now contains data, with id values generated by Mobile Services, and that columns have been automatically added to the table to match the TodoItem class in the app.

This concludes the **Get started with data** tutorial for Xamarin.Android.

## Get completed example
Download the [completed example project]. Be sure to update the **applicationURL** and **applicationKey** variables with your own Azure settings. 

## <a name="next-steps"> </a>Next steps

This tutorial demonstrated the basics of enabling a Xamarin.Android app to work with data in Mobile Services. 

Next, consider completing one of the following tutorials that is based on the GetStartedWithData app that you created in this tutorial:

* [Validate and modify data with scripts]
  Learn more about using server scripts in Mobile Services to validate and change data sent from your app.

* [Refine queries with paging]
  Learn how to use paging in queries to control the amount of data handled in a single request.

Once you have completed the data series, try these other Xamarin.Android tutorials:

* [Get started with authentication] 
	Learn how to authenticate users of your app.

* [Get started with push notifications] 
  Learn how to send a very basic push notification to your app with Mobile Services.

<!-- Anchors. -->

[Get the Windows Store app]: #download-app
[Create the mobile service]: #create-service
[Add a data table for storage]: #add-table
[Update the app to use Mobile Services]: #update-app
[Test the app against Mobile Services]: #test-app
[Next Steps]:#next-steps

<!-- Images. -->





[5]: ./media/partner-xamarin-mobile-services-android-get-started-data/mobile-data-tab-empty.png
[6]: ./media/partner-xamarin-mobile-services-android-get-started-data/mobile-create-todoitem-table.png
[8]: ./media/partner-xamarin-mobile-services-android-get-started-data/mobile-dashboard-tab.png
[9]: ./media/partner-xamarin-mobile-services-android-get-started-data/mobile-todoitem-data-browse.png
[13]: ./media/partner-xamarin-mobile-services-android-get-started-data/mobile-quickstart-startup-android.png

<!-- URLs. TODO:: update 'Download the Android app project' download link, 'GitHub', completed project, etc. -->
[Validate and modify data with scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-xamarin-android
[Refine queries with paging]: /develop/mobile/tutorials/add-paging-to-data-xamarin-android
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-xamarin-android
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-xamarin-android
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-xamarin-android

[Azure Management Portal]: https://manage.windowsazure.cn/
[Management Portal]: https://manage.windowsazure.cn/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[Download the Android app project]: http://www.google.com/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331302
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125

[completed example project]: http://go.microsoft.com/fwlink/p/?LinkId=331302
