<properties 
	pageTitle="How to Use Azure Redis Cache | Azure" 
	description="Learn how to improve the performance of your Azure applications with Azure Redis Cache" 
	services="redis-cache,app-service" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags
	ms.service="cache"
	ms.date="06/09/2016"
	wacn.date=""/>

# How to Use Azure Redis Cache

> [AZURE.SELECTOR]
- [.NET](/documentation/articles/cache-dotnet-how-to-use-azure-redis-cache/)
- [ASP.NET](/documentation/articles/cache-web-app-howto/)
- [Node.js](/documentation/articles/cache-nodejs-get-started/)
- [Java](/documentation/articles/cache-java-get-started/)
- [Python](/documentation/articles/cache-python-get-started/)

This guide shows you how to get started using **Azure Redis Cache**. Azure Redis Cache is based on the popular open source Redis Cache. It gives you access to a secure, dedicated Redis cache, managed by Azure.cn. A cache created using Azure Redis Cache is accessible from any application within Azure.

Azure Redis Cache is available in the following tiers:

-	**Basic** - Single node. Multiple sizes up to 53 GB.
-	**Standard** - Two-node Primary/Replica. Multiple sizes up to 53 GB. 99.9% SLA.
-	**Premium** - Two-node Primary/Replica with up to 10 shards. Multiple sizes from 6 GB to 530 GB (contact us for more). All Standard tier features and more including support for [Redis cluster](/documentation/articles/cache-how-to-premium-clustering/), [Redis persistence](/documentation/articles/cache-how-to-premium-persistence/), and [Azure Virtual Network](/documentation/articles/cache-how-to-premium-vnet/). 99.9% SLA.

Each tier differs in terms of features and pricing. For information on pricing, see [Cache Pricing Details][].

This guide shows you how to use the [StackExchange.Redis][] client using C\# code. The scenarios covered include **creating and configuring a cache**, **configuring cache clients**, and **adding and removing objects from the cache**. For more information on using Azure Redis Cache, refer to the [Next Steps][] section. For a step-by-step tutorial of building an ASP.NET MVC web app with Redis Cache, see [How to create a Web App with Redis Cache](/documentation/articles/cache-web-app-howto/).

## <a name="getting-started-cache-service"></a> Get Started with Azure Redis Cache

Getting started with Azure Redis Cache is easy. To get started, you provision and configure a cache. Next, you configure the cache clients so they can access the cache. Once the cache clients are configured, you can begin working with them.

-	[Create the cache][]
-	[Configure the cache clients][]

## <a name="create-cache" id="create-a-cache"></a> Create a cache

To create a cache, first sign in to the [Azure Portal](https://portal.azure.cn), and click **New**, **Data + Storage**, **Redis Cache**.

>[AZURE.NOTE] If you don't have an Azure account, you can [Open an Azure account for free](/pricing/1rmb-trial/?WT.mc_id=redis_cache_hero) in just a couple of minutes.

![New cache](../../includes/media/redis-cache-create/redis-cache-new-cache-menu.png)

>[AZURE.NOTE] In addition to creating caches in the Azure Portal, you can also create them using ARM templates, PowerShell, or Azure CLI.
>
>-	To create a cache using ARM templates, see [Create a Redis cache using a template](/documentation/articles/cache-redis-cache-arm-provision/).
>-	To create a cache using Azure PowerShell, see [Manage Azure Redis Cache with Azure PowerShell](/documentation/articles/cache-howto-manage-redis-cache-powershell/).
>-	To create a cache using Azure CLI, see [How to create and manage Azure Redis Cache using the Azure Command-Line Interface (Azure CLI)](/documentation/articles/cache-manage-cli/).

In the **New Redis Cache** blade, specify the desired configuration for the cache.

![Create cache](../../includes/media/redis-cache-create/redis-cache-cache-create.png) 

-	In **Dns name**, enter a cache name to use for the cache endpoint. The cache name must be a string between 1 and 63 characters and contain only numbers, letters, and the `-` character. The cache name cannot start or end with the `-` character, and consecutive `-` characters are not valid.
-	For **Subscription**, select the Azure subscription that you want to use for the cache. If your account has only one subscription, it will be automatically selected and the **Subscription** drop-down will not be displayed.
-	In **Resource group**, select or create a resource group for your cache. For more information, see [Using Resource groups to manage your Azure resources](/documentation/articles/resource-group-overview). 
-	Use **Location** to specify the geographic location in which your cache is hosted. For the best performance, Microsoft strongly recommends that you create the cache in the same region as the cache client application.
-	Use **Pricing Tier** to select the desired cache size and features.
-	**Redis cluster** allows you to create caches larger than 53 GB and to shard data across multiple Redis nodes. For more information, see [How to configure clustering for a Premium Azure Redis Cache](/documentation/articles/cache-how-to-premium-clustering/).
-	**Redis persistence** offers the ability to persist your cache to an Azure Storage account. For instructions on configuring persistence, see [How to configure persistence for a Premium Azure Redis Cache](/documentation/articles/cache-how-to-premium-persistence/).
-	**Virtual Network** provides enhanced security and isolation by restricting access to your cache to only those clients within the specified Azure Virtual Network. You can use all the features of VNet such as subnets, access control policies, and other features to further restrict access to Redis. For more information, see [How to configure Virtual Network support for a Premium Azure Redis Cache](/documentation/articles/cache-how-to-premium-vnet/).

Once the new cache options are configured, click **Create**. It can take a few minutes for the cache to be created. To check the status, you can monitor the progress on the startboard. After the cache has been created, your new cache has a **Running** status and is ready for use with default settings.

![Cache created](../../includes/media/redis-cache-create/redis-cache-cache-created.png)



### To access your cache after it's created

Caches can be accessed in the [Azure Portal](https://portal.azure.cn) using the **Browse** blade.

![Azure Redis Cache Browse Blade](../../includes/media/redis-cache-browse/redis-cache-browse.png)

Click **Browse > Redis Caches** to view your caches. If you have recently browsed to a Redis Cache, you can click **Redis Caches** directly from the list without clicking **Browse**.

Select the desired cache to view the properties for that cache.

![Azure Redis Cache Browse Cache List](../../includes/media/redis-cache-browse/redis-caches.png)

Click **Settings** or **All settings** to view and configure your cache.

![Redis Cache All Settings](../../includes/media/redis-cache-browse/redis-cache-blade.png)

For more information about configuring your cache, see [How to configure Azure Redis Cache](/documentation/articles/cache-configure/).

## <a name="NuGet"></a> Configure the cache clients

.NET applications can use the **StackExchange.Redis** cache client, which can be configured in Visual Studio using a NuGet package that simplifies the configuration of cache client applications. 

>[AZURE.NOTE] For more information, see the [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github page and  the [StackExchange.Redis cache client documentation](http://github.com/StackExchange/StackExchange.Redis#documentation).

To configure a client application in Visual Studio using the StackExchange.Redis NuGet package, right-click the project in **Solution Explorer** and choose **Manage NuGet Packages**. 

![Manage NuGet packages](../../includes/media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Type **StackExchange.Redis** or **StackExchange.Redis.StrongName** into the search text box, select the desired version from the results, and click **Install**.

>[AZURE.NOTE] If you prefer to use a strong-named version of the **StackExchange.Redis** client library, choose **StackExchange.Redis.StrongName**; otherwise choose **StackExchange.Redis**.

![StackExchange.Redis NuGet package](../../includes/media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

The NuGet package downloads and adds the required assembly references for your client application to access Azure Redis Cache with the StackExchange.Redis cache client.

>[AZURE.NOTE] If you have previously configured your project to use StackExchange.Redis, you can check for updates to the package from the **NuGet Package Manager**. To check for and install updated versions of the StackExchange.Redis NuGet package, click **Updates** in the the **NuGet Package Manager** window. If an update to the StackExchange.Redis NuGet package is available, you can update your project to use the updated version.



Once your client project is configured for caching, you can use the techniques described in the following sections for working with your cache.

## <a name="working-with-caches"></a> Working with Caches

The steps in this section describe how to perform common tasks with Cache.

-	[Connect to the cache][]
-   [Add and retrieve objects from the cache][]
-   [Work with .NET objects in the cache](#work-with-net-objects-in-the-cache)

## <a name="connect-to-cache" id="connect-to-the-cache"></a> Connect to the cache

In order to programmatically work with a cache, you need a reference to the cache. Add the following to the top of any file from which you want to use the StackExchange.Redis client to access an Azure Redis Cache.

    using StackExchange.Redis;

>[AZURE.NOTE] The StackExchange.Redis client requires .NET Framework 4 or higher.

The connection to the Azure Redis Cache is managed by the `ConnectionMultiplexer` class. This class is designed to be shared and reused throughout your client application, and does not need to be created on a per operation basis. 

To connect to an Azure Redis Cache and be returned an instance of a connected `ConnectionMultiplexer`, call the static `Connect` method and pass in the cache endpoint and key like the following example. Use the key generated from the Azure Portal as the password parameter.

	ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.chinacloudapi.cn,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Warning: Never store credentials in source code. To keep this sample simple, I'm showing them in the source code. See [How Application Strings and Connection Strings Work][] for information on how to store credentials.

If you don't want to use SSL, either set `ssl=false` or omit the `ssl` parameter.

>[AZURE.NOTE] The non-SSL port is disabled by default for new caches. For instructions on enabling the non-SSL port, see the [Access Ports](/documentation/articles/cache-configure/#access-ports)..

One approach to sharing a `ConnectionMultiplexer` instance in your application is to have a static property that returns a connected instance, similar to the following example. This provides a thread-safe way to initialize only a single connected `ConnectionMultiplexer` instance. In these examples `abortConnect` is set to false, which means that the call will succeed even if a connection to the Azure Redis Cache is not established. One key feature of `ConnectionMultiplexer` is that it will automatically restore connectivity to the cache once the network issue or other causes are resolved.

	private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
	{
	    return ConnectionMultiplexer.Connect("contoso5.redis.cache.chinacloudapi.cn,abortConnect=false,ssl=true,password=...");
	});
	
	public static ConnectionMultiplexer Connection
	{
	    get
	    {
	        return lazyConnection.Value;
	    }
	}

For more information on advanced connection configuration options, see [StackExchange.Redis configuration model][].

To connect to an Azure Redis Cache instance, cache clients need the host name, ports, and keys of the cache. Some clients may refer to these items by slightly different names. To retrieve these items, [browse](/documentation/articles/cache-configure/#configure-redis-cache-settings) to your cache in the [Azure portal](https://portal.azure.cn) and click **Settings** or **All settings**.

![Redis cache settings](../../includes/media/redis-cache-access-keys/redis-cache-settings.png)

### Host name and ports

To access the host name and ports click **Properties**.

![Redis cache properties](../../includes/media/redis-cache-access-keys/redis-cache-properties.png)

### Access keys

To retrieve the access keys, click **Access keys**.

![Redis cache access keys](../../includes/media/redis-cache-access-keys/redis-cache-access-keys.png)

Once the connection is established, return a reference to the redis cache database by calling the `ConnectionMultiplexer.GetDatabase` method. The object returned from the `GetDatabase` method is a lightweight pass-through object and does not need to be stored.

	// Connection refers to a property that returns a ConnectionMultiplexer
	// as shown in the previous example.
	IDatabase cache = Connection.GetDatabase();

	// Perform cache operations using the cache object...
	// Simple put of integral data types into the cache
	cache.StringSet("key1", "value");
	cache.StringSet("key2", 25);

	// Simple get of data types from the cache
	string key1 = cache.StringGet("key1");
	int key2 = (int)cache.StringGet("key2");

Now that you know how to connect to an Azure Redis Cache instance and return a reference to the cache database, let's take a look at working with the cache.

## <a name="add-object" id="add-and-retrieve-objects-from-the-cache"></a> Add and retrieve objects from the cache

Items can be stored in and retrieved from a cache by using the `StringSet` and `StringGet` methods.

	// If key1 exists, it is overwritten.
	cache.StringSet("key1", "value1");

	string value = cache.StringGet("key1");

Redis stores most data as Redis strings, but these strings can contain many types of data, including serialized binary data, which can be used when storing .NET objects in the cache.

When calling `StringGet`, if the object exists, it is returned, and if it does not, `null` is returned. In this case you can retrieve the value from the desired data source and store it in the cache for subsequent use. This is known as the cache-aside pattern.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

To specify the expiration of an item in the cache, use the `TimeSpan` parameter of `StringSet`.

	cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="store-session" id="work-with-net-objects-in-the-cache"></a> Work with .NET objects in the cache

Azure Redis Cache can cache .NET objects as well as primitive data types, but before a .NET object can be cached it must be serialized. This is the responsibility of the application developer, and gives the developer flexibility in the choice of the serializer.

One simple way to serialize objects is to use the `JsonConvert` serialization methods in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) and serialize to and from JSON. The following example shows a get and set using an `Employee` object instance.


	class Employee
	{
	    public int Id { get; set; }
	    public string Name { get; set; }
	
	    public Employee(int EmployeeId, string Name)
	    {
	        this.Id = EmployeeId;
	        this.Name = Name;
	    }
	}

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

## <a name="next-steps"></a> Next Steps

Now that you've learned the basics, follow these links to learn more about Azure Redis Cache.

-	Check out the ASP.NET providers for Azure Redis Cache.
	-	[Azure Redis Session State Provider](/documentation/articles/cache-aspnet-session-state-provider/)
	-	[Azure Redis Cache ASP.NET Output Cache Provider](/documentation/articles/cache-aspnet-output-cache-provider/)
-	[Enable cache diagnostics](/documentation/articles/cache-how-to-monitor/#enable-cache-diagnostics) so you can [monitor](/documentation/articles/cache-how-to-monitor/) the health of your cache. You can view the metrics in the Azure Portal and you can also [download and review](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) them using the tools of your choice.
-	Check out the [StackExchange.Redis cache client documentation][].
	-	Azure Redis Cache can be accessed from many Redis clients and development languages. For more information, see [http://redis.io/clients][].
-	Azure Redis Cache can also be used with third-party services and tools such as Redsmin and Redis Desktop Manager.
	-	For more information about Redsmin, see  [How to retrieve an Azure Redis connection string and use it with Redsmin][].
	-	Access and inspect your data in Azure Redis Cache with a GUI using [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-	See the [redis][] documentation and read about [redis data types][] and [a fifteen minute introduction to Redis data types][].



<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect to the cache]: #connect-to-cache
[Add and retrieve objects from the cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: /documentation/services/redis-cache
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: /documentation/articles/cache-aspnet-session-state-provider/
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/zh-cn/library/azure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/zh-cn/library/azure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/zh-cn/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/zh-cn/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/zh-cn/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: /documentation/articles/cache-dotnet-how-to-use-azure-redis-cache/#working-with-caches


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: /pricing/details/redis-cache/
[Azure Portal]: https://portal.azure.cn/

[Overview of Azure Redis Cache]: /documentation/services/redis-cache
[Azure Redis Cache]: /documentation/services/redis-cache

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: /documentation/articles/resource-group-overview/

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction to Redis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


